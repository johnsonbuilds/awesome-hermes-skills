---
name: ffmpeg-skill
description: "FFmpeg media processing skill — 40 tools for clipping, stitching, noise removal, subtitles, transcoding. No cloud API required."
version: 1.0.0
author: kajisho5 | Weekly Discovery
license: MIT
platforms: [linux, macos]
metadata:
  hermes:
    tags: [ffmpeg, video, media, processing, mcp]
---

# FFmpeg Skill

Provides local FFmpeg media processing capabilities for AI Agents. 40 structured tools covering common video/audio operations.

## Source

- GitHub: https://github.com/kajisho5/ffmpeg-skill
- Stars: ~935 (2026-09)
- License: MIT

## Trigger Conditions

- Need to process video/audio files (clip, stitch, transcode)
- Need batch processing of media files
- Need to generate subtitles or synchronize audio
- Need to convert video formats or resolutions
- Prefer local processing without cloud API uploads

## Core Concepts

### SPEC Specification
Tools use a SPEC (Specification) format where input contracts are auto-generated from argparse. This means:
- Type-safe argument validation
- Auto-generated documentation
- Consistent tool interface across all 40 tools

### Lossless Editing Priority
When possible, uses stream copy (`-c copy`) to avoid re-encoding:
- Clip without quality loss
- Stitch videos without re-encoding
- Extract audio tracks without conversion

### MCP Server Mode
Can run as an MCP server for Claude Code, Codex, or other MCP-compatible agents:
```bash
headroom mcp install
```

## Core Tools (40 Total)

### Video Operations
| Tool | Description |
|------|-------------|
| `ff_clip` | Clip video by time range |
| `ff_concat` | Concatenate multiple videos |
| `ff_split` | Split video into segments |
| `ff_rotate` | Rotate video by angle |
| `ff_resize` | Resize video dimensions |
| `ff_convert` | Convert video format |
| `ff_extract_audio` | Extract audio from video |
| `ff_add_audio` | Add audio track to video |
| `ff_speed` | Change playback speed |
| `ff_reverse` | Reverse video playback |

### Audio Operations
| Tool | Description |
|------|-------------|
| `af_normalize` | Normalize audio volume |
| `af_remove_silence` | Remove silent sections |
| `af_trim` | Trim audio by time range |
| `af_convert` | Convert audio format |
| `af_mix` | Mix multiple audio tracks |
| `af_vocal_remove` | Remove vocals from audio |

### Subtitle Operations
| Tool | Description |
|------|-------------|
| `ff_subtitles` | Add subtitles to video |
| `ff_extract_subs` | Extract subtitles from video |
| `ff_translate_subs` | Translate subtitles (via API) |

### Advanced Operations
| Tool | Description |
|------|-------------|
| `ff_watermark` | Add watermark/image overlay |
| `ff_gif` | Convert video to GIF |
| `ff_thumbnail` | Extract thumbnail frames |
| `ff_metadata` | View/edit video metadata |
| `ff_info` | Get detailed video information |
| `ff_merge` | Merge video and audio files |

## Usage Examples

### Clip a Video
```python
from ffmpeg_skill import ff_clip

result = ff_clip(
    input_file="video.mp4",
    start_time="00:01:30",
    end_time="00:02:45",
    output_file="clip.mp4"
)
```

### Extract Audio
```python
from ffmpeg_skill import ff_extract_audio

result = ff_extract_audio(
    input_file="video.mp4",
    output_file="audio.mp3",
    codec="libmp3lame"
)
```

### Remove Silence from Audio
```python
from ffmpeg_skill import af_remove_silence

result = af_remove_silence(
    input_file="podcast.wav",
    threshold=-50,  # dB
    output_file="cleaned.wav"
)
```

## Pitfalls

- **File paths** — use absolute paths to avoid working directory issues
- **Large files** — processing large videos may take significant time; use progress flags
- **Codec compatibility** — ensure output codec is supported by target player
- **Subtitle formats** — SRT, ASS, VTT supported; choose based on target use case
- **Stream copy limitations** — can't change resolution/bitrate with `-c copy`; must re-encode

## Verification

```bash
# Test basic clip operation
ffmpeg_skill ff_clip --input test.mp4 --start 00:00:00 --end 00:00:10 --output test_clip.mp4

# Verify output exists and is valid
ffprobe test_clip.mp4
```

## References

- GitHub: https://github.com/kajisho5/ffmpeg-skill
- FFmpeg Docs: https://ffmpeg.org/documentation.html
