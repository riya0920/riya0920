## Riya Soni

ML/AI engineer. I build inference systems that hold a latency budget under real load, and evaluation harnesses that can fail a build.

MS Computer Science, Stevens Institute of Technology (May 2026). Currently Data Analyst at Giraffe Media Group, building SMS analytics infrastructure on Redshift and shipping internal AI tooling.

**Open to ML/AI Engineer roles starting May 2026.** New York metro.

---

### Featured

**[streaming-tts-serving](https://github.com/riya0920/streaming-tts-serving)** — VITS behind a decoupled Triton backend written in C++, TensorRT FP16 engines under it, and a Go gateway holding the WebSockets. Latency stops tracking sentence length: **3,200 held sessions across 2 GPUs at 113.8 ms p99 time-to-first-audio, 0 underruns and 0 rejections**, 350x aggregate real-time factor.
[**Demo with audio**](https://riya0920.github.io/streaming-tts-serving/) · `Triton` `TensorRT` `C++` `Go` `CUDA`

**[asr-serving-vllm-k8s](https://github.com/riya0920/asr-serving-vllm-k8s)** — Whisper-Large on vLLM, autoscaled on Kubernetes by *queue depth* rather than GPU utilization (KEDA + Prometheus), shipped through Argo CD behind a word-error-rate gate in CI. **21.8x throughput over a sequential HF baseline, 1.60% WER reproduced to four decimals across three hosts, 92% lower cost per audio hour.**
[**Run it in Colab**](https://colab.research.google.com/github/riya0920/asr-serving-vllm-k8s/blob/main/notebooks/demo.ipynb) · `vLLM` `Kubernetes` `KEDA` `Argo CD` `Prometheus`

**[rag-eval-harness](https://github.com/riya0920/rag-eval-harness)** — Hybrid retrieval (BM25 + dense, RRF fusion) written without a framework, a hand-authored golden set, and a CI gate that blocks a retrieval regression — plus an inverted CI step that fails the build if the gate itself stops catching a planted regression. `docs/LIMITATIONS.md` states in writing that this project's own headline comparison sits inside the noise band.
[**Evaluation results**](https://riya0920.github.io/rag-eval-harness/) · `Python` `BM25` `RRF` `GitHub Actions`

**[earnings-intelligence-platform](https://github.com/riya0920/earnings-intelligence-platform)** — Self-evaluating RAG over SEC 10-K filings: 12 configurations (3 chunking strategies x 4 retrieval strategies) scored with RAGAS and tracked end to end in MLflow. Quantitative questions skip the prose path entirely and route through a Hunter → Forensic Auditor → Arbiter chain that returns structured numbers with paragraph-level provenance, because a confident LLM reading a forecast figure as an actual is the failure mode that matters in financial NLP.
`ChromaDB` `sentence-transformers` `RAGAS` `MLflow` `LangGraph`

**[self-healing-rl-pipeline](https://github.com/riya0920/self-healing-rl-pipeline)** — Four agents over MCP and A2A — Monitor, Diagnostics, Repair, Verification — that detect drift in a DQN recommender, diagnose the root cause, retrain the policy, and verify the fix with no human in the loop.
`PyTorch` `FastAPI` `MCP` `A2A` `LangSmith`

**[predictive-maintenance-rul-platform](https://github.com/riya0920/predictive-maintenance-rul-platform)** — Remaining-useful-life prediction on real NASA C-MAPSS data, all four sub-datasets: PHM08 asymmetric scoring, piecewise RUL cap, alpha-lambda accuracy, an alarm policy priced on lead time against nuisance-alarm cost, an ONNX edge benchmark, and a drift-triggered retrain rule. `docs/RESULTS.md` is written *by* the training run, not ahead of it.
`PyTorch` `ONNX` `scikit-learn`

---

### Domain portfolios

Forty-five further repositories, nine per domain. Each was built to its differentiator first — the machinery a screener stops for — then extended by closing the worst gap its own README named. No project claims a number it did not measure, and every README states what is **not** built rather than leaving it to be discovered.

| Domain | A few of them |
|---|---|
| **Systems / infra** | [job-queue-guarantees](https://github.com/riya0920/job-queue-guarantees) — crash storm, 0 lost, 0 double-effects · [streaming-quality-pipeline](https://github.com/riya0920/streaming-quality-pipeline) — exactly-once over at-least-once · [recsys-serving-platform](https://github.com/riya0920/recsys-serving-platform) — 436 RPS at 73.6 ms p99, degradable model path |
| **Manufacturing** | [plant-oee-platform](https://github.com/riya0920/plant-oee-platform) — OPC-UA to OEE, waterfall reconciling to 5.8e-11 s · [spc-analytics-platform](https://github.com/riya0920/spc-analytics-platform) — ARL bake-off at calibrated ARL0 · [visual-inspection-line-economics](https://github.com/riya0920/visual-inspection-line-economics) — PPV 0.107 at real prevalence vs 0.960 balanced |
| **Healthcare** | [clinical-nlp-extraction](https://github.com/riya0920/clinical-nlp-extraction) — ConText assertion layer, gold set scored once · [imaging-shortcut-audit](https://github.com/riya0920/imaging-shortcut-audit) — planted confounds recovered three ways · [hl7-fhir-interop](https://github.com/riya0920/hl7-fhir-interop) — HL7 v2 → FHIR R4 over real MLLP |
| **Finance** | [walk-forward-backtest-engine](https://github.com/riya0920/walk-forward-backtest-engine) — Reality Check + SPA, vectorbt cross-check · [governed-fraud-detection](https://github.com/riya0920/governed-fraud-detection) — retraining pipeline that stops at a human · [double-entry-ledger-core](https://github.com/riya0920/double-entry-ledger-core) |
| **Retail** | [product-search-relevance](https://github.com/riya0920/product-search-relevance) — segmented NDCG, LTR scored per millisecond · [marketplace-experimentation](https://github.com/riya0920/marketplace-experimentation) — switchback designs priced on bias and variance · [basket-completion-substitution](https://github.com/riya0920/basket-completion-substitution) |

---

### Research

Mechanistic interpretability of decoder-only transformers, advised by Matthew Finch (Senior ML Engineer, Bloomberg LP). I measure polysemanticity across GPT-2 and Pythia attention heads and prune parasitic SVD components post-hoc, with no retraining — eleven notebooks, each standalone.

The headline pruning result is currently in-sample: greedy selection and reporting run over the same corpus, and the effect sizes sit near the per-document noise floor. Held-out validation is the next step, and I would rather write that here than quote the number.

---

### Stack

`Python` `PyTorch` `C++` `Go` `TypeScript` · `Triton Inference Server` `TensorRT` `vLLM` `ONNX` · `Kubernetes` `KEDA` `Argo CD` `Docker` `GitHub Actions` · `LangGraph` `MCP` `ChromaDB` `FastAPI` · `SQL (Redshift, PostgreSQL, DuckDB)` `dbt` `Kafka` `Prometheus` `Grafana` `AWS`

---

### Reach me

[LinkedIn](https://linkedin.com/in/riya-soni-ml-engineer) · [Email](mailto:riyaasoni2001@gmail.com) · Hoboken, NJ
