# Testable Java corpus — JV_V24_ANTIVY_SHADEDUBERJARRELOCATED_MICRO

Grid cell `ANT-SHADE-S` of the 24-cell Java grid.

## Project type

Order pricing and risk domain. The domain layer is byte-identical across all 24 branches
in this family, so any difference in tool output is attributable to the branch variables
below and not to the code the tool was pointed at.

## Branches

This repository holds 432 orphan branches, one per grid cell across every Java version, build system and packaging combination in this corpus. `main` carries the title
only. See `dataset.json` for the machine-readable description of this branch.

## Branch variables

| Variable | Value |
|---|---|
| Java version | 24 |
| Host JDK | 25 |
| Build system | Ant + Ivy |
| Packaging | Shaded uber-jar (relocated) |
| Architecture | Microservices |

## Build

```
ant clean package
```

Main and test sources both compile at Java 24 (bytecode major version 68), built by
`javac 25` with `--release 24`.

**Java 24 added no final language syntax** - every language JEP it carried was a preview:
primitive types in patterns (2nd), module import declarations (2nd), flexible constructor
bodies (3rd), implicitly declared classes (4th), structured concurrency (4th) and scoped
values (4th). Java 24 is the **sixth** API-only family, after 12, 13, 18, 19, 20 and 23.

What it did finalise is substantial - two APIs that had previewed in 22 and 23:

| Lock | File | Since |
|---|---|---|
| stream gatherers: `Stream.gather`, `Gatherers.windowFixed`/`windowSliding`/`scan`/`fold` | `analysis/LegGatherer.java` | 24 |
| the Class-File API: `java.lang.classfile` | `util/ClassFileProbe.java` | 24 |
| `Reader.of(CharSequence)` | `util/ClassFileProbe.java` | 24 |

26 errors at `--release 23`: 11 in `LegGatherer`, 15 in `ClassFileProbe`. Whole-family and
per-file agree - no parse-time lock to halt on.

### Gatherers reopened a closed part of the stream API

Before Java 24 the intermediate stage of a stream was fixed: `map`, `filter`, `flatMap`,
`mapMulti` and nothing else. A `Collector` could be written by anyone, but an *intermediate*
operation could not, so anything with a window or a running total had to leave the stream, be
done in a loop, and come back. `Gatherers.scan` is the clearest case - stateful and
intermediate, expressible by neither `map` nor a `Collector`. `LegGatherer.runningUnits`
returns one running total per leg in a single pass, and the test asserts that its last element
equals what `Gatherers.fold` produces independently.

### The Class-File API is directly relevant to this corpus's own findings

Every tool in the roster that reads bytecode ships its own copy of a parser, each pinned to a
different version, each needing an update before it can read the next release's class files.
That is precisely the ceiling this corpus keeps measuring: **PIT stops at Java 25 because its
bundled ASM 9.10.1 does, and CK stops at Java 16 because its bundled Eclipse JDT 3.26.0 does.**
A parser maintained inside the JDK cannot fall behind the JDK, which is the whole argument for
JEP 484 - and this family is where the corpus can measure the difference rather than assert it.

`ClassFileProbe` reads a class file's declared major version, name, superclass and members,
and writes one too. The test parses this family's own compiled classes and asserts major
version 68, which is a nice closing of the loop: the branch uses the Java 24 API to confirm
the Java 24 bytecode level it was compiled at.

One incidental finding the test records: the API counts **every declared field**, statics
included, so `ShipmentLeg` reports 5 fields - its 3 record components plus 2 constants - not
the 3 a reader might expect from the record header.

### Fourth and fifth instances of the preview-API diagnostic

At `--release 23` neither lock fails with *"cannot find symbol"*. Both fail with **"is a
preview API and is disabled by default"** - `Gatherers` and `ClassFile` alike, because both
existed in Java 23 as preview. With `String.formatted` at release 14, `ScopedValue` at 21 and
FFM at 21, that is five independent instances across the corpus. The rule is settled: **a
version lock is often a policy error rather than a missing symbol.**

Forward-checked at `--release 25`: clean.

Java 24 is **not an LTS release**. It shipped March 2025 and reached end of life in September
2025, six months later. It is in this corpus to complete the version axis, not as
a recommendation.

Produces: `jv-430-all.jar (shaded, packages relocated)`

## Run

```
java -jar <artifact> O-1234
```

## Test

```
ant test
```

## Workspace projects

- `jv-430-domain/`
- `jv-430-pricing/`
- `jv-430-risk/`
- `jv-430-catalog/`


## Planted CVE pins

| Dependency | Version |
|---|---|
| `commons-collections:commons-collections` | 3.2.1 |
| `org.apache.commons:commons-text` | 1.9 |
| `com.fasterxml.jackson.core:jackson-databind` | 2.9.10.1 |
| `log4j:log4j` | 1.2.17 |
| `org.yaml:snakeyaml` | 1.30 |

Deliberately vulnerable versions, so the SCA metrics have known true positives. Do not
upgrade them.

<!-- tools-non-triggering -->
## Tools non triggering (Synthetic Data)

One of five per-branch data folders in the original combined `testable-platform/java-corpus` repo -- this repo carries only this one.

Clean makes each tool run and report nothing wrong. Invalid makes it run and
report something. This folder holds data with nothing in it for any tool to
catch -- and, where no program would reach the tool at all, nothing for it to
start on. It is the negative control that tells *correctly detected nothing*
apart from *the scan never ran*.

`Tools non triggering (Synthetic Data)/` holds 20 tool-named folders, matching the names in Clean
and Invalid so the data sets line up name-for-name:

* **8 tools read source**, so they get their own minimal program, for java24 -- one class or one function, no branching, no
  duplication, no dependency, no dead export, no magic number.
* **12 tools cannot be answered by a program** -- they read a lockfile,
  a coverage report, compiled bytecode or the commit history -- so they carry
  the subject matter as a plain record instead, with the reason stated in that
  folder's own README.

**No manifest, no lockfile, no tool configuration, no runner and no
`trigger.yaml` anywhere in it**, so the folder adds no discovered project and
no task to a run.

See `Tools non triggering (Synthetic Data)/README.md` for the per-tool table, the mechanism each tool is
inert by, and what was measured.
