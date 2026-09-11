---
name: headroom-token-compression
description: "使用 Headroom 压缩 AI Agent 的工具输出、日志和 RAG 内容，减少 20-95% token 消耗。"
version: 1.0.0
author: weekly-skill-discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [token-compression, cost-reduction, mcp, agent-optimization]
---

# Headroom Token Compression

压缩发送给 LLM 的内容（工具输出、日志、RAG 块、文件），在保持答案质量的同时大幅减少 token 消耗。

## 触发条件
- Agent 处理大量工具输出时 token 成本高
- 需要压缩日志、代码搜索结果、JSON 数据
- 使用 MCP 服务器返回大量数据
- 想要降低 Claude Code、Codex、Cursor 等工具的 token 消耗

## 安装

```bash
# 使用 uv（推荐）
uv tool install --python 3.13 "headroom-ai[all]"

# 或使用 pip
pip install "headroom-ai[all]"
```

## 使用方式

### 1. 一键部署（最简方式）
```bash
headroom deploy
```
自动配置本地部署和 agent 配置。

### 2. Wrap 常用 Agent
```bash
# 包装 Claude Code
headroom wrap claude

# 包装 Codex
headroom wrap codex

# 包装 OpenCode
headroom wrap opencode
```

使用 `headroom unwrap <tool>` 移除包装。

### 3. 启动代理模式（零代码修改）
```bash
headroom proxy --port 8787
```
任何 OpenAI-compatible 客户端都可以通过设置 API base URL 使用此代理。

### 4. Python 库内联使用
```python
from headroom import compress
from openai import OpenAI

messages = [{"role": "user", "content": "分析这些结果"}]
result = compress(messages, model="gpt-4o")

client = OpenAI()
response = client.chat.completions.create(model="gpt-4o", messages=result.messages)
print(f"Saved {result.tokens_saved} tokens ({result.compression_ratio:.0%})")
```

### 5. MCP Server 模式
```bash
headroom mcp install
```
为任何 MCP 客户端提供 `headroom_compress`、`headroom_retrieve`、`headroom_stats` 工具。

## 验证安装
```bash
# 健康检查
headroom doctor

# 查看节省统计
headroom perf

# 实时仪表盘（需运行 proxy）
headroom dashboard
```

## 压缩效果

| 场景 | 压缩前 | 压缩后 | 节省 |
|------|--------|--------|------|
| 代码搜索 (100结果) | 17,199 tokens | 13,597 tokens | **21%** |
| SRE 事件调试 | 55,957 tokens | 24,340 tokens | **57%** |
| 代码库探索 | 58,801 tokens | 33,895 tokens | **42%** |
| GitHub Issue 分类 | 46,067 tokens | 32,429 tokens | **30%** |

JSON 重复数据可节省 90%+，文本质内容压缩较少。

## 输出压缩（减少模型回复 token）

```bash
# 启用输出压缩
export HEADROOM_OUTPUT_SHAPER=1
headroom proxy --port 8787

# 自动学习用户的简洁程度偏好
headroom learn --verbosity
headroom learn --verbosity --apply
```

## 支持的 Agent

| Agent | 支持 |
|-------|------|
| Claude Code | ✅ |
| Codex | ✅ |
| OpenCode | ✅ |
| Cursor | 手动配置 |
| Aider | ✅ |
| Copilot CLI | ✅ |
| Cline | ✅ |
| Continue | ✅ |
| Goose | ✅ |
| OpenHands | ✅ |

## Pitfalls

- **数据本地处理**：压缩在本地运行，不会发送内容到外部服务
- **原始内容缓存**：原始内容缓存在本地，模型可通过 `headroom_retrieve` 获取完整内容
- **Proxy 模式**：使用 proxy 模式时，需要配置客户端使用 `http://localhost:8787` 作为 API base
- **Output shaping 是可选的**：默认关闭，需手动启用

## 参考资源

- GitHub: https://github.com/headroomlabs-ai/headroom
- 文档: https://docs.headroomlabs.ai/docs
- 基准测试: https://docs.headroomlabs.ai/docs/benchmarks
