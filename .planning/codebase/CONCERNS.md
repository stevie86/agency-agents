# Codebase Concerns

**Analysis Date:** 2026-03-29

## Tech Debt

**Incomplete Agent Registry:**
- Issue: `agents-registry.md` only lists 19 agents out of 185+ actual agent files
- Files: `agents-registry.md`, `.opencode/agent/sisyphus-integration.md`
- Impact: Sisyphus routing cannot delegate to most agents; routing logic fails for 90% of available specialists
- Fix approach: Regenerate registry from actual agent files; implement auto-generation in `convert.sh` or a dedicated script

**Inconsistent Agent Structure:**
- Issue: Many agents missing recommended sections (Identity, Core Mission, Critical Rules)
- Files: Multiple across all divisions (e.g., `marketing-marketing-growth-hacker.md` uses "Role Definition" instead of "Identity")
- Impact: Reduces predictability for users and tools; violates documented template
- Fix approach: Enforce section names via linter (warn→error); provide migration script for existing agents

**Mixed Content in Strategy Directory:**
- Issue: `strategy/` directory contains runbooks, playbooks, and executive briefs that are not agent definitions
- Files: `strategy/runbooks/*.md`, `strategy/playbooks/*.md`, `strategy/EXECUTIVE-BRIEF.md`
- Impact: Confusing directory structure; lint script excludes `strategy/` from scanning
- Fix approach: Move non-agent content to `examples/` or `docs/`; rename to `guides/`; update lint script to include if they should be agents

**Sisyphus Integration Ambiguity:**
- Issue: `sisyphus-integration.md` references "Task tool" but no Task tool is defined in tools list
- Files: `.opencode/agent/sisyphus-integration.md`
- Impact: Delegation mechanism unclear; may not work in practice
- Fix approach: Clarify tool definitions; update integration file with actual tool names

## Known Bugs

**Registry Routing Gap:**
- Symptoms: When Sisyphus reads `agents-registry.md`, it only sees 19 agents; most tasks will not match any agent
- Files: `agents-registry.md`, `.opencode/agent/sisyphus-integration.md`
- Trigger: Any task involving agents not in registry (e.g., "design UI", "mobile app", "game development")
- Workaround: Manually edit registry or bypass routing

**Missing Section Warnings Ignored:**
- Symptoms: Linter warns about missing recommended sections but does not fail; many agents lack these sections
- Files: Multiple agent files across divisions
- Trigger: Running `./scripts/lint-agents.sh` shows warnings but passes
- Workaround: None needed for functionality, but reduces quality

## Security Considerations

**Bash Script Security:**
- Risk: `convert.sh` and `install.sh` use `eval`-like patterns with user input (tool names, paths)
- Files: `scripts/convert.sh`, `scripts/install.sh`
- Current mitigation: Input validation exists (valid_tools array); paths are relative to repo root
- Recommendations: Use more strict validation; consider shellcheck integration in CI

**Generated File Injection:**
- Risk: Agent frontmatter fields are interpolated into generated files without sanitization
- Files: `scripts/convert.sh` (multiple converter functions)
- Current mitigation: Fields are extracted via `get_field()` which uses awk; likely safe
- Recommendations: Add escaping for special characters; validate frontmatter values

## Performance Bottlenecks

**Slow Conversion for All Tools:**
- Problem: `./scripts/convert.sh --tool all` processes 185+ agents for 9 tools sequentially
- Files: `scripts/convert.sh`
- Cause: Sequential processing; parallel option exists but only for some tools
- Improvement path: Use parallel processing for all tools; cache unchanged agents

**Large Integration Directory:**
- Problem: `integrations/` directory contains thousands of generated files
- Files: `integrations/*/` (multiple subdirectories)
- Cause: Each agent generates multiple files across tools
- Improvement path: Consider gitignore for generated files; provide regeneration on demand

## Fragile Areas

**Agent File Naming Convention:**
- Files: All agent files in division directories
- Why fragile: Naming convention `{division}-{agent-name}.md` is not enforced; some agents may have inconsistent names
- Safe modification: Use lint script to validate naming; rename if needed
- Test coverage: None for naming convention

