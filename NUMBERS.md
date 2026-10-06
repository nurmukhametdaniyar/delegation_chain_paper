# NUMBERS.md — source audit for every reported number

Every number in the abstract, prose and captions of `paper.tex` that comes from the implementation or the
benchmark is listed here with its source. Generated tables (`generated/tables/*.tex`) and figures
(`generated/figures/*.pdf`) are included unedited and are not re-listed cell by cell.

**Sources.**
- Every row cites the implementation repository (`delegation_chain_impl`) at commit `25242b8`, except rows 140–149.
  - Those cite `c6bbbaa`, the commit that adds the encoding-check run.
  - Its `BENCHMARKS.md` §8 block is identical at `60819a5`, the latest commit on 2026-10-06.
- `BM` = `BENCHMARKS.md` at `25242b8`. Line numbers refer to that file.
- `impl:` = any other file in the implementation repository at `25242b8`.
- `gen:` = `generated/` in this repository, copied byte-for-byte from `impl:paper/` at `25242b8`.
- **Re-checked on 2026-10-06.** Rows 1–113 and 40a–40b were first logged against commit `8ffe91b`, and each was re-checked at `25242b8`.
  - Every cited value is unchanged.
  - Line numbers were updated where the files moved:
    - `BENCHMARKS.md` by 6 lines after its new ratio-figure subsection;
    - `PAPER_ISSUES.md`, `DECISIONS.md` and `scripts/paper_figures.py` by their new text.
  - Lines whose wording changed but whose values did not are rows 61, 104 and 105: Q9's flag text and §7's resume note.
  - `BENCH_PLAN_FROZEN.md`, the test reports and `tests/concurrency.rs` are unchanged between the two commits.
- **The encoding-check run** was committed in `c6bbbaa`, and §8.8 now reports it (rows 140–149).

**Out of scope.** The following are not data and are not listed: section, RFC, algorithm-line and threat
numbers; years; the policy example's literal values, which are unchanged from earlier revisions; and the
40.55% figure, which is cited from Zhou et al. and unchanged.

**Rounding.**
- "Exact" means the paper prints the value exactly as the source prints it.
- For a range, the minimum and maximum cells are named.
- A derived value shows its computation.

## Abstract

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 1 | Abstract | no soundness violations | gen:tables/oracle.tex; impl:docs/test-reports/policy-oracle-m4.json | oracle.tex row "Contains, all well-formed pairs"; JSON `contains.soundness_violations` | 0 | exact |
| 2 | Abstract | 108,370 well-formed scope pairs | same | oracle.tex same row, "Cases"; JSON `contains.wellformed_pairs` | 108370 | exact |
| 3 | Abstract | 12.2–24.2× (with prefix caching) | BM | Summary, l. 7; Verdicts table, B/D column (ll. 55–70) | min 12.198 (large, N=10); max 24.151 (small, N=1) | 1 d.p.; BM Summary prints 12.2–24.2 |
| 4 | Abstract | 4.9–16.0× (every call a new chain) | BM | Summary, l. 7; Verdicts, A/C warm column | min 4.857 (large, N=5); max 15.984 (small, N=1) | 1 d.p.; BM Summary prints 4.9–16.0 |
| 5 | Abstract | at most 8.8% of a chain's bytes | BM | §4 Q2, l. 774; Q2 small table, N=10 (l. 173) | "8.8% smaller (small)"; 2953 against 3237 bytes | 1 − 2953/3237 = 0.0877 → 8.8%. This is the grid maximum (small, N=10). |
| 6 | Abstract | three-agent chain | impl:BENCH_PLAN_FROZEN.md; BM | §3 grid; the N = 3 cells | N = 3 | Derived: N = 3 counts the bodies after the session body, so agents A_1 to A_3. |
| 7 | Abstract | 109 µs | BM | Q1 medium, C warm, N = 3 (l. 96) | 109 [109, 109] | exact |
| 8 | Abstract | 26 µs (prefix cached) | BM | Q1 medium, D warm+prefix, N = 3 (l. 96) | 26.4 [26.4, 26.4] | rounded to the integer in the abstract; §8.4 prints 26.4 |

## §1 Introduction

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 9 | §1, measurement paragraph | one 96-byte signature | BM | §5 claims table, l. 846 | "A's signature bytes are 96 at N = 1 and 96 at N = 10, in every profile" | exact |
| 10 | §1, measurement paragraph | 12.2–24.2× | as #3 | | | as #3 |
| 11 | §1, measurement paragraph | at most 8.8% | as #5 | | | as #5 |

