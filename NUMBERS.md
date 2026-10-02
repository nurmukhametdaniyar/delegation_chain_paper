# NUMBERS.md — source audit for every reported number

Every number in the abstract, prose and captions of `paper.tex` that comes from the implementation or the
benchmark is listed here with its source. Generated tables (`generated/tables/*.tex`) and figures
(`generated/figures/*.pdf`) are included unedited and are not re-listed cell by cell.

**Sources.**
- `BM` = `benchmark_outputs/BENCHMARKS.md`. Line numbers refer to that file, which is byte-identical to the
  implementation repository's copy at commit `8ffe91b`.
- `impl:` = a file in the implementation repository (`delegation_chain_impl`) at commit `8ffe91b`.
- `gen:` = `generated/` in this repository, copied byte-for-byte from `impl:paper/` at `8ffe91b`.

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
| 5 | Abstract | at most 8.8% of a chain's bytes | BM | §4 Q2, l. 768; Q2 small table, N=10 (l. 167) | "8.8% smaller (small)"; 2953 against 3237 bytes | 1 − 2953/3237 = 0.0877 → 8.8%. This is the grid maximum (small, N=10). |
| 6 | Abstract | three-agent chain | impl:BENCH_PLAN_FROZEN.md; BM | §3 grid; the N = 3 cells | N = 3 | Derived: N = 3 counts the bodies after the session body, so agents A_1 to A_3. |
| 7 | Abstract | 109 µs | BM | Q1 medium, C warm, N = 3 (l. 90) | 109 [109, 109] | exact |
| 8 | Abstract | 26 µs (prefix cached) | BM | Q1 medium, D warm+prefix, N = 3 (l. 90) | 26.4 [26.4, 26.4] | rounded to the integer in the abstract; §8.4 prints 26.4 |

## §1 Introduction

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 9 | §1, measurement paragraph | one 96-byte signature | BM | §5 claims table, l. 840 | "A's signature bytes are 96 at N = 1 and 96 at N = 10, in every profile" | exact |
| 10 | §1, measurement paragraph | 12.2–24.2× | as #3 | | | as #3 |
| 11 | §1, measurement paragraph | at most 8.8% | as #5 | | | as #5 |

## §2–§7 (sizes and costs)

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 12 | §2.2 Ed25519 | public keys 32 bytes | RFC 8032 (`rfc8032`); BM | RFC 8032 §5.1.5; BM Summary, l. 9 | 32 octets; "Ed25519's are 32" | exact |
| 13 | §2.2 Ed25519; §4.3; Figure 1 box | signatures 64 bytes | RFC 8032; BM | RFC 8032 §5.1.6; BM Q2 "C signatures" column, N=1 (l. 173) | 64 octets; 128 = 2 × 64 at N = 1 | exact |
| 14 | §2.2 BLS; §4.8 | signature in G2, 96 bytes compressed | `ietf-pairing-curves`, `ietf-bls-signature`; BM | BM Q2 "A signatures" column (ll. 158–212) | 96 | exact |
| 15 | §2.2 BLS; §4.8 | public key in G1, 48 bytes | same; BM | BM Summary, l. 9 | "48-byte BLS keys" | exact |
| 16 | §2.2 (l. 275); Figure 1 caption; §4.8 (×2) | one 96-byte G2 element; constant size 96 bytes regardless of N | BM | §5 claims table, l. 840 | 96 at N = 1 and N = 10, every profile | exact |
| 17 | §4.2 | 32-byte SHA-256 digest | FIPS 180-4 | SHA-256 output length | 256 bits | 256/8 = 32 |
| 18 | §4.8, closing sentence | saves only a few percent of a chain's bytes | BM | §4 Q2, ll. 768–769; Q2 medium, N=3 (l. 175) | 2.9% (medium, N=3) to 8.8% (small, N=10) | qualitative; consistent with #5 and #40 |
| 19 | §6.4 after Prop. 2 | 9.0 µs, 4 rules × 4 atoms, worst case | BM | Q7/Q8 table, `policy/contains/worst/r4_a4` (l. 428) | 9.0 [9.0, 9.1] | exact; also §4 Q8, l. 798 |
| 20 | §6.4 after Prop. 2 | 5146 µs at 64 rules × 8 atoms | BM | `policy/contains/worst/r64_a8` (l. 432) | 5146 [5120, 5166] | exact; also l. 798 |

