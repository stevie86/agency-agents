# External Integrations

**Analysis Date:** 2026-03-29

## Supported AI Tools

The Agency integrates with **11 major AI coding tools** through a unified conversion and installation system.

### Tool Matrix

| Tool | Format | Installation | Config Location |
|------|--------|--------------|-----------------|
| **Claude Code** | Native `.md` | Direct copy | `~/.claude/agents/` |
| **GitHub Copilot** | Native `.md` | Direct copy | `~/.github/agents/` + `~/.copilot/agents/` |
| **Antigravity (Gemini)** | `SKILL.md` per agent | Converted | `~/.gemini/antigravity/skills/` |
| **Gemini CLI** | Extension + skills | Converted | `~/.gemini/extensions/agency-agents/` |
| **OpenCode** | `.md` with frontmatter | Converted | `.opencode/agents/` (project) or `~/.config/opencode/agents/` (global) |
| **Cursor** | `.mdc` rule files | Converted | `.cursor/rules/` (project-scoped) |
| **Aider** | Single `CONVENTIONS.md` | Converted | `./CONVENTIONS.md` (project) |
| **Windsurf** | Single `.windsurfrules` | Converted | `./.windsurfrules` (project) |
| **OpenClaw** | `SOUL.md` + `AGENTS.md` + `IDENTITY.md` | Converted | `~/.openclaw/agency-agents/` |
| **Qwen Code** | `.md` SubAgent files | Converted | `.qwen/agents/` (project) or `~/.qwen/agents/` (global) |
| **Kimi Code** | YAML + system prompt | Converted | `~/.config/kimi/agents/` |

## Conversion Workflow

### Step 1: Convert Agents to Tool Formats

**Script:** `scripts/convert.sh`

**Usage:**
```bash
# Convert all tools (slow, ~147 agents × 9 tools)
./scripts/convert.sh

# Convert specific tool (recommended)
./scripts/convert.sh --tool cursor
./scripts/convert.sh --tool opencode
./scripts/convert.sh --tool antigravity

# Parallel conversion (faster)
./scripts/convert.sh --parallel --jobs 8

# Preview without writing
./scripts/convert.sh --dry-run

# List available tools
./scripts/convert.sh --list
```

**Output:** `integrations/<tool>/` directories

**Conversion logic per tool:**

1. **Antigravity** - Creates `SKILL.md` files with metadata frontmatter
2. **Gemini CLI** - Creates skill directories + `gemini-extension.json` manifest
3. **OpenCode** - Creates `.md` files with resolved color hex values
4. **Cursor** - Creates `.mdc` rule files with `alwaysApply: false`
5. **Aider** - Accumulates all agents into single `CONVENTIONS.md`
6. **Windsurf** - Accumulates all agents into single `.windsurfrules`
7. **OpenClaw** - Splits into `SOUL.md` (persona) + `AGENTS.md` (operations) + `IDENTITY.md`
8. **Qwen Code** - Creates minimal frontmatter SubAgent files
9. **Kimi Code** - Creates `agent.yaml` + `system.md` pairs

### Step 2: Install to Local Tools

**Script:** `scripts/install.sh`

**Usage:**
```bash
# Interactive installer (auto-detects installed tools)
./scripts/install.sh

# Install specific tool
./scripts/install.sh --tool cursor
./scripts/install.sh --tool opencode

# Non-interactive (for CI/scripts)
./scripts/install.sh --no-interactive --tool all

# Parallel installation
./scripts/install.sh --no-interactive --parallel --jobs 4
```

**Installation targets:**

| Tool | Installation Path | Scope |
|------|-------------------|-------|
| Claude Code | `~/.claude/agents/` | User-wide |
| Copilot | `~/.github/agents/` + `~/.copilot/agents/` | User-wide |
| Antigravity | `~/.gemini/antigravity/skills/` | User-wide |
| Gemini CLI | `~/.gemini/extensions/agency-agents/` | User-wide |
| OpenCode | `~/.config/opencode/agents/` | User-wide |
| OpenClaw | `~/.openclaw/agency-agents/` | User-wide |
| Cursor | `.cursor/rules/` | Project-scoped |
| Aider | `./CONVENTIONS.md` | Project-scoped |
| Windsurf | `./.windsurfrules` | Project-scoped |
| Qwen Code | `.qwen/agents/` | Project-scoped |
| Kimi Code | `~/.config/kimi/agents/` | User-wide |

### Interactive Selector UI

When run in a terminal, `install.sh` displays an interactive checkbox UI:

```
  +------------------------------------------------+
  |   The Agency -- Tool Installer                 |
  +------------------------------------------------+

  System scan: [*] = detected on this machine

  [x]  1)  [*]  Claude Code     (claude.ai/code)
  [x]  2)  [*]  Copilot         (~/.github + ~/.copilot)
  [x]  3)  [*]  Antigravity     (~/.gemini/antigravity)
  [ ]  4)  [ ]  Gemini CLI      (gemini extension)
  [ ]  5)  [ ]  OpenCode        (opencode.ai)
  [ ]  6)  [ ]  OpenClaw        (~/.openclaw)
  [x]  7)  [*]  Cursor          (.cursor/rules)
  [ ]  8)  [ ]  Aider           (CONVENTIONS.md)
  [ ]  9)  [ ]  Windsurf        (.windsurfrules)
  [ ] 10)  [ ]  Qwen Code       (~/.qwen/agents)
  [ ] 11)  [ ]  Kimi Code       (~/.config/kimi/agents)

  [1-11] toggle   [a] all   [n] none   [d] detected
  [Enter] install   [q] quit
```

