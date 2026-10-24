# Java Corpus -- Tools non triggering (Synthetic Data)

`main` carries only this overview -- it holds no project code. Every other
branch in this repository is one cell of `testable-platform/java-corpus`, split out to carry
only the `Tools non triggering (Synthetic Data)/` folder (plus that branch's own build scaffolding).

This repository was produced by splitting `testable-platform/java-corpus` by task folder.
`testable-platform/java-corpus` itself was not modified by this split and still holds the
full combined corpus.

## Scope

This repo covers the 5 boundary Java families (java9) across the full build-system x packaging x architecture combination (10 branches total). Each branch carries only its own family's data inside `Tools non triggering (Synthetic Data)/` -- not all 18 (or even all 5 boundary) families bundled together.

## Branch naming

Branch names are unchanged from `testable-platform/java-corpus`:

```
JV_V{version}_{BUILD_SYSTEM}_{PACKAGING}_{ARCHITECTURE}
```

## Finding a branch

Each branch's own `README.md` describes its exact cell (Java version, build
system, packaging, architecture) and its tool roster, trimmed to the parts
relevant to `Tools non triggering (Synthetic Data)/`. `dataset.json` on each branch is the
machine-readable answer key for that cell (unmodified by this split -- it
still describes the branch's original full 19-tool/5-folder state).

## History

Each branch's commit history is carried over unchanged from `testable-platform/java-corpus`,
with one additional commit on top that removes the folders not relevant to
this repo. Nothing was squashed or rewritten.
