# Inference Lab

**A workload-replay and measurement toolkit for understanding and improving LLM serving performance.**

Inference Lab focuses on the gap between a model's theoretical capability and the service users experience: queueing, time-to-first-token (TTFT), inter-token latency (ITL), throughput, memory pressure, KV-cache behavior, and tail latency under realistic traffic. It is a measurement and decision-support layer—not a new inference engine.

## Problem
A serving configuration that performs well for one prompt length, output length, concurrency, or arrival pattern may perform poorly for another. Reporting only tokens/second hides queueing and user-perceived latency. Optimizing average latency can also worsen p95/p99 behavior.

## Research questions
- Which workload characteristics explain TTFT, ITL, throughput, and tail latency?
- How do prompt/output length, concurrency, burstiness, and request mix affect KV-cache pressure?
- When do batching and continuous batching help or hurt individual requests?
- Can measured workload profiles recommend safer serving configurations?
- How do results differ across vLLM, SGLang, and compatible serving backends?

## Proposed differentiator
A reproducible **workload replay + bottleneck attribution + configuration recommendation** workflow. It captures workload shape, replays it against a serving backend, measures service-level metrics, and explains trade-offs. Recommendations must be evidence-backed and never presented as universal settings.

## Planned features
- Workload trace schema with prompt/output lengths, arrival times, and optional tenant classes.
- Open-loop and closed-loop load generation.
- Metrics: TTFT, ITL, end-to-end latency, queue wait, request/token throughput, p50/p95/p99, error/timeout rates.
- GPU/CPU and memory telemetry, KV-cache utilization where exposed.
- Backend adapters for vLLM and SGLang first.
- Experiment manifests, repeated runs, baseline comparison, and regression detection.
- Configuration sweeps with guardrails and recommendation reports.

## Non-goals
- Reimplementing vLLM, SGLang, CUDA kernels, or a scheduler.
- Claiming one backend is universally faster.
- Tuning production systems without operator approval.
- Collecting private prompts or customer traces without authorization.

## Documentation
- [Research brief](docs/RESEARCH.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Roadmap](docs/ROADMAP.md)
- [Experiment protocol](docs/EXPERIMENTS.md)

## Status
<badge>Research / design phase</badge>

No performance numbers are claimed until reproducible runs are checked in. Hardware, model, backend version, and workload must accompany every result.
