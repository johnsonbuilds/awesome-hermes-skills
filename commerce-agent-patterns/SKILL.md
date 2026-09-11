---
name: commerce-agent-patterns
description: "Anthropic 官方电商 Agent 参考实现 - 购物助手和商户管理 Agent 的设计模式。"
version: 1.0.0
author: weekly-skill-discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [commerce, agent-patterns, anthropic, reference-implementation]
---

# Commerce Agent Patterns

Anthropic 官方发布的电商 Agent 参考蓝图。包含购物助手和商户管理两种 Agent，展示如何构建可嵌入应用的 AI Agent。

## 触发条件
- 需要构建购物助手或电商 Agent
- 学习 Anthropic 推荐的 Agent 架构模式
- 想要了解安全的支付/交易 Agent 设计
- 参考实现用于学习或扩展

## 项目概览

两个核心 Agent：
1. **购物 Agent**：搜索、比价、规划、填购物车、回答订单问题
5 个流程（skills）：搜索、比较、规划、购物车、订单管理
2. **商户 Agent**：性能分析、Listing 维护、库存和订单预警、定价促销、活动草稿
5 个流程（skills）：分析、目录、库存、定价、营销活动

每个 Agent 定义一次（prompt、skills、tool contracts、gates），可在三种运行时上运行。

## 快速开始

```bash
git clone https://github.com/anthropics/commerce-agents.git
cd commerce-agents

python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env  # 添加 ANTHROPIC_API_KEY

(cd examples && npm ci)
python scripts/run_demo.py retail  # API :8000 + storefront :3000
```

### 可用 Verticals
- `retail` - 零售 (:3000, portal :3100)
- `travel` - 旅游 (:3001, :3101)
- `telecom` - 电信 (:3002, :3102)
- `entertainment` - 娱乐 (:3003, :3103)

## 三种运行方式

### 1. Messages API（推荐用于学习）
```python
from pathlib import Path
from shopping_agent import ShoppingAgentConfig
from shopping_agent_runtime import ShoppingAgent

agent = ShoppingAgent(
    backend=your_backend,
    skills_dir=Path("shopping-agent/skills"),
    config=ShoppingAgentConfig(brand_name="Your Store")
)

async for event in agent.stream_turn(messages, session, state):
    # text_delta, tool_call, ui, cart_update
    ...

await agent.update_memory(messages, session)
```

### 2. Agent SDK（带控制台）
```bash
python shopping-agent/runtime-agent-sdk/main.py --once "a two-person tent under $250"
python merchant-agent/runtime-agent-sdk/main.py  # 审批待处理变更
```

### 3. Managed Agents（托管部署）
```bash
scripts/deploy_managed_agent.sh shopping-agent/managed-agents/shopping-agent
```

## 安全设计

关键安全机制：
- **Fencing**：限制 Agent 可执行的操作范围
- **Provenance gates**：验证数据来源
- **Caps**：设置操作上限
- **Memory validation**：验证存储的记忆
- **Approval gate**：商户写入操作需人工审批

所有写操作都是 staged 的，需要人工批准后才生效。

## 核心架构

```
commerce-common/        # 共享：配置、围栏、记忆、skills、grounding
├── shopping-agent/core/       # 购物 Agent 核心
│   ├── backend.py            # StorefrontBackend 接口
│   ├── prompt.py             # 提示词模板
│   └── tools/                # 工具定义
├── merchant-agent/core/      # 商户 Agent 核心
│   ├── backend.py            # MerchantBackend 接口
│   └── tools/                # 工具定义
└── examples/               # 4 个 vertical 示例
```

## 自定义指南

### 实现 Backend
```python
class MyBackend(StorefrontBackend):
    async def search_products(self, query: str, ...):
        # 调用你的产品搜索 API
        pass
    
    async def add_to_cart(self, product_id: str, ...):
        # 调用你的购物车 API
        pass
```

### 关闭不需要的功能
```python
config = ShoppingAgentConfig(
    brand_name="My Store",
    enable_cart=False,      # 无购物车
    enable_checkout=False,  # 无结账
)
```

### 添加自定义 Skill
在 `skills/` 目录下创建新目录，包含 `SKILL.md` 文件。

## Pitfalls

- **不是生产代码**：这是参考实现，需要根据业务需求调整
- **无认证**：示例中没有认证，生产环境需要添加
- **Sandboxed MCP**：MCP 服务器绑定到 localhost，不能直接暴露
- **依赖锁定**：CI 验证包名未注册在公共索引上，本地开发需从目录安装

## 参考资源

- GitHub: https://github.com/anthropics/commerce-agents
- 文档: https://github.com/anthropics/commerce-agents/blob/main/docs/
- Safety: https://github.com/anthropics/commerce-agents/blob/main/docs/safety.md
- Backends Guide: https://github.com/anthropics/commerce-agents/blob/main/docs/backends.md
