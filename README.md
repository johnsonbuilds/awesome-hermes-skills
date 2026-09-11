# Awesome Hermes Skills 🎯

A curated collection of reusable [Hermes Agent](https://hermes-agent.nousresearch.com) skills — share, install, and contribute.

Hermes Agent skills are procedural knowledge documents that teach the agent how to handle specific tasks. Each skill includes proven workflows, commands, pitfalls, and verification steps.

## Quick Start

Add this repository as a skill source:

```bash
hermes skills tap add https://github.com/johnsonbuilds/awesome-hermes-skills
```

Then search and install any skill:

```bash
hermes skills search wavespeed
hermes skills install wavespeed
```

Or install a specific skill directly by URL:

```bash
hermes skills install https://raw.githubusercontent.com/johnsonbuilds/awesome-hermes-skills/main/creative/wavespeed/SKILL.md
```

## Skills

<!--
Add new skills to this table as you add them to the repo.
Alphabetical order by category, then by name.
-->

| Category | Skill | Description |
|----------|-------|-------------|
| creative | [anything2explainer](./creative/anything2explainer/SKILL.md) | Turn any topic into animated explainer videos (Remotion + TTS full pipeline) |
| creative | [gods-eye-view](./creative/gods-eye-view/SKILL.md) | Browser-based spy satellite simulator using real open-source spatial intelligence data |
| creative | [handraw-style](./creative/handraw-style/SKILL.md) | 261 hand-drawn style prompt catalog with numbered system for AI image generation |
| creative | [wavespeed](./creative/wavespeed/SKILL.md) | Generate or edit AI media (image, video, audio, 3D) via WaveSpeed CLI |
| governance | [skill-health-check](./governance/skill-health-check/SKILL.md) | Analyze Hermes skills health, detect duplicates, and evaluate governance issues using skill-inspector |
| media | [ffmpeg](./media/ffmpeg/SKILL.md) | FFmpeg media processing — 40 tools for clipping, stitching, noise removal, subtitles, transcoding |
| mlops | [gpustack](./mlops/gpustack/SKILL.md) | GPU cluster manager for AI model serving — deploy vLLM/SGLang/TensorRT-LLM with auto-scaling |
| mlops | [llmfit](./mlops/llmfit/SKILL.md) | CLI tool to find which LLM models run on your local hardware (GPU/CPU/RAM detection) |
| mlops | [token-compression](./mlops/token-compression/SKILL.md) | Compress AI Agent tool outputs, logs, and RAG content — reduce token usage by 20-95% |
| productivity | [i-have-adhd](./productivity/i-have-adhd/SKILL.md) | ADHD-friendly output formatting — 10 rules to make AI responses actionable and scannable |

## Repository Structure

```
awesome-hermes-skills/
├── README.md
├── creative/
│   ├── anything2explainer/
│   │   └── SKILL.md
│   ├── gods-eye-view/
│   │   └── SKILL.md
│   ├── handraw-style/
│   │   └── SKILL.md
│   └── wavespeed/
│       └── SKILL.md
├── governance/
│   └── skill-health-check/
│       └── SKILL.md
├── media/
│   └── ffmpeg/
│       └── SKILL.md
├── mlops/
│   ├── gpustack/
│   │   └── SKILL.md
│   ├── llmfit/
│   │   └── SKILL.md
│   └── token-compression/
│       └── SKILL.md
└── productivity/
    └── i-have-adhd/
        └── SKILL.md
```

Skills are organized by category directory, matching the Hermes Agent skills layout convention. Each skill lives in its own subdirectory with a `SKILL.md` file.

## Contributing

### Adding a new skill

1. Create a directory under the appropriate category (or create a new category)
2. Write your `SKILL.md` following the [Hermes skill format](https://hermes-agent.nousresearch.com/docs)
3. Add an entry to the Skills table in this README
4. Open a Pull Request

### Skill format

Each `SKILL.md` requires YAML frontmatter:

```yaml
---
name: your-skill-name
description: One-line description of what this skill does
---
```

Then write the body in Markdown with clear sections:
- **Prerequisites** — what tools/configs are needed
- **Steps** — numbered instructions with exact commands
- **Pitfalls** — common mistakes and how to avoid them
- **Verification** — how to confirm it worked

## License

Apache License 2.0
