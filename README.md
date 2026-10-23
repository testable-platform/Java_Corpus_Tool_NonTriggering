# Testable Java corpus — JV_V9_GRADLEGROOVYDSL_THINJAR_MICRO

Grid cell `GRG-THIN-S` of the 24-cell Java grid.

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
| Java version | 9 |
| Host JDK | 11 |
| Build system | Gradle / Groovy DSL |
| Packaging | Thin jar |
| Architecture | Microservices |

## Build

```
./gradlew build
```

Main and test sources both compile at Java 9 (bytecode major version 53). The code
uses private interface methods, `Set.of`, `Optional.or`, `Optional.stream`,
`Stream.takeWhile`, try-with-resources on an effectively-final variable and the diamond
operator on an anonymous class, so it fails to compile under `--release 8` — the version differentiation is real, not
declared.

Produces: `dist/jv-134.jar or target/jv-134-1.0.0.jar`

## Run

```
java -jar <artifact> O-1234
```

## Test

```
./gradlew test
```

## Workspace projects

- `jv-134-domain/`
- `jv-134-pricing/`
- `jv-134-risk/`
- `jv-134-catalog/`


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

* **8 tools read source**, so they get their own minimal program, for java9 -- one class or one function, no branching, no
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