## §2–§7 (sizes and costs)

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 12 | §2.2 Ed25519 | public keys 32 bytes | RFC 8032 (`rfc8032`); BM | RFC 8032 §5.1.5; BM Summary, l. 9 | 32 octets; "Ed25519's are 32" | exact |
| 13 | §2.2 Ed25519; §4.3; Figure 1 box | signatures 64 bytes | RFC 8032; BM | RFC 8032 §5.1.6; BM Q2 "C signatures" column, N=1 (l. 179) | 64 octets; 128 = 2 × 64 at N = 1 | exact |
| 14 | §2.2 BLS; §4.8 | signature in G2, 96 bytes compressed | `ietf-pairing-curves`, `ietf-bls-signature`; BM | BM Q2 "A signatures" column (ll. 164–218) | 96 | exact |
| 15 | §2.2 BLS; §4.8 | public key in G1, 48 bytes | same; BM | BM Summary, l. 9 | "48-byte BLS keys" | exact |
| 16 | §2.2 (l. 275); Figure 1 caption; §4.8 (×2) | one 96-byte G2 element; constant size 96 bytes regardless of N | BM | §5 claims table, l. 846 | 96 at N = 1 and N = 10, every profile | exact |
| 17 | §4.2 | 32-byte SHA-256 digest | FIPS 180-4 | SHA-256 output length | 256 bits | 256/8 = 32 |
| 18 | §4.8, closing sentence | saves only a few percent of a chain's bytes | BM | §4 Q2, ll. 774–775; Q2 medium, N=3 (l. 181) | 2.9% (medium, N=3) to 8.8% (small, N=10) | qualitative; consistent with #5 and #40 |
| 19 | §6.4 after Prop. 2 | 9.0 µs, 4 rules × 4 atoms, worst case | BM | Q7/Q8 table, `policy/contains/worst/r4_a4` (l. 434) | 9.0 [9.0, 9.1] | exact; also §4 Q8, l. 804 |
| 20 | §6.4 after Prop. 2 | 5146 µs at 64 rules × 8 atoms | BM | `policy/contains/worst/r64_a8` (l. 438) | 5146 [5120, 5166] | exact; also l. 804 |

## §8.1 Implementation

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 21 | §8.1 | nine crates | impl:crates/ | directory listing | dc-baselines, dc-bench, dc-cbor, dc-chain, dc-crypto, dc-policy, dc-registry, dc-types, dc-verifier | count = 9 |
| 22 | §8.1 | one operating-system call (the only unsafe code) | impl:MILESTONES.md | M8, l. 316 | "The QoS FFI call in `qos.rs`, the one permitted `unsafe`" | exact; Cargo.toml `unsafe_code = "deny"` |
| 23 | §8.1 | blst 0.3.17; ed25519-dalek 2.2.0; biscuit-auth 6.0.0 | BM | §1 Environment, "Crates" row (l. 21) | blst 0.3.17; ed25519-dalek 2.2.0; biscuit-auth 6.0.0 | exact |
| 24 | §8.1 | rustc 1.97.1 | BM | §1 Environment, "Toolchain" row (l. 20) | rustc 1.97.1 (8bab26f4f 2026-07-14) | exact |
| 25 | §8.1 | 16 specification defects | impl:PAPER_ISSUES.md | "Sources" bullet (l. 11) | P-15 to P-25 (pre-M0 review); P-26; P-27; P-28 and P-29; P-30 | 11 + 1 + 1 + 2 + 1 = 16. P-01 to P-14 predate the implementation and are not counted. |

