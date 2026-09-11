---
name: agent-organization-patterns
description: "多 Agent 组织模式 - 参考 Headcount 的 16 部门 172 skills 架构设计。"
version: 1.0.0
author: weekly-skill-discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [multi-agent, organization, architecture, skill-design]
---

# Agent Organization Patterns

参考 Headcount 项目的多 Agent 组织架构模式。16 个部门、172 个 skills，每个部门独立可安装，按职责划分 Agent 组织。

## 触发条件
- 需要设计多 Agent 协作系统
- 想要学习如何组织大规模 Agent 技能
- 构建企业级 Agent 平台
- 设计 Agent 层级和职责划分

## 核心概念

### 部门模式
每个部门是一个独立的可安装插件，包含特定职能的 skills：

| 部门 | 职责 | Skills 数量 |
|------|------|-------------|
| Office of the CEO | 战略决策、投资关系 | 7 |
| Technology (CTO) | 架构、工程交付 | 19 |
| Security (CISO) | 安全架构、威胁建模 | 8 |
| IT Operations (CIO) | 基础设施、服务台 | 12 |
| Product (CPO) | 产品策略、UX | 11 |
| Marketing (CMO) | 品牌、内容营销 | 19 |
| Demand Generation | 获客、SEO、广告 | 12 |
| Revenue (CRO) | 销售、定价、留存 | 10 |
| Finance (CFO) | 财务建模、预算 | 13 |
| Operations (COO) | 运营流程 | 12 |

### Skill 寻址方式
```
department:skill-name
```
例如：
- `security:threat-modeling`
- `finance:unit-economics`
- `product:product-discovery`

这种命名方式确保名称不会冲突。

## 安装与使用

### Claude Code 插件安装
```bash
/plugin marketplace add cbrock84/headcount
/plugin install security@headcount
/plugin install finance@headcount
```

### 按需安装部门
只安装项目需要的部门，而非全部加载：
```bash
# 只安装安全部门
/plugin install security@headcount

# 只安装产品部门
/plugin install product@headcount
```

### 自动触发
当用户请求匹配部门领域时，对应 skill 自动加载：
- "这个着陆页转化率为什么低？" → `demand-generation:landing-page-cro-expert`
- "审查这个设计" → `security:threat-modeling`
- "我们能负担得起这个 hire 吗？" → `finance:unit-economics`

### 直接调用
```
/finance:financial-modeling
/security:threat-modeling
```

## 跨部门协作

### SOC 2 需求场景
1. Security 部门进行威胁建模
2. Technology 部门设计架构
3. Finance 部门评估成本
4. Executive 部门做最终决策

### 跨部门检查清单
- 明确每个部门的决策权限
- 定义部门间的接口和依赖
- 设置审批 gates
- 记录跨部门流程

## 设计模式

### 1. 职责单一原则
每个 skill 只做一件事，做得很好：
```
好的例子：
- security:threat-modeling（只做威胁建模）
- security:incident-response（只做事件响应）

不好的例子：
- security:everything（什么都做）
```

### 2. 独立可安装
每个部门是独立插件，可单独安装卸载：
```
优点：
- 减少内存占用
- 按需加载
- 便于维护
```

### 3. 命名空间隔离
使用 `department:skill` 避免冲突：
```
finance:pricing ≠ product:pricing
```

### 4. Reviewer 类部门
安全、审计类部门是 reviewer 角色，只审查不执行：
```python
# Security 部门只做审查
def threat_modeling():
    # 识别风险
    # 提出建议
    # 但不直接修改代码
```

## 架构扩展

### 添加新部门
```
new-department/
├── SKILL.md              # 部门描述
├── skills/
│   ├── skill1.md
│   └── skill2.md
└── agents/
    └── department-chief.md  # 部门负责人
```

### 添加新 Skill
```markdown
---
name: skill-name
description: 一句话描述
trigger: 触发条件
steps:
  - 步骤1
  - 步骤2
---
```

## Pitfalls

- **不要过度设计**：从小开始，按需添加部门
- **避免职责重叠**：确保每个部门有清晰边界
- **性能考虑**：太多部门会增加加载时间
- **维护成本**：每个 skill 都需要维护

## 参考资源

- GitHub: https://github.com/cbrock84/headcount
- 交互组织图: https://cbrock84.github.io/headcount/org-chart.html
- 使用案例: https://github.com/cbrock84/headcount/blob/main/docs/USE-CASES.md
- 快速入门: https://github.com/cbrock84/headcount/blob/main/docs/GETTING-STARTED.md