## §8.1 Implementation

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 21 | §8.1 | nine crates | impl:crates/ | directory listing | dc-baselines, dc-bench, dc-cbor, dc-chain, dc-crypto, dc-policy, dc-registry, dc-types, dc-verifier | count = 9 |
| 22 | §8.1 | one operating-system call (the only unsafe code) | impl:MILESTONES.md | M8, l. 316 | "The QoS FFI call in `qos.rs`, the one permitted `unsafe`" | exact; Cargo.toml `unsafe_code = "deny"` |
| 23 | §8.1 | blst 0.3.17; ed25519-dalek 2.2.0; biscuit-auth 6.0.0 | BM | §1 Environment, "Crates" row (l. 21) | blst 0.3.17; ed25519-dalek 2.2.0; biscuit-auth 6.0.0 | exact |
| 24 | §8.1 | rustc 1.97.1 | BM | §1 Environment, "Toolchain" row (l. 20) | rustc 1.97.1 (8bab26f4f 2026-07-14) | exact |
| 25 | §8.1 | 16 specification defects | benchmark_outputs/PAPER_ISSUES.md | "Sources" bullet (l. 11) | P-15 to P-25 (pre-M0 review); P-26; P-27; P-28 and P-29; P-30 | 11 + 1 + 1 + 2 + 1 = 16. P-01 to P-14 predate the implementation and are not counted. |

