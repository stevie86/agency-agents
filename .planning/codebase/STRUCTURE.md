# Codebase Structure

**Analysis Date:** 2026-03-29

## Directory Layout

```
agency-agents/
├── .opencode/                    # OpenCode integration configuration
├── .planning/                    # Planning documents (codebase analysis)
├── academic/                     # Academic division agents (5 files)
├── design/                       # Design division agents (8 files)
├── engineering/                  # Engineering division agents (26 files)
├── examples/                     # Multi-agent workflow examples
├── game-development/             # Game development division (20 files)
│   ├── blender/                  # Blender engine-specific agents
│   ├── godot/                    # Godot engine-specific agents
│   ├── roblox-studio/            # Roblox-specific agents
│   ├── unity/                    # Unity engine-specific agents
│   └── unreal-engine/            # Unreal Engine-specific agents
├── integrations/                 # Generated tool-specific agent formats
│   ├── aider/                    # Aider integration (single CONVENTIONS.md)
│   ├── antigravity/              # Antigravity skill files
│   ├── claude-code/              # Claude Code native .md files
│   ├── cursor/                   # Cursor .mdc rule files
│   ├── gemini-cli/               # Gemini CLI extension files
│   ├── kimi/                     # Kimi Code YAML agent specs
│   ├── mcp-memory/               # MCP memory integration example
│   ├── openclaw/                 # OpenClaw workspace files
│   ├── opencode/                 # OpenCode agent files
│   └── windsurf/                 # Windsurf .windsurfrules
├── marketing/                    # Marketing division agents (29 files)
├── paid-media/                   # Paid media division agents (7 files)
├── product/                      # Product division agents (5 files)
├── project-management/           # Project management division (6 files)
├── sales/                        # Sales division agents (8 files)
├── scripts/                      # Build and installation scripts
│   ├── convert.sh                # Converts agents to tool-specific formats
│   ├── install.sh                # Installs agents to local AI tools
│   └── lint-agents.sh            # Validates agent file structure
├── spatial-computing/            # Spatial computing division (6 files)
├── specialized/                  # Specialized division agents (28 files)
├── strategy/                     # Strategic documents (not agents)
│   ├── coordination/             # Multi-agent coordination docs
│   ├── playbooks/                # Workflow playbooks
│   └── runbooks/                 # Operational runbooks
├── support/                      # Support division agents (6 files)
├── testing/                      # Testing division agents (8 files)
├── agents-registry.md            # Keyword-to-agent mapping for routing
├── AGENTS.md                     # Main agent documentation and guidelines
├── CONTRIBUTING.md               # Contribution guidelines
├── CONTRIBUTING_zh-CN.md         # Chinese contribution guidelines
├── LICENSE                       # MIT License
├── README.md                     # Project documentation and roster
└── workflow-sequences.md         # Multi-agent pipeline patterns
```

## Directory Purposes

**Division Directories (academic/, design/, engineering/, etc.):**
- Purpose: Store agent definitions organized by functional domain
- Contains: Agent markdown files with YAML frontmatter
- Key files: `{division}-{agent-slug}.md` (e.g., `engineering-frontend-developer.md`)

**game-development/ (special case):**
- Purpose: Organize game development agents by engine/platform
- Contains: Engine-specific subdirectories (unity/, unreal-engine/, godot/, etc.)
- Key files: Cross-engine agents in root, engine-specific in subdirectories

**integrations/:**
- Purpose: Store generated tool-specific agent formats (DO NOT EDIT)
- Contains: Generated files created by `convert.sh`
- Key files: One subdirectory per supported tool (cursor/, opencode/, etc.)

**scripts/:**
- Purpose: Build automation for agent conversion and installation
- Contains: Bash scripts for conversion, installation, and validation
- Key files: `convert.sh` (primary), `install.sh` (distribution), `lint-agents.sh` (validation)

**examples/:**
- Purpose: Demonstrate multi-agent workflow patterns
- Contains: Example workflow documents showing agent collaboration
- Key files: `nexus-spatial-discovery.md`, `workflow-*.md`