## §8.2 Correctness Evaluation

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 26 | §8.2 Security suite | each instantiation runs 52 tests, 50 shared and 2 specific to it (54 distinct); all passed under both | impl:DECISIONS.md; impl:MILESTONES.md; impl:tests/security.rs; gen:tables/security.tex | D-84, l. 1039; MILESTONES step 3, l. 527; `#[test]` count; table footer | "Each instantiation runs 52 tests: 50 shared and 2 of its own"; 54 `#[test]` in tests/security.rs; footer "Default: 53 of 53 tests passed. Aggregate variant: 53 of 53 tests passed." | 54 = 50 + 2 + 2. The table's 53 per instantiation is these 52 plus `theorem_5_concurrent_replay` (tests/concurrency.rs), which the paper names separately. The table has 55 rows: 51 shared, 2 default-only, 2 aggregate-only. Updated at `25242b8`. |
| 27 | §8.2 Concurrent replay | 64 threads | impl:docs/test-reports/concurrent-replay-m6.json | `threads` | 64 | exact |
| 28 | §8.2 Concurrent replay | 1,000 rounds | same | `rounds` | 1000 | exact |
| 29 | §8.2 Concurrent replay | 64,000 verifications | same | `verifications` | 64000 | exact (= 64 × 1000) |
| 30 | §8.2 Concurrent replay | every round accepted exactly one copy | same | `rounds_by_accept_count` | {"1": 1000} | exact |
| 31 | §8.2 Concurrent replay | 49,638 at line 17 | same | `rejected_at_line_17` | 49638 | exact |
| 32 | §8.2 Concurrent replay | 13,362 at line 50 | same | `rejected_at_line_50` | 13362 | exact; 49638 + 13362 + 1000 = 64000 |
| 33 | §8.2 Cache equivalence | 10,000 chains per run; no disagreement | impl:docs/test-reports/cache-equivalence-p30-{ed25519-list,bls-aggregate,bls-list}.json; impl:MILESTONES.md | `chains`; MILESTONES l. 336 | 10000 each; "10,000 chains per run, no disagreement in any" | exact |
| 34 | §8.2 Cache equivalence | separate run of 10,000 chains; pairing cache agrees on every one | impl:docs/test-reports/pairing-cache-equivalence-m7.json | `chains`, `agreeing_with_aggregate_verify`, `disagreeing` | 10000; 10000; 0 | exact |
| 35 | §8.2 Oracle | 150,000 generated scope pairs | impl:docs/test-reports/policy-oracle-m4.json | `contains.cases` | 150000 | exact |
| 36 | §8.2 Oracle | 108,370 well-formed | as #2 | | 108370 | exact |
| 37 | §8.2 Oracle | 49,467 malformed scopes rejected | gen:tables/oracle.tex; JSON | oracle.tex reflexivity row; JSON `contains.malformed_scopes_rejected` | 49467 | exact. Counted in scopes, not pairs, so 108,370 + 49,467 ≠ 150,000; the paper says "scopes". |
| 38 | §8.2 Oracle | 250,533 reflexivity checks, no failures | same | `reflexivity_checks`, `reflexivity_failures` | 250533; 0 | exact |
| 39 | §8.2 Oracle | completeness 0.9197 overall | same | `completeness_overall.completeness`; oracle.tex | 0.9196892188161083; table 0.9197 | 4 d.p., as the table |
| 40 | §8.2 Oracle | 0.9991 when no child rule is unsatisfiable | same | `completeness_when_no_child_rule_is_unsatisfiable.completeness` | 0.9991157289709296; table 0.9991 | 4 d.p. |
| 40a | §8.2 Oracle | 4,176 missed containments | impl:docs/test-reports/policy-oracle-m4.json; impl:PAPER_ISSUES.md; impl:MILESTONES.md | JSON `contains.misses_by_cause.total`; PAPER_ISSUES P-27, l. 276; MILESTONES l. 182 | 4176 | Exact. 51998 − 47822 = 4176 (oracle true minus procedure true, `completeness_overall`). |
| 40b | §8.2 Oracle | unsatisfiable child rules account for 4,138 | same | JSON `contains.misses_by_cause.other["child rule unsatisfiable"]`; PAPER_ISSUES P-27, l. 277; MILESTONES l. 183 | 4138 | exact |
| 41 | §8.2 Oracle | half of its pairs are independent | impl:DECISIONS.md | D-58, l. 531 | "half are independent pairs; in the other half, S2 is derived from S1 by mutation" | exact |
| 42 | §8.2 Oracle | three bugs planted, all caught | impl:MILESTONES.md | M4, l. 165 | "three planted bugs, all caught (D-58)" | exact |
| 43 | Table 1 (oracle) caption | seed 56324 | gen:tables/oracle.tex; JSON | header comment; `seed` | 56324 | exact |
| 44 | §8.2 Fuzzing | ten minutes | impl:MILESTONES.md | M8, l. 323 | "A 10-minute run" | exact |
| 45 | §8.2 Fuzzing | 111,517,984 executions, no crash | same | l. 323 | "111,517,984 executions" | exact |

