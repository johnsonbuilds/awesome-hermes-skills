---
name: headroom-token-compression
description: "Compress AI Agent tool outputs, logs, and RAG content using Headroom — reduce token usage by 20-95%"
version: 1.0.0
author: weekly-skill-discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [token-compression, cost-reduction, mcp, agent-optimization]
---

# Headroom Token Compression

Compress content sent to LLMs (tool outputs, logs, RAG chunks, files) while maintaining answer quality — reduces token consumption by 20-95%.

## Source

- GitHub: https://github.com/headroomlabs-ai/headroom
- Stars: ~71,000
- License: MIT

## Trigger Conditions

- Agent processing large tool outputs with high token costs
- Need to compress logs, code search results, JSON data
- Using MCP servers that return large amounts of data
- Want to reduce token consumption in Claude Code, Codex, Cursor, etc.

## Installation

```bash
# Using uv (recommended)
uv tool install --python 3.13 "headroom-ai[all]"

# Or using pip
pip install "headroom-ai[all]"
```

## Usage

### 1. One-Click Deploy (Simplest)
```bash
headroom deploy
```
Automatically configures local deployment and agent integration.

### 2. Wrap Common Agents
```bash
# Wrap Claude Code
headroom wrap claude

# Wrap Codex
headroom wrap codex

# Wrap OpenCode
headroom wrap opencode
```
Use `headroom unwrap <tool>` to remove wrapping.

### 3. Proxy Mode (Zero Code Changes)
```bash
headroom proxy --port 8787
```
Any OpenAI-compatible client can use this proxy by setting API base URL.

### 4. Python Library (Inline Use)
```python
from headroom import compress
from openai import OpenAI

messages = [{"role": "user", "content": "Analyze these results"}]
result = compress(messages, model="gpt-4o")

client = OpenAI()
response = client.chat.completions.create(model="gpt-4o", messages=result.messages)
print(f"Saved {result.tokens_saved} tokens ({result.compression_ratio:.0%})")
```

### 5. MCP Server Mode
```bash
headroom mcp install
```
Provides `headroom_compress`, `headroom_retrieve`, `headroom_stats` tools to any MCP client.

## Verify Installation

```bash
# Health check
headroom doctor

# View savings stats
headroom perf

# Real-time dashboard (requires running proxy)
headroom dashboard
```

## Compression Results

| Scenario | Before | After | Savings |
|----------|--------|-------|---------|
| Code search (100 results) | 17,199 tokens | 13,597 tokens | **21%** |
| SRE incident debugging | 55,957 tokens | 24,340 tokens | **57%** |
| Codebase exploration | 58,801 tokens | 33,895 tokens | **42%** |
| GitHub Issue classification | 46,067 tokens | 32,429 tokens | **30%** |

JSON duplicate data can save 90%+. Plain text content compresses less.

## Output Compression (Reduce Model Response Tokens)

```bash
# Enable output compression
export HEADROOM_OUTPUT_SHAPER=1
headroom proxy --port 8787

# Auto-learn your verbosity preferences
headroom learn --verbosity
headroom learn --verbosity --apply
```

## Supported Agents

| Agent | Support |
|-------|---------|
| Claude Code | ✅ |
| Codex | ✅ |
| OpenCode | ✅ |
| Cursor | Manual config |
| Aider | ✅ |
| Copilot CLI | ✅ |
| Cline | ✅ |
| Continue | ✅ |
| Goose | ✅ |
| OpenHands | ✅ |

## Pitfalls

- **Local processing** — compression runs locally, content never sent to external services
- **Original content cached** — raw content cached locally, models can retrieve full content via `headroom_retrieve`
- **Proxy mode** — when using proxy mode, configure client to use `http://localhost:8787` as API base
- **Output shaping is optional** — disabled by default, must be enabled manually

## References

- GitHub: https://github.com/headroomlabs-ai/headroom
- Docs: https://docs.headroomlabs.ai/docs
- Benchmarks: https://docs.headroomlabs.ai/docs/benchmarks
