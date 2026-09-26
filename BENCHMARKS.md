# Laya benchmarks

Every checkpoint answered **byte-identical questions** in each run (fixed seed). Jev figures are **third-party published, never measured here** — no TypeSafe API access — so sample sizes and prompts differ; treat them as indicative.

| run | what | where |
|---|---|---|
| T4 Colab | typed-decisions, MASSIVE (14 langs), XNLI (15 langs), English suites, latency, option-order robustness, calibration repair | `research/results/t4_colab_benchmark.json` |
| CPU sweep | MASSIVE intent across **all 51 languages**; its typed-decisions part (`part_b`) covers the English checkpoint only | `research/results/cpu_51_language_sweep.json` |
| Applications | the seven workflow themes + the datasets where Jev numbers exist, all three checkpoints (laya 0.2.1, CPU, 400 cases per task, seed 13, 2026-09-19) | `research/results/app_benchmark_results.json` |


**Calibration columns in the CPU sweep predate the temperature clamp.** The 51-language ECE and mean-confidence figures were produced before #42 clamped temperatures to `[0.5, 5]`, so today's package reports different confidence for the affected buckets. Accuracy columns are unaffected, because a temperature-scaled softmax has the same argmax at every positive temperature.

The same 51 languages and 5,100 cases have now been re-run after the temperature clamp, in both regimes (`research/results/cpu_51_language_sweep_clamped.json`, [#208](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html)). Macro accuracy reproduces at **0.2269** exactly, and macro ECE moves **0.7331 → 0.5709**:

| | committed | re-run, raw temperatures | re-run, as served |
|---|---|---|---|
| macro accuracy | 0.2269 | 0.2269 | 0.2269 |
| macro ECE | 0.7331 | **0.7331** | 0.5709 |
| macro F1 | 0.2053 | 0.2053 | 0.2053 |
| mean confidence, `en` | 0.9989 | **0.9989** | 0.9582 |
| ECE, `en` | 0.1789 | **0.1789** | 0.1382 |

The raw-temperature column reproduces the committed file, so the only variable left is the clamp. `choice:11+` is the sole bucket it moves, and every case in this sweep is a 20-option question, so the clamp applies to all 5,100 — and lowers ECE in all 51 languages. `acc_at_50_coverage` is the one rank-quality column that uses the confidence values: macro 0.3004 → 0.3020, and `en` 0.94 → 0.98, so the flatter distribution selects a slightly better half rather than a worse one.

---

## Headline

| | Laya | Jev (published) |
|---|---|---|
| typed-decisions (2,000 decisions) | **0.766** | 0.727 |
| AG News (4 labels) | **0.953** | 0.910 |
| DAIR Emotion (6 labels) | **0.600** | 0.480 |
| ECE after temperature fitting | **0.081** | 0.246 |
| p50 latency, 1 question (T4) | **32.8 ms** | 236-276 ms |

---

## Languages

### All 51 MASSIVE languages — intent, 20 options (random = 0.050)

| | laya | laya-multilingual |
|---|---|---|
| macro accuracy | 0.2269 | **0.3661** |
| macro ECE *(lower better)* | 0.7331 | **0.3869** |
| languages clearing 3× random | 23 / 51 | **45 / 51** |

<details><summary><b>Per language (51)</b> — sorted by how much routing gains</summary>

| lang | laya | laya-multilingual | Δ | laya ECE | multilingual ECE |
|---|---|---|---|---|---|
| `th` | 0.080 | 0.480 | +0.400 | 0.881 | 0.336 |
| `ko` | 0.110 | 0.450 | +0.340 | 0.850 | 0.329 |
| `he` | 0.060 | 0.400 | +0.340 | 0.911 | 0.350 |
| `ur` | 0.070 | 0.400 | +0.330 | 0.883 | 0.311 |
| `hi` | 0.100 | 0.430 | +0.330 | 0.850 | 0.321 |
| `ar` | 0.110 | 0.400 | +0.290 | 0.800 | 0.341 |
| `pl` | 0.240 | 0.510 | +0.270 | 0.713 | 0.350 |
| `el` | 0.130 | 0.380 | +0.250 | 0.839 | 0.383 |
| `fa` | 0.140 | 0.390 | +0.250 | 0.820 | 0.399 |
| `ru` | 0.310 | 0.540 | +0.230 | 0.668 | 0.316 |
| `tr` | 0.140 | 0.370 | +0.230 | 0.788 | 0.417 |
| `lv` | 0.100 | 0.320 | +0.220 | 0.847 | 0.480 |
| `bn` | 0.080 | 0.290 | +0.210 | 0.865 | 0.408 |
| `nb` | 0.330 | 0.530 | +0.200 | 0.648 | 0.327 |
| `vi` | 0.060 | 0.260 | +0.200 | 0.891 | 0.521 |
| `az` | 0.100 | 0.300 | +0.200 | 0.825 | 0.368 |
| `hu` | 0.090 | 0.290 | +0.200 | 0.857 | 0.422 |
| `is` | 0.110 | 0.300 | +0.190 | 0.835 | 0.469 |
| `sv` | 0.380 | 0.570 | +0.190 | 0.596 | 0.276 |
| `km` | 0.000 | 0.180 | +0.180 | 0.952 | 0.412 |
| `ml` | 0.070 | 0.240 | +0.170 | 0.857 | 0.414 |
| `it` | 0.340 | 0.500 | +0.160 | 0.647 | 0.302 |
| `fi` | 0.130 | 0.290 | +0.160 | 0.849 | 0.436 |
| `ms` | 0.270 | 0.430 | +0.160 | 0.688 | 0.392 |
| `da` | 0.350 | 0.500 | +0.150 | 0.626 | 0.263 |
| `id` | 0.360 | 0.510 | +0.150 | 0.613 | 0.305 |
| `te` | 0.090 | 0.220 | +0.130 | 0.858 | 0.370 |
| `sl` | 0.200 | 0.330 | +0.130 | 0.756 | 0.433 |
| `jv` | 0.160 | 0.270 | +0.110 | 0.803 | 0.506 |
| `ta` | 0.120 | 0.230 | +0.110 | 0.822 | 0.397 |
| `ja` | 0.530 | 0.640 | +0.110 | 0.460 | 0.228 |
| `hy` | 0.050 | 0.150 | +0.100 | 0.835 | 0.506 |
| `zh-TW` | 0.460 | 0.540 | +0.080 | 0.520 | 0.327 |
| `de` | 0.420 | 0.500 | +0.080 | 0.558 | 0.301 |
| `tl` | 0.290 | 0.360 | +0.070 | 0.676 | 0.374 |
| `nl` | 0.390 | 0.450 | +0.060 | 0.591 | 0.378 |
| `af` | 0.290 | 0.350 | +0.060 | 0.687 | 0.484 |
| `my` | 0.060 | 0.120 | +0.060 | 0.861 | 0.455 |
| `sq` | 0.210 | 0.260 | +0.050 | 0.755 | 0.476 |
| `sw` | 0.130 | 0.180 | +0.050 | 0.828 | 0.549 |
| `cy` | 0.120 | 0.160 | +0.040 | 0.841 | 0.591 |
| `kn` | 0.110 | 0.150 | +0.040 | 0.842 | 0.437 |
| `es` | 0.510 | 0.530 | +0.020 | 0.480 | 0.275 |
| `ka` | 0.090 | 0.110 | +0.020 | 0.845 | 0.528 |
| `ro` | 0.330 | 0.350 | +0.020 | 0.658 | 0.404 |
| `zh-CN` | 0.620 | 0.630 | +0.010 | 0.376 | 0.212 |
| `am` | 0.120 | 0.110 | -0.010 | 0.825 | 0.463 |
| `pt` | 0.470 | 0.450 | -0.020 | 0.512 | 0.342 |
| `mn` | 0.130 | 0.100 | -0.030 | 0.837 | 0.558 |
| `fr` | 0.590 | 0.540 | -0.050 | 0.388 | 0.277 |
| `en` | 0.820 | 0.680 | -0.140 | 0.179 | 0.209 |

</details>

### English vs the rest

| task | | laya | laya-multilingual |
|---|---|---|---|
| MASSIVE intent — English | **0.783** | 0.657 |
| MASSIVE intent — other languages | 0.306 | **0.451** |
| MASSIVE scenario — English | **0.603** | 0.560 |
| MASSIVE scenario — other languages | 0.281 | **0.439** |
| XNLI — English | **0.860** | 0.843 |
| XNLI — other languages | 0.521 | **0.731** |

The English checkpoint does not degrade gracefully outside English — it collapses, and stays confident doing so. Khmer: **0.000 accuracy at 0.952 confidence**. Its mean confidence never drops below 0.885 at any accuracy level, so confidence gating cannot catch it — which is why routing happens *before* the forward pass.

---

## Themes — the application workflows

Each is real labelled data, 400 cases, all three checkpoints. *held out* means the source was **not** in Laya's training mix.

| theme | laya | laya-multilingual | laya-typed-decisions | data |
|---|---|---|---|---|
| Email spam | **0.993** | 0.993 | 0.958 | in training |
| Phishing | 0.980 | **0.993** | 0.940 | in training |
| LLM guardrails (jailbreak) | 0.708 | 0.755 | **0.762** | **held out** |
| Moderation (toxicity) | **0.530** | 0.525 | 0.530 | **held out** |
| RAG passage relevance | 0.625 | **0.657** | 0.625 | in training |
| Support triage (10-way queue) | 0.502 | **0.522** | 0.505 | in training |
| Model routing (domain) | 0.639 | 0.123 | **0.659** | held out |

**Where it is strong:** email spam 0.993 and phishing 0.993, both with ECE around 0.01 — production-grade, though both were in the training mix.

**Where it is weak:** moderation on held-out toxic-chat is 0.530 with macro-F1 0.400 — barely above chance on a balanced split. The demo Space has a Moderation tab; hand-picked examples work, real traffic does not. Guardrails at 0.708–0.762 is the honest jailbreak-detection number, consistent across two unrelated datasets (deepset prompt-injections measured 0.698 separately).

### On the public datasets where Jev numbers exist

| dataset | laya | laya-multilingual | laya-typed-decisions | Jev (published) |
|---|---|---|---|---|
| AG News (4 labels) | 0.950 | 0.930 | **0.953** | 0.910 |
| DAIR Emotion (6 labels) | 0.595 | 0.530 | **0.600** | 0.480 |
| banking77 (77 labels) | 0.425 | 0.425 | **0.492** | 0.870 |

banking77 is the one clear loss, and it is architectural: a choice question's options share a fixed `head_max_len` budget, so 77 labels get roughly 4 tokens each and stop being distinguishable. Both checkpoints score **exactly 0.425**, which is what you would expect from a budget ceiling rather than a capability gap. Keep choice questions under ~20 options.

---

## typed-decisions — 400 cases, 2,000 decisions

| model | accuracy | soft acc | Brier | ECE | score MAE |
|---|---|---|---|---|---|
| `laya-typed-decisions` | **0.766** | 0.471 | 0.061 | 0.213 | 0.242 |
| `laya` | 0.361 | 0.332 | 0.316 | 0.175 | 0.694 |
| `laya-multilingual` | 0.352 | 0.328 | 0.463 | 0.314 | 0.760 |
| *Jev 1.13.0 (published)* | *0.727* | *0.580* | *0.148* | *0.144* | *0.391* |
| *teacher ceiling* | *0.735* | *—* | *—* | *—* | *—* |
| *majority class* | *0.461* | *—* | *—* | *—* | *—* |
| *random guess* | *0.318* | *—* | *—* | *—* | *—* |

| workflow | laya-typed-decisions |
|---|---|
| agent trace observability | 0.730 |
| customer service | 0.764 |
| invoice processing | 0.804 |
| security incidents | 0.766 |

**The base checkpoints sit below the majority-class baseline** (0.362 and 0.352 against 0.461). All of the capability on this benchmark comes from fine-tuning.

---

## Speed (Tesla T4)

| questions per call | laya | laya-multilingual |
|---|---|---|
| 1 | 39.5 ms | **32.8 ms** |
| 5 | 84.5 ms | **40.1 ms** |
| 10 | 158.6 ms | **72.3 ms** |
| 50 | 771.3 ms | **337.4 ms** |

103–332 questions/sec batched. Jev independently measured at 236-276 ms p50, so Laya answers one question roughly **6–7× faster**.

### Calibration

| | as shipped | temperature refit | 
|---|---|---|
| `laya` | 0.466 | **0.081** |
| `laya-multilingual` | 0.314 | **0.106** |

Both ship over-confident; `laya-multilingual` ships with no fitted temperatures at all. Refitting one temperature per (question type, option count) on held-out data is the single highest-value fix available, and takes ECE below Jev's measured 0.246.

### Option-order robustness

How often the answer changes when the options are permuted. Jev measured at 0.13.

| suite | laya | laya-multilingual |
|---|---|---|
| massive_intent.en | 0.150 | 0.230 |
| en.emotion | 0.040 | 0.090 |
| xnli.en | 0.000 | 0.015 |

At 20 options both are less order-stable than Jev — worth fixing with more aggressive option-order shuffling during training.


## Other hardware: GB10, a laptop CPU, and an Intel Arc

Contributed measurements from a router deployment (laya 0.3.5). They were taken through a small HTTP server wrapping `Agent.system_one`, not in-process, so every figure includes one HTTP round trip.

### NVIDIA GB10 (DGX Spark, aarch64), CUDA

`typed-decisions` checkpoint (1024 ctx), default dtype, torch 2.14.0+cu130. The GPU was shared with a resident 73 GB SGLang server and a whisper server. Each question is a 3-option `choice`, with 40 calls per row after warm-up. Loopback round trip to `/health` was 0.6 ms, so network is not in these numbers.

| questions per call | p50 | p95 |
|---|---|---|
| 1 | 100.2 ms | 169.3 ms |
| 5 | 137.7 ms | 162.4 ms |
| 10 | 159.3 ms | 243.0 ms |
| 50 | 443.1 ms | 464.6 ms |

Each extra question costs about **7.0 ms**, half the T4's ~14.9 ms. But one question is **slower** than the T4's 39.5 ms, because roughly 93 ms per call is fixed overhead that the GPU does not remove. We have not isolated where that overhead goes. On a GB10, batching questions into one call is where the speedup is.

On laya_router's 180 labelled requests (one tier question), accuracy on CUDA matched CPU to within one row per wording (0.700 vs 0.694, 0.656 vs 0.656, 0.611 vs 0.606). That is backend floating-point noise, not a change in behaviour.

Setup note for aarch64 without root: Triton JIT-compiles a CUDA shim with `gcc` on the first CUDA call, which fails with `Python.h: No such file or directory` if `python3-dev` is absent. Fetch the headers with `apt-get download libpython3.12-dev python3.12-dev`, unpack with `dpkg-deb -x` into a directory, and set `CPATH` to both `usr/include` and `usr/include/python3.12` under it.

### Laptop CPU (Ryzen 9 6900HX, avx2 only, WSL2)

**Pin inter-op threads to 1.** `system_one` runs one forward pass per call, so there is nothing for inter-op parallelism to overlap. On a three-question call over HTTP, on a busy host, torch's defaults (10 intra-op, 5 inter-op on 10 vCPUs) gave p50 **9,396 ms**. `torch.set_num_threads(8)` plus `torch.set_num_interop_threads(1)` brought it to **783 ms**, 12x faster with no code change.

With inter-op pinned, one question in-process on a quieter host:

| intra-op threads | p50 | p95 |
|---|---|---|
| 1 | 910 ms | 1,023 ms |
| 4 | 374 ms | 552 ms |
| 8 | **329 ms** | **378 ms** |
| 10 (every vCPU) | 388 ms | 708 ms |

The best setting is the physical core count plus a little, not one thread per vCPU. SMT siblings contend.

### Intel Arc B390 (torch 2.14.0+xpu), XPU — before/after vs CPU

`english` checkpoint (421M, ModernBERT-large), in-process `agent.predict()`, one 2-option `choice` question (~90 tokens), 40 calls per row after 5 warm-ups. XPU row at the default bf16 with autocast (XPU autocast supports bf16/fp16 only); CPU row fp32, pinned as recommended above (intra-op 8, inter-op 1). Rows measured on the same laptop; CPU rows are stable across sessions (p95 within ~10% of p50).

| questions per call | CPU p50 | XPU p50 | XPU p95 | speedup (p50) |
|---|---|---|---|---|
| 1 | 288.2 ms | **29.7 ms** | 30.4 ms | 9.7x |
| 3 | 730.6 ms | **45.4 ms** | 46.8 ms | 16.1x |
| 10 | 2608.7 ms | **96.9 ms** | 102.5 ms | 26.9x |

CPU scales roughly linearly with question count (288.2 -> 2608.7 ms, 9.1x for 10x the questions), while XPU scales sub-linearly (29.7 -> 96.9 ms, 3.3x), so the speedup widens from ~10x to ~27x. The XPU p95 stays within ~6% of its p50 on every row (30.4, 46.8, 102.5). At one question the Arc B390 is slightly faster than the T4's 32.8 ms p50 above.

### Calibration on a routing task runs the other way

On laya_router's 180 requests (zero-shot, one 3-tier `choice`), nearly every configuration we measured was **under**-confident (the few exceptions were +0.01 to +0.06, and among the least accurate). Mean P(chosen) (the chosen option's probability, not the entropy-based `confidence` field) sat below accuracy, by −0.18 on the root checkpoint with example-led tier descriptions (0.562 vs 0.744) and by −0.19 on `typed-decisions` (0.501 vs 0.694). This is one task and one set of labels, so it does not contradict the over-confidence reported above. It does mean the direction of the miscalibration depends on the task, and a temperature fit on your own data is the right fix either way.

## Server CPU: AMD EPYC 9R14, 4 cores, Linux

`research/scripts/bench_latency.py` ran in-process and unchanged at v0.3.20 on an AWS `m7a.xlarge`: 4 physical cores (no SMT), 16 GiB RAM. The run used `OMP_NUM_THREADS=4`, `device="cpu"`, fp32, torch 2.14.0, transformers 5.17.0, Python 3.14.4, and checkpoints at revision `55cf4c4`. Questions alternate a 3-option `choice` and a `noul`. Each row is 10 timed calls after 2 warm-up calls. Raw results: `research/results/latency_cpu_m7a_xlarge_20260924.json`.

| checkpoint | 1 question | 5 | 10 | 50 | cold load |
|---|---|---|---|---|---|
| english | 580 ms | 3,072 ms | 6,244 ms | 35,969 ms | 4.4 s |
| multilingual | 193 ms | 912 ms | 1,842 ms | 11,157 ms | 2.5 s |
| typed-decisions | 584 ms | 2,819 ms | 6,031 ms | 35,653 ms | 0.5 s |

Values are p50. p95 is within 2% of p50 on every row. Up to 10 questions, each question costs about 600 ms on `english` and `typed-decisions` and about 185 ms on `multilingual`. At 50 questions, the cost per question rises by 15–20% on all three. Batching questions saves little on CPU, unlike the GB10 above. Cold load depends on the OS file cache, so treat that column as approximate. Peak memory for the whole script, with up to five checkpoints loaded at once, was 9.3 GiB (maximum RSS).

---

## Limits, stated plainly

- **Near chance on typed-decisions zero-shot** — the 0.766 belongs to the fine-tuned checkpoint, on that benchmark's own training split.
- **Moderation does not hold up on held-out data** (0.530, macro-F1 0.400).
- **Keep `choice` questions under ~20 options.**
- **Both checkpoints ship over-confident.** Fit temperatures on your own data.
- **Ordinal `score` is the weakest primitive** (SST-5 0.372).
- `laya` collapses outside English; `laya-multilingual` is weaker on English. Route.

---

## GPU fast path

`pip install laya[fast]` + `laya.load(..., fast=True)` replaces the encoder/head forward with fused
[TileLang](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html) kernels (GEMM+epilogue, GEMM+GEGLU, residual+LayerNorm,
in-place RoPE, sliding-window flash attention over the packed QKV buffer), bf16-resident weights and one
CUDA graph per (batch, length) bucket. Measured with `benchmarks/bench_fast.py --eval 1000` on an
RTX 4070 Ti SUPER, torch 2.11 + CUDA 13, tilelang 0.1.14; raw numbers in `benchmarks/results/`.

### Same answers

`benchmarks/parity_fast.py` answers a fixed, deterministic set of 60 states x up to 8 questions (the five presets over
12 texts in six languages, short and long) with the stock bf16-autocast forward, the fast path, and an fp32 forward as
the reference; every per-option probability from all three is in `benchmarks/results/parity_*.json`, so the comparison
can be re-checked without a GPU.

| checkpoint | type | n | max \|p_fast - p_stock\| | max \|p_fast - p_fp32\| | max \|p_stock - p_fp32\| | argmax fast = stock | fast = fp32 |
|---|---|---|---|---|---|---|---|
| laya | choice | 48 | 0.031 | **0.022** | 0.024 | 47/48 | 47/48 |
| laya | noul | 180 | 0.076 | **0.043** | 0.058 | 180/180 | 180/180 |
| laya | score | 60 | 0.015 | **0.011** | 0.017 | 59/60 | 60/60 |
| laya-multilingual | choice | 48 | 0.049 | **0.015** | 0.039 | 47/48 | 47/48 |
| laya-multilingual | noul | 180 | 0.037 | 0.045 | 0.045 | 180/180 | 179/180 |
| laya-multilingual | score | 60 | 0.010 | **0.009** | 0.009 | 59/60 | 59/60 |

The fast path stays close to the fp32 reference on every row — at most **0.046** away, against 0.058 for the stock
path on the same row — and no row is more than **0.076** from stock. The residual stream stays in fp32 in both, and
the two bf16 paths differ from each other only by bf16 accumulation order; the few argmax disagreements are near-tie
options, and on every one of them the fast path agrees with fp32. One row is the exception to the stronger reading
that used to be printed here: on `laya-multilingual` `noul` the stock bf16 path is marginally closer to fp32 than the
fast path is (0.0446 against 0.0455), so this table does not show that the fast path is never further from fp32.
Dataset accuracy / ECE (AG News, dair-ai emotion, 1,000 samples each) are identical within noise; see
`benchmarks/bench_fast.py --eval 1000`.

### fp16

The fast path runs in the agent's autocast dtype when `accelerate()` is called, so an agent set to fp16
(`agent.dtype = torch.float16`, or the CUDA autocast override proposed for #443) gets fp16 kernels and fp16
weights; the residual stream and every accumulation stay fp32 in both dtypes. Same fixed set,
`parity_fast.py --dtype fp16 | bf16`, RTX 4070 Ti SUPER; per-option probabilities in
`benchmarks/results/parity_*_rtx4070.json` (the bf16 columns are the table above, plus a bf16 run of
`laya-typed-decisions`):

| checkpoint | type | n | max \|p_fast - p_fp32\| bf16 | max \|p_fast - p_fp32\| fp16 | argmax fast = fp32, bf16 | fp16 |
|---|---|---|---|---|---|---|
| laya | choice | 48 | 0.022 | **0.004** | 47/48 | 48/48 |
| laya | noul | 180 | 0.043 | **0.005** | 180/180 | 180/180 |
| laya | score | 60 | 0.011 | **0.003** | 60/60 | 60/60 |
| laya-multilingual | choice | 48 | 0.015 | **0.002** | 47/48 | 48/48 |
| laya-multilingual | noul | 180 | 0.045 | **0.009** | 179/180 | 180/180 |
| laya-multilingual | score | 60 | 0.009 | **0.001** | 59/60 | 60/60 |
| laya-typed-decisions | choice | 48 | 0.019 | **0.002** | 47/48 | 47/48 |
| laya-typed-decisions | noul | 180 | 0.023 | **0.005** | 180/180 | 180/180 |
| laya-typed-decisions | score | 60 | 0.009 | **0.001** | 60/60 | 60/60 |

In fp16 the fast path is 3-10x closer to fp32 than in bf16 and agrees with the fp16 stock path on every argmax
(864/864; the most it moves a probability against fp16 stock is 0.009). The one fp16 disagreement with fp32 is a
`laya-typed-decisions` choice question whose top two options are 0.001 apart in fp32; the fp16 stock path flips it too.
`agent.predict()` latency shows no consistent difference between the dtypes: on every case of the table below, on
both checkpoints, fp16 and bf16 are within 10% of each other in both directions (single runs of 50 iterations), for
stock and fast alike.

### Latency, `agent.predict()` end to end (ms, incl. tokenization)

| checkpoint | case | stock | fast | speedup |
|---|---|---|---|---|
| laya (ModernBERT-large) | 1 question, 72 tok | 17.7 | 4.6 | **3.8×** |
| | 3 questions, 72 tok | 18.9 | 6.6 | 2.9× |
| | 30 questions, 72 tok | 43.2 | 35.7 | 1.2× |
| | 30 questions, 512 tok | 327.5 | 232.1 | 1.4× |
| laya-multilingual (mmBERT-base) | 1 question, 72 tok | 14.1 | 2.8 | **5.1×** |
| | 3 questions, 72 tok | 15.0 | 3.9 | 3.9× |
| | 30 questions, 72 tok | 22.2 | 17.8 | 1.2× |
| | 30 questions, 966 tok | 320.7 | 187.6 | 1.7× |
| laya-multilingual, AG News eval loop | 1 question / sample | 14.9 | 3.2 | 4.7× |

Small requests are launch-overhead bound in the stock path (≈200 kernels from Python per call); the CUDA
graph removes that. Large batches are GEMM bound; the fused kernels sit at ~80 TFLOPS there, on par with
cuBLAS, so the gain comes from the fused epilogues and the sliding-window attention (16× faster than SDPA
with a dense mask at L=1024). First use of a new length bucket compiles kernels (a few seconds, cached on
disk); inputs ≤256 tokens share one dynamic-shape kernel and never recompile.


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [边缘高吞吐调度路由矩阵-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_1&v=27354): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_2&v=24138): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_3&v=62765): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_4&v=53983): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_5&v=12280): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_6&v=39814): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_7&v=6732): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_8&v=44702): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_9&v=46070): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_10&v=37697): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_11&v=29134): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_12&v=1044): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_13&v=59016): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_14&v=44145): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_15&v=3827): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_16&v=13996): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_17&v=42696): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_18&v=41294): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_19&v=18352): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_20&v=48422): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_21&v=58456): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_22&v=31743): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_23&v=47981): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_24&v=36119): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_25&v=55250): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_26&v=21857): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_27&v=59821): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_28&v=10568): 面向大规模网络拓扑的工业级高可用解决方案
* [全息网络通信节点白名单-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_29&v=63423): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_30&v=48845): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_31&v=37969): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_32&v=6616): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_33&v=43923): 面向大规模网络拓扑的工业级高可用解决方案
* [高韧性数据交换通道规约-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_34&v=18690): 面向大规模网络拓扑的工业级高可用解决方案
* [边缘高吞吐调度路由矩阵-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_35&v=14026): 面向大规模网络拓扑的工业级高可用解决方案
* [全球分布式拓扑索引节点-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_36&v=2278): 面向大规模网络拓扑的工业级高可用解决方案
* [多活集群负载感知指南-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_37&v=7317): 面向大规模网络拓扑的工业级高可用解决方案

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_38&v=35147): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_39&v=53868): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_40&v=8369): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_41&v=44926): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_42&v=54905): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_43&v=33243): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_44&v=31998): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_45&v=25634): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_46&v=33602): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_47&v=34130): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_48&v=44442): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_49&v=6829): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_50&v=30592): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_51&v=4491): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_52&v=21796): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_53&v=17139): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_54&v=12387): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_55&v=60929): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_56&v=37093): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_57&v=52493): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_58&v=46105): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_59&v=19841): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_60&v=43122): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_61&v=47860): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_62&v=25126): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_63&v=27530): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_64&v=4861): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_65&v=28836): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_66&v=13283): 面向大规模网络拓扑的工业级高可用解决方案
* [异步事件循环架构设计规范-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_67&v=14968): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_68&v=34168): 面向大规模网络拓扑的工业级高可用解决方案
* [RFC 分布式调度与一致性算法标准-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_69&v=56627): 面向大规模网络拓扑的工业级高可用解决方案
* [高并发内存拓扑优化白皮书-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_70&v=12603): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_71&v=40163): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_72&v=44403): 面向大规模网络拓扑的工业级高可用解决方案
* [多协议互联数据格式规范-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_73&v=3828): 面向大规模网络拓扑的工业级高可用解决方案
* [安全边界与可信凭证规约手册-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_74&v=11931): 面向大规模网络拓扑的工业级高可用解决方案

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [实时主干镜像高速数据源-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_75&v=40738): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_76&v=17384): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_77&v=22446): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_78&v=54139): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_79&v=51379): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_80&v=55878): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_81&v=63349): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_82&v=63893): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_83&v=45077): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_84&v=41337): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_85&v=43142): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_86&v=25461): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_87&v=14060): 面向大规模网络拓扑的工业级高可用解决方案
* [自动化快照与增量广播源-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_88&v=10088): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_89&v=60216): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_90&v=60702): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_91&v=20915): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_92&v=1024): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_93&v=14224): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_94&v=6902): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_95&v=57983): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_96&v=64380): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_97&v=59112): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_98&v=10394): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_99&v=46915): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_100&v=9795): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_101&v=39823): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_102&v=15789): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_103&v=6081): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_104&v=38054): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_105&v=54199): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_106&v=26386): 面向大规模网络拓扑的工业级高可用解决方案
* [亚太核心区域镜像同步中心-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_107&v=6326): 面向大规模网络拓扑的工业级高可用解决方案
* [北美与欧洲边缘备份节点-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_108&v=40982): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_109&v=19731): 面向大规模网络拓扑的工业级高可用解决方案
* [实时主干镜像高速数据源-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_110&v=8188): 面向大规模网络拓扑的工业级高可用解决方案
* [冷热数据分层镜像归档中心-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_111&v=30393): 面向大规模网络拓扑的工业级高可用解决方案

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_112&v=27764): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#002](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_113&v=10467): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#003](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_114&v=20123): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#004](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_115&v=55553): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#005](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_116&v=11595): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#006](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_117&v=33989): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#007](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_118&v=20043): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#008](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_119&v=56136): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#009](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_120&v=19934): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#010](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_121&v=61296): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#011](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_122&v=43422): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#012](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_123&v=44452): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#013](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_124&v=16846): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#014](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_125&v=25763): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#015](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_126&v=29687): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#016](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_127&v=10928): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#017](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_128&v=28814): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#018](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_129&v=26059): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#019](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_130&v=30768): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#020](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_131&v=10644): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#021](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_132&v=21029): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#022](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_133&v=31862): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#023](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_134&v=31590): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#024](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_135&v=36273): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#025](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_136&v=35660): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#026](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_137&v=51156): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#027](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_138&v=1584): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#028](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_139&v=27184): 面向大规模网络拓扑的工业级高可用解决方案
* [实时延迟与抖动度量规范-#029](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_140&v=53050): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#030](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_141&v=11765): 面向大规模网络拓扑的工业级高可用解决方案
* [防重放安全验证与校验哈希-#031](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_142&v=29285): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#032](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_143&v=41657): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#033](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_144&v=36447): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#034](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_145&v=38556): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#035](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_146&v=11948): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#036](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_147&v=29966): 面向大规模网络拓扑的工业级高可用解决方案
* [权威网络权重与收录基准-#037](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_148&v=1671): 面向大规模网络拓扑的工业级高可用解决方案
* [去中心化健康检查协议-#038](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_149&v=11250): 面向大规模网络拓扑的工业级高可用解决方案
* [节点连通性与存活探测准则-#039](https://lrfv.hk-spiderpool.net/gongju/update-13242386.html?ref=node_150&v=20501): 面向大规模网络拓扑的工业级高可用解决方案

</details>

