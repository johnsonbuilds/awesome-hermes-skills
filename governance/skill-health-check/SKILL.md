---
name: skill-health-check
description: Analyze the current Hermes skills using skill-inspector. Use when users want to inspect, audit, review, evaluate, troubleshoot, or understand their installed skills, skill packages, health scores, duplicates, complexity, governance issues, recommendations, or overall skill library organization.
---

## Prerequisites

Before running any commands, ensure ``skill-inspector`` is installed:

```bash
git clone https://github.com/johnsonbuilds/skill-inspector.git
cd skill-inspector
pip install -e .
```

---

## Steps

When asked to analyze skills, follow these steps:

1. **Install skill-inspector** (if not already installed)
2. **Run the desired command** (`scan-packages` or `health`)
3. **Summarize the generated report** and send the original report to the user.
---

## Usage

```bash
skill-inspector <command> [options]
```

Available commands:

### scan-packages

Scan Hermes skills (package-aware) and generate ``report.md``.

```bash
skill-inspector scan-packages [--data-dir DATA_DIR] [--output OUTPUT] [--duplicate-threshold DUPLICATE_THRESHOLD]
```

**Options:**
- ``--data-dir DATA_DIR``: Directory containing ``config.yaml`` and ``skills/`` (default: ``/opt/data``)
- ``--output OUTPUT``: Report path (default: ``report.md``)
- ``--duplicate-threshold DUPLICATE_THRESHOLD``: Cosine similarity threshold for duplicate clusters (default: ``0.82``)

**Example:**

```bash
# Use default data directory
skill-inspector scan-packages

# Specify custom data directory and output
skill-inspector scan-packages --data-dir /path/to/data --output my-report.md --duplicate-threshold 0.85
```

> **Note:** `skill-inspector scan-packages` may take a considerable amount of time to complete depending on the number of skills. You can run it in the background and be notified upon completion:
>
> ```bash
> skill-inspector scan-packages &> scan-output.log &
> echo $! > scan.pid
> wait $(cat scan.pid) && notify-send "skill-inspector scan-packages completed" || notify-send "skill-inspector scan-packages failed"
> ```

---

### health

Generate health report only.

```bash
skill-inspector health [--data-dir DATA_DIR] [--output OUTPUT] [--duplicate-threshold DUPLICATE_THRESHOLD]
```

**Options:**
- ``--data-dir DATA_DIR``: Directory containing ``config.yaml`` and ``skills/`` (default: ``/opt/data``)
- ``--output OUTPUT``: Report path (default: ``health-report.md``)
- ``--duplicate-threshold DUPLICATE_THRESHOLD``: Cosine similarity threshold for duplicate clusters (default: ``0.82``)

**Example:**

```bash
# Use default settings
skill-inspector health

# Specify custom data directory and output
skill-inspector health --data-dir /path/to/data --output health.md
```

---

