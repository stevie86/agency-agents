# Architecture

**Analysis Date:** 2026-03-29

## Pattern Overview

**Overall:** Content Repository with Build Pipeline

**Key Characteristics:**
- Centralized agent definition store (147+ markdown files)
- Division-based taxonomy (12-13 divisions)
- YAML frontmatter + Markdown body pattern for each agent
- Multi-format conversion pipeline (convert.sh → integrations/)
- Installation system for distributing agents to AI tools (install.sh)
- Validation system via linter (lint-agents.sh)

## Layers

**Source Layer (Agent Definitions):**
- Purpose: Store human-readable agent specifications
- Location: Division directories (e.g., `engineering/`, `design/`, `marketing/`)
- Contains: Agent markdown files with YAML frontmatter and structured markdown
- Depends on: Nothing (source of truth)
- Used by: Build layer (convert.sh)

**Build Layer (Conversion Pipeline):**
- Purpose: Transform source agents into tool-specific formats
- Location: `scripts/convert.sh` (main), `scripts/install.sh` (distribution)
- Contains: Bash scripts with YAML parsing and template generation
- Depends on: Source Layer, Integration Layer structure
- Used by: CI/CD, manual regeneration

**Integration Layer (Generated Tool Formats):**
- Purpose: Store tool-specific agent representations
- Location: `integrations/` directory (one subdirectory per tool)
- Contains: Generated files (SKILL.md, .mdc, CONVENTIONS.md, etc.)
- Depends on: Source Layer via Build Layer
- Used by: External AI tools (Claude Code, Cursor, Copilot, etc.)

**Validation Layer (Quality Assurance):**
- Purpose: Ensure agent files meet quality standards
- Location: `scripts/lint-agents.sh`, `.github/workflows/`
- Contains: Bash validation script, CI workflow definitions
- Depends on: Source Layer
- Used by: CI/CD, contributors

## Data Flow

**Agent Creation Flow:**
1. Contributor creates/edits agent markdown file in appropriate division directory
2. File follows naming convention: `{division}-{agent-slug}.md`
3. YAML frontmatter validated by linter on commit/PR
4. `convert.sh` regenerates integration files for all tools
5. `install.sh` distributes generated files to local tool directories

**Conversion Flow:**
1. `convert.sh` scans division directories for `*.md` files
2. Parses YAML frontmatter to extract metadata (name, description, color, emoji)
3. Applies tool-specific transformations (e.g., .mdc for Cursor, SKILL.md for Antigravity)
4. Writes output to `integrations/{tool}/` directory
5. Optionally runs in parallel across tools

**Installation Flow:**
1. `install.sh` detects installed tools on system
2. Presents interactive UI for tool selection
3. Copies generated integration files to tool-specific locations
4. Handles both project-scoped and global installations

## Key Abstractions

**Agent Definition:**
- Purpose: Encapsulate a specialized AI persona with personality, workflows, and deliverables
- Examples: `engineering/engineering-frontend-developer.md`, `design/design-whimsy-injector.md`
- Pattern: YAML frontmatter (metadata) + Markdown body (structured content)

**Division Taxonomy:**
- Purpose: Organize agents by functional domain
- Examples: `engineering/`, `marketing/`, `game-development/`
- Pattern: Directory per division, agents prefixed with division name

**Tool Adapter:**
- Purpose: Transform agent definitions into tool-specific formats
- Examples: `integrations/cursor/`, `integrations/antigravity/`
- Pattern: One converter function per tool in `convert.sh`

**Color System:**
- Purpose: Provide visual identity for agents in tool UIs
- Examples: Named colors (cyan, blue) resolved to hex values
- Pattern: Frontmatter `color` field processed by converter

## Entry Points

**Agent Creation:**
- Location: Division directories (e.g., `engineering/engineering-new-agent.md`)
- Triggers: Manual file creation, PR review
- Responsibilities: Define new agent persona, follow template, pass linter

**Conversion Trigger:**
- Location: `scripts/convert.sh`
- Triggers: Manual execution, CI/CD pipeline, pre-commit hook
- Responsibilities: Regenerate all integration files from source agents

**Installation Trigger:**
- Location: `scripts/install.sh`
- Triggers: Manual execution after conversion
- Responsibilities: Distribute generated files to local AI tool directories

**Validation Trigger:**
- Location: `.github/workflows/lint-agents.yml`
- Triggers: Pull request to main branch
- Responsibilities: Validate all changed agent files meet quality standards

## Error Handling

**Strategy:** Validation-first with clear error messages

**Patterns:**
- YAML frontmatter validation in linter (missing required fields → error)
- File naming convention enforcement (incorrect pattern → warning)
- Body content length check (less than 50 words → warning)
- Tool-specific format validation during conversion

## Cross-Cutting Concerns

**Consistency:** All agents follow same structural template (7 recommended sections)
**Maintainability:** Single source of truth (division directories), generated outputs
**Extensibility:** Adding new tool support requires new adapter in convert.sh
**Quality:** Automated linting ensures minimum standards across all agents
**Documentation:** README.md, AGENTS.md, CONTRIBUTING.md provide comprehensive guidance

---

*Architecture analysis: 2026-03-29*