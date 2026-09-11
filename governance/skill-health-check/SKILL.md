---
name: skill-health-check
description: Use when users ask to analyze skills, audit skills, check skill health, generate skill reports, inspect installed skills, review skill packages, find duplicate skills, identify unused skills, evaluate skill quality, or run Skill Inspector.
---

## Prerequisites

Before running any commands, ensure `skill-inspector` is installed.

To install or upgrade to the latest version:

```bash
pip install --upgrade git+https://github.com/johnsonbuilds/skill-inspector.git
```

To install/upgrade to a specific version or tag (e.g., `v1.0.0`):

```bash
pip install --upgrade git+https://github.com/johnsonbuilds/skill-inspector.git@v1.0.0
```

---

## Steps

When asked to analyze skills, follow these steps:

1. **Install or upgrade skill-inspector** (if not already installed or to ensure the latest version is used):
   ```bash
   pip install --upgrade git+https://github.com/johnsonbuilds/skill-inspector.git
   ```
2. **Run the desired command** (`scan-packages` or `health`)
3. **Summarize the generated report** and send the original report file to the user.
---

## Usage

```bash
skill-inspector <command> [options]
```

Available commands:

### scan-packages(default)

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

