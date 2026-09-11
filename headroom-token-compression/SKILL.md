---
name: headroom-token-compression
description: "使用 Headroom 压缩 AI Agent 的工具输出、日志和 RAG 内容，减少 20-95% token 消耗。"
version: 1.1.0
author: weekly-skill-discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [token-compression, cost-reduction, mcp, agent-optimization]
---

# Headroom Token Compression

压缩发送给 LLM 的内容（工具输出、日志、RAG 块、文件），在保持答案质量的同时大幅减少 token 消耗。

> **⚠️ 隐私与数据安全提醒（使用前必读）**
>
> Headroom 会在本地缓存**原始未压缩内容**（供后续检索使用）。这意味着：
> - 你发送给 agent 的提示词、工具输出、日志等会被缓存在本机
> - 数据**不会**发送到外部服务（全部本地处理）
> - 但缓存的文件在本地磁盘上，确保你的设备是安全的
>
> **不适合处理的敏感数据：**
> - 包含 API key、密码、私人 token 的内容
> - 涉及个人隐私（PII）的数据
> - 公司内部机密代码或商业信息
> - 任何你不想留在本地磁盘上的内容
>
> 如果处理敏感数据，请确认：① 设备加密存储；② 定期清理 headroom 缓存目录。

---

## 触发条件

**仅在以下明确场景下使用此 skill：**

- 你**主动要求**压缩某段内容（如："帮我压缩这段日志"、"把这份代码搜索结果压缩一下"）
- 你正在处理明显的**大量冗余数据**（如 100+ 条搜索结果、超长日志、重复 JSON）
- 你已确认要压缩的内容**不包含敏感信息**（见上方隐私提醒）
- 你在 Claude Code、Codex、Cursor 等工具中遇到 token 成本过高问题

**不适用的场景：**
- 日常对话或一般性问答
- 处理含有敏感数据的日志或代码
- 没有明确压缩需求的普通任务

---

## 安装

```bash
# 使用 uv（推荐）
uv tool install --python 3.13 "headroom-ai[all]"

# 或使用 pip
pip install "headroom-ai[all]"
```

---

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

> **注意**：wrap 操作会修改 agent 的启动配置，确保你知道自己在做什么。

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

---

## 验证安装

```bash
# 健康检查
headroom doctor

# 查看节省统计
headroom perf

# 实时仪表盘（需运行 proxy）
headroom dashboard
```

---

## 压缩效果

| 场景 | 压缩前 | 压缩后 | 节省 |
|------|--------|--------|------|
| 代码搜索 (100结果) | 17,199 tokens | 13,597 tokens | **21%** |
| SRE 事件调试 | 55,957 tokens | 24,340 tokens | **57%** |
| 代码库探索 | 58,801 tokens | 33,895 tokens | **42%** |
| GitHub Issue 分类 | 46,067 tokens | 32,429 tokens | **30%** |

JSON 重复数据可节省 90%+，文本质内容压缩较少。

---

## 输出压缩（减少模型回复 token）

```bash
# 启用输出压缩
export HEADROOM_OUTPUT_SHAPER=1
headroom proxy --port 8787

# 自动学习用户的简洁程度偏好
headroom learn --verbosity
headroom learn --verbosity --apply
```

---

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

---

## Pitfalls

- **本地缓存**：原始内容缓存在本地磁盘，模型可通过 `headroom_retrieve` 获取完整内容。如需清理缓存，删除 `~/.headroom/cache` 目录。
- **Proxy 模式**：使用 proxy 模式时，需要配置客户端使用 `http://localhost:8787` 作为 API base。
- **Output shaping 是可选的**：默认关闭，需手动启用。
- **敏感数据风险**：如前所述，原始数据会缓存在本地。处理敏感内容前请确认设备安全。

---

## 参考资源

- GitHub: https://github.com/headroomlabs-ai/headroom
- 文档: https://docs.headroomlabs.ai/docs
- 基准测试: https://docs.headroomlabs.ai/docs/benchmarks
