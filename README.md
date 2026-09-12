## Riya Soni

I build inference systems that have to hold a latency budget under real load, and evaluation harnesses that are allowed to fail a build. A lot of these repos lead with a number that did not work out, because that is usually the part worth reading.

MS Computer Science, Stevens Institute of Technology, 2026. Currently Data Analyst at Giraffe Media Group, building SMS analytics on Redshift and shipping internal AI tooling.

**Available immediately for ML/AI Engineer, Data Science, and Software Engineering roles.** US based, open to remote.

Jump to: [Serving and inference](#serving-and-inference) · [LLM systems and agents](#llm-systems-and-agents) · [Data engineering and quality](#data-engineering-and-quality) · [Applied ML and decision systems](#applied-ml-and-decision-systems) · [Healthcare data](#healthcare-data) · [Financial systems](#financial-systems)

---

### Serving and inference

**[streaming-tts-serving](https://github.com/riya0920/streaming-tts-serving)**
VITS chunked through a decoupled Triton backend written in C++, TensorRT FP16 engines under it, a Go gateway holding the WebSockets. The point of the shape is that latency stops tracking sentence length: 9.5x the words costs 1.4x the time to first audio. 3,200 held sessions across two RTX 6000 Ada at 113.8 ms p99 time to first audio, 0 underruns and 0 rejections. The load generator shares the host, so the number is not a network measurement, and the README says so.
`Triton` `TensorRT` `C++` `Go` `CUDA`

**[asr-serving-vllm-k8s](https://github.com/riya0920/asr-serving-vllm-k8s)**
Whisper-Large on vLLM, autoscaled on Kubernetes by queue depth rather than GPU utilization (KEDA), shipped through Argo CD behind a word-error-rate gate in CI. 21.8x throughput over a sequential HuggingFace baseline, 1.60% WER, cost per audio hour down 92%. Measured on rented A40, H100 NVL and A10 GPUs. The gate exists for one failure mode: a bad preprocessing config or a truncated weight file that still serves.
`vLLM` `Kubernetes` `KEDA` `Argo CD` `Terraform`

**[recsys-serving-platform](https://github.com/riya0920/recsys-serving-platform)**
Two stage recommender, ANN retrieval into a learned ranker, behind an HTTP service whose model path is built to be killed. It degrades to a popularity fallback instead of returning 5xx. 436 RPS at p99 73.6 ms with zero errors, and the answer is a sweep rather than one number because the next concurrency level buys 10% more throughput and blows the tail by 56%. Offline metrics only, and the shadow path measures whether a candidate can serve, not whether anyone wants it.
`FAISS` `FastAPI` `ANN`

**[profiling-training-study](https://github.com/riya0920/profiling-training-study)**
An optimisation ladder where every rung changes one thing and the profile picks the order. The `workers` rung is +298%. The eight rungs after it sum to about +17%. Data-wait never drops below 79%, so the GPU stays starved through every kernel-level tweak, which is the actual finding. An earlier version claimed communication was 16.7% of step time. That was noise, the claim was wrong, and the correction is in the README with the spread that killed it.
`PyTorch` `DDP` `CUDA` `torch.compile`

---

### LLM systems and agents

**[earnings-intelligence-platform](https://github.com/riya0920/earnings-intelligence-platform)**
Self-evaluating RAG over SEC 10-K filings. It benchmarks 12 configurations (three chunking strategies by four retrieval strategies) with RAGAS and MLflow. For numeric questions the retrieved chunks go through a vendored multi-agent auditor (Hunter, Forensic Auditor, Arbiter) that returns numbers with paragraph-level provenance, because a plain RAG answer will confidently read a forecast figure as an actual.
`RAG` `RAGAS` `MLflow` `cross-encoder`

**[rag-eval-harness](https://github.com/riya0920/rag-eval-harness)**
Hybrid retrieval (BM25 plus dense, RRF fusion) with no framework, a hand-authored golden set, and a CI gate that blocks a regression. There is also an inverted CI step that fails the build if the gate stops catching a planted regression. `docs/LIMITATIONS.md` states in writing that this project's own headline comparison sits inside the noise band. The NLI judge was built, measured, and did not beat the rules, so it did not ship.
`BM25` `RRF` `GitHub Actions`

**[self-healing-rl-pipeline](https://github.com/riya0920/self-healing-rl-pipeline)**
Four agents over MCP and A2A (Monitor, Diagnostics, Repair, Verification) sitting on a DQN recommender. When the content stream drifts to categories the policy never saw, the loop detects the reward drop, diagnoses it as out-of-distribution, retrains, and verifies the fix without a human in it.
`PyTorch` `DQN` `MCP` `A2A`

**[shadow-it-compliance-agent](https://github.com/riya0920/shadow-it-compliance-agent)**
A five-stage LangGraph pipeline that flags corporate data leaks in chat, with a finding that surprised me: gpt-4o scored six F1 points below gpt-4o-mini on the same adversarial data (0.783 vs 0.846), and chain-of-thought without retrieval was worse than no chain-of-thought at all. The multi-agent plus RAG configuration is the only one that holds both precision and recall (F1 0.896).
`LangGraph` `Pydantic` `RAG`

---

### Data engineering and quality

**[streaming-quality-pipeline](https://github.com/riya0920/streaming-quality-pipeline)**
Bronze to silver to gold with exactly-once effects over at-least-once delivery, shown by killing the consumer mid-batch 40 times and checking the result against the producer's own manifest. Bad data is quarantined, never dropped and never fatal. Silver is a Delta table; SQLite stays as the offset coordinator because that is where the single-transaction guarantee actually holds.
`Delta Lake` `Kafka` `exactly-once`

**[market-data-stream-parity](https://github.com/riya0920/market-data-stream-parity)**
A market data pipeline on a live Kraken feed, through a partitioned log into a Parquet archive with a DuckDB serving layer. Event-time bars, per-key watermarks, a rebuild drill that recomputes from the archive and diffs, a schema registry doing real compatibility checking, and the in-process log cross-checked against a real Kafka 4.3.1 broker. There is an on-call runbook written for whoever is holding the pager.
`Kafka` `DuckDB` `Parquet`

**[reconciliation-platform](https://github.com/riya0920/reconciliation-platform)**
Two independently generated feeds that should agree and do not, with six break types planted at realistic rates so the engine is scored rather than eyeballed. Stream completeness proven without a trailer record, a resumable date-range backfill, and a DuckDB reporting mart with drill-down that ties. The controls doc includes a table of the gaps that remain.
`DuckDB` `reconciliation` `audit trail`

**[job-queue-guarantees](https://github.com/riya0920/job-queue-guarantees)**
At-least-once delivery with exactly-once effects, proven by a crash storm that spawns real OS processes and kills them with `os._exit(137)`, which skips every cleanup path. 1,200 jobs, 0 lost, 0 double effects, 21 dead-lettered, which is exactly the poison count.
`queue` `idempotency` `crash-recovery`

---

### Applied ML and decision systems

**[predictive-maintenance-rul-platform](https://github.com/riya0920/predictive-maintenance-rul-platform)**
Remaining-useful-life prediction on real NASA C-MAPSS data, all four sub-datasets. PHM08 asymmetric scoring, a piecewise RUL cap, an alarm policy priced on lead time against nuisance-alarm cost, an ONNX edge benchmark, and a drift-triggered retrain rule. `docs/RESULTS.md` is written by the training run, not ahead of it.
`PyTorch` `ONNX` `scikit-learn`

**[customer-analytics-suite](https://github.com/riya0920/customer-analytics-suite)**
Segmentation, CLV, and attribution on one dataset with the handoffs computed. A real dbt pipeline with a leakage test that fails the build, Shapley at twelve channels with its sampled approximation checked and repaired, and an Airflow DAG. The most useful result is negative: there is a planted unobserved confounder that no attribution method here can beat, and the writeup says so.
`dbt` `Airflow` `PySpark` `Snowflake`

**[marketplace-experimentation](https://github.com/riya0920/marketplace-experimentation)**
An agent-based marketplace where a treatment consumes shared supply, which is what breaks naive A/B tests. The bias is measured against known truth by market tightness. Switchback designs, CUPED, synthetic control with placebo inference, an MDE curve, and a decision-tree playbook for which design to use when.
`causal-inference` `CUPED` `synthetic-control`

**[product-search-relevance](https://github.com/riya0920/product-search-relevance)**
Hybrid product search with graded relevance judgments, BM25F over real fields, FAISS, and a LambdaMART re-ranker. There is a simulated shopper who disagrees with the labels on purpose. Several of the headline results are negative, and one of them overturned a caveat the project had carried for two passes.
`FAISS` `LambdaMART` `BM25F`

---

### Healthcare data

**[hl7-fhir-interop](https://github.com/riya0920/hl7-fhir-interop)**
HL7 v2 to FHIR R4 over real MLLP on TCP, with a hand-rolled v2 parser that survives out-of-order and duplicate messages and Z-segments. There is a PHI access audit log the application cannot write around, plus break-the-glass. 191 tests, and the title says nine known gaps because there are nine.
`HL7v2` `FHIR R4` `MLLP`

**[clinical-nlp-extraction](https://github.com/riya0920/clinical-nlp-extraction)**
Clinical note to structured data with a hand-rolled ConText assertion layer and FHIR output that never turns family history into a patient condition. No real clinical text is used anywhere, on purpose: real notes are PHI, so every sentence is generated or hand-authored, and the gold set is scored once.
`ConText` `FHIR R4` `NER`

**[imaging-shortcut-audit](https://github.com/riya0920/imaging-shortcut-audit)**
An audit harness for shortcut learning in imaging models, with a model attached so the audit has ground truth to check against. Three confounds planted at known strengths, three independent methods to recover them, and the finding that the three methods disagree, for a reason that matters more than any single one. Patient-level splits, and a leak demo that shows what image-level splits hide.
`Grad-CAM` `DICOM` `audit`

**[claims-readmission-risk](https://github.com/riya0920/claims-readmission-risk)**
Thirty-day readmission risk from synthetic payer claims, scored at discharge and evaluated the way a care-management program is actually run, at capacity. A claim-runout temporal guard stops the model reading the future. The README is blunt that this is the load-bearing part of a system and not a deployable one.
`scikit-learn` `calibration` `synthetic-claims`

---

### Financial systems

**[walk-forward-backtest-engine](https://github.com/riya0920/walk-forward-backtest-engine)**
Walk-forward backtesting with White's Reality Check and Hansen's SPA on a stationary bootstrap, a deflated Sharpe, a risk model, and multi-day execution. There is no strategy claim in the repo; the strategies are test cargo and the default series is a random walk with no signal, because anything that looks good on it is a bug. An independent cross-check against vectorbt found two defects in this engine.
`backtesting` `bootstrap` `vectorbt`

**[double-entry-ledger-core](https://github.com/riya0920/double-entry-ledger-core)**
A double-entry ledger with invariants enforced by database trigger, idempotent payment endpoints, and a measured latency curve that shows the single-writer contention rather than hiding it. There is a PostgreSQL 18 port under real SERIALIZABLE that measured something arguing against this project's own central design decision.
`PostgreSQL` `idempotency` `hypothesis`

**[credit-risk-scorecard](https://github.com/riya0920/credit-risk-scorecard)**
A credit scorecard with a LightGBM challenger on the same characteristics, swap-set analysis, and reject inference scored against counterfactual truth. Fair-lending analysis runs on real HMDA applicants with real protected attributes, plus a redlining check on real census tracts.
`LightGBM` `fair-lending` `HMDA`

**[banking-mart-scd2-pit](https://github.com/riya0920/banking-mart-scd2-pit)**
A dbt banking mart with a bitemporal dimension and a bitemporal fact, so you can ask both what was true and what we knew when. Entity resolution scored both ways.
`dbt` `bitemporal` `SCD2`

---

### Currently exploring

**RSNA knee MRI** (Kaggle, deadline October 2026). Working the constraints more than the leaderboard: the test set ships no reports, and there are 58 gold studies, which is too few to select models on cleanly.

**Mechanistic interpretability of GPT-2 and Pythia**, advised by Senior ML Engineer, Bloomberg LP. Measuring polysemanticity across attention heads and pruning parasitic SVD components post-hoc with no retraining. The headline pruning result is still in-sample: greedy selection and reporting run over the same corpus and the effect sizes sit near the per-document noise floor. Held-out validation is the next step, and I would rather write that than quote the number.

---

### Stack

`Python` `C++` `Go` `TypeScript` `SQL` · `Triton` `TensorRT` `vLLM` `ONNX` `PyTorch` · `Kubernetes` `KEDA` `Argo CD` `Docker` `Terraform` `GitHub Actions` · `LangGraph` `MCP` `A2A` `FastAPI` · `dbt` `Airflow` `PySpark` `Kafka` `Delta Lake` `DuckDB` · `Redshift` `PostgreSQL` `Snowflake` · `Prometheus` `Grafana` `MLflow` `AWS`

---

### Reach me

[LinkedIn](https://linkedin.com/in/riya-soni-ml-engineer) · [Email](mailto:riyaasoni2001@gmail.com) · US based, open to remote
