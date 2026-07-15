# ai-infra-lab

A working lab for the infrastructure side of AI: running open-weight LLMs locally,
measuring what they actually cost in memory and throughput, and building out the serving,
deployment, and observability layers around them the way production systems do it.

I'm a DevOps engineer with 7 years across Terraform, Kubernetes, and AWS, mostly in
regulated fintech. This repo is where I get current on AI-era infrastructure hands-on
instead of from videos. Every claim in it comes with a measurement or a manifest.

## What's here

| Piece | Status | What it shows |
|---|---|---|
| `benchmarks.md` | ✅ | Local model serving with Ollama. Quantization (q4 vs q8) measured on my own hardware: memory footprint and tokens/sec |
| `gateway/` | 🔜 | FastAPI service fronting the local model server, so caching, fallback, and cost tracking live in one place |
| `k8s/` | 🔜 | Ollama + gateway deployed on a local kind cluster: real manifests, probes, resource limits |
| `terraform/` | 🔜 | The cluster and supporting pieces provisioned as code |
| `observability/` | 🔜 | Prometheus + Grafana scraping the gateway: latency, throughput, and the inference signals that matter (TTFT, tokens/sec) |

Statuses update as pieces complete.

## Why this order

Serving first, because everything else hangs off a model you can actually run. Quantization
first among the measurements, because it's the single biggest cost lever in self-hosted
inference: 4-bit weights cut memory roughly 4x for a small quality cost (see the numbers).
Then the layers a production system wraps around that. A gateway so operational concerns
live in one place, Kubernetes because that's where real deployments run, and metrics
because an inference service without TTFT and tokens/sec dashboards is a liability.

## Running the benchmarks yourself

1. Install [Ollama](https://ollama.com)
2. `ollama pull llama3.2:3b` (and variants listed in `benchmarks.md`)
3. `ollama run llama3.2:3b --verbose "your prompt"` and read the `eval rate` line for generation speed
4. `ollama ps` in a second terminal for the memory footprint

Hardware, prompts, and method are documented in `benchmarks.md` so results are reproducible.