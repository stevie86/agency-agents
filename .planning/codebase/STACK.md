# Technology Stack

**Analysis Date:** 2026-03-29

## Project Type & Purpose

**Type:** Static content repository (AI agent definitions)
**Purpose:** Curated collection of 147+ specialized AI agent personas in markdown format
**License:** MIT
**Repository:** `msitarzewski/agency-agents`

This is NOT a traditional software project with compiled code. It is a **markdown content library** with shell scripts for converting agents into tool-specific formats.

## Languages

**Primary:**
- **Markdown** - Agent definitions (147+ files across 12 division directories)
- **Bash** - Build/install/lint scripts (`scripts/convert.sh`, `scripts/install.sh`, `scripts/lint-agents.sh`)

**Secondary:**
- **YAML** - Frontmatter in agent files, GitHub Actions workflow
- **JSON** - `.opencode/package.json`, integration manifest files

## Runtime & Environment

**Shell Environment:**
- Bash 3.2+ (Linux, macOS, Windows Git Bash/WSL)
- No Node.js, Python, or other runtime required for core functionality

**Package Manager:**
- Not applicable for the content repo itself
- `.opencode/package.json` exists for OpenCode plugin integration: `@opencode-ai/plugin: "1.3.0"`

## Build System

**No traditional build/compile step.** The repository contains markdown only.

### Conversion Scripts (in `scripts/`)

1. **`scripts/convert.sh`** - Converts agent `.md` files to tool-specific formats
   - Supports: antigravity, gemini-cli, opencode, cursor, aider, windsurf, openclaw, qwen, kimi
   - Output: `integrations/<tool>/` directories
   - Features: `--parallel`, `--jobs N`, `--tool <name>`, `--list`, `--dry-run`

2. **`scripts/install.sh`** - Installs converted agents to local tool directories
   - Supports: claude-code, copilot, antigravity, gemini-cli, opencode, openclaw, cursor, aider, windsurf, qwen, kimi
   - Interactive selector with system detection
   - Features: `--tool <name>`, `--interactive/--no-interactive`, `--parallel`, `--jobs N`

3. **`scripts/lint-agents.sh`** - Validates agent markdown structure
   - Checks: YAML frontmatter (name, description, color required)
   - Warns: Recommended sections (Identity, Core Mission, Critical Rules)
   - Validates: Body content length (≥50 words)

## Configuration Files

**Repository-level:**
- `.gitignore` - Excludes generated integration files, OS files, editor files
- `.github/workflows/lint-agents.yml` - CI workflow for PR validation
- `.github/FUNDING.yml` - GitHub sponsors configuration
- `.github/PULL_REQUEST_TEMPLATE.md` - PR template

**Tool-specific (in `.opencode/`):**
- `.opencode/package.json` - OpenCode plugin dependency
- `.opencode/agent/` - OpenCode agent configuration directory

## Agent File Structure

**Format:** Markdown with YAML frontmatter

**Required frontmatter fields:**
```yaml
---
name: Agent Name                    # REQUIRED
description: One-line description   # REQUIRED  
color: cyan | "#RRGGBB"            # REQUIRED
emoji: 🎯                          # Optional
vibe: Personality hook              # Optional
services:                           # Optional external services
  - name: Service Name
    url: https://service-url.com
    tier: free | freemium | paid
---
```

**Naming convention:** `{division}-{agent-slug}.md` (e.g., `engineering-frontend-developer.md`)

**Directory structure:**
```
agency-agents/
├── academic/           # Scholarly agents
├── design/             # UI/UX and creative agents
├── engineering/        # Software development specialists
├── game-development/   # Game design agents (Unity, Unreal, Godot, etc.)
├── integrations/       # Generated tool-specific files (DON'T EDIT)
├── marketing/          # Growth and marketing specialists
├── paid-media/         # Paid acquisition specialists
├── product/            # Product management agents
├── project-management/ # PM and coordination agents
├── scripts/            # Build/install/lint scripts
├── sales/              # Sales specialists
├── spatial-computing/  # AR/VR/XR specialists
├── specialized/        # Unique specialists
├── support/            # Operations and support agents
└── testing/            # QA and testing specialists
```

## CI/CD

**Platform:** GitHub Actions

**Workflow:** `.github/workflows/lint-agents.yml`
- Triggers on: Pull requests affecting agent directories
- Action: Runs `scripts/lint-agents.sh` on changed files
- Validates: Frontmatter structure, required fields, content quality
- Fails PR: If errors found (missing required frontmatter fields)

## Key Dependencies

**None required for core functionality.** The scripts use standard bash utilities:
- `awk` - Frontmatter parsing
- `find` - File discovery
- `xargs` - Parallel execution
- `grep` - Pattern matching
- Standard Unix tools: `sed`, `tr`, `wc`, `basename`, `dirname`

**Optional integration:**
- `openclaw` CLI - For registering agents with OpenClaw
- `code` (VS Code) - Detection for Copilot integration
- `cursor` - Detection for Cursor integration
- `gemini` - Detection for Gemini CLI integration

## Platform Requirements

**Development:**
- Linux, macOS, or Windows with Git Bash/WSL
- Bash 3.2+
- Git (for cloning and contribution)

**Production/Usage:**
- Any AI coding tool (Claude Code, Cursor, Copilot, OpenCode, etc.)
- No server-side components

---

*Stack analysis: 2026-03-29*