## §8.3 Performance Method

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 46 | §8.3 Workloads | small: one rule, two parameters, one atom | impl:BENCH_PLAN_FROZEN.md | §3, l. 33 | "1 rule, 2 parameters, 1 atom" | exact |
| 47 | §8.3 Workloads | large: 16 rules, 4 parameters, 3 atoms; two approval rules | same | §3, ll. 36–37 | "16 rules × 4 parameters × 3 atoms"; "2 approval rules, each before the permissive rule it overlaps" | exact |
| 48 | §8.3 Workloads | four profiles; four states | same; BM | §3; BM Q1 column heads | small, medium, large, medium-approval; cold, warm, warm+prefix, prefix-miss | count |
| 49 | §8.3 Grid | N ∈ {1, 2, 3, 5, 10}; medium-approval at N = 3 only | impl:BENCH_PLAN_FROZEN.md | l. 45 | same | exact |
| 50 | §8.3 Grid | 231 latency configurations per run | BM | §2 Method, l. 30 | 231 | exact |
| 51 | §8.3 Grid | 1,000 warm-up and 10,000 measured verifications; fewer for injected latency | BM | §2 Method, l. 31 | "1,000 warm-up and 10,000 measured … except Q5's injected-latency configurations (D-44)" | exact |
| 52 | §8.3 Grid | three runs | BM | §2 Method, l. 30 | "over 3 runs in separate processes" | exact |
| 53 | §8.3 Machine | 10 performance and 4 efficiency cores | BM | §1 Environment, l. 17 | "10 performance + 4 efficiency cores" | exact |
| 54 | §8.3 Machine | macOS 26.5.2 | BM | §1 Environment, l. 18 | macOS 26.5.2 (25F84) | exact |
| 55 | §8.3 Statistics | 95% CIs; ±10% margin; 1.10; 0.90 | BM; impl:BENCH_PLAN_FROZEN.md | BM Verdicts, l. 47; plan §6, l. 111 | ±10%; lower bound > 1.10; upper bound < 0.90 | exact |
| 56 | §8.3 Deviations | more than 10% of its configurations (safety valve) | impl:BENCH_PLAN_FROZEN.md | l. 89 | "more than 10% of a run's configurations" | exact |
| 57 | §8.3 Deviations | two amendments, then two deviations | BM | §7, ll. 876–884 | four BENCH_LOG entries: two before measurement, two after runs 1 and 2 | count |
| 58 | §8.3 Deviations | 200 ms busy spin | BM | §7 item 4, l. 884 | "a 200 ms busy spin before every probe" | exact |
| 59 | §8.3 Deviations | twenty-six configurations flagged | BM | §7, l. 888 | "Every throttling re-run (26; …)" | exact |
| 60 | §8.3 Deviations | the two flagged again | BM | §7, ll. 903, 907 | two entries "re-run **flagged again**" | count |
| 61 | §8.3 Deviations | three attempts to resume refused | BM | §7, l. 917 | "Three resume attempts refused" | exact |

