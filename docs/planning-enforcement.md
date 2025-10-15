# Planning Workflow Enforcement

**Last Updated**: 2025-01-15

## Overview

Mandatory commands for planning compliance. Prevents workflow violations and ensures proper subagent/Zen MCP usage.

## Commands

### Pre-Work Validation

**`/planning-checklist [task]`** - Analyze task complexity and determine planning requirements BEFORE starting work

### Automated Workflow

**`/planning-auto-invoke [task]`** - Auto-detect complexity and launch appropriate planning workflow with subagents and Zen MCP

### Post-Work Compliance

**`/planning-audit [timeframe]`** - Validate completed work followed Sequential Orchestration Framework

## When to Use

### ALWAYS run `/planning-checklist` BEFORE

- Multi-file changes (3+ files)
- Multi-service integration
- Database schema changes/migrations
- Architectural decisions
- Performance optimization (>1000 ops/sec)
- Algorithmic challenges
- Scalability planning

### Complexity Scoring

```
SIMPLE (0-1 indicators):  Direct implementation
MEDIUM (2 indicators):    Optional planning
COMPLEX (3+ indicators):  MANDATORY planning phase
```

## Required for COMPLEX Tasks

### Planning Phase MUST include

1. **Zen MCP Brainstorming**: `mcp__zen__chat` with o3-mini or o3
2. **Planning Subagents**: backend-designer, api-designer, database-admin, jenny
3. **Planning Documentation**: Save to `docs/planning/[task-slug]-planning.md`
4. **Architecture Review**: Document decisions, trade-offs, rationale

**Violations will be flagged by `/planning-audit`**

## Integration

- Part of Sequential Orchestration Framework (Planning → Development → Debugging → Verification)
- Works with existing `/zen-brainstorm` and `/tdd-check` commands
- Prevents technical debt from skipped planning
- Ensures multi-dimensional problem analysis

## Command Files Location

See command files in `.claude/commands/planning-*.json` for detailed usage

## Sequential Orchestration Framework

### Phase Sequence

1. **Planning**: backend-designer → api-designer → database-admin → jenny
2. **Development**: [context-agent] → ui-comprehensive-tester → code-quality-pragmatist
3. **Debugging**: ultrathink-debugger
4. **Verification**: code-reviewer → task-completion-validator → karen → jenny

### Usage Pattern

```typescript
// Planning Phase
Task({
  subagent_type: "backend-designer",
  prompt: "Design system architecture for [feature]",
  description: "Design system architecture"
})

// Development Phase
Task({
  subagent_type: "electron-pro",
  prompt: "Implement IPC handlers for [feature]",
  description: "Implement IPC handlers"
})

// Verification Phase
Task({
  subagent_type: "task-completion-validator",
  prompt: "Verify [feature] meets requirements",
  description: "Verify feature completion"
})
```

## Planning Documentation Template

```markdown
# [Feature Name] Planning

**Date**: YYYY-MM-DD
**Complexity**: SIMPLE/MEDIUM/COMPLEX

## Requirements Analysis

[Requirements gathered with jenny subagent]

## Architecture Design

[Architecture planned with backend-designer]

## API Contracts

[API design from api-designer]

## Database Schema

[Schema design from database-admin]

## Zen MCP Analysis

[Complex problem analysis from mcp__zen__chat]

## Implementation Plan

[Step-by-step implementation sequence]

## Trade-offs & Decisions

[Architectural decisions and rationale]
```

## Best Practices

1. **Always assess complexity first**: Use `/planning-checklist` before starting
2. **Document planning decisions**: Save to `docs/planning/`
3. **Use Zen MCP for complex problems**: Leverage o3-mini for multi-dimensional analysis
4. **Follow phase sequence**: Planning → Development → Debugging → Verification
5. **Verify with audit**: Run `/planning-audit` after completion

## Common Violations

### Skipped Planning Phase

**Symptom**: Jumping directly to implementation for complex tasks

**Fix**: Run `/planning-checklist` and follow recommended workflow

### Missing Zen MCP Analysis

**Symptom**: Complex architectural decisions without multi-dimensional analysis

**Fix**: Use `/zen-brainstorm` or `mcp__zen__chat` for complex problems

### Incomplete Planning Documentation

**Symptom**: Planning decisions not documented for future reference

**Fix**: Create planning document in `docs/planning/` with template

### Wrong Phase Sequence

**Symptom**: Using verification subagents during development phase

**Fix**: Follow Sequential Orchestration Framework phase order

## Integration with Other Workflows

### Multi-Instance Coordination

- Instance 3 coordinates planning phase
- Planning documents shared across instances
- Zen MCP used for collaborative brainstorming

### TDD Workflow

- Planning phase includes test strategy
- TDD Guard MCP enforces test-first development
- Test coverage requirements defined in planning

### Git Workflow

- Planning documents committed before implementation
- Feature branches follow planning decisions
- Checkpoints created after planning phase

## Related Documentation

- **[Subagent Guide](./subagent-guide.md)** - Complete subagent reference
- **[MCP Usage Guide](../../claude-framework/docs/mcp-usage-guide.md)** - Zen MCP integration
- **[Multi-Agent Orchestration](../../claude-framework/docs/multiagent.md)** - Sequential orchestration
- **[TDD Requirements](../testing/tdd-requirements.md)** - Test-driven development

---

**Navigation**: [Development Documentation](../index.md#development-patterns--best-practices) | [CLAUDE.md](../../CLAUDE.md)
