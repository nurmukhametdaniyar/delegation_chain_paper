# Generated tables and figures — do not edit

Every file under `tables/` and `figures/` is produced by the benchmark
harness of the reference implementation (`dc-bench paper`,
`crates/dc-bench/src/paper.rs` and `scripts/paper_figures.py`; see
`ARTIFACT.md` in the implementation repository). They are copied here
byte-for-byte so that the paper builds from a clean clone.

- Never edit a cell or a figure by hand. If one is wrong, fix the
  generator, regenerate, and copy again.
- To refresh: copy `paper/tables/` and `paper/figures/` (including both
  `captions.tex` files) from the implementation repository over these
  files, then rebuild.

Copied on 2026-10-04 from implementation commit `25242b8`
("The cost of D-81's encoding checks: an exploratory micro-benchmark
(D-87), not run yet").