## §8.4 Results

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 62 | §8.4 Verdicts | 21.995 [21.994, 21.995] | BM | Summary, l. 7; Verdicts, l. 49 | 21.995 [21.994, 21.995] | exact |
| 63 | §8.4 Verdicts | runs 22.003, 21.951, 21.997 | BM | same | 22.003, 21.951, 21.997 | exact |
| 64 | §8.4 Verdicts | B 12.2–24.2× slower than D | as #3 | | | as #3 |
| 65 | §8.4 Verdicts | A 4.9–16.0× slower than C | as #4 | | | as #4 |
| 66 | §8.4 Verdicts | A 5.8–19.5× slower than C-batch | BM | Summary, l. 7; Verdicts, A/C-batch column | min 5.806 (large, N=5); max 19.457 (small, N=1) | 1 d.p. |
| 67 | §8.4 Verdicts | all 16 cells of each comparison | BM | Summary, l. 7 | "(16 of 16 cells)"; "16 of 16 and 16 of 16" | exact |
| 68 | §8.4 Verdicts | no N from 1 to 10 reverses this | BM | Summary, l. 7 | same | exact |
| 69 | §8.4 Caching | 578–589 µs (B hit, medium) | BM | Summary, l. 7; Q1 medium, B warm+prefix (ll. 94–98) | 578 (N=1) to 589 (N=10) | exact |
| 70 | §8.4 Caching | 24.5–33.8 µs (D hit, medium) | BM | Summary, l. 7; Q1 medium, D warm+prefix | 24.5 (N=1) to 33.8 (N=10) | exact |
| 71 | §8.4 Caching | 531 µs, one BLS verification | BM | Q7/Q8 table, `bls/verify` (l. 396) | 531 [531, 532] | exact |
| 72 | §8.4 Caching | 20.6 µs, one Ed25519 verification | BM | `ed25519/verify_strict` (l. 414) | 20.6 [20.6, 20.6] | exact |
| 73 | §8.4 Caching | 109 µs without prefix cache, N=3, medium | as #7 | | 109 | exact |
| 74 | §8.4 Caching | 26.4 µs with it | as #8 | | 26.4 | exact |
| 75 | §8.4 Bytes | saves only from N = 2 (small, medium, large), N = 3 with a receipt | BM | Q2 break-even lines (ll. 160, 175, 190, 205); §4 l. 773 | small 2, medium 2, large 2, medium-approval 3 | exact |
| 76 | §8.4 Bytes | larger at N = 1 by 13 bytes (61 with a receipt) | BM | §4 Q2, l. 772; Q2 tables, N=1 rows (ll. 164, 179, 194, 209) | +13 (small, medium, large); +61 (medium-approval) | exact |
| 77 | §8.4 Bytes | largest saving 8.8% (small, N = 10) | as #5 | | | as #5 |
| 78 | §8.4 Bytes | 2.9% at N = 3, medium (1826 against 1881 bytes) | BM | Summary, l. 9; Q2 medium, N=3 (l. 181) | "1826 bytes against C's 1881, 2.9% smaller"; A/C 0.971 | exact; 1 − 1826/1881 = 0.0292 |
| 79 | §8.4 Bytes | the 96-byte aggregate is 5.3% of A's chain at N = 3, medium | BM | §4 Q2, l. 775 | "5.3% of A's chain at N = 3, medium" | exact; 96/1826 = 0.0526 |
| 80 | §8.4 Bytes | 13.6% for C's N+1 signatures | BM | §4 Q2, l. 775 | "against 13.6% for C's N + 1 signatures" | exact; 256/1881 = 0.1361 |
| 81 | §8.4 Bytes | 48-byte BLS keys where Ed25519's are 32 bytes | BM | Summary, l. 9 | "48-byte BLS keys where Ed25519's are 32" | exact |
| 82 | Figure 5 caption | means over 20 sampled chains; break-even from N = 2 | BM; impl:scripts/paper_figures.py | Q2, l. 156; Q2 medium, l. 175; figures script l. 233 | "exact means over 20 sampled chains"; "break-even: N = 2" | exact. Since `25242b8` the caption is the generated macro (`gen:figures/captions.tex`), not typed. |
| 83 | Figures 3–5 captions | pooled medians of three runs; error bars are the range of the three runs' medians | impl:scripts/paper_figures.py | `ERRBARS`, l. 217 | "error bars are the range of the three runs' medians" | exact. Since `25242b8` the caption is the generated macro (`gen:figures/captions.tex`), not typed. |
| 84 | §8.4 Cost model | α = 566 µs, β = 193 µs | BM | Q3 table, warm medium (l. 274); §4 Q3, l. 779 | 566 [566, 566]; 193 [193, 193] | exact |
| 85 | §8.4 Cost model | R² = 0.9999 | BM | same | 0.9999 | exact |
| 86 | §8.4 Cost model | N = α/β ≈ 2.9 | BM | §4 Q3, l. 780 | "N = α/β ≈ 2.9" | 566/193 = 2.93 |
| 87 | §8.4 Cost model | β is 0.84–0.94 of hash to G2 plus Miller loop (224 µs) | BM | §4 Q3, l. 783; §5 l. 845 | "0.84–0.94 … (224 µs, … hash-to-G2 proxy 96.1 µs, Miller loop 127 µs)" | 189/224 = 0.84 (small), 211/224 = 0.94 (large). The rounded components sum to 223.1; the source's 224 is used. |
| 88 | §8.4 Cost model | cold β 800 µs | BM | Q3 table, cold medium (l. 277); §4 l. 782 | 800 [800, 800] | exact |
| 89 | §8.4 Against A-ind | 0.387–0.594 warm for N ≥ 2 | BM | §4 Q4, l. 785; §5 l. 844 | "0.387–0.594 warm for N ≥ 2" | min 0.387 (small, N=10; l. 290). Max 0.594 is medium-approval, N=3: 1721/2899 = 0.5937 from Q1 (l. 114). The Q4 table omits the medium-approval row (see the report). |
| 90 | §8.4 Against A-ind | 0.701–0.840 cold | BM | §4 Q4, l. 785 | "0.701–0.840 cold" | min 0.701 (small, N=10; l. 290); max 0.840 (large, N=1; l. 296). Includes N = 1. |
| 91 | §8.4 Against A-ind | saves 98.0 bytes per hop | BM | Summary, l. 9; §4 Q2, l. 776 | 98.0 | exact; (981 − 99)/9 = 98.0 from the ablation table (ll. 231–235) |
| 92 | §8.4 Cold path | 4 resolver calls and 1 policy-store call | BM | Q5 table (ll. 306–313); §4 l. 787 | 4.00; 1.00 | exact |
| 93 | §8.4 Cold path | 3574 µs (A) and 208 µs (C), no injected latency | BM | Q5, ll. 306, 310 | 3574 [3574, 3574]; 208 [208, 208] | Exact. Q5's 0 ms row; the cold-state table's separate A cell is 3573 (l. 134). |
| 94 | §8.4 Cold path | 17,470 and 9456 µs at 1 ms | BM | Q5, ll. 307, 311 | 17470 [17336, 17668]; 9456 [9455, 9457] | exact |
| 95 | §8.4 Cold path | 469,530 and 445,618 µs at 80 ms | BM | Q5, ll. 309, 313 | 469530; 445618 | exact |
| 96 | §8.4 Throughput | D 37,242/s on one thread, 341,883 on ten | BM | Q6, ll. 337, 341 | 37242; 341883 | exact (median over runs) |
| 97 | §8.4 Throughput | B 1712 and 15,924 | BM | Q6, ll. 325, 329 | 1712; 15924 | exact |
| 98 | §8.4 Throughput | C 9270 and 84,520 | BM | Q6, ll. 331, 335 | 9270; 84520 | exact |
| 99 | §8.4 Throughput | A 872 and 7997 | BM | Q6, ll. 319, 323 | 872; 7997 | exact |
| 100 | §8.4 Throughput | four efficiency cores (14 threads) | BM | Q6 "14 (includes efficiency cores)"; §1 l. 17 | 14 | exact |
| 101 | §8.4 Throughput | A p99 1294 → 4092 µs | BM | §4 Q6, l. 794; Q6 ll. 323–324 | 1294 (runs 1337, 1294, 1229); 4092 (runs 4090, 4092, 4096) | median of the three runs, as l. 794 |
| 102 | §8.4 Throughput | D p99 33.0 → 98.4 µs | BM | §4 Q6, l. 794; Q6 ll. 341–342 | 33.0 (runs 33.0, 33.2, 33.0); 98.4 (runs 97.9, 98.4, 98.6) | median of the three runs |

