# Claude Framework

**Reusable development patterns and tools for Claude/Claude Code projects**

## Overview

Claude Framework provides standardized development patterns, multi-agent coordination, API design patterns, and workflow management for projects using Claude or Claude Code as the primary development assistant.

## Features

### 🤖 Multi-Agent Coordination
- **16 specialized subagents** for different development domains
- **Parallel execution patterns** for maximum development velocity
- **Territory assignment** and conflict resolution protocols
- **Status broadcasting** for coordination

### 🏗️ Development Workflow
- **Test-Driven Development (TDD)** integration
- **Task management patterns** with progress tracking
- **Phase-based development** (Planning → Development → Debugging → Verification)
- **Quality gates** and validation checkpoints

### 🔌 API Design Patterns
- **Standardized response formats** across all services
- **Error handling strategies** with recovery instructions
- **Rate limiting** and connection pooling patterns
- **Authentication** and security best practices

### 📝 Documentation Standards
- **Living documentation** patterns
- **Example-driven** API documentation
- **Progressive disclosure** of complexity
- **Troubleshooting** and solution guides

## Quick Start

### 1. Add as Submodule
```bash
# In your project root
git submodule add https://github.com/[your-username]/claude-framework.git claude-framework
git submodule update --init --recursive
```

### 2. Project Setup
```bash
# Copy template to your project root
cp claude-framework/CLAUDE.template.md CLAUDE.md

# Edit CLAUDE.md with your project details
# Replace {{PROJECT_NAME}}, {{PROJECT_DESCRIPTION}}, etc.
```

### 3. Configure Subagents
```bash
# Copy agent configurations (if needed)
cp -r claude-framework/.claude/ .claude/

# Customize agent configurations for your project
```

## Directory Structure

```
claude-framework/
├── .claude/                    # Subagent configurations
│   ├── agents/                # 16 specialized subagents
│   ├── commands/              # Custom Claude commands
│   └── settings.local.json    # Local settings template
├── docs/
│   ├── api-patterns.md        # Generic API design patterns
│   ├── development-workflow.md # TDD & task management
│   └── multiagent.md          # Multi-agent coordination
├── CLAUDE.template.md         # Template for project CLAUDE.md
├── README.md                  # This file
└── .gitignore                # Framework-specific ignores
```

## Available Subagents

### Development & Architecture
- **ai-engineer**: AI/ML system design, TensorFlow/PyTorch, model deployment
- **api-designer**: REST/GraphQL APIs, OpenAPI specs, endpoint design
- **backend-designer**: Microservices, scalability, system architecture
- **database-admin**: PostgreSQL/SQLite optimization, schema design, migrations
- **deployment-engineer**: CI/CD pipelines, Docker/Kubernetes, blue-green deployments
- **electron-pro**: Desktop apps, IPC communication, native OS integration

### Documentation & Quality
- **api-documenter**: OpenAPI/Swagger docs, Postman collections, dev guides
- **code-reviewer**: Security audits, static analysis, design patterns
- **code-quality-pragmatist**: Anti-pattern detection, complexity reduction
- **claude-md-compliance-checker**: Project standards verification

### Testing & Validation
- **ui-comprehensive-tester**: E2E testing, Puppeteer/Playwright, cross-browser
- **task-completion-validator**: Feature verification, requirement validation
- **ultrathink-debugger**: Complex debugging, root cause analysis, performance issues

### Project Management
- **jenny**: Requirements vs implementation verification, spec compliance
- **karen**: Reality check on completion status, honest project assessment
- **multiagent-organizer**: Workflow orchestration, parallel task coordination

## Usage Patterns

### Single-Agent Development
```typescript
// Use specific subagent for focused tasks
Task({
  subagent_type: "api-designer",
  prompt: "Design user authentication API endpoints"
})
```

### Multi-Agent Coordination
```typescript
// Launch multiple agents in parallel
Task({ subagent_type: "backend-designer", prompt: "Design service architecture" })
Task({ subagent_type: "database-admin", prompt: "Design data schemas" })
Task({ subagent_type: "api-designer", prompt: "Design API contracts" })
```

### Development Phases
1. **Planning Phase**: backend-designer → api-designer → database-admin → jenny
2. **Development Phase**: [context-specific] → ui-comprehensive-tester → code-quality-pragmatist
3. **Debugging Phase**: ultrathink-debugger → code-reviewer
4. **Verification Phase**: task-completion-validator → karen → jenny

## Integration Examples

### Project CLAUDE.md Template
```markdown
# CLAUDE.md - Your Project Name

## Subagents
**16 specialized subagents** - See `claude-framework/docs/multiagent.md`

## Development Workflow
**TDD Integration** - See `claude-framework/docs/development-workflow.md`

## API Patterns
**Standard Patterns** - See `claude-framework/docs/api-patterns.md`
```

### Package.json Scripts
```json
{
  "scripts": {
    "tdd": "jest --watch",
    "test:agent:backend": "jest src/backend --coverage",
    "test:agent:frontend": "jest src/frontend --coverage",
    "docs:framework": "cd claude-framework && ls docs/"
  }
}
```

## Best Practices

### Documentation
- Follow progressive disclosure patterns
- Include complete examples for all APIs
- Update documentation with every change
- Use living documentation that stays current

### Development
- Always use TDD workflow phases
- Validate with appropriate subagents
- Maintain clear separation of concerns
- Document architectural decisions

### Multi-Agent Coordination
- Use parallel execution when possible
- Respect agent territory assignments
- Broadcast status updates
- Test integration points between agents

## Contributing

1. Fork the repository
2. Create a feature branch
3. Follow existing patterns and conventions
4. Add tests for new functionality
5. Update documentation
6. Submit a pull request

## Versioning

This framework follows semantic versioning:
- **Major**: Breaking changes to patterns or interfaces
- **Minor**: New patterns or subagents added
- **Patch**: Bug fixes or documentation improvements

## License

MIT License - See LICENSE file for details

## Related Projects

- [Claude Code](https://claude.ai/code) - Official Claude CLI
- [Claude Documentation](https://docs.claude.com/) - Complete Claude documentation

---

**Version 1.0.0** - Initial Claude Framework release
**Last Updated**: 2025-09-26