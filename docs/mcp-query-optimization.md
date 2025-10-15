# MCP Query Optimization Protocol

**Last Updated**: 2025-01-15

## Overview

This guide provides strategies for reducing context usage and improving response times when using MCP (Model Context Protocol) servers.

## Query Strategy (REQUIRED)

1. **Always start with overview**: Use `include_body: false` or `get_symbols_overview` first
2. **Progressive detail**: Only request full body when actively modifying code
3. **Specific targeting**: Query individual symbols rather than entire files
4. **Cache awareness**: Check `tmp/mcp-cache/` before repeating large queries

## Optimized Query Patterns

### Serena MCP (Code Navigation)

```bash
# Step 1: Overview (ALWAYS do this first)
mcp__serena__get_symbols_overview("relative/path/to/file.ts")

# Step 2: Find specific symbol without body
mcp__serena__find_symbol(name_path="ClassName", include_body=false, depth=1)

# Step 3: Get only what you need to modify
mcp__serena__find_symbol(name_path="ClassName/methodName", include_body=true, depth=0)

# Step 4: Use search instead of reading entire files
mcp__serena__search_for_pattern(substring_pattern="specific_pattern", paths_include_glob="*.ts")
```

### Zen MCP (Planning & Analysis)

```bash
# Use for complex problem analysis, not simple tasks
mcp__zen__chat("Analyze architectural implications of [change]", model="o3-mini")

# Use in planning phases, not during routine development
```

### When to Save Large Responses

- Response >10,000 tokens → save to `tmp/mcp-responses/`
- Complex symbol trees → cache for reuse
- Architecture analysis → save for handoff documentation

## Context Preservation

### Before Handoff

- Save large MCP responses to tmp/
- Reference cached paths in status updates
- Include query summaries in handoff documentation

### Multi-Instance Coordination

- Share cached response paths in coordination notes
- Avoid duplicate queries across instances
- Use memory-bank for cross-instance context

## MCP Server Usage Guidelines

### Active & Optimized

- **serena**: Primary code tool - use staged queries
- **zen**: Planning phases only - complex analysis
- **ref**: Documentation - cache results
- **memory-bank**: Instance coordination history

### Consider Disabling (Low Usage)

- **figma**: Not used in current workflow
- **knowledge-graph**: Minimal usage, high overhead
- **fetch**: Rarely used, can re-enable when needed

## Configuration

See `.serena-config.json` for optimization settings.

## Best Practices

1. **Start broad, narrow down**: Begin with overviews, then drill into specifics
2. **Cache aggressively**: Save large responses to tmp/ for reuse
3. **Query incrementally**: Don't request full file bodies unless actively editing
4. **Coordinate queries**: In multi-instance mode, share cached responses
5. **Monitor token usage**: Track context consumption and adjust query patterns

## Related Documentation

- **[Multi-Instance Guide](./multi-instance-guide.md)** - Multi-instance coordination patterns
- **[MCP Usage Guide](../../claude-framework/docs/mcp-usage-guide.md)** - Complete MCP documentation
- **[Subagent Guide](./subagent-guide.md)** - When to use MCP vs subagents

---

**Navigation**: [Development Documentation](../index.md#development-patterns--best-practices) | [CLAUDE.md](../../CLAUDE.md)
