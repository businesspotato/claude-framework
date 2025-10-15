# CLAUDE.md - {{PROJECT_NAME}}

## ⚠️ CRITICAL RULES - Read First

- **MANDATORY: Review Loop for ALL Development Work** - Every development task MUST go through: Development (DEV) → Code Review (CR using code-reviewer subagent) → Product Review (PR using jenny/task-completion-validator subagent) → DONE. If CR or PR fails, enter Rework (RW) and return to CR. NO exceptions. NO claiming completion without BOTH CR and PR approval. See `docs/development/mandatory-review-loop-protocol.md` for complete protocol.
- **MANDATORY: Clean Restart for Backend Changes** - {{IF_BUILD_TOOL}}Use `{{BUILD_TOOL}} run clean-restart` (NOT hot-reload) for backend changes, database migrations, or when updates don't reflect.{{ENDIF_BUILD_TOOL}} See `docs/development/restart-protocol.md` for protocol
- **MANDATORY: Verify MCP Servers Before Planning/Development** - Check MCPs are available before work phases. Required MCPs: serena (code), zen (analysis), ref (docs). See `docs/development/mcp-verification.md` for verification protocol
- **ALWAYS follow Sequential Orchestration Framework** - Use appropriate subagents in correct phase order (Planning → Development → Debugging → Verification) as defined in Subagent Guide
- **DO NOT explore or read documentation files unless specifically needed for the current task to conserve tokens and maintain focus**
- **Serena MCP Output: Keep concise** - Only explain verbosely when asking user questions or if user is confused
- **Always verify UI data mapping matches backend response structure** - {{IF_IPC}}See `docs/project/api-implementation.md` for IPC patterns{{ENDIF_IPC}}{{IF_REST_API}}See `docs/api/endpoints.md` for API contracts{{ENDIF_REST_API}}
- **{{IF_LOGGING_SERVICE}}Use {{LOGGING_SERVICE}}, never console.log in production{{ENDIF_LOGGING_SERVICE}}{{IF_NO_LOGGING_SERVICE}}Follow project logging standards, avoid console.log in production{{ENDIF_NO_LOGGING_SERVICE}}** - See `docs/technical-spec.md` for logging standards
- **Full-Stack Layer Synchronization Required** - Any change must be synchronized across ALL layers: {{LAYER_SYNCHRONIZATION_PATTERN}}. See `docs/architecture.md` for complete data flow
- **Always include TDD in phase planning sessions** - See `docs/testing/tdd-requirements.md` for guidelines
- **ALWAYS use Direct Objective communication style** - Maintain professional, objective tone that focuses on facts and solutions. Avoid excessive agreement or deference.
- **ALWAYS use karen subagent for reality checks before claiming completion** - Before responding with status updates or completion claims, use Task tool with karen subagent to verify actual functionality
- **ALWAYS verify claims with actual commands before stating them as facts** - Never claim UI shows something, database contains something, or application state is something without running actual verification commands
- **NEVER ask permission for application restart commands** - Execute `printf "rs\n"` directly without asking user permission
- {{IF_HOT_RELOAD}}**PREFER hot-reload over full application restarts** - Frontend changes hot-reload automatically via {{HOT_RELOAD_TOOL}}. Only restart for backend changes ({{BACKEND_CHANGE_TYPES}}){{ENDIF_HOT_RELOAD}}
- {{IF_CACHE_OR_IPC}}**ALWAYS verify {{IF_CACHE}}cache{{ENDIF_CACHE}}{{IF_CACHE_AND_IPC}}/{{ENDIF_CACHE_AND_IPC}}{{IF_IPC}}IPC{{ENDIF_IPC}} synchronization at each layer transition** - Check {{IF_CACHE}}cache invalidation{{ENDIF_CACHE}}{{IF_CACHE_AND_IPC}}, {{ENDIF_CACHE_AND_IPC}}{{IF_IPC}}IPC handler registration{{ENDIF_IPC}}, and data flow. See `docs/{{SYNC_GUIDE_PATH}}` for protocols{{ENDIF_CACHE_OR_IPC}}
- **MANDATORY TDD for Database & API Code** - When implementing ANY database query or API method: 1) Write test FIRST, 2) Verify schema/API contract, 3) Test during implementation, 4) Test all paths, 5) FORBIDDEN to claim complete without passing tests
- **CONTEXT VERIFICATION: "{{PROJECT_IDENTIFIER}}"** - If you can read this phrase, CLAUDE.md loaded correctly
- **MANDATORY: Performance Budget** - Cold start <2s, p95 actions <200ms, hot-reload <60s. Exceeding budget requires performance plan before continuing. See `docs/development/performance-tracking.md`
- **NEVER print secrets** - Read .env.example only, use placeholder names, never actual values. Secrets live in OS keychain