## §8.5–§8.8

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 103 | §8.5 | one claim was not supported | BM | §5 claims table, l. 850 | 1 "Not supported" row of 8 | count; also gen:tables/claims.tex |
| 104 | §8.6 (exploratory) | arm E 0.26–0.29 of AIP's time, small | BM | Q9, ll. 350–353; §4 l. 809 | 0.26 (N=1) to 0.29 (N=5) | exact as printed (2 d.p.) |
| 105 | §8.6 (exploratory) | 0.32–0.37, medium | BM | Q9, ll. 355–358 | 0.32 (N=1) to 0.37 (N=3, 5) | exact |
| 106 | §8.6 (exploratory) | 1.11–1.26, large | BM | Q9, ll. 360–363 | 1.11 (N=1) to 1.26 (N=2, 3) | exact |
| 107 | §8.6 (exploratory) | verifies every block's signature twice | BM | Q9 comparison table, "Signatures" row (l. 815) | "verified twice" | exact |
| 108 | §8.6 (exploratory) | mean of 100 single timings | BM | Q9 comparison table, "Iterations and statistic" row (l. 818) | "the arithmetic mean of 100 single timings" | exact |
| 109 | §8.6 (exploratory) | x, y and the positioning table | — | — | filled at `25242b8` | superseded by rows 120–124 |
| 110 | §8.7 (exploratory) | phase breakdown | — | — | filled at `25242b8` | superseded by rows 125–134 |

## §9 Discussion

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 111 | §9.2 Limitations | certificate 185 bytes under Ed25519 | BM | Q2, l. 267 | "Ed25519 185 bytes" | exact |
| 112 | §9.2 Limitations | 249 under BLS | BM | Q2, l. 267; §5 l. 847 | "BLS 249 bytes" | exact |
| 113 | §9.2 Limitations | N + 1 certificates on a cold verifier | BM | §5 l. 850 | "Line 24 verifies N + 1 certificates before phase 6" | exact |