**strategy/:**
- Purpose: Strategic planning documents (not agent definitions)
- Contains: Coordination docs, playbooks, runbooks
- Key files: `nexus-strategy.md`, `QUICKSTART.md`, `EXECUTIVE-BRIEF.md`

**.opencode/:**
- Purpose: OpenCode-specific integration configuration
- Contains: Agent configuration for OpenCode tool
- Key files: `agent/sisyphus-integration.md`

**.planning/:**
- Purpose: Codebase analysis documents for GSD workflow
- Contains: Architecture, structure, conventions, concerns analysis
- Key files: `codebase/ARCHITECTURE.md`, `codebase/STRUCTURE.md`

## Key File Locations

**Entry Points:**
- `README.md`: Project documentation and agent roster
- `AGENTS.md`: Detailed agent design guidelines and build commands
- `scripts/convert.sh`: Primary conversion entry point
- `scripts/install.sh`: Installation entry point

**Configuration:**
- `.opencode/agent/sisyphus-integration.md`: OpenCode agent routing config
- `agents-registry.md`: Keyword-to-agent mapping for auto-delegation
- `workflow-sequences.md`: Multi-agent pipeline patterns

**Core Logic:**
- Division directories (e.g., `engineering/`): Source agent definitions
- `scripts/convert.sh`: Conversion logic (700+ lines)
- `scripts/lint-agents.sh`: Validation logic

**Testing:**
- `scripts/lint-agents.sh`: Validates agent file structure
- `.github/workflows/lint-agents.yml`: CI validation workflow

## Naming Conventions

**Files:**
- Pattern: `{division}-{agent-slug}.md`
- Examples: `engineering-frontend-developer.md`, `design-whimsy-injector.md`
- Slugs: lowercase kebab-case (e.g., `frontend-developer`)

**Directories:**
- Pattern: Lowercase kebab-case for divisions
- Examples: `engineering/`, `game-development/`, `spatial-computing/`
- Engine subdirectories: lowercase (e.g., `unity/`, `unreal-engine/`)

**Integration Files:**
- Pattern: Tool-specific naming conventions
- Cursor: `.mdc` extension (e.g., `engineering-frontend-developer.mdc`)
- Antigravity: Directory per agent with `SKILL.md`
- Aider: Single `CONVENTIONS.md` combining all agents

## Where to Add New Code

**New Agent:**
- Primary code: Appropriate division directory (e.g., `engineering/`)
- File name: `{division}-{agent-slug}.md`
- Tests: Validation via `scripts/lint-agents.sh`
- Regenerate: Run `scripts/convert.sh` after creation

**New Division:**
- Directory: Create new lowercase kebab-case directory
- Update: `README.md` roster, `AGENTS.md` guidelines
- Regenerate: All integration files

**New Tool Integration:**
- Implementation: Add new adapter in `scripts/convert.sh`
- Output: New subdirectory in `integrations/`
- Documentation: Update `README.md` supported tools list

**Workflow Examples:**
- Location: `examples/` directory
- File naming: `workflow-{description}.md`
- Content: Multi-agent collaboration demonstration

## Special Directories

**integrations/ (Generated):**
- Purpose: Tool-specific agent formats created by conversion scripts
- Generated: Yes (by `scripts/convert.sh`)
- Committed: Yes (for distribution without requiring conversion)
- DO NOT EDIT: Manual changes will be overwritten

**game-development/ (Hierarchical):**
- Purpose: Organize game development agents by engine/platform
- Structure: Root for cross-engine agents, subdirectories per engine
- Generated: No
- Committed: Yes

**strategy/ (Non-Agent):**
- Purpose: Strategic planning and operational documents
- Contains: Not agent definitions, but coordination and planning materials
- Generated: No
- Committed: Yes

**.opencode/ (Tool-Specific):**
- Purpose: OpenCode integration configuration
- Contains: Agent routing and workflow integration
- Generated: Partially (some files may be generated)
- Committed: Yes

---

*Structure analysis: 2026-03-29*