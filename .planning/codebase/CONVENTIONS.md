# Coding Conventions

**Analysis Date:** 2026-03-29

## Agent File Format

**Structure:**
Each agent is a Markdown file with YAML frontmatter followed by Markdown content.

**Frontmatter Template:**
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

**Required vs Optional Fields:**
- **Required:** `name`, `description`, `color`
- **Optional:** `emoji`, `vibe`, `services`
- Frontmatter must start with `---` on line 1 and end with `---` on a separate line.

## Naming Conventions

**Files:**
- Pattern: `{division}-{agent-slug}.md`
- Examples: `engineering-frontend-developer.md`, `design-whimsy-injector.md`
- Division directories: `engineering/`, `design/`, `game-development/`, `marketing/`, `paid-media/`, `sales/`, `product/`, `project-management/`, `testing/`, `support/`, `spatial-computing/`, `specialized/`, `academic/`

**Agent Names:**
- Title Case with spaces (e.g., "Frontend Developer", "Whimsy Injector")
- Must match the `name` field in frontmatter.

**Slugs:**
- Lowercase kebab-case derived from agent name (e.g., "frontend-developer", "whimsy-injector")
- Used for file naming and tool integrations (e.g., `integrations/opencode/agents/frontend-developer.md`)

## Color Naming Rules

**Named Colors (auto-resolved to hex by `convert.sh`):**
- Supported: `cyan`, `blue`, `green`, `red`, `purple`, `orange`, `teal`, `indigo`, `pink`, `gold`, `amber`, `neon-green`, `neon-cyan`, `metallic-blue`, `yellow`, `violet`, `rose`, `lime`, `gray`, `fuchsia`
- Mapping defined in `scripts/convert.sh` function `resolve_opencode_color()`
- Example: `color: cyan` → `#00FFFF`

**Hex Colors:**
- Must be in format `#RRGGBB` (uppercase or lowercase)
- Example: `color: "#3498DB"`
- Invalid hex defaults to `#6B7280` (gray)

## Content Style Guidelines

**Markdown Structure:**
1. **Identity & Memory** - Role, personality, background
2. **Core Mission** - Primary responsibilities with deliverables
3. **Critical Rules** - Domain-specific constraints
4. **Technical Deliverables** - Concrete examples and templates
5. **Workflow Process** - Step-by-step methodology
6. **Communication Style** - Tone and example phrases
7. **Success Metrics** - Measurable outcomes

**Writing Style:**
- **Specific**: "Reduce page load by 60%" not "Make it faster"
- **Concrete**: "Create React components with TypeScript" not "Build UIs"
- **Memorable**: Give agents personality, not generic corporate speak
- **Practical**: Include real code, not pseudo-code

**Formatting:**
- Use **emojis** for section headers (enhances readability)
- Use **bold** for emphasis, `code` for technical terms
- Include **code blocks** with language specification
- Use **tables** for comparing options or showing metrics
- Code examples should include language specification and comments

**Example Code Block:**
```typescript
// Always include:
// 1. Language specification for syntax highlighting
// 2. Comments explaining key concepts
// 3. Real, runnable code (not pseudo-code)
// 4. Modern best practices

interface AgentExample {
  name: string;
  specialty: string;
  deliverables: string[];
}
```

## Recommended Sections

The linter (`scripts/lint-agents.sh`) checks for these recommended sections (warnings only):
- "Identity"
- "Core Mission" 
- "Critical Rules"

While not required, including these improves agent quality and integration compatibility.

## Body Content Requirements

- Minimum 50 words (linter warns if shorter)
- Should have meaningful content describing the agent's capabilities
- No specific required sections beyond the recommended ones

## Tool-Specific Compatibility

**OpenCode Compatibility:**
- Agent bodies support `${variable}` templating for dynamic context (e.g., `${project_name}`)
- Color names are resolved to hex values

**Qwen Code Compatibility:**
- SubAgents use minimal frontmatter: only `name` and `description` are required
- `color`, `emoji`, and `version` fields are omitted

## File Location Conventions

**Agent Directories:**
- `engineering/` - Software development specialists
- `design/` - UX/UI and creative specialists
- `game-development/` - Game design and development specialists
- `marketing/` - Growth and marketing specialists
- `paid-media/` - Paid acquisition and media specialists
- `sales/` - Sales specialists
- `product/` - Product management specialists
- `project-management/` - PM and coordination specialists
- `testing/` - QA and testing specialists
- `support/` - Operations and support specialists
- `spatial-computing/` - AR/VR/XR specialists
- `specialized/` - Unique specialists that don't fit elsewhere
- `academic/` - Scholarly specialists

**Integration Outputs:**
- `integrations/` - Generated tool-specific files (never edit directly)
- Each tool has its own subdirectory: `opencode/`, `cursor/`, `antigravity/`, etc.

---

*Convention analysis: 2026-03-29*