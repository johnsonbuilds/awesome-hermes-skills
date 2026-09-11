---
name: ffmpeg-skill
description: "FFmpeg媒体处理Skill — 40个工具，剪辑/拼接/去静音/字幕/转码，无云API"
version: 1.0.0
author: kajisho5 | Weekly Discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [ffmpeg, video, media, processing, mcp]
---

# FFmpeg Skill

为AI Agent提供本地FFmpeg媒体处理能力。40个工具覆盖剪辑、拼接、去静音、字幕、转码等常见需求。

## 来源
- GitHub: [kajisho5/ffmpeg-skill](https://github.com/kajisho5/ffmpeg-skill)
- Stars: ~935 (2026-09)
- License: MIT

## 触发条件
- 需要处理视频/音频文件（剪辑、拼接、转码）
- 需要批量处理媒体文件
- 需要生成字幕或字幕同步
- 需要转换视频格式或分辨率
- 不需要上传到云服务的媒体处理

## 核心概念

### 工作流程
```
probe → 分析 → edit(可选无损) → check → verify
```

### 关键设计原则
1. **Real files first** - 每个任务从`probe`开始，基于实际测量参数决策
2. **Structured tools** - 每个操作是带类型参数的脚本，非shell字符串
3. **Machine-readable contract** - `contract --json`描述所有工具
4. **Verification after execution** - 执行后probe检查结果
5. **Local first** - 无云、无API key、无Python依赖

## 安装

### Claude Code / Cursor / Codex
```bash
npx ffmpeg-skill
```

### 检查环境
```bash
npx ffmpeg-skill doctor
```

### 查看工具契约
```bash
npx ffmpeg-skill contract --json
```

## 工具列表 (40个)

| 类别 | 工具 | 功能 |
|------|------|------|
| **Probe** | `probe` | 分析媒体文件（分辨率、帧率、时长、编码） |
| **Cut** | `cut`, `trim`, `split` | 裁剪视频片段 |
| **Join** | `join`, `concat` | 拼接多个视频/音频 |
| **Duration** | `fit-duration`, `pad-duration` | 适配或填充到指定时长 |
| **Aspect** | `fit-aspect`, `pad-aspect` | 调整宽高比 |
| **Silence** | `remove-silence`, `detect-silence` | 去静音/检测静音 |
| **Audio** | `audio-clean`, `normalize`, `ducking`, `sync` | 音频清洗、标准化、动态压缩、同步 |
| **Captions** | `caption`, `karaoke`, `subtitle` | 字幕添加、卡拉OK效果 |
| **Overlay** | `overlay`, `picture-in-picture`, `motion-graphics` | 画中画、动态图形 |
| **HDR/LUT** | `hdr-to-sdr`, `apply-lut` | HDR转SDR、应用LUT |
| **Drift** | `correct-drift` | 修正音视频同步漂移 |
| **Multicam** | `multicam` | 多机位合成 |
| **Delivery** | `check-delivery`, `batch-render` | 交付检查、批量渲染 |

## 使用示例

### 剪辑视频片段
```
"剪切 interview.mp4 的 0:45-3:10 和 5:00-6:30 两段"
```

### 制作Reels
```
"取 1080p 竖版 60秒的Reels版本"
```

### 去静音
```
"移除所有超过3秒的静音片段"
```

### 添加字幕
```
"给视频添加英文字幕，文件在 subtitles.srt"
```

### 批量处理
```
"处理 videos/ 目录下所有 .mp4，转为 1080p webm"
```

## 与Agent框架集成

### MCP Server
ffmpeg-skill同时作为MCP工具暴露，可在任意MCP客户端使用：
```json
{
  "name": "ffmpeg_probe",
  "description": "Analyze media file properties",
  "inputSchema": { ... }
}
```

### 与其他Video Production Skill协作
- **brain层**: `video-production-agent` / `AI-video-production-OS` - 决策剪辑点、审核交付
- **hands层**: `ffmpeg-skill` - 实际执行媒体处理
- **analysis层**: `media-analysis-skill` - 分析内容
- **transcription层**: `transcription-skill` - 语音转文字
- **subtitle层**: `subtitle-skill` - 字幕生成
- **qc层**: `qc-skill` - 质量检查

## 技术规格
- FFmpeg版本: 5.0+
- Python版本: 3.9+
- 输出格式: JSON结构化报告
- 无损编辑: 支持stream copy模式

## Pitfalls
1. **不要跳过probe** - 先分析再决定如何处理
2. **无损优先** - 只改container/codec时不用重编码
3. **Check before delete** - 删除文件前确认新文件生成成功
4. **Doctor检查** - 环境缺失会导致部分工具不可用

## 参考链接
- [GitHub仓库](https://github.com/kajisho5/ffmpeg-skill)
- [NPM Package](https://www.npmjs.com/package/ffmpeg-skill)
- [SPEC规范文档](https://github.com/kajisho5/ffmpeg-skill#what-is-spec)
