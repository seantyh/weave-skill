---
name: weave
description: Work with Weave research notebooks. Use when creating, editing, or tracing numbered Jupyter notebooks in a Weave project, including parameterized runs, data dependencies, exported artefacts, and SHA-1 records.
---

# Weave

Weave is a working convention for research notebooks. Treat notebooks and data files as a directed network. A file points to the notebook that consumes it, and a notebook points to the file it produces. Tracing those links lets a reader see where an analysis gets its inputs and where its results go. The goal is to make the research record legible. A SHA-1 records the bytes observed at a path. Recreating a run can also depend on its environment, parameters, and execution state, and how much of that to capture depends on the research task.

## Number notebooks like sections and pages

- Use a prefix such as `00.00` in `00.00-sandbox.ipynb`. The first two digits identify the section and the next two identify the page within it. Start at `00.00` when setting up a series and extend it as needed. Gaps are fine.
- Use an extra component such as `10.10.2` to distinguish a run or variation of `10.10`, for example with different parameters. Some existing notebooks distinguish runs by a descriptive filename suffix or a parameter label. Follow the pattern of the series you are editing.
- Treat the components as hierarchical labels. Determine execution dependencies from actual reads and writes. Keep established notebook identifiers stable during routine edits.

## Record inputs and outputs

- Near the beginning, add a **Data Dependencies** section for the data files the notebook actually reads. Near the end, add an **Export Artefacts** section for the files it writes for later use. Preserve recognizable variations in existing notebooks. A notebook with no file inputs or exports can say so briefly when useful.
- For each file, make its path visible and record its SHA-1 when possible. Use `sha1sum` or a kernel-native equivalent. Hash the input actually selected for the run and hash each output after it has been fully written and closed. With parameterized paths, use the same resolved paths in the hash cells as in the read and write cells.
- If a file is unavailable or a notebook has yet to run, leave the planned path visible and make the hash status explicit. Saved notebook outputs are evidence of a prior run. If the current file differs, report the discrepancy and preserve the prior digest as historical evidence.
- To trace a link, look for a notebook that writes the same repository-relative path, then compare its recorded export digest with the consumer's recorded input digest when both are available. Producer prefixes in data filenames can guide the search. Check the actual path, parameter label, and recorded hashes before calling the link verified.

## Place files

- Put notebook data inputs and exported data files under the repository-root `data/`. Reference them using paths that resolve from the notebook's actual working directory. For example, a notebook running from `etc/base/` can refer to `../../data/`.
- Put shared code and substantial code better kept outside notebook cells under repository-root `src/`. Put notebook runner scripts under repository-root `bin/` when using standalone runners.
- Inspect nearby notebook cells and runner scripts before editing. Older notebooks may keep data or runners elsewhere and may have saved parameterized copies. Follow their paths when tracing past runs. Scope any move or rerun to the work requested.
