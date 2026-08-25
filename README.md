## Riya Soni

ML/AI engineer. I build inference systems that hold a latency budget under real load, and evaluation harnesses that can fail a build.

MS Computer Science, Stevens Institute of Technology (May 2026). Currently Data Analyst at Giraffe Media Group, building SMS analytics infrastructure on Redshift and shipping internal AI tooling.

**Open to ML/AI Engineer roles starting May 2026.** New York metro.

---

### Featured

**[streaming-tts-serving](https://github.com/riya0920/streaming-tts-serving)**
VITS behind a decoupled Triton backend written in C++, TensorRT FP16 engines under it, and a Go gateway holding the WebSockets. Latency stops tracking sentence length: **3,200 held sessions across 2 GPUs at 113.8 ms p99 time-to-first-audio, 0 underruns and 0 rejections**, 350x aggregate real-time factor.
[**Demo with audio**](https://riya0920.github.io/streaming-tts-serving/) · `Triton` `TensorRT` `C++` `Go` `CUDA`

**[asr-serving-vllm-k8s](https://github.com/riya0920/asr-serving-vllm-k8s)**
Whisper-Large on vLLM, autoscaled on Kubernetes by *queue depth* rather than GPU utilization (KEDA + Prometheus), shipped through Argo CD behind a word-error-rate gate in CI. **21.8x throughput over a sequential HF baseline, 1.60% WER reproduced to four decimals across three hosts, 92% lower cost per audio hour.**
[**Run it in Colab**](https://colab.research.google.com/github/riya0920/asr-serving-vllm-k8s/blob/main/notebooks/demo.ipynb) · `vLLM` `Kubernetes` `KEDA` `Argo CD` `Prometheus`

**[rag-eval-harness](https://github.com/riya0920/rag-eval-harness)**
Hybrid retrieval (BM25 + dense, RRF fusion) written without a framework, a hand-authored golden set, and a CI gate that blocks a retrieval regression, plus an inverted CI step that fails the build if the gate itself stops catching a planted regression. `docs/LIMITATIONS.md` states in writing that this project's own headline comparison sits inside the noise band.
[**Evaluation results**](https://riya0920.github.io/rag-eval-harness/) · `Python` `BM25` `RRF` `GitHub Actions`

**[profiling-training-study](https://github.com/riya0920/profiling-training-study)**
An optimisation ladder where every rung changes exactly one thing and the profile picks the order, not a blog post. Run on CPU, then on a rented NVIDIA L4: **one rung (`workers`) is +298% and the eight rungs after it sum to +17%**, while data-wait never falls below 79%, so the GPU stays starved through every kernel-level tweak. bf16 flips sign between the two machines and is still reported as unestablished, because the spread is 18% wide. Negative results stay in the table.
[**Measured results**](https://riya0920.github.io/profiling-training-study/) · `PyTorch` `DDP` `CUDA` `torch.compile`

**[self-healing-rl-pipeline](https://github.com/riya0920/self-healing-rl-pipeline)**
Four agents over MCP and A2A (Monitor, Diagnostics, Repair, Verification) that detect drift in a DQN recommender, diagnose the root cause, retrain the policy, and verify the fix with no human in the loop.
`PyTorch` `FastAPI` `MCP` `A2A` `LangSmith`

**[predictive-maintenance-rul-platform](https://github.com/riya0920/predictive-maintenance-rul-platform)**
Remaining-useful-life prediction on real NASA C-MAPSS data, all four sub-datasets: PHM08 asymmetric scoring, piecewise RUL cap, alpha-lambda accuracy, an alarm policy priced on lead time against nuisance-alarm cost, an ONNX edge benchmark, and a drift-triggered retrain rule. `docs/RESULTS.md` is written *by* the training run, not ahead of it.
`PyTorch` `ONNX` `scikit-learn`

---

### Research

Mechanistic interpretability of decoder-only transformers, advised by Matthew Finch (Senior ML Engineer, Bloomberg LP). I measure polysemanticity across GPT-2 and Pythia attention heads and prune parasitic SVD components post-hoc, with no retraining. Eleven notebooks, each standalone.

The headline pruning result is currently in-sample: greedy selection and reporting run over the same corpus, and the effect sizes sit near the per-document noise floor. Held-out validation is the next step, and I would rather write that here than quote the number.

---

### Stack

`Python` `PyTorch` `C++` `Go` `TypeScript` · `Triton Inference Server` `TensorRT` `vLLM` `ONNX` · `Kubernetes` `KEDA` `Argo CD` `Docker` `GitHub Actions` · `LangGraph` `MCP` `ChromaDB` `FastAPI` · `SQL (Redshift, PostgreSQL, DuckDB)` `dbt` `Kafka` `Prometheus` `Grafana` `AWS`

---

### Reach me

[LinkedIn](https://linkedin.com/in/riya-soni-ml-engineer) · [Email](mailto:riyaasoni2001@gmail.com) · New York, NY
