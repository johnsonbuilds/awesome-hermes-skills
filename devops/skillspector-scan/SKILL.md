---
name: skillspector-scan
description: "Scan newly created or modified Hermes skills with NVIDIA SkillSpector before committing — detect prompt injection, data exfiltration, and other security issues. Skips if SkillSpector is not installed."
version: 1.0.0
author: Johnson | GetClawCloud
license: MIT
platforms: [linux]
metadata:
  hermes:
    tags: [skillspector, security, skill-governance, scan, compliance]
---

# SkillSpector Scan

Run NVIDIA SkillSpector security scans on new or modified Hermes Agent skills before commit. This skill ensures all new skills pass the security gate that ClawHub AI uses.

## When to Use

**Run this skill after creating or modifying a SKILL.md file.** Execute the scan before committing the change to any repository (awesome-hermes-skills, personal skills, etc.).

Trigger conditions:
- You just created a new skill (SKILL.md file written)
- You modified an existing skill's content
- User explicitly asks to scan a skill
- Committing to awesome-hermes-skills repository

## Installation Check

SkillSpector must be installed in a Python environment. Check first:

```bash
which skillspector || pip show skillspector 2>/dev/null || echo "NOT_INSTALLED"
```

If not installed, install it (one-time setup):

```bash
uv tool install git+https://github.com/NVIDIA/skillspector.git
# Or from source:
git clone --depth=1 https://github.com/NVIDIA/SkillSpector.git /tmp/skillspector
cd /tmp/skillspector && uv venv .venv && source .venv/bin/activate && make install
```

## Scan Commands

### Scan a single skill directory

```bash
skillspector scan <skill-directory-path> --no-llm
```

Example:
```bash
skillspector scan ./productivity/worklittle-jobs-mcp --no-llm
```

### Scan with baseline (suppress known findings)

```bash
# Generate baseline (run once after first clean scan)
skillspector baseline <skill-directory-path> -o <skill-directory-path>/.skillspector-baseline.yaml --no-llm

# Scan using baseline (findings matching baseline are suppressed from score)
skillspector scan <skill-directory-path> --no-llm
# Note: baseline is auto-detected; no flag needed for .skillspector-baseline.yaml
```

### Scan with LLM analysis (requires API key)

```bash
# Option A: Claude CLI (uses existing session)
SKILLSPECTOR_PROVIDER=claude_cli skillspector scan <path>

# Option B: OpenAI-compatible endpoint
SKILLSPECTOR_PROVIDER=openai_compatible \
  SKILLSPECTOR_COMPAT_API_KEY=<key> \
  SKILLSPECTOR_COMPAT_BASE_URL=<url> \
  skillspector scan <path>
```

### Recursive scan of multiple skills

```bash
skillspector scan <directory-with-skill-subdirs> --recursive --no-llm
```

## Scan Result Interpretation

| Score | Severity | Action |
|-------|----------|--------|
| 0-20 | LOW | Pass — safe to ship |
| 21-50 | MEDIUM | Review findings, fix or add to baseline |
| 51-79 | HIGH | Must fix before commit |
| 80-100 | CRITICAL | Block commit, investigate immediately |

### Finding Codes Reference

| Code | Category | What to look for |
|------|----------|-----------------|
| SQP-1 | Vague Triggers | Ambiguous instruction triggers (low severity) |
| SQP-2 | Missing User Warnings | No privacy/security warnings in skill docs |
| SDI-* | Semantic Intent | LLM-analyzed intent mismatches (requires LLM) |
| SSD-* | Security Discovery | Prompt injection, data exfiltration (requires LLM) |
| PE1-5 | Privilege Escalation | Sudo/root commands, credential access |
| RP1 | Risky Package | Unpinned MCP servers, untagged Docker images |
| YR1 | YARA Rules | Malicious code patterns (false positives common) |
| AS3 | Agent Snooping | Reading other skills' data |

## Known False Positives (Skip These)

These are **not real security issues** and can be added to baseline:

1. **YR1 (YARA rules)** — triggers on any command string resembling malware patterns; common false positive for legitimate `pip install`, `docker run` commands in skill docs
2. **PE3 (Credential Access)** — flags if `.git/config` or similar paths appear in documentation examples; not executable code
3. **RP1 (Risky Package)** — flags unpinned MCP server versions; acceptable for documentation-only skills
4. **PE2 (Sudo/Root)** — triggers on Dockerfile instructions; expected for container-based skills
5. **reference_unresolved ledger exceptions** — not findings, just informational notes about relative paths in markdown

## Workflow

1. **Write the skill** — author the skill documentation file
2. **Run SkillSpector** — `skillspector scan <path> --no-llm`
3. **Review results**:
   - **Score 0-20**: Skip to step 5
   - **Score 21+**: Read each finding, determine if real or false positive
4. **Fix or baseline**:
   - Real issue → update the skill file to address the finding
   - False positive → run `skillspector baseline` and commit `.skillspector-baseline.yaml`
5. **Commit** — push only after passing or baselined

## Pitfalls

- **Baseline is per-skill** — `.skillspector-baseline.yaml` lives inside the skill directory, not global
- **`--no-llm` is recommended for CI** — faster, deterministic; LLM analysis needs API keys and can vary
- **`.skillspector-baseline.yaml` itself is scanned** — but excluded from scoring since it's in-scope
- **`--use-shipped-baseline`** — suppresses baseline suppressions (useful to verify what the baseline is hiding)
- **Semantic analyzers (SDI, SSD) skip without API key** — a LOW score with no LLM analyzers may miss semantic issues
- **`--recursive` does not auto-detect sub-skills** — must have proper SKILL.md per subdirectory
- **Git metadata (`.git/`) is scanned** — causes false positives; scan individual skill dirs, not entire repos
