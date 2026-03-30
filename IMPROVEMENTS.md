# Improvements & Enhancements

This document tracks all improvements made to this fork of the agency-agents repository compared to the upstream original.

## Overview

**Repository**: `stevie86/agency-agents` (fork of `msitarzewski/agency-agents`)
**Status**: 45 commits ahead of upstream
**Focus**: Developer Experience, Orchestration Integration, GSD Workflow

---

## 1. Script Improvements

### 1.1 convert.sh - Enhanced UX

**Before:**
- No visibility into available tools
- Silent execution
- No warning about slow operations
- No progress indication

**After:**
- ✅ `--list` option: Shows all 9 tools + 162 agent count
- ✅ `--dry-run` option: Preview without writing files
- ✅ Per-agent progress: `Processing 1/162 (1%) AgentName`
- ✅ Warning when using `all` tools: "This is slow (162 agents). Tip: use --tool <name>"
- ✅ Clear install messages: "162 agents -> /home/user/.config/opencode/agents (global)"

**Files Changed:** `scripts/convert.sh`

### 1.2 install.sh - Clearer Messaging

**Before:**
- "OpenCode: project-scoped. Run from your project root to install there." (confusing)

**After:**
- ✅ Clear scope indicators: "global" vs "this project only"
- ✅ Instructions for project-specific setup: `mkdir -p .opencode/agents && cp ...`
- ✅ Tool-specific guidance (OpenCode vs Cursor vs etc.)

**Files Changed:** `scripts/install.sh`

---

## 2. Sisyphus Integration (NEW)

### 2.1 Context Gate - Adaptive Output

**Problem:** One-size-fits-all responses don't match user needs

**Solution:** Context Gate that asks once per session:
- **Role**: Technical founder / Eng manager / Non-technical / Other
- **Depth**: Explain / Recommend / Implement / Deep dive

**Calibration:**
| User Type | Depth | Output Style |
|-----------|-------|--------------|
| Non-technical | Explain | Concepts only, no code, analogies |
| Eng manager | Recommend | Tradeoffs, pros/cons, decision matrix |
| Technical founder | Implement | Code with minimal explanation |
| Deep dive | Full | Research + code + tests + documentation |

**File:** `.opencode/agent/sisyphus-integration.md`

### 2.2 Agent Registry - Keyword-Based Routing

**Problem:** Users can't memorize 162+ agents

**Solution:** `agents-registry.md` - quick reference for routing

**Features:**
- 19 key agents with descriptions
- "When to use" keywords for auto-detection
- Domain grouping (Frontend, Backend, DevOps, etc.)

**File:** `agents-registry.md`

### 2.3 Workflow Sequences - Multi-Agent Pipelines

**Problem:** Complex tasks need multiple specialists in sequence

**Solution:** Predefined workflows in `workflow-sequences.md`

**Examples:**
- **Feature Development**: backend-architect → security-engineer → frontend-developer → qa-engineer
- **Performance Issue**: backend-architect → database-optimizer → frontend-developer
- **Security Audit**: security-engineer → code-reviewer → devops-automator
- **New Mobile App**: ui-designer → backend-architect → frontend-developer → qa-engineer

**File:** `workflow-sequences.md`

### 2.4 Auto-Routing Rules

Sisyphus now automatically detects intent and delegates:

| User Says | Routes To |
|-----------|-----------|
| "frontend", "React", "UI" | frontend-developer |
| "backend", "API", "database" | backend-architect |
| "deploy", "CI/CD", "Docker" | devops-automator |
| "security", "auth", "vulnerability" | security-engineer |
| "add feature", "implement" | Full sequence (4 agents) |
| "app is slow", "performance" | Performance sequence (3 agents) |

**File:** `.opencode/agent/sisyphus-integration.md`

---

## 3. GSD Workflow Integration (NEW)

### 3.1 Codebase Mapping

**Created:** `.planning/codebase/` with 7 documents (1,282 lines)

| Document | Purpose | Lines |
|----------|---------|-------|
| STACK.md | Technology stack analysis | 146 |
| INTEGRATIONS.md | Tool integration details | 267 |
| ARCHITECTURE.md | Repository architecture | 132 |
| STRUCTURE.md | Directory/file structure | 190 |
| CONVENTIONS.md | Content conventions | 143 |
| TESTING.md | Validation patterns | 224 |
| CONCERNS.md | Issues & opportunities | 180 |

**Process:** 4 parallel gsd-codebase-mapper agents

### 3.2 Configuration

**Created:** `.planning/config.json`

```json
{
  "model_profile": "smart",
  "workflow": {
    "research": true,
    "plan_check": true,
    "verifier": true,
    "nyquist_validation": true
  },
  "model_overrides": {
    "researcher": "ollama-cloud/minimax-m2.7",
    "synthesizer": "ollama-cloud/minimax-m2.7",
    "roadmapper": "ollama-cloud/minimax-m2.7"
  }
}
```

