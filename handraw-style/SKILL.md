---
name: handraw-style
description: "手绘风格提示词库 — 261种编号风格，支持GPT Image、Midjourney等"
version: 1.0.0
author: yang0 | Weekly Discovery
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [image-generation, prompt-engineering, art-style, drawing]
---

# Handraw Style Prompter

261种手绘风格编号画廊与双语提示词Skill。不会描述画风？记住一个编号就能出图。

## 来源
- GitHub: [yang0/handraw-style](https://github.com/yang0/handraw-style)
- Stars: ~973 (2026-09)

## 触发条件
- 需要生成特定画风的图片但不确定如何描述
- 希望保持多张图片视觉风格一致
- 想探索不同的手绘艺术风格
- 为公众号、小红书、短视频创建统一视觉资产

## 核心功能

### 风格编号系统
```
001-035  国际社论漫画 / 幽默手绘
036-054  国际绘本 / 叙事型手绘
055-082  现代平面 / 艺术化人物体系
083-123  日本作者 / 当代插画体系
... (共261种)
```

### 使用流程
```
1. 浏览编号画廊 → 记下喜欢的编号
2. 输入"编号 + 主题" → 获取中英双语提示词
3. 复制提示词到生图AI
```

### 示例
```
> 041号风格，主题：秋天的第一杯奶茶
> 210号风格，主题：小男孩在雪地里点鞭炮
> 193号风格，主题：大唐夜宴
```

## 风格激活策略

### 优先级
1. **作者名称 + 风格名称** → 直接激活
2. **正向核心风格特征** → 补充描述
3. **传对应编号单图** → 最后手段

### 是否传图判断
- `001`这类可由名称激活的风格 → 不传图
- 没有核心特征或不足以激活的编号 → 传图

## 适用场景

### 内容创作者
- 公众号封面统一风格
- 小红书笔记配图一致性
- 短视频封面批量生产

### 品牌设计
- Logo衍生视觉资产
- 产品包装风格探索
- 品牌色+手绘风格融合

### 个人创作
- 家庭照片艺术化处理
- 宠物照片生成传说卡牌
- 节日贺图批量制作

## 与生图模型配合

### GPT Image 2
- 已建立首轮能力清单
- 支持直接名称激活
- 智能判断是否需要参考图

### Midjourney / DALL-E
- 提示词结构适配
- 编号可转为风格描述词
- 支持多风格融合指令

## 安装

### Claude Code / Codex
```bash
git clone https://github.com/yang0/handraw-style.git
ln -s "$PWD/handraw-style" ~/.claude/skills/handraw-style
```

### 浏览器使用
访问[编号画廊](https://github.com/yang0/handraw-style/blob/master/handdraw-style-prompter/gallery/index.html)在线浏览

## 使用示例

### 直接询问
```
> 用041风格生成一杯奶茶图片
> 193风格，主题：长安十二时辰夜景
```

### 批量生成
```
> 用041-050风格各生成一张秋日主题图
> 生成10张不同风格的咖啡主题图
```

### 风格探索
```
> 推荐5种适合商业插画的编号
> 有哪些适合儿童绘本的风格？
```

## Pitfalls
1. **编号可能变化** - 持续更新中，建议查看最新画廊
2. **生图模型差异** - 不同模型对风格理解有差异
3. **标题翻译** - 提示词含中英文，按模型要求选用
4. **版权注意** - 部分风格参考现实艺术家，商用需确认

## 参考
- [GitHub仓库](https://github.com/yang0/handraw-style)
- [编号画廊](https://github.com/yang0/handraw-style/blob/master/handdraw-style-prompter/gallery/index.html)
- [风格说明文档](https://github.com/yang0/handraw-style#三个例子)
