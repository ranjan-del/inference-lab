# Roadmap — Inference Lab

## Phase 0 — Benchmark design
- [ ] Review existing load generators and GenAI inference benchmarks.
- [ ] Freeze metric definitions and request lifecycle boundaries.
- [ ] Define privacy-safe workload trace schema.
**Exit:** clear differentiator and reproducible protocol.

## Phase 1 — One-backend MVP
- [ ] Implement trace validation and synthetic workload generation.
- [ ] Implement one backend adapter.
- [ ] Measure TTFT, ITL, end-to-end latency, throughput, errors.
- [ ] Export JSON and Markdown reports.
**Exit:** repeatable baseline run with tests.

## Phase 2 — Load and resource analysis
- [ ] Add open-loop and closed-loop load.
- [ ] Add concurrency/arrival sweeps and tail metrics.
- [ ] Capture GPU/CPU/memory telemetry.
- [ ] Add KV-cache metrics where exposed by backend.
**Exit:** identify saturation and explain likely bottlenecks.

## Phase 3 — Multi-backend and recommendations
- [ ] Add second backend adapter.
- [ ] Compare same trace/configuration envelope.
- [ ] Add SLO-aware configuration sweep and recommendation output.
- [ ] Add confidence and out-of-scope warnings.
**Exit:** useful recommendations backed by controlled experiments.

## Phase 4 — Release
- [ ] Publish reproducibility bundles and sample reports.
- [ ] Document limitations and benchmark ethics.
- [ ] Invite runtime maintainers to review adapters.