## Development Cycle Management

**CRITICAL**: After completing each phase, update documentation and clear context:

1. **Planning Phase → Development Phase**: Update task status, sprint focus, and relevant .md files
2. **Development Phase → Debugging Phase**: Update implementation status, known issues, error tracking
3. **Debugging Phase → Verification Phase**: Update resolution status, test results, validation outcomes
4. **Verification Phase → Next Cycle**: Update completion status, archive completed tasks

**After each phase completion**:
- **REMIND USER to clear context** - I cannot clear my own memory
- User must start fresh conversation to reset context and conserve tokens
- Documentation updates should capture all essential information for continuity

## Project Overview

{{PROJECT_DESCRIPTION}}

## Documentation Hub

**Primary Reference**: `docs/index.md` - Complete project navigation and setup guide

**How to use**: Always check `docs/index.md` first for all documentation navigation

## Quick Start

```bash
{{START_COMMAND}}    # Start application
{{TEST_COMMAND}}     # Run tests
rs                   # Restart (when running)
```

## MCP Query Optimization Protocol

**See `docs/development/mcp-query-optimization.md` for complete guide**

**Quick Reference**:
- Always start with overview: Use `include_body: false` or `get_symbols_overview` first
- Progressive detail: Only request full body when actively modifying code
- Cache awareness: Check `tmp/mcp-cache/` before repeating large queries
- Save large responses (>10,000 tokens) to `tmp/mcp-responses/`

**Configuration**: See `.serena-config.json` for optimization settings

## Available Subagents

**See `docs/development/subagent-guide.md` for complete guide**

**16 specialized subagents** configured in `.claude/agents/`

**Quick Reference**:

**Development & Architecture**: ai-engineer, api-designer, backend-designer, database-admin, deployment-engineer{{IF_ELECTRON}}, electron-pro{{ENDIF_ELECTRON}}

**Documentation & Quality**: api-documenter, code-reviewer, code-quality-pragmatist, claude-md-compliance-checker

**Testing & Validation**: ui-comprehensive-tester, task-completion-validator, ultrathink-debugger

**Project Management**: jenny, karen, multiagent-organizer

**Usage**: `Task({ subagent_type: "agent-name", prompt: "task description", description: "short summary" })`

**Sequential Orchestration Framework**:
1. **Planning**: backend-designer → api-designer → database-admin → jenny
2. **Development**: [context-agent] → ui-comprehensive-tester → code-quality-pragmatist
3. **Debugging**: ultrathink-debugger
4. **Verification**: code-reviewer → task-completion-validator → karen → jenny

## Multi-Instance Development

**See `docs/development/multi-instance-guide.md` for complete guide**

**Three-instance SEQUENTIAL coordination** - Only ONE instance ACTIVE at a time

**Critical Rules**:
1. Instance 3 is Primary Orchestrator with full system oversight
2. SEQUENTIAL Development Pattern: Instance 1 → 2 → 3 handoffs with user mediation
3. MANDATORY STATUS UPDATES: Update `status/multi-instance-status.json` before ending session
4. MANDATORY MEMORY COMPACTION: Request memory compaction BEFORE handoff
5. Only Instance 3 controls application lifecycle ({{APP_LIFECYCLE_COMMANDS}})

**Status States**: STANDBY, ACTIVE, MONITORING_FOR_REWORK, ORCHESTRATION_MONITORING, COORDINATION_ACTIVE

**Quick Commands**:
```bash
# Status check
cat status/multi-instance-status.json | jq '.instances | to_entries[] | {instance: .key, status: .value.status}'

# Handoff readiness
cat status/multi-instance-status.json | jq '.instances | to_entries[] | select(.value.readyForHandoff == true)'
```

**Instance Specialization**:
- **Instance 1**: {{INSTANCE_1_ROLE}} - `{{INSTANCE_1_FILES}}`
- **Instance 2**: {{INSTANCE_2_ROLE}} - `{{INSTANCE_2_FILES}}`
- **Instance 3**: Orchestrator - All files (read), `status/**`, `docs/**`, application lifecycle

