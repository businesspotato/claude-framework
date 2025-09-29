# Claude Framework

A comprehensive framework for structured development with Claude Code, featuring autonomous multi-instance coordination, specialized subagents, and systematic verification protocols.

## Overview

Claude Framework provides advanced development capabilities including autonomous multi-instance coordination, specialized subagent orchestration, architecture-agnostic templates, and systematic verification protocols for scalable, reliable development workflows.

## 🚀 Key Features

### Autonomous Multi-Instance Coordination
- **3+ Instance Coordination**: Backend, Frontend, Infrastructure, and Coordinator instances
- **Autonomous Handoffs**: Automatic verification and approval without manual intervention
- **Integration Oversight**: Comprehensive layer verification prevents data pipeline breaks
- **Failure Recovery**: Systematic remediation protocols and emergency escalation

### Specialized Subagent Orchestration
- **16 Specialized Subagents**: From API design to security auditing
- **Sequential Workflows**: Structured phase progression (Planning → Development → Testing → Verification)
- **Context-Aware Routing**: Automatic subagent selection based on task complexity
- **Quality Gates**: Built-in validation at each development phase

### Architecture-Agnostic Templates
- **Web Applications**: React/Vue/Angular + Express/FastAPI/Django
- **Microservices**: Service mesh, API gateway, container orchestration
- **Mobile Applications**: React Native, Flutter, native iOS/Android
- **Custom Architectures**: Fully customizable instance roles and verification

### Systematic Verification Framework
- **Layer-Based Verification**: Database → Service → Communication → Presentation
- **Automated Quality Gates**: Test coverage, performance benchmarks, security scans
- **Integration Testing**: End-to-end workflows across all architectural layers
- **Performance Monitoring**: Real-time regression detection and optimization

## 📁 Framework Structure

```
claude-framework/
├── docs/                                    # Core framework documentation
│   ├── autonomous-multi-instance-framework.md    # Complete framework guide
│   ├── asynchronous-handoff-protocols.md        # Handoff automation
│   ├── multi-instance-coordination.md           # Instance coordination
│   └── multiagent.md                           # Subagent orchestration
├── templates/                              # Project setup templates
│   ├── quick-setup-guide.md               # Getting started guide
│   ├── multi-instance-setup-template.yaml # Generic configuration
│   ├── verification-script-template.js    # Verification framework
│   └── architecture-patterns/             # Architecture-specific templates
│       ├── web-application.yaml          # Web app configuration
│       ├── microservices.yaml           # Microservices configuration
│       └── mobile-application.yaml      # Mobile app configuration
└── .claude/agents/                        # Subagent configurations
    ├── ai-engineer.md                     # AI/ML system design
    ├── api-designer.md                    # REST/GraphQL APIs
    ├── backend-designer.md               # System architecture
    ├── code-reviewer.md                  # Security & quality audits
    ├── database-admin.md                 # Database optimization
    ├── deployment-engineer.md            # CI/CD & deployment
    ├── electron-pro.md                   # Desktop applications
    ├── jenny.md                          # Requirements validation
    ├── karen.md                          # Reality checking
    ├── multiagent-organizer.md          # Workflow orchestration
    ├── task-completion-validator.md      # Feature verification
    ├── ui-comprehensive-tester.md        # E2E testing
    └── ultrathink-debugger.md           # Complex debugging
```

## 🎯 Quick Start

### 1. Choose Your Architecture

```bash
# Web Application (React + Express)
cp claude-framework/templates/architecture-patterns/web-application.yaml project-config.yaml

# Microservices (Docker + Kubernetes)
cp claude-framework/templates/architecture-patterns/microservices.yaml project-config.yaml

# Mobile App (React Native)
cp claude-framework/templates/architecture-patterns/mobile-application.yaml project-config.yaml

# Custom Architecture
cp claude-framework/templates/multi-instance-setup-template.yaml project-config.yaml
```

### 2. Set Up Project Structure

```bash
# Create required directories
mkdir -p docs/verification-failures/{failure-templates,failure-history}
mkdir -p scripts/verification
mkdir -p status
mkdir -p docs/instance-logs

# Copy verification framework
cp claude-framework/templates/verification-script-template.js scripts/verification/project-verification.js

# Initialize status tracking
echo '{}' > status/multi-instance-status.json
```

### 3. Configure Instances

Edit your `project-config.yaml` and replace template variables:

```yaml
project_config:
  name: "Your Project Name"
  architecture_type: "web_app"  # or microservices, mobile_app
  coordination_mode: "THREE_INSTANCE_AUTONOMOUS"

instances:
  backend:
    role: "BACKEND_DEVELOPMENT"
    specializations:
      - "API development"
      - "Database operations"
    file_patterns:
      - "src/api/**/*"
      - "src/backend/**/*"
```

### 4. Test Your Setup

```bash
# Test verification script
node scripts/verification/project-verification.js

# Verify configuration
jq . project-config.yaml
```

## 🏗️ Architecture Patterns

### Web Application (3 Instances)
- **Backend Instance**: API development, database, authentication
- **Frontend Instance**: Components, state management, UI/UX
- **Coordinator Instance**: Integration testing, performance monitoring

### Microservices (4 Instances)
- **Services Instance**: Individual microservice development
- **Gateway Instance**: API gateway, routing, security
- **Infrastructure Instance**: Deployment, monitoring, orchestration
- **Coordinator Instance**: Cross-service integration, system health

### Mobile Application (3 Instances)
- **Mobile UI Instance**: Screens, components, navigation
- **App Logic Instance**: State, business logic, API integration
- **Coordinator Instance**: Cross-platform testing, app store compliance

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

## Output Style Configuration

### Direct Objective Style
The framework includes a "Direct Objective" output style that maintains professional, objective communication focused on facts and solutions. This style:

- **Avoids excessive agreement or deference**
- **Uses measured language without sycophantic patterns**
- **Focuses on technical accuracy and problem-solving**
- **Provides direct, objective technical information**
- **Prioritizes truthfulness over validation**

### Settings Template
The framework provides two settings templates:

#### settings.template.json
Basic template with core permissions and Direct Objective style.

#### settings.local.template.json
Extended template with project-specific placeholders:
- `{{USERNAME}}` - Replace with your system username
- `{{PROJECT_COMMAND_PREFIX}}` - Replace with your project's CLI command prefix

### Configuration Options
```json
{
  "outputStyle": "Direct Objective",
  "permissions": {
    "allow": [
      "Bash(printf \"rs\n\")",
      "mcp__zen__*",
      "Task(*)"
    ]
  }
}
```

### Integration
1. Copy template to your project: `cp claude-framework/.claude/settings.local.template.json .claude/settings.local.json`
2. Replace placeholders with your project details
3. Customize permissions as needed for your specific project requirements

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

## Related Documentation

- [Multi-Agent Coordination](docs/multiagent.md) - Advanced parallel development patterns
- [Development Workflow](docs/development-workflow.md) - TDD integration and task management
- [API Design Patterns](docs/api-patterns.md) - Standardized API development guidelines
- [MCP Best Practices](docs/mcp-best-practices.md) - Critical MCP integration rules and patterns
- [MCP Usage Guide](docs/mcp-usage-guide.md) - Comprehensive MCP workflow documentation
- [Output Style Setup](docs/output-style-setup.md) - Communication style configuration guide

## Related Projects

- [Claude Code](https://claude.ai/code) - Official Claude CLI
- [Claude Documentation](https://docs.claude.com/) - Complete Claude documentation

---

**Version 1.0.0** - Initial Claude Framework release
**Last Updated**: 2025-09-26