# Testable Java corpus — JV_V16_ANTIVY_THINJAR_MONO

Grid cell `ANT-THIN-M` of the 24-cell Java grid.

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
| Java version | 16 |
| Host JDK | 17 |
| Build system | Ant + Ivy |
| Packaging | Thin jar |
| Architecture | Monolith |

## Build

```
ant clean package
```

Main and test sources both compile at Java 16 (bytecode major version 60), built by
`javac 17` with `--release 16`.

**Two more preview arcs land here.** Records (JEP 395) and pattern matching for
{@code instanceof} (JEP 394) both became final in Java 16 after preview in 14 and a second
preview in 15 - the third and fourth three-release arcs in this corpus, after switch
expressions (12, 13, final 14) and text blocks (13, 14, final 15). Four headline features,
four families that could not use them, and the reason java12 and java13 have API-only locks.

The Java 16 lock is deliberately split across three files, one lock kind per file:

| File | Lock | Kind |
|---|---|---|
| `model/ShipmentLeg.java` | record declaration + compact canonical constructor | parse-time |
| `analysis/RouteDescriber.java` | pattern matching for `instanceof` | parse-time |
| `analysis/RoutePlanner.java` | `Stream.toList`, `Stream.mapMulti` | attribution-time |

### Why the split, and why the lock count depends on how you compile

`javac` halts at the first syntax error **in a file**, so an API lock that shares a file with
a syntax lock can never be reported. Compiling the whole family at `--release 15` gives
**2 errors**. Compiling each file separately, with the family's own compiled classes on the
classpath, gives **5**:

```
ShipmentLeg.java     records are not supported in -source 15
RouteDescriber.java  pattern matching in instanceof is not supported in -source 15
RoutePlanner.java    cannot find symbol: method mapMulti(...)
RoutePlanner.java    cannot find symbol: method toList()
RoutePlanner.java    cannot find symbol: method toList()
```

Same source, same release, two and a half times the locks. The whole-family number is not
wrong, it just answers a different question - *does this branch build?* rather than *how many
independent things pin it to this version?* Earlier families measured the second number by
neutralising the syntax lock in a scratch copy, which needs a hand-written stand-in per
family and silently under-reports if the stand-in drifts. Per-file compilation needs nothing
hand-written and cannot drift, so it replaces that step from this family on.

**Sealed types are deliberately absent.** They were in their second preview in Java 16 and
became final in Java 17, so they belong to that family - and they are what java17's
`model/PricingEvent.java` already uses.

Forward-checked at `--release 17, 21` and `25`: clean.

Java 16 is **not an LTS release**. It shipped March 2021 and reached end of life in
September 2021, six months later. It is in this corpus to complete the version axis, not as
a recommendation.

Produces: `dist/jv-283.jar or target/jv-283-1.0.0.jar`

## Run

```
java -jar <artifact> O-1234
```

## Test

```
ant test
```

## Workspace projects

- `src/main/java/` (single module)


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

* **8 tools read source**, so they get their own minimal program, for java16 -- one class or one function, no branching, no
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
