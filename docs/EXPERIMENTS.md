# Experiment protocol

## Minimum experiment manifest
- experiment ID and git commit;
- model identifier and immutable revision;
- tokenizer revision;
- serving backend and version/commit;
- hardware, driver, runtime, and container details;
- quantization and parallelism configuration;
- workload trace hash and arrival model;
- warm-up duration, measured duration, repetitions;
- metric definitions and SLO thresholds.

## Baseline experiment matrix
1. Single-request latency across prompt and output lengths.
2. Concurrency sweep with fixed workload distribution.
3. Open-loop arrival-rate sweep to identify saturation and queue growth.
4. Burst test to inspect recovery and tail latency.
5. Mixed short/long prompt workload.
6. KV-cache pressure sweep across context lengths and concurrency.
7. Backend comparison on identical hardware and trace, where feasible.

## Reporting
Report p50/p95/p99 TTFT, ITL, end-to-end latency, input/output token throughput, request throughput, errors, timeouts, queue delay, and available resource/KV metrics. Include sample counts and run-to-run variability.

## Acceptance checks
- Every request has a traceable lifecycle or explicit failure.
- Metric boundaries are unit-tested.
- Warm-up is excluded consistently.
- Load generator itself is not saturated.
- No result is presented without environment metadata.
- A recommendation is only valid within the measured configuration envelope.
