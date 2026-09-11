---
name: gpustack
description: "Open-source GPU cluster manager for AI model serving — deploy vLLM/SGLang/TensorRT-LLM with auto-scaling and load balancing"
version: 1.0.0
author: GPUStack Team | Weekly Discovery
license: Apache-2.0
platforms: [linux]
metadata:
  hermes:
    tags: [gpu, inference, vllm, sglang, model-serving, kubernetes]
---

# GPUStack — GPU Cluster Manager for AI

Deploy and manage GPU clusters for high-performance AI model serving. Supports vLLM, SGLang, TensorRT-LLM with automatic engine selection.

## Source

- GitHub: https://github.com/gpustack/gpustack
- Stars: ~5,600
- License: Apache-2.0
- Language: Python/Go

## What It Does

GPUStack automates GPU cluster management for AI inference:
- **Multi-cluster support**: on-prem, Kubernetes, cloud providers
- **Pluggable engines**: vLLM, SGLang, TensorRT-LLM, or custom
- **Day-0 model support**: deploy new models immediately after release
- **Performance optimized**: pre-tuned modes for low latency or high throughput
- **KV cache extension**: LMCache, HiCache support for reduced TTFT
- **Speculative decoding**: EAGLE3, MTP, N-grams built-in

## Installation

```bash
# Docker (recommended)
sudo docker run -d --name gpustack \
    --restart unless-stopped \
    -p 80:80 \
    --volume gpustack-data:/var/lib/gpustack \
    gpustack/gpustack

# Alternative: Quay mirror
sudo docker run -d --name gpustack \
    --restart unless-stopped \
    -p 80:80 \
    --volume gpustack-data:/var/lib/gpustack \
    quay.io/gpustack/gpustack
```

## Prerequisites

- Linux worker nodes with NVIDIA GPU (AMD GPU, Ascend NPU also supported)
- Docker + NVIDIA Container Toolkit on workers
- CPU node for server (optional, can coexist with GPU node)

## Supported Accelerators

- NVIDIA GPU
- AMD GPU
- Ascend NPU
- Hygon DCU
- MThreads GPU
- Iluvatar GPU
- MetaX GPU
- Cambricon MLU
- T-Head PPU

## Usage

```bash
# Check status
sudo docker logs -f gpustack

# Access web UI
# http://<server-ip>

# Deploy a model via API
curl -X POST http://localhost/v1/models \
  -H "Content-Type: application/json" \
  -d '{"model_name": "llama-3.1-8b", "engine": "vllm"}'

# List running models
curl http://localhost/v1/models
```

## Architecture

```
┌─────────────────────────────────────┐
│         GPUStack Server             │
│  (CPU node, no GPU required)        │
│  - Scheduler, API, UI, Monitoring   │
└──────────────┬──────────────────────┘
               │
    ┌──────────┼──────────┐
    │          │          │
┌───▼───┐  ┌──▼───┐  ┌───▼───┐
│Worker1│  │Worker2│  │Worker3│
│ GPU   │  │ GPU   │  │ GPU   │
│ vLLM  │  │SGLang │  │ TRT   │
└───────┘  └───────┘  └───────┘
```

## Key Features

| Feature | Description |
|---------|-------------|
| Auto engine selection | Picks optimal inference engine per model |
| Load balancing | Distributes requests across workers |
| Failure recovery | Auto-restarts crashed inference processes |
| Monitoring | Real-time GPU utilization, throughput, latency |
| Multi-tenant | User authentication, access control, metering |
| Extended KV cache | Reduces Time-To-First-Token (TTFT) |

## Integration with Hermes Agent

```python
# Use GPUStack as backend for local LLM calls
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost/v1",
    api_key="dummy"  # GPUStack supports dummy API key
)

response = client.chat.completions.create(
    model="llama-3.1-8b",
    messages=[{"role": "user", "content": "Hello"}]
)
```

## Pitfalls

- **Linux only** — worker nodes must be Linux; Windows/macOS not supported for workers
- **Docker required** — all deployments use containers
- **GPU drivers** — NVIDIA driver + CUDA must be installed on workers
- **Network topology** — workers need low-latency connection to server
- **Resource planning** — ensure sufficient VRAM for target models

## Performance Benchmarks

GPUStack reports significant throughput improvements over default vLLM configurations:
- H200 GPU: up to 2.5x throughput improvement with auto-tuning
- TTFT reduction: 40-60% with KV cache extension

See: https://docs.gpustack.ai/latest/performance-lab/overview/

## Verification

```bash
# Check deployment
sudo docker ps | grep gpustack

# Access UI
curl -I http://localhost

# Check GPU utilization
# Via web UI: Resources → Workers → GPU metrics
```

## References

- GitHub: https://github.com/gpustack/gpustack
- Documentation: https://docs.gpustack.ai/
- Discord: https://discord.gg/VXYJzuaqwD