**Color Resolution in OpenCode Conversion:**
- Files: `scripts/convert.sh` function `resolve_opencode_color()`
- Why fragile: Hardcoded color mapping; new color names may not be mapped
- Safe modification: Add new color mappings as needed
- Test coverage: None for color resolution

## Scaling Limits

**Agent Count Growth:**
- Current capacity: 185+ agents across 12 divisions
- Limit: Registry cannot scale; workflow sequences limited to 5
- Scaling path: Auto-generate registry from agent files; implement dynamic routing based on agent frontmatter

**Integration File Volume:**
- Current capacity: ~10 tools, each with hundreds of files
- Limit: Disk space and git repository size
- Scaling path: Consider on-demand generation; exclude integrations from git (use .gitignore)

## Dependencies at Risk

**Bash 3.2+ Compatibility:**
- Risk: Scripts use bash arrays and process substitution; may fail on older systems
- Impact: Installation fails on systems with older bash
- Migration path: Use more portable bash constructs; test on macOS (bash 3.2)

**External Tool Detection:**
- Risk: `install.sh` relies on command detection (e.g., `command -v cursor`); may give false positives
- Impact: Attempts to install for tools that are not actually usable
- Migration path: Improve detection logic; add version checks

## Missing Critical Features

**Agent Quality Scoring:**
- Problem: No automated quality check beyond frontmatter validation
- Blocks: Ensuring agents meet quality standards (personality, examples, metrics)
- Solution: Implement quality linter that checks for recommended sections, code examples, success metrics

**Agent Versioning:**
- Problem: No version tracking for individual agents
- Blocks: Tracking changes, rolling back, compatibility
- Solution: Add version field to frontmatter; track in CHANGELOG per agent

**User Feedback Integration:**
- Problem: No mechanism for users to report agent quality issues
- Blocks: Continuous improvement based on real usage
- Solution: Add GitHub issue templates; integrate with discussions

## Test Coverage Gaps

**Agent Content Validation:**
- What's not tested: Quality of agent content (personality, examples, workflows)
- Files: All agent files
- Risk: Agents may be vague, generic, or missing key components
- Priority: High

**Integration Generation Validation:**
- What's not tested: Correctness of generated integration files
- Files: `integrations/*/`
- Risk: Generated files may be malformed or incomplete
- Priority: Medium

**Script Functionality Testing:**
- What's not tested: Bash scripts (`convert.sh`, `install.sh`, `lint-agents.sh`)
- Files: `scripts/*.sh`
- Risk: Scripts may fail in edge cases or on different platforms
- Priority: Medium

## Improvement Opportunities

**Auto-Generate Agent Registry:**
- Opportunity: Create script to generate `agents-registry.md` from agent frontmatter
- Benefit: Keep registry in sync with actual agents; enable complete routing
- Implementation: Add to `convert.sh` or create new script `generate-registry.sh`

**Enhance Sisyphus Integration:**
- Opportunity: Improve routing logic to use actual agent frontmatter (name, description, keywords)
- Benefit: More accurate delegation; support all agents
- Implementation: Update `sisyphus-integration.md` to read from agent files directly

**Add Agent Templates:**
- Opportunity: Create template files for new agents with proper sections
- Benefit: Ensure consistency; reduce missing sections
- Implementation: Add `templates/` directory with example agent files

**Implement Agent Quality Metrics:**
- Opportunity: Define and measure agent quality (word count, sections present, code examples)
- Benefit: Track improvement; identify weak agents
- Implementation: Extend lint script to compute quality score

**Compare with Marketingskills Best Practices:**
- Opportunity: Review similar projects (marketingskills) for patterns like:
  - Consistent section naming
  - Auto-generated indexes
  - Quality gates
  - Versioning
- Benefit: Adopt proven patterns; avoid reinventing
- Implementation: Research marketingskills repository; adapt patterns

---

*Concerns audit: 2026-03-29*