## §8.2 Correctness Evaluation

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 26 | §8.2 Security suite | All 52 tests passed | gen:tables/security.tex | last line | "% 52 of 52 tests passed." | exact; per-suite count, no project-wide total (author decision) |
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
| 40a | §8.2 Oracle | 4,176 missed containments | impl:docs/test-reports/policy-oracle-m4.json; benchmark_outputs/PAPER_ISSUES.md; impl:MILESTONES.md | JSON `contains.misses_by_cause.total`; PAPER_ISSUES P-27, l. 262; MILESTONES l. 182 | 4176 | Exact. 51998 − 47822 = 4176 (oracle true minus procedure true, `completeness_overall`). |
| 40b | §8.2 Oracle | unsatisfiable child rules account for 4,138 | same | JSON `contains.misses_by_cause.other["child rule unsatisfiable"]`; PAPER_ISSUES P-27, l. 263; MILESTONES l. 183 | 4138 | exact |
| 41 | §8.2 Oracle | half of its pairs are independent | impl:DECISIONS.md | D-58, l. 529 | "half are independent pairs; in the other half, S2 is derived from S1 by mutation" | exact |
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
| 57 | §8.3 Deviations | two amendments, then two deviations | BM | §7, ll. 870–878 | four BENCH_LOG entries: two before measurement, two after runs 1 and 2 | count |
| 58 | §8.3 Deviations | 200 ms busy spin | BM | §7 item 4, l. 878 | "a 200 ms busy spin before every probe" | exact |
| 59 | §8.3 Deviations | twenty-six configurations flagged | BM | §7, l. 882 | "Every throttling re-run (26; …)" | exact |
| 60 | §8.3 Deviations | the two flagged again | BM | §7, ll. 897, 901 | two entries "re-run **flagged again**" | count |
| 61 | §8.3 Deviations | three attempts to resume refused | BM | §7, l. 911 | "Three resume attempts refused" | exact |

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
| 69 | §8.4 Caching | 578–589 µs (B hit, medium) | BM | Summary, l. 7; Q1 medium, B warm+prefix (ll. 88–92) | 578 (N=1) to 589 (N=10) | exact |
| 70 | §8.4 Caching | 24.5–33.8 µs (D hit, medium) | BM | Summary, l. 7; Q1 medium, D warm+prefix | 24.5 (N=1) to 33.8 (N=10) | exact |
| 71 | §8.4 Caching | 531 µs, one BLS verification | BM | Q7/Q8 table, `bls/verify` (l. 390) | 531 [531, 532] | exact |
| 72 | §8.4 Caching | 20.6 µs, one Ed25519 verification | BM | `ed25519/verify_strict` (l. 408) | 20.6 [20.6, 20.6] | exact |
| 73 | §8.4 Caching | 109 µs without prefix cache, N=3, medium | as #7 | | 109 | exact |
| 74 | §8.4 Caching | 26.4 µs with it | as #8 | | 26.4 | exact |
| 75 | §8.4 Bytes | saves only from N = 2 (small, medium, large), N = 3 with a receipt | BM | Q2 break-even lines (ll. 154, 169, 184, 199); §4 l. 767 | small 2, medium 2, large 2, medium-approval 3 | exact |
| 76 | §8.4 Bytes | larger at N = 1 by 13 bytes (61 with a receipt) | BM | §4 Q2, l. 766; Q2 tables, N=1 rows (ll. 158, 173, 188, 203) | +13 (small, medium, large); +61 (medium-approval) | exact |
| 77 | §8.4 Bytes | largest saving 8.8% (small, N = 10) | as #5 | | | as #5 |
| 78 | §8.4 Bytes | 2.9% at N = 3, medium (1826 against 1881 bytes) | BM | Summary, l. 9; Q2 medium, N=3 (l. 175) | "1826 bytes against C's 1881, 2.9% smaller"; A/C 0.971 | exact; 1 − 1826/1881 = 0.0292 |
| 79 | §8.4 Bytes | the 96-byte aggregate is 5.3% of A's chain at N = 3, medium | BM | §4 Q2, l. 769 | "5.3% of A's chain at N = 3, medium" | exact; 96/1826 = 0.0526 |
| 80 | §8.4 Bytes | 13.6% for C's N+1 signatures | BM | §4 Q2, l. 769 | "against 13.6% for C's N + 1 signatures" | exact; 256/1881 = 0.1361 |
| 81 | §8.4 Bytes | 48-byte BLS keys where Ed25519's are 32 bytes | BM | Summary, l. 9 | "48-byte BLS keys where Ed25519's are 32" | exact |
| 82 | Figure 5 caption | means over 20 sampled chains; break-even from N = 2 | BM; impl:scripts/paper_figures.py | Q2, l. 150; Q2 medium, l. 169; figures script l. 231 | "exact means over 20 sampled chains"; "break-even: N = 2" | exact |
| 83 | Figures 3–5 captions | pooled medians of three runs; error bars are the range of the three runs' medians | impl:scripts/paper_figures.py | `ERRBARS`, l. 215 | "error bars are the range of the three runs' medians" | exact |
| 84 | §8.4 Cost model | α = 566 µs, β = 193 µs | BM | Q3 table, warm medium (l. 268); §4 Q3, l. 773 | 566 [566, 566]; 193 [193, 193] | exact |
| 85 | §8.4 Cost model | R² = 0.9999 | BM | same | 0.9999 | exact |
| 86 | §8.4 Cost model | N = α/β ≈ 2.9 | BM | §4 Q3, l. 774 | "N = α/β ≈ 2.9" | 566/193 = 2.93 |
| 87 | §8.4 Cost model | β is 0.84–0.94 of hash to G2 plus Miller loop (224 µs) | BM | §4 Q3, l. 777; §5 l. 839 | "0.84–0.94 … (224 µs, … hash-to-G2 proxy 96.1 µs, Miller loop 127 µs)" | 189/224 = 0.84 (small), 211/224 = 0.94 (large). The rounded components sum to 223.1; the source's 224 is used. |
| 88 | §8.4 Cost model | cold β 800 µs | BM | Q3 table, cold medium (l. 271); §4 l. 776 | 800 [800, 800] | exact |
| 89 | §8.4 Against A-ind | 0.387–0.594 warm for N ≥ 2 | BM | §4 Q4, l. 779; §5 l. 838 | "0.387–0.594 warm for N ≥ 2" | min 0.387 (small, N=10; l. 284). Max 0.594 is medium-approval, N=3: 1721/2899 = 0.5937 from Q1 (l. 108). The Q4 table omits the medium-approval row (see the report). |
| 90 | §8.4 Against A-ind | 0.701–0.840 cold | BM | §4 Q4, l. 779 | "0.701–0.840 cold" | min 0.701 (small, N=10; l. 284); max 0.840 (large, N=1; l. 290). Includes N = 1. |
| 91 | §8.4 Against A-ind | saves 98.0 bytes per hop | BM | Summary, l. 9; §4 Q2, l. 770 | 98.0 | exact; (981 − 99)/9 = 98.0 from the ablation table (ll. 225–229) |
| 92 | §8.4 Cold path | 4 resolver calls and 1 policy-store call | BM | Q5 table (ll. 300–307); §4 l. 781 | 4.00; 1.00 | exact |
| 93 | §8.4 Cold path | 3574 µs (A) and 208 µs (C), no injected latency | BM | Q5, ll. 300, 304 | 3574 [3574, 3574]; 208 [208, 208] | Exact. Q5's 0 ms row; the cold-state table's separate A cell is 3573 (l. 128). |
| 94 | §8.4 Cold path | 17,470 and 9456 µs at 1 ms | BM | Q5, ll. 301, 305 | 17470 [17336, 17668]; 9456 [9455, 9457] | exact |
| 95 | §8.4 Cold path | 469,530 and 445,618 µs at 80 ms | BM | Q5, ll. 303, 307 | 469530; 445618 | exact |
| 96 | §8.4 Throughput | D 37,242/s on one thread, 341,883 on ten | BM | Q6, ll. 331, 335 | 37242; 341883 | exact (median over runs) |
| 97 | §8.4 Throughput | B 1712 and 15,924 | BM | Q6, ll. 319, 323 | 1712; 15924 | exact |
| 98 | §8.4 Throughput | C 9270 and 84,520 | BM | Q6, ll. 325, 329 | 9270; 84520 | exact |
| 99 | §8.4 Throughput | A 872 and 7997 | BM | Q6, ll. 313, 317 | 872; 7997 | exact |
| 100 | §8.4 Throughput | four efficiency cores (14 threads) | BM | Q6 "14 (includes efficiency cores)"; §1 l. 17 | 14 | exact |
| 101 | §8.4 Throughput | A p99 1294 → 4092 µs | BM | §4 Q6, l. 788; Q6 ll. 317–318 | 1294 (runs 1337, 1294, 1229); 4092 (runs 4090, 4092, 4096) | median of the three runs, as l. 788 |
| 102 | §8.4 Throughput | D p99 33.0 → 98.4 µs | BM | §4 Q6, l. 788; Q6 ll. 335–336 | 33.0 (runs 33.0, 33.2, 33.0); 98.4 (runs 97.9, 98.4, 98.6) | median of the three runs |

