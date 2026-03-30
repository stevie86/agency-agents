# AGENTS.md - The Agency: AI Agent Definitions

## Project Overview

This repository contains **147+ specialized AI agent definitions** across 12 divisions. Each agent is a markdown file with YAML frontmatter that defines a persona for use with AI coding tools (Claude Code, Cursor, Copilot, OpenCode, etc.).

## Build & Lint Commands

### Convert agents to tool-specific formats
```bash
# Convert all agents (serial)
./scripts/convert.sh

# Convert in parallel (faster)
./scripts/convert.sh --parallel

# Convert for specific tool only
./scripts/convert.sh --tool cursor
./scripts/convert.sh --tool opencode
./scripts/convert.sh --tool copilot
```

### Install agents to local tools
```bash
# Interactive installer (auto-detects tools)
./scripts/install.sh

# Install for specific tool
./scripts/install.sh --tool cursor
./scripts/install.sh --tool opencode

# Non-interactive install for all detected tools
./scripts/install.sh --no-interactive --parallel
```

### Lint agent files (CI validation)
```bash
# Lint all agents
./scripts/lint-agents.sh

# Lint specific files
./scripts/lint-agents.sh engineering/engineing-frontend-developer.md

# Run the same check as CI (on changed files)
git diff --name-only origin/main...HEAD -- '**/*.md' | xargs ./scripts/lint-agents.sh
```

### No traditional build/test commands
This repository contains markdown content only - no TypeScript, Python, or other compiled code. Validation is done via the linter script above.

## Agent File Structure

### Required YAML Frontmatter
```yaml
---
name: Agent Name                    # REQUIRED
description: One-line description   # REQUIRED  
color: cyan | "#RRGGBB"            # REQUIRED (color name or hex)
emoji: 🎯                          # Optional
vibe: Personality hook              # Optional
services:                           # Optional external services
  - name: Service Name
    url: https://service-url.com
    tier: free | freemium | paid
---
```

### Recommended Markdown Sections
1. **Identity & Memory** - Role, personality, background
2. **Core Mission** - Primary responsibilities with deliverables
3. **Critical Rules** - Domain-specific constraints
4. **Technical Deliverables** - Concrete examples and templates
5. **Workflow Process** - Step-by-step methodology
6. **Communication Style** - Tone and example phrases
7. **Success Metrics** - Measurable outcomes

### Frontmatter Validation (enforced by linter)
- Must start with `---` on line 1
- Must include: `name`, `description`, `color`
- Body should have meaningful content (≥50 words)
- Recommended sections should be present (warns only)

## Code Style Guidelines

### Markdown Content Style
- Use **emojis** for section headers (enhances readability)
- Use **bold** for emphasis, `code` for technical terms
- Include **code blocks** with language specification
- Be **specific and concrete** (not vague descriptions)
- Each agent must have a **distinct personality** and voice
- Include **measurable success metrics** with numbers

### Naming Conventions
- File names: `{division}-{agent-name}.md` (e.g., `engineering-frontend-developer.md`)
- Agent names: Title Case with spaces (e.g., "Frontend Developer")
- Slugs: lowercase kebab-case (e.g., "frontend-developer")

### Frontmatter Colors
Supports named colors that are auto-resolved to hex:
- `cyan`, `blue`, `green`, `red`, `purple`, `orange`, `teal`, `indigo`, `pink`, `gold`, `amber`, `neon-green`, `neon-cyan`, `metallic-blue`, `yellow`, `violet`, `rose`, `lime`, `gray`, `fuchsia`
- Or use hex directly: `"#3498DB"`

### Content Guidelines
- Avoid generic "helpful assistant" language
- Include concrete code examples (not pseudo-code)
- Define success metrics with specific numbers
- Provide step-by-step workflows
- Show real deliverables, not vague descriptions

## Directory Structure

```
agency-agents/
├── academic/           # Scholarly agents (anthropologist, historian)
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
├── testing/            # QA and testing specialists
├── CONTRIBUTING.md     # Detailed contribution guide
├── README.md           # Main documentation
└── AGENTS.md           # This file
```

## CI/CD Integration

### GitHub Actions Workflow
The repository runs `lint-agents.yml` on pull requests:
- Triggers on changes to any agent directory
- Validates YAML frontmatter structure
- Checks required fields exist
- Warns on missing recommended sections
- Fails PR if errors found

### Pre-commit Checklist
Before submitting a PR:
1. Run `./scripts/lint-agents.sh` locally
2. Ensure agent follows template structure
3. Include personality and voice
4. Add concrete code/template examples
5. Define success metrics
6. Test in real scenarios

## Common Tasks

### Adding a New Agent
1. Create file: `{division}/{division}-{agent-slug}.md`
2. Add required frontmatter (name, description, color)
3. Follow recommended section structure
4. Run linter: `./scripts/lint-agents.sh {new-file}`
5. Regenerate integrations: `./scripts/convert.sh`

### Improving Existing Agents
1. Edit the agent markdown file
2. Run linter to validate
3. Regenerate integrations if needed
4. Test with actual AI tools

### Multi-Tool Compatibility
Agent files work across tools with automatic format conversion:
- **Claude Code**: Direct `.md` files
- **Cursor**: Converted to `.mdc` rule files
- **OpenCode**: Converted with color resolution
- **Copilot**: Direct `.md` files
- **Aider/Windsurf**: Single combined files

## Key Rules for AI Agents Working in This Repo

1. **Never edit files in `integrations/`** - These are generated by `convert.sh`
2. **Always run linter before committing** - CI will fail on errors
3. **Follow the agent template structure** - See CONTRIBUTING.md for details
4. **Maintain distinct personalities** - Each agent must be unique
5. **Include concrete examples** - Vague descriptions are rejected
6. **Use proper file naming** - `{division}-{agent-slug}.md`
7. **Validate frontmatter** - Required fields must be present

## Tool Integration Quick Reference

```bash
# After adding/editing agents, always regenerate:
./scripts/convert.sh --parallel

# To install for your local tools:
./scripts/install.sh --tool <tool-name>

# To validate before push:
./scripts/lint-agents.sh
```

## Sisyphus Integration

For orchestrator (Sisyphus) integration with auto-routing:
- See `.opencode/agent/sisyphus-integration.md` for routing config
- See `agents-registry.md` for keyword-to-agent mapping
- See `workflow-sequences.md` for multi-agent pipeline patterns

### Context Gate (Adaptive Output)

Sisyphus asks user context ONCE per session:
- **Role**: Technical founder / Eng manager / Non-technical
- **Depth**: Explain / Recommend / Implement / Deep dive

Output adapts accordingly — no code for non-technical, full implementation for technical founders.

## Change Log

| Date | What Changed | Why |
|------|--------------|-----|
| 2026-03-29 | Added comprehensive IMPROVEMENTS.md | Document all enhancements vs upstream |
| 2026-03-29 | Added Context Gate to Sisyphus integration | Adaptive output based on user role and desired depth |
| 2026-03-29 | Added workflow-sequences.md | Multi-agent pipeline patterns (feature dev, security audit, etc.) |
| 2026-03-29 | Added agents-registry.md | Keyword-based routing for auto-delegation |
| 2026-03-29 | Improved convert.sh UX | Added --list, per-agent progress, warnings |
| 2026-03-29 | Improved install.sh messages | Clearer global vs project-scoped messaging |

**See [IMPROVEMENTS.md](IMPROVEMENTS.md) for complete before/after comparison.**
