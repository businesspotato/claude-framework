# Output Style Setup Guide

## Overview

The Claude Framework includes standardized output style configurations to ensure consistent, professional communication across all projects. The "Direct Objective" style promotes fact-based, solution-focused interactions.

## Quick Setup

### 1. Copy Settings Template
```bash
# From your project root
cp claude-framework/.claude/settings.local.template.json .claude/settings.local.json
```

### 2. Customize Placeholders
Edit `.claude/settings.local.json` and replace:

- `{{USERNAME}}` → Your system username (e.g., `johnsmith`)
- `{{PROJECT_COMMAND_PREFIX}}` → Your project's CLI prefix (e.g., `optcg`, `myapp`)

### 3. Verify Configuration
```bash
# Check that the file exists and is valid JSON
cat .claude/settings.local.json | jq .
```

## Configuration Details

### Output Style Options

#### Direct Objective
```json
{
  "outputStyle": "Direct Objective"
}
```

**Characteristics:**
- Professional, objective tone
- Focus on facts and solutions
- Avoids excessive agreement patterns
- Prioritizes technical accuracy
- Minimal unnecessary elaboration

### Permission Templates

#### Basic Template (settings.template.json)
- Core Claude Code tools
- Standard bash commands
- MCP server integrations
- Basic file operations

#### Extended Template (settings.local.template.json)
- Project-specific command patterns
- Docker and container operations
- Script execution permissions
- Advanced MCP features

### Example Customization

```json
{
  "permissions": {
    "allow": [
      "Bash(printf \"rs\\n\")",
      "Read(//Users/myusername/**)",
      "Bash(myapp:*)",
      "Bash(./scripts/**)",
      "mcp__zen__*",
      "Task(*)"
    ],
    "deny": [],
    "ask": []
  },
  "outputStyle": "Direct Objective"
}
```

## Integration with CLAUDE.md

Add output style reference to your project's CLAUDE.md:

```markdown
## Communication Style

**Direct Objective** - Professional, fact-focused communication style
- See `claude-framework/docs/output-style-setup.md` for configuration details
- Configured via `.claude/settings.local.json`
```

## Troubleshooting

### Common Issues

#### Invalid JSON
```bash
# Validate JSON syntax
cat .claude/settings.local.json | jq .
```

#### Permission Errors
- Check that paths use forward slashes
- Ensure username in paths matches system username
- Verify command prefixes match actual project commands

#### Template Not Found
```bash
# Verify framework is available
ls claude-framework/.claude/settings.local.template.json
```

### Validation Commands

```bash
# Check current Claude settings
cat .claude/settings.local.json | jq '.outputStyle'

# Verify permissions structure
cat .claude/settings.local.json | jq '.permissions.allow | length'

# List all configured permissions
cat .claude/settings.local.json | jq -r '.permissions.allow[]'
```

## Advanced Configuration

### Custom Output Styles
While "Direct Objective" is recommended, you can customize:

```json
{
  "outputStyle": "Custom",
  "customStyleRules": {
    "tone": "professional",
    "verbosity": "concise",
    "technical_depth": "high"
  }
}
```

### Project-Specific Permissions
Add permissions for your specific tools:

```json
{
  "permissions": {
    "allow": [
      "Bash(your-custom-tool:*)",
      "Bash(./your-scripts/**)",
      "mcp__your-custom-mcp__*"
    ]
  }
}
```

## Security Considerations

### Safe Defaults
The templates include safe defaults:
- No destructive operations without approval
- Restricted file system access
- Controlled command execution

### Customization Guidelines
- Add permissions incrementally
- Test new permissions in isolated environments
- Document project-specific additions
- Regular security review of permissions

## Version Compatibility

| Claude Framework | Claude Code | Compatible |
|------------------|-------------|------------|
| 1.0.0+          | Latest      | ✅         |
| 0.9.x           | Latest      | ⚠️ Limited |

## Related Documentation

- [Multi-Agent Setup](multiagent.md)
- [Development Workflow](development-workflow.md)
- [API Patterns](api-patterns.md)

---

**Last Updated**: 2025-09-27
**Framework Version**: 1.0.0