## §8.5–§8.8

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 103 | §8.5 | one claim was not supported | BM | §5 claims table, l. 844 | 1 "Not supported" row of 8 | count; also gen:tables/claims.tex |
| 104 | §8.6 (exploratory) | arm E 0.26–0.29 of AIP's time, small | BM | Q9, ll. 344–347; §4 l. 803 | 0.26 (N=1) to 0.29 (N=5) | exact as printed (2 d.p.) |
| 105 | §8.6 (exploratory) | 0.32–0.37, medium | BM | Q9, ll. 349–352 | 0.32 (N=1) to 0.37 (N=3, 5) | exact |
| 106 | §8.6 (exploratory) | 1.11–1.26, large | BM | Q9, ll. 354–357 | 1.11 (N=1) to 1.26 (N=2, 3) | exact |
| 107 | §8.6 (exploratory) | verifies every block's signature twice | BM | Q9 comparison table, "Signatures" row (l. 809) | "verified twice" | exact |
| 108 | §8.6 (exploratory) | mean of 100 single timings | BM | Q9 comparison table, "Iterations and statistic" row (l. 812) | "the arithmetic mean of 100 single timings" | exact |
| 109 | §8.6 (exploratory) | x µs, y µs, positioning table | — | — | not available | `\pending{}`: no same-machine DC-vs-Biscuit row exists in the sources |
| 110 | §8.7 (exploratory) | phase breakdown | BM | §8, l. 941 | "_Not run yet_" | `\pending{}` |

## §9 Discussion

| # | Location | As written | Source file | Location in source | Exact source value | Note |
|---|---|---|---|---|---|---|
| 111 | §9.2 Limitations | certificate 185 bytes under Ed25519 | BM | Q2, l. 261 | "Ed25519 185 bytes" | exact |
| 112 | §9.2 Limitations | 249 under BLS | BM | Q2, l. 261; §5 l. 841 | "BLS 249 bytes" | exact |
| 113 | §9.2 Limitations | N + 1 certificates on a cold verifier | BM | §5 l. 844 | "Line 24 verifies N + 1 certificates before phase 6" | exact |