## Tool-Specific Details

### Claude Code

**Format:** Native `.md` files (no conversion needed)
**Installation:** Direct copy from agent directories
**Activation:** "Use the Frontend Developer agent to review this component."
**Config:** `~/.claude/agents/`

### GitHub Copilot

**Format:** Native `.md` files (no conversion needed)
**Installation:** Direct copy to two locations
**Activation:** "Use the Frontend Developer agent to review this component."
**Config:** `~/.github/agents/` + `~/.copilot/agents/`

### Antigravity (Gemini)

**Format:** `SKILL.md` files with metadata frontmatter
**Installation:** `~/.gemini/antigravity/skills/agency-<slug>/SKILL.md`
**Activation:** `@agency-frontend-developer review this React component`
**Frontmatter:** Includes `risk`, `source`, `date_added` fields

### Gemini CLI

**Format:** Extension with skill directories
**Installation:** `~/.gemini/extensions/agency-agents/`
**Files:** `gemini-extension.json` manifest + `skills/<slug>/SKILL.md`
**Activation:** Via Gemini CLI extension system

### OpenCode

**Format:** `.md` files with YAML frontmatter
**Installation:** `.opencode/agents/` (project) or `~/.config/opencode/agents/` (global)
**Color handling:** Named colors resolved to hex (e.g., `cyan` → `#00FFFF`)
**Activation:** `@backend-architect design this API.`

### Cursor

**Format:** `.mdc` rule files
**Installation:** `.cursor/rules/` (project-scoped)
**Frontmatter:** `description`, `globs`, `alwaysApply: false`
**Activation:** "Use the @security-engineer rules to review this code."

### Aider

**Format:** Single `CONVENTIONS.md` file
**Installation:** `./CONVENTIONS.md` (project root)
**Content:** All agents accumulated with separators
**Activation:** "Use the Frontend Developer agent to refactor this component."

### Windsurf

**Format:** Single `.windsurfrules` file
**Installation:** `./.windsurfrules` (project root)
**Content:** All agents accumulated with ASCII separators
**Activation:** "Use the Reality Checker agent to verify this is production ready."

### OpenClaw

**Format:** Workspace with 3 files per agent
**Installation:** `~/.openclaw/agency-agents/<slug>/`
**Files:**
- `SOUL.md` - Persona, tone, boundaries (identity, communication, critical rules sections)
- `AGENTS.md` - Mission, deliverables, workflow (everything else)
- `IDENTITY.md` - Emoji + name + vibe from frontmatter
**Registration:** `openclaw agents add <name> --workspace <path>`

### Qwen Code

**Format:** `.md` SubAgent files with minimal frontmatter
**Installation:** `.qwen/agents/` (project) or `~/.qwen/agents/` (global)
**Frontmatter:** Only `name` and `description` required (Qwen doesn't use color/emoji)
**Templating:** Supports `${variable}` substitution for dynamic context
**Activation:** "Use the frontend-developer agent to review this component."

### Kimi Code

**Format:** YAML agent spec + separate system prompt
**Installation:** `~/.config/kimi/agents/<slug>/`
**Files:**
- `agent.yaml` - Version, name, extend: default, system_prompt_path
- `system.md` - Full agent body as system prompt
**Usage:** `kimi --agent-file ~/.config/kimi/agents/frontend-developer/agent.yaml`

## CI/CD Integration

**GitHub Actions workflow:** `.github/workflows/lint-agents.yml`
- Triggers on: PRs affecting agent directories
- Validates: Frontmatter structure, required fields, content quality
- Fails PR: If validation errors found

**Pre-commit checklist:**
1. Run `./scripts/lint-agents.sh` locally
2. Ensure agent follows template structure
3. Include personality and voice
4. Add concrete code/template examples
5. Define success metrics
6. Regenerate integrations: `./scripts/convert.sh --parallel`

## Regeneration After Changes

When agents are added or modified:

```bash
# Regenerate all tools (serial, slower)
./scripts/convert.sh

# Regenerate all in parallel (faster)
./scripts/convert.sh --parallel

# Regenerate specific tool
./scripts/convert.sh --tool cursor
./scripts/convert.sh --tool opencode
```

Generated files are gitignored (see `.gitignore` lines 69-80). Only README files in `integrations/*/` are committed.

## Integration File Locations

**Committed files:**
- `integrations/README.md`
- `integrations/*/README.md` (tool-specific documentation)
- `scripts/convert.sh`, `scripts/install.sh`, `scripts/lint-agents.sh`

**Generated files (gitignored):**
- `integrations/antigravity/agency-*/SKILL.md`
- `integrations/gemini-cli/skills/`
- `integrations/gemini-cli/gemini-extension.json`
- `integrations/opencode/agents/`
- `integrations/cursor/rules/`
- `integrations/aider/CONVENTIONS.md`
- `integrations/windsurf/.windsurfrules`
- `integrations/openclaw/*`
- `integrations/qwen/agents/`
- `integrations/kimi/*/`

---

*Integration audit: 2026-03-29*