**Features:**
- Smart model profile (research/planning + execution separation)
- Full workflow agents enabled (research, plan_check, verifier)
- Nyquist validation for test coverage
- Model overrides to specific providers

---

## 4. Documentation Improvements

### 4.1 AGENTS.md - Enhanced

**Added:**
- ✅ Change Log section with dated entries
- ✅ Sisyphus Integration section
- ✅ Context Gate documentation
- ✅ Clearer installation instructions

### 4.2 Change Management Rules

**New Rule:** When making changes to agent routing/workflows:
1. Update the relevant file
2. Note changes in `AGENTS.md` under "Change Log"
3. Include: date, what changed, why
4. Keep AGENTS.md as single source of truth

---

## 5. Development Workflow

### 5.1 Branch Structure

- **main**: Synced with upstream + our changes
- **dev**: Active development branch
- Clean separation from upstream for easy PRs

### 5.2 Commits Made

| Commit | Description |
|--------|-------------|
| `104ce61` | feat: add Context Gate and Change Log |
| `8079eb4` | feat(convert.sh): add per-agent progress indicator |
| `2443369` | feat(convert.sh): improve usability with --list and warning |
| `ad428c6` | docs: add codebase mapping (7 documents, 1282 lines) |
| `03c83b3` | chore: update GSD config (smart profile, full workflow) |
| `854ab54` | chore: update GSD config with model overrides |

---

## 6. Known Issues & Gaps

From `CONCERNS.md`:

### High Priority
- **Incomplete Agent Registry**: Only 19 of 185+ agents documented
- **No Foundation Skill**: Unlike marketingskills' `product-marketing-context`, we lack a shared context skill

### Medium Priority
- **No Trigger Phrases**: Agent descriptions lack "when to use" keywords
- **No CLI Tools**: marketingskills has 51 executable CLI tools; we have instructions only

### Low Priority
- **No Validation Improvements**: Basic lint vs marketingskills' comprehensive validation
- **No SkillKit Integration**: Manual scripts vs automated multi-tool installation

---

## 7. Comparison with Best Practices

### What marketingskills Does Better

| Feature | marketingskills | agency-agents (this fork) |
|---------|-----------------|---------------------------|
| **Shared Context** | `product-marketing-context` read by all skills | ❌ Missing |
| **Installation** | `npx skills` + SkillKit for multi-tool | Manual scripts |
| **Validation** | Shell script + GitHub Actions + official spec | Basic lint |
| **Trigger Phrases** | Detailed "use when user says..." | Brief descriptions |
| **CLIs** | 51 Node.js executable tools | ❌ None |
| **References** | `references/` dir for detailed docs | Single file |
| **Cross-referencing** | Skills reference each other | ❌ No linking |

### What This Fork Adds

| Feature | This Fork | Upstream |
|---------|-----------|----------|
| **Orchestration** | Sisyphus integration with routing | ❌ None |
| **Context Gate** | Adaptive output by user type | ❌ None |
| **Workflow Sequences** | Multi-agent pipelines | ❌ None |
| **Progress Indicators** | Per-agent progress | ❌ Silent |
| **GSD Workflow** | Full planning/execution framework | ❌ None |
| **Codebase Mapping** | 7-document analysis | ❌ None |

---

## 8. Recommendations

### For Users

**If you're a technical founder who codes:**
- Install with `./scripts/install.sh --tool opencode`
- Use Sisyphus for auto-routing
- Reference `agents-registry.md` for available specialists

**If you're non-technical:**
- Use the Context Gate to get explanations, not code
- Focus on the 5-10 agents you'll actually use

### For Contributors

**High Impact:**
1. Expand `agents-registry.md` to cover all 185+ agents
2. Create `project-context` foundation skill (like marketingskills)
3. Add trigger phrases to agent descriptions

**Medium Impact:**
4. Create executable CLI tools (not just instructions)
5. Add SkillKit integration for easier multi-tool install

**Low Impact:**
6. Add `references/` directories for detailed docs
7. Improve validation to match marketingskills spec

---

## 9. Summary

This fork transforms agency-agents from a **static collection** into an **orchestrated system**:

```
BEFORE: Browse 162 agents → Pick manually → Generic interaction
AFTER:  Describe task → Auto-route to specialist → Adaptive output
```

**Key Achievements:**
- ✅ Developer Experience improvements (scripts, progress, clarity)
- ✅ Sisyphus integration with auto-routing and sequences  
- ✅ GSD workflow for structured planning/execution
- ✅ Context Gate for adaptive output
- ✅ Codebase mapping for understanding

**Key Gaps:**
- ⚠️ Incomplete agent registry (19/185+ documented)
- ⚠️ No foundation skill for shared context
- ⚠️ No executable tools (instructions only)

---

**Last Updated:** 2026-03-29
**Maintainer:** Stefan Pirker (@stevie86)
