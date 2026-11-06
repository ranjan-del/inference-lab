# Architecture — Inference Lab

## System overview
```text
Workload manifest / trace
   -> validator + privacy filter
   -> load generator (open-loop / closed-loop)
   -> backend adapter (vLLM / SGLang)
   -> request event collector
   -> host + accelerator telemetry
   -> metric normalization
   -> analysis / bottleneck attribution
   -> report + configuration comparison
```

## Components
- **Trace schema:** request ID, arrival timestamp or relative offset, prompt source/reference, expected output token budget, sampling settings, optional workload class.
- **Trace loader:** validates schema and redacts or synthesizes sensitive content.
- **Load generator:** supports controlled concurrency and arrival distributions; records client-side timestamps.
- **Backend adapters:** normalize request submission, streaming events, completion, cancellation, and error handling.
- **Telemetry collectors:** CPU/GPU utilization, memory, power, and backend-specific KV-cache metrics.
- **Metric engine:** computes latency distributions, throughput, goodput, and confidence intervals.
- **Analyzer:** compares runs and identifies likely bottleneck signatures.
- **Report generator:** writes machine-readable results and human-readable summaries.

## Important measurement boundaries
Record client submit, server accept (if available), first byte, first token, each token event, completion, and cancellation separately. Do not label first byte as TTFT unless the protocol guarantees it represents the first generated token.

## Workload schema principles
- Store prompts by reference or synthetic fixture by default.
- Support prompt-length/output-length distributions without retaining user text.
- Record arrival process and whether it is open-loop or closed-loop.
- Include cancellation and timeout behavior.
- Hash trace manifests and configuration.

## Bottleneck attribution (initial heuristics)
- Rising TTFT with stable ITL can indicate queueing or prefill pressure.
- Rising ITL under decode load may indicate contention or memory-bandwidth pressure.
- High KV-cache occupancy plus preemption/eviction can indicate memory capacity pressure.
- High GPU utilization does not alone prove GPU compute is the bottleneck.
These are hypotheses for investigation, not definitive diagnoses. Correlate with backend and hardware telemetry.

## Safety and production use
The first versions run in isolated test environments. No automatic production changes. Recommendations include tested scope, confidence, and rollback guidance. Avoid logging raw prompts, credentials, or tenant identifiers.
