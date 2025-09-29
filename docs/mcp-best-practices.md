# MCP Best Practices & Integration Patterns

## Critical MCP Rules

### Server Availability Checking
**ALWAYS check MCP server availability before planning or development phases**

```bash
# Verify MCP servers are functional
mcp__zen__version
```

**Why it matters:**
- Prevents workflow failures mid-development
- Ensures all planned tools are available
- Avoids context switching when servers are down

### Serena MCP Output Guidelines
**Keep Serena MCP output concise**

- Only explain verbosely when asking user questions or if user is confused
- Avoid repeating function names and parameters in output
- Focus on results, not process descriptions

**Example:**
```typescript
// Good
"Found 3 authentication methods in src/auth.ts"

// Avoid
"Using mcp__serena__find_symbol with parameters name_path='auth' and include_body=true to search for authentication methods in the src/auth.ts file"
```

## Zen MCP Integration Patterns

### Automatic Activation Triggers
Use Zen MCP for complex problems with these characteristics:

- **Multi-service integration** - Services spanning multiple domains/APIs
- **Performance optimization (>1000 ops/sec)** - High-throughput requirements
- **Real-time processing requirements** - Sub-second response times
- **Algorithmic challenges** - Complex data structures or algorithms
- **Scalability planning (>10x growth)** - Architecture for significant scale

### Planning Sequence Integration

#### Complex Problems (Auto-invokes Zen)
```
mcp__zen__planner → mcp__zen__thinkdeep → backend-designer → api-designer → jenny
```

#### Simple Problems
```
backend-designer → api-designer → database-admin → jenny
```

### Manual Override
Prefix with `@zen:` to force Zen brainstorming for any problem:
```
@zen: How should we implement user authentication?
```

### Core Zen Functions Reference

| Function | Usage | When to Use |
|----------|-------|-------------|
| `mcp__zen__planner` | Interactive sequential planning with revisions | Complex multi-step projects |
| `mcp__zen__thinkdeep` | Multi-stage investigation and analysis | Root cause analysis, architecture decisions |
| `mcp__zen__consensus` | Multi-model consensus through structured debate | Technology choices, design decisions |
| `mcp__zen__debug` | Systematic debugging and root cause analysis | Complex bugs, performance issues |
| `mcp__zen__codereview` | Step-by-step code review with expert validation | Security reviews, quality gates |
| `mcp__zen__precommit` | Git changes validation before committing | Large commits, breaking changes |
| `mcp__zen__chat` | Collaborative thinking partner | Brainstorming, second opinions |

## Application Restart Verification

### Restart Protocol
When restarting applications during development:

1. **Failover Strategy**: Use kill/start instead of `printf "rs\n"` if rs fails
2. **Startup Verification**: Wait for complete startup indicators
   - "Window ready to show" message
   - "📊 System Health: X/X services healthy" status
3. **Error Monitoring**: Monitor stderr for critical errors
   - UnhandledPromiseRejectionWarning
   - Database schema errors
   - Connection failures
4. **Functionality Verification**: Verify core functionality loads
   - Expected data appears
   - No React crashes
   - UI responds correctly
5. **Success Criteria**: Only claim success after comprehensive verification

### Log Monitoring Requirements
**ALWAYS monitor application logs with BashOutput after startup**

```bash
# Monitor both stdout and stderr
BashOutput tool usage after restart

# Look for specific error patterns:
# - React errors
# - Console warnings  
# - Unhandled promise rejections
# - Database errors
```

**Create automated feedback loop:**
- Check issues immediately rather than waiting for user reports
- Use BashOutput to create continuous monitoring
- Report specific errors found, not just "success"

### Never Ask Permission Rules
**Execute `printf "rs\n"` directly without asking user permission**

- This is a standard development command for hot-reloading
- Should be used freely during development cycles
- Part of normal development workflow

## Reality Check Patterns

### Karen Subagent Integration
**ALWAYS use karen subagent for reality checks before claiming completion**

```typescript
// Before claiming completion
Task({ 
  subagent_type: "karen", 
  prompt: "Verify that the authentication system actually works as claimed" 
})
```

**Karen provides:**
- Honest assessment of what actually works vs what should work
- Reality check on completion status
- Prevention of false completion claims

### Verification Requirements
**NEVER claim application restart is successful without verification**

- Always check actual application status
- Use BashOutput, process logs, or explicit verification
- False success claims waste user time and break trust

## Full-Stack Layer Synchronization

### Critical Synchronization Rule
**Any change must be synchronized across ALL layers:**

```
Database → Backend Services → IPC Channels → Frontend Hooks → UI Components → Cache
```

### Example: Backend Handler Change
Changing a backend handler from 'collection:delete-card' requires updating:

1. **Backend IPC handler registration**
2. **Frontend hook invoke calls** 
3. **Cache invalidation keys**
4. **UI component imports**

**Missing ANY layer causes runtime failures**

### Architecture Documentation Reference
See `docs/project/system-architecture.md` for complete data flow patterns

## Token Conservation Strategies

### Documentation Reading Rules
**DO NOT explore or read documentation files unless specifically needed**

- Conserve tokens by reading only task-relevant documentation
- Use targeted searches instead of full document reads
- Focus on specific sections needed for current task

### Context Management
**After each development phase completion:**

- Update relevant documentation
- REMIND USER to clear context (Claude cannot clear own memory)
- User must start fresh conversation to reset context
- Documentation updates should capture essential information for continuity

## Security & Performance Patterns

### Logging Standards
**Use loggingService, never console.log in production**

- Reference project-specific technical implementation docs
- Maintain consistent logging patterns across services
- Enable proper log aggregation and monitoring

### Cache Verification
**Check cache to ensure changes are actually taking place**

- Verify cache invalidation after updates
- Test with specific examples (e.g., "price tracking issue tested w/ Sanji OP12-041")
- Ensure UI reflects backend changes

## Integration Checklist

### Before Starting Development
- [ ] Check MCP server availability (`mcp__zen__version`)
- [ ] Identify complexity level (simple vs Zen-required)
- [ ] Plan sequential orchestration framework usage
- [ ] Verify documentation access needs

### During Development  
- [ ] Use appropriate subagents for each phase
- [ ] Monitor application restarts with verification protocol
- [ ] Synchronize changes across all architectural layers
- [ ] Use karen for reality checks before claiming completion

### After Development
- [ ] Verify actual functionality (not just claimed completion)
- [ ] Update relevant documentation
- [ ] Monitor logs for errors post-restart
- [ ] Remind user to clear context for next phase

---

**Last Updated**: 2025-09-27
**Framework Version**: 1.0.0