## Placeholder pass (implementation commit `25242b8`)

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 114 | Figure 3 (ratios) | caption: ±10% band, three runs | gen:figures/captions.tex | `\figcapRatios` | generated macro | included unedited, not typed |
| 115 | §8.6 (exploratory) | AIP's code took 0.44–0.46 of its published times | BM | §8 "AIP's own benchmark", table "Here ÷ published (means)", ll. 1026–1031 | 0.44 (depth 0), 0.46 (depths 1, 2, 3), 0.45 (depths 4, 5) | Min 0.44, max 0.46. By AIP's own statistic: the median of the three unmodified runs' means. BM text l. 1036 cites only depths 0 and 4. |
| 116 | §8.6 (exploratory) | arm E took 0.60–0.82 of AIP's code here | BM | l. 1039; table "E ÷ AIP here (medians; small / medium)" | "0.60–0.64 (small) and 0.75–0.82 (medium)" | Min 0.60 (small, depth 0); max 0.82 (medium, depth 4). |
| 117 | §8.6 (exploratory) | 0.60–0.64 small, 0.75–0.82 medium | BM | l. 1039 | same | exact |
| 118 | §8.6 (exploratory) | within the factor of 3 the plan set as a sanity bound | impl:BENCH_PLAN_FROZEN.md; BM | plan l. 173; BM l. 1039 | "more than about 3× at matching depth"; "within the sanity rule's 3×" | exact |
| 119 | §8.6 (exploratory) | cold start: first process slower than the next two at the shallowest depths; published figures from a single process | BM | l. 1041; per-run columns ll. 1026–1027 | depth 0 means 0.164, 0.082, 0.082 ms; depth 1: 0.192, 0.134, 0.133 | qualitative, as the source states it |
| 120 | §8.6 (exploratory) | default instantiation 109 µs at N = 3, medium | gen:tables/positioning.tex; BM | row "3 (2)", "C, warm"; l. 1069 | 109 | exact; = row #7 |
| 121 | §8.6 (exploratory) | 147 µs for Biscuit at matching depth | same | row "3 (2)", "E" | 147 | exact |
| 122 | §8.6 (exploratory) | 0.68–0.86 of arm E's time | same | column "C ÷ E" (ll. 1067–1071) | 0.86 (N=1) to 0.68 (N=10) | min and max of the column |
| 123 | §8.6 (exploratory) | prefix-cache hit 0.08–0.40 | same | column "D ÷ E" | 0.40 (N=1) to 0.08 (N=10) | min and max of the column |
| 124 | Table 6 (positioning) | caption | gen:tables/captions.tex | `\tabcapPositioning` | generated macro | Included unedited. It names "M9", "D-79" and "paper Table 1", which is Table 7 here; flagged for a generator fix. |
| 125 | §8.7 (exploratory) | N = 3; arms A and C; small, medium, large; three runs | BM | §8 "Phase breakdown", "What ran" (l. 935) | same | exact |
| 126 | §8.7 (exploratory) | five categories and their algorithm lines | BM | "The categories" (ll. 937–942) | decoding l. 2, 4–6; policy 32–37; identity 23–28; cryptography 47–48 and 24, 43, 49, plus point validation | exact |
| 127 | §8.7 (exploratory) | shares agree across runs to within 0.7 percentage points | BM | l. 1001; "Run agreement" table | 0.7 (C warm N=3 small) is the largest share difference | exact |
| 128 | §8.7 (exploratory) | arm A cryptography 88.1–99.1% | BM | "By category", Cryptography row (l. 956); l. 1002 | 99.1% (small), 98.0% (medium), 88.1% (large) | min and max |
| 129 | §8.7 (exploratory) | arm A decoding 0.7–6.1% | BM | Decoding row (l. 953) | 0.7% (small) to 6.1% (large) | min and max |
| 130 | §8.7 (exploratory) | arm A policy 0.0–4.8% | BM | Policy row (l. 954) | 0.0% (small) to 4.8% (large) | min and max |
| 131 | §8.7 (exploratory) | arm C cryptography 90.1% (small) to 39.1% (large) | BM | Cryptography row (l. 956); l. 1006 | 90.1%; 80.0% (medium); 39.1% | exact |
| 132 | §8.7 (exploratory) | arm C large: decoding 30.9%, policy 25.2% | BM | Decoding and Policy rows, "C, large" (ll. 953–954) | 30.9%; 25.2% | exact |
| 133 | §8.7 (exploratory) | identity at most 0.2% | BM | Identity row (l. 955); l. 1009 | max 0.2% (C small, C medium) | exact |
| 134 | §8.7 (exploratory) | probe flagged 5 of the 18 configurations, none re-run; the valve would have aborted | BM | l. 1010 | "flagged 5 of 18 configurations … None was re-run … would have aborted the run" | exact; the valve is more than 10% (row #56) |
| 135 | §5.6 (P-33) | one value, 60 seconds, for both | impl:DECISIONS.md | D-82, l. 1016 | "keeps its default of 60 seconds. One bound on clock skew serves both the nonce TTL and revocation retention." | exact |
| 136 | §4.7 (P-32) | signature checks at line 2; key checks at registration and line 24; small-order R at line 49 | impl:DECISIONS.md; impl:PAPER_ISSUES.md | D-81; P-32 "What the implementation does" | "A failure in a chain or receipt is L02"; keys at registration and certificate decoding (line 24); "rejects a small-order R at line 49" | line numbers, not measurements |
| 137 | Alg. 1 line 2 (P-31) | (B_0,…,B_N, σ) | impl:PAPER_ISSUES.md | P-31, suggested fix | "(B0, …, BN, σ) ← Decode(C)" | edit in place; numbering 1–52 unchanged |
| 138 | Appendix A | result column per instantiation | gen:tables/security.tex | header "Default", "Aggregate" | two columns | generated |
| 139 | Data availability | measured commits named in ARTIFACT.md | impl:ARTIFACT.md | §5, ll. 102–104 | M9 at `4e134dc` and `b175599`; the exploratory session at `b1cc711` | named by reference only |
| 140 | §8.8 | the measured binaries predate §4.7's checks; an exploratory run on the same machine | BM@c6bbbaa; impl:BENCH_LOG.md@c6bbbaa | BM §8 (l. 925, "Exploratory analyses"), block "The cost of D-81's encoding checks" (l. 1043); BENCH_LOG l. 174 | "D-81 added decode-time canonical-encoding checks … after M9"; run at `25242b8` on M9's machine state | Filled at `c6bbbaa`; it replaces the `\pending`. The run is exploratory, and the text says so. |
| 141 | §8.8 | byte comparisons | impl:DECISIONS.md@c6bbbaa | D-81, l. 1002; D-87, l. 1106 | "Both are byte comparisons"; "the checks are a few byte comparisons" | exact |
| 142 | §8.8 | a few nanoseconds per signature or key | BM@c6bbbaa | "The checks" table (ll. 1065–1068) | 1.17 to 11.16 ns | qualitative summary of rows 143–145 |
| 143 | §8.8 | about 1.9 ns per signature, honest | BM@c6bbbaa | l. 1065, "signature checks (R's y < p, s < ℓ), honest signatures" | 1.89 ns (runs 1.85, 1.85, 1.89) | rounded to 1 d.p. |
| 144 | §8.8 | about 1.2 ns per key, honest | BM@c6bbbaa | l. 1067, "key check (y < p), honest keys" | 1.17 ns (runs 1.22, 1.19, 1.14) | rounded to 1 d.p. |
| 145 | §8.8 | at most about 11 ns on the worst passing inputs | BM@c6bbbaa | l. 1066, signature worst passing input; l. 1068, key worst passing input | 11.16 ns (signature); 6.10 ns (key) | max = 11.16, rounded to the integer |
| 146 | §8.8 | both arms of the default instantiation, every cache state, N ∈ {1, 3, 10} | BM@c6bbbaa | "What they add to a chain" table (ll. 1078–1089) | C warm, C cold, D hit, D miss at N = 1, 3, 10 | 12 rows |
| 147 | §8.8 | in the medium profile | BM@c6bbbaa | table heading, l. 1072: "What they add to a chain (medium profile)" | medium profile only | **Added to the author's text.** The source computes shares for the medium profile only. Applying the same counts to the small profile's D hit at N = 10 (30.2 µs) would give 0.41% worst-case, above 0.4%. So the claim must not be stated for every profile. |
| 148 | §8.8 | under 0.1% on honest inputs | BM@c6bbbaa | "Share" column, l. 1088 (D, hit, N = 10) | max 0.0614% (min 0.0065%, D miss N = 10) | max < 0.1 |
| 149 | §8.8 | under 0.4% on worst-case inputs | BM@c6bbbaa | "Share" column, bracketed, l. 1088 (D, hit, N = 10) | max 0.3628% | max < 0.4 |
