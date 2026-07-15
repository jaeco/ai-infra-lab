# Local LLM Benchmarks

**Machine:** i7-8700K @ 3.70Ghz, 48GB of RAM, NVidia GeForce RTX 3060 12GB
**Date:** 2026-07-15
**Prompt used:** "Explain what a Kubernetes liveness probe does."
**Method:** `ollama run <model> --verbose`, 3 runs each, eval rate averaged.

| Model | Quantization | Memory (ollama ps) | Tokens/sec (eval rate) | Notes |
|---|---|---|---|---|
| llama3.2:3b | q4_K_M (default) | 2.6GB | 346.13/s | Answer was adequate but summarized lacking depth |
| llama3.2:3b-instruct-q8_0 | Q8_0 | 4.0GB | 1695.60/s | Answer offered more depth and useful examples of code snippets |