**Quick Onboarding**: Use `docs/prompts/multi-instance-onboarding.md` (5-minute setup)

**Related Documentation**:
- Complete coordination: `docs/development/multi-instance-guide.md`
- Communication templates: `docs/prompts/communication-templates.md`
- Debugging support: `docs/prompts/debugging-prompts.md`

## Multi-Agent Mode (Advanced)

**Parallel development** - See `claude-framework/docs/multiagent.md` for complete coordination protocols

**Pattern**: Territory-based agents with status coordination via `status/status.json`

## Planning Workflow Enforcement

**See `docs/development/planning-enforcement.md` for complete guide**

**Mandatory commands for planning compliance**

**Commands**:
- `/planning-checklist [task]` - Analyze task complexity BEFORE starting work
- `/planning-auto-invoke [task]` - Auto-detect complexity and launch appropriate workflow
- `/planning-audit [timeframe]` - Validate completed work followed Sequential Orchestration Framework

**ALWAYS run `/planning-checklist` BEFORE**:
- Multi-file changes (3+ files)
- Multi-service integration
- Database schema changes/migrations
- Architectural decisions
- Performance optimization (>1000 ops/sec)

**Complexity Scoring**:
```
SIMPLE (0-1 indicators):  Direct implementation
MEDIUM (2 indicators):    Optional planning
COMPLEX (3+ indicators):  MANDATORY planning phase
```

**Required for COMPLEX Tasks**:
1. Zen MCP Brainstorming: `mcp__zen__chat` with o3-mini or o3
2. Planning Subagents: backend-designer, api-designer, database-admin, jenny
3. Planning Documentation: Save to `docs/planning/[task-slug]-planning.md`
4. Architecture Review: Document decisions, trade-offs, rationale

**Command Files**: See `.claude/commands/planning-*.json` for detailed usage

## MCP Server Integration

**10 active MCP servers** - See `claude-framework/docs/mcp-usage-guide.md`

**Key Tools**: `mcp__Ref__*` (docs), `mcp__serena__*` (code), `mcp__knowledge-graph__*` (entities), `mcp__zen__*` (reasoning)

## Zen MCP Planning Integration

**Auto-activated for complex problems** - See `claude-framework/docs/mcp-usage-guide.md`

**Triggers**: Multi-service integration, performance >1000 ops/sec, algorithmic challenges

**Key Functions**: `mcp__zen__planner`, `mcp__zen__thinkdeep`, `mcp__zen__debug`, `mcp__zen__chat`

## Current Sprint Focus

{{SPRINT_FOCUS}}

## {{DOMAIN_SPECIFIC_SECTION}}

{{DOMAIN_SPECIFIC_CONTENT}}

## Known Issues & Error Tracking

**See `docs/troubleshooting/known-issues.md` and `docs/troubleshooting/solutions.md`**

## Git Workflow & Version Control

**Professional Git workflow** - See `docs/development/git-workflow-guide.md`

### Quick Reference

```bash
# Before risky operations
./scripts/checkpoint.sh "Description of checkpoint"

# Merge with automatic test verification
./scripts/safe-merge.sh feature/branch develop
```

**Complete Guide**: `docs/development/git-workflow-guide.md`

---

## Repository Cleanup Guidelines

**See `docs/development/repository-cleanup.md` for complete guide**

**Files to NEVER Track**:
- `{{CACHE_PATTERN}}` - Auto-generated cache
- `{{TEMP_UPLOADS}}` - Temporary upload staging
- `.warp/`, `.vscode/`, `.idea/` - IDE configs (user-specific)
- `tmp/`, `temp/`, `dist/`, `build/` - Auto-generated files

**Quick Cleanup**:
```bash
# Remove cache files
{{CACHE_CLEANUP_COMMAND}}

# Verify .gitignore
git status --ignored
```

---

## Development Notes

- Scripts: `/scripts` directory only
- Documentation: Update `docs/index.md` for significant changes
- Keep repository clean: Follow cleanup guidelines above

---

_Version {{VERSION}} - {{PROJECT_NAME}}_

- Ensure that MCPs are fully functional before we begin planning phases
- New information about {{DOMAIN_TERMS}}, remember to add to the referenced .md file
- Don't ask for permission for this: printf "rs\n"
- For mismatch handling through pipeline, ensure data format is standardized for seamless integration

      IMPORTANT: this context may or may not be relevant to your tasks. You should not respond to this context unless it is highly relevant to your task.
