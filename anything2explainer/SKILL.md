---
name: anything2explainer
description: "主题生成解说视频Skill — Remotion驱动，TTS配音，中英文支持"
version: 1.0.0
author: Vincentwei1021 | Weekly Discovery
license: PolyForm Noncommercial
platforms: [linux, macos]
metadata:
  hermes:
    tags: [video, animation, remotion, tts, explainer]
---

# Anything2Explainer

将任何主题转化为黑色背景动画解说视频，支持TTS配音、字幕和章节进度条。中英双语，每帧由代码绘制。

## 来源
- GitHub: [Vincentwei1021/anything2explainer](https://github.com/Vincentwei1021/anything2explainer)
- Stars: ~711 (2026-09)
- License: PolyForm Noncommercial (商业需授权)

## 触发条件
- 需要将技术概念转化为可视化视频
- 需要生成教育类/科普类解说视频
- 需要批量生成主题介绍视频
- 中英文内容均可处理

## 核心特性

### 输入
- 主题（"explain vector databases"）
- 文章/文档内容
- 视频长度（2-8分钟）
- 语言（中文/英文）

### 输出
- 1280×720 H.264 MP4
- 同步TTS配音
- 字边界对齐字幕
- 章节进度条
- 完整工作记录（研究文档、脚本、分镜、QC报告）

## 工作流程（9个阶段）

```
1. Scaffold     → 搭建Remotion项目
2. Research     → 调研（带来源的研究文档）
3. Narration    → 撰写脚本 + TTS配音
4. Timeline     → 生成精确时间轴
5. Storyboard   → 分镜设计（每镜头一行）
6. Overlays     → 标题、章节卡、HUD、图标
7. Pilot        → 第一批镜头 + 30秒预览
8. Parallel Build → 并行构建剩余镜头
9. Render + QC  → 渲染 + 质量检查
```

## 四个检查点

Agent在以下节点暂停等待确认：
1. **长度和语言** - 脚本撰写前
2. **脚本确认** - TTS生成前
3. **配音引擎** - 选择edge-tts或kokoro
4. **前30秒预览** - 视觉风格确认后批量生产

## 安装

```bash
git clone https://github.com/Vincentwei1021/anything2explainer.git
ln -s "$PWD/anything2explainer" ~/.claude/skills/anything2explainer
ln -s "$PWD/anything2explainer" ~/.codex/skills/anything2explainer
```

### 依赖
```bash
brew install ffmpeg
python3 -m venv ~/.venvs/a2e && source ~/.venvs/a2e/bin/activate
pip install 'edge-tts==7.2.8' numpy pillow scipy
# 英文配音（可选，本地推理）
pip install kokoro soundfile
brew install espeak-ng
```

## 使用示例

### 基本用法
```
> Make me an explainer video about vector databases.
> 讲一下向量数据库，做成一条讲解视频
```

### 指定长度
```
> 做一个3分钟的RAG系统介绍视频
> 生成5分钟的机器学习概览
```

### 使用文档
```
> 把这篇论文变成视频: [文档链接]
> 将我的产品文档转化为营销视频
```

## 视频规格

| 长度 | 中文字数 | 英文词数 | 镜头数 | 构建Agent数 | 时间 |
|------|---------|---------|--------|------------|------|
| 2-3分钟 | 700-950 | 280-420 | 24-32 | 4-6 | ≈1h |
| 3-5分钟 | 1200-1500 | 420-700 | 40-50 | 8 | ≈2h |
| 5-8分钟 | 1800-2400 | 700-1150 | 60-80 | 10-14 | ≈2-3h |

## 视觉风格

- **背景**: 黑色画布 + 两种选择（星场/点阵波）
- **元素**: 白色线条艺术 + 紫色强调色
- **字体**: 超粗标题字体
- **UI**: 44px黑白描边字幕、底部章节进度条、顶部HUD

## 与Manim对比

| 特性 | Manim | Anything2Explainer |
|------|-------|-------------------|
| 语言 | Python | React/TypeScript |
| 驱动 | 手动编码 | Agent驱动全流程 |
| 配音 | 无 | 内置TTS |
| 字幕 | 需手动 | 自动对齐 |
| QC | 无 | 量化帧检查 |

## Pitfalls
1. **脚本一旦确认难修改** - 改一个字需重新计时整个视频
2. **首30秒预览最关键** - 在此阶段调整风格成本最低
3. **无GPU渲染** - CPU渲染，长视频需预留足够时间
4. **商业使用需授权** - 当前为非商用许可

## 参考
- [GitHub仓库](https://github.com/Vincentwei1021/anything2explainer)
- [示例视频](https://github.com/Vincentwei1021/anything2explainer#what-it-does)
- [SKILL.md流程](https://github.com/Vincentwei1021/anything2explainer/blob/main/SKILL.md)
