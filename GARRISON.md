# Garrison branch

This branch is upstream pdf-inspector **1.25.2** plus fixes used by [Garrison](https://github.com/edgerunner-ai/Garrison) through [edgerunner-ai/anydoc](https://github.com/edgerunner-ai/anydoc/tree/garrison). The full write-up (symptoms, measurements, and how to update) is in Garrison's [`docs/utils/anydoc-fork.md`](https://github.com/edgerunner-ai/Garrison/blob/feat/anydoc-document-digestion/docs/utils/anydoc-fork.md).

## Fixes

| Commit | Fix |
|---|---|
| `0bb3d21` (+ `2f27e0f` formatting) | Markdown page-number filter: judge turned (`/Rotate`d) pages in their own frame, and keep the numeric cells of table rows in the page margins. Since 1.18.0 a sideways table could lose a whole numeric column. |
| `823a7c1` (+ `814669a` test fixture) | Ruled-line tables up to 25 columns (`MAX_TABLE_COLUMNS`), like the rect and heuristic detectors; the cap was 20. |
| `ca73bdc` (+ `69e35cc` rustfmt) | Thin-rect table fallback: a filled band spanning the grid (shaded header row or column) contributes its edges as rules, so the header no longer absorbs the first data row. |

Each has unit tests. Garrison's `garrison/tests/fixtures/capuldi_2024_p22_landscape_table.pdf` exercises all three.

## Fork-only tooling

- `.github/workflows/garrison-test.yml`: `cargo test`, clippy (as upstream CI) and rustfmt on GitHub-hosted runners, on every push to `garrison`.
- `.github/workflows/garrison-probe.yml` (`43c7b5c`, `83b059e`): builds the Python wheel and `pdf2md.exe` for Windows at any ref. Use it to compare output between commits; `RUST_LOG=debug pdf2md.exe file.pdf` logs table and layout decisions.
- Upstream's workflows need Firecrawl's Blacksmith runners and are disabled on this fork.

## Updating

Rebase this branch onto the new upstream release tag, drop any fix that upstream now covers, and check that the Garrison tests above still pass. Then update the `[patch.crates-io]` rev in the anydoc fork.
