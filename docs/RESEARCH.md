# Research brief — Inference Lab

## 1. Serving problem
LLM serving is a queueing and resource-allocation problem as well as a model execution problem. User experience depends on when generation begins (TTFT), how quickly tokens arrive (ITL), total completion time, and tail behavior under contention. Aggregate throughput alone is insufficient.

## 2. Concepts and metrics
- **TTFT:** time from request submission to first generated token; includes queueing and prefill effects depending on measurement boundary.
- **ITL:** interval between successive generated tokens; report distribution, not only mean.
- **End-to-end latency:** submission to completed response.
- **Queue wait:** time waiting before execution/admission, if backend exposes it.
- **Throughput:** requests/sec and input/output tokens/sec; define whether completed or generated tokens are counted.
- **Tail latency:** p50/p90/p95/p99 under a stated sample size.
- **KV-cache pressure:** cache occupancy, blocks/pages, evictions/preemption, or backend-specific equivalent.
- **Goodput:** requests meeting a defined latency/quality SLO, not raw throughput alone.

Measurement boundaries must be documented because tools and engines may define metrics differently.

## 3. Prior art and ecosystem
- **vLLM / PagedAttention:** memory-efficient KV-cache management and high-throughput serving.
- **SGLang:** structured generation and serving runtime with its own scheduling and caching design.
- **TensorRT-LLM:** optimized NVIDIA inference stack.
- **LMCache:** KV-cache reuse/offload ecosystem.
- **GenAI-Perf and serving benchmarks:** existing load generation and measurement tools that should be evaluated and integrated rather than duplicated.

The project must explicitly compare its contribution with existing benchmarking and profiling tools. A dashboard over existing metrics is not enough. The stronger contribution is trace replay, apples-to-apples experiment manifests, bottleneck attribution, and workload-conditioned configuration guidance.

## 4. Hypotheses
- H1: Workload shape (prompt/output distributions and arrival process) explains performance differences that a single fixed benchmark misses.
- H2: Open-loop tests reveal queueing and overload behavior that closed-loop tests can hide.
- H3: Tail latency and goodput are more useful for capacity decisions than peak throughput alone.
- H4: KV-cache pressure and prefill/decode mix explain some nonlinear degradation as concurrency rises.
- H5: Configuration recommendations can be learned from controlled sweeps, but must be bounded to the tested hardware/model/backend region.

## 5. Experimental variables
- Model architecture, weights, quantization, context length.
- GPU model/count, memory, driver, CUDA/runtime versions.
- Backend and exact commit/version.
- Tensor/pipeline parallelism and batching limits.
- Prompt and output token distributions.
- Arrival pattern, concurrency, burstiness, request cancellation.
- Cache state, warm-up, prefix reuse, and sampling settings.

## 6. Fair comparison rules
- Use the same model weights, tokenizer, quantization, hardware, and workload trace where possible.
- Separate cold-start from steady-state.
- Report warm-up and run duration.
- Repeat runs and report variability.
- Distinguish open-loop from closed-loop load.
- Never compare throughput measured with different output lengths without normalization and explanation.
- Publish failed runs, errors, and resource limits.

## 7. Candidate research contribution
A trace-driven evaluation format that lets teams:
1. describe realistic request arrivals and token-length distributions;
2. replay the trace against multiple backends;
3. collect standardized latency, throughput, and resource metrics;
4. identify whether the bottleneck is queueing, prefill, decode, memory, or backend errors;
5. evaluate configuration changes against explicit SLOs;
6. generate a reproducible report and bounded recommendation.

## 8. References / tools to verify
- vLLM: https://github.com/vllm-project/vllm
- PagedAttention paper: https://arxiv.org/abs/2309.06180
- SGLang: https://github.com/sgl-project/sglang
- TensorRT-LLM: https://github.com/NVIDIA/TensorRT-LLM
- LMCache: https://github.com/LMCache/LMCache
- GenAI-Perf: https://github.com/triton-inference-server/perf_analyzer (verify current project/tool naming and status before adopting)

Treat upstream docs and papers as authoritative for implementation-specific details; this brief is a research plan, not a claim that every integration is already implemented.
