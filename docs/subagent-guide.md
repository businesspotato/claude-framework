# Subagent Guide

**Last Updated**: 2025-01-15

## Overview

This project uses 16 specialized subagents configured in `.claude/agents/` for different development phases. This guide explains when and how to use each subagent effectively.

## How to Use Subagents

### Syntax

```typescript
Task({
  subagent_type: "agent-name",
  prompt: "detailed task description",
  description: "short 3-5 word task summary"
})
```

### Example

```typescript
Task({
  subagent_type: "backend-designer",
  prompt: "Design database schema for variant pricing with support for alternate art and parallel cards. Include migration strategy from current schema.",
  description: "Design variant pricing schema"
})
```

## Development & Architecture Subagents

### ai-engineer
**Purpose**: AI/ML system design, TensorFlow/PyTorch, model deployment, neural architectures

**Use When**:
- Designing AI/ML systems
- Implementing neural network architectures
- Optimizing model performance
- Deploying AI models to production

### api-designer
**Purpose**: REST/GraphQL APIs, OpenAPI specs, endpoint design, API contracts

**Use When**:
- Designing API endpoints
- Creating OpenAPI specifications
- Defining API contracts
- Planning API versioning strategies

### backend-designer
**Purpose**: Microservices, system architecture, scalability, service design

**Use When**:
- Designing system architecture
- Planning microservices structure
- Scalability planning
- Service integration strategies

### database-admin
**Purpose**: PostgreSQL/SQLite optimization, schema design, migrations, indexing

**Use When**:
- Designing database schemas
- Creating migrations
- Optimizing database queries
- Planning indexing strategies

### deployment-engineer
**Purpose**: CI/CD pipelines, Docker/Kubernetes, blue-green deployments, infrastructure

**Use When**:
- Setting up CI/CD pipelines
- Configuring Docker/Kubernetes
- Planning deployment strategies
- Infrastructure optimization

### electron-pro
**Purpose**: Desktop applications, IPC communication, native OS integration, preload scripts

**Use When**:
- Implementing IPC handlers
- Creating Electron preload scripts
- Native OS integration
- Desktop application features

## Documentation & Quality Subagents

### api-documenter
**Purpose**: OpenAPI/Swagger documentation, Postman collections, developer guides

**Use When**:
- Creating API documentation
- Building Postman collections
- Writing developer guides
- Documenting API endpoints

### code-reviewer
**Purpose**: Security audits, static analysis, design patterns, vulnerability detection

**Use When**:
- Conducting security audits
- Reviewing code quality
- Detecting vulnerabilities
- Enforcing design patterns

### code-quality-pragmatist
**Purpose**: Anti-pattern detection, complexity reduction, refactoring suggestions

**Use When**:
- Identifying code smells
- Reducing complexity
- Suggesting refactorings
- Preventing over-engineering

### claude-md-compliance-checker
**Purpose**: Project standards verification, CLAUDE.md rule adherence

**Use When**:
- Verifying project standards
- Checking CLAUDE.md compliance
- Enforcing development guidelines
- Validating implementation patterns

## Testing & Validation Subagents

### ui-comprehensive-tester
**Purpose**: E2E testing, Puppeteer/Playwright, cross-browser testing, integration tests

**Use When**:
- Creating E2E tests
- Testing UI components
- Cross-browser testing
- Integration testing

### task-completion-validator
**Purpose**: Feature verification, requirement validation, acceptance criteria checks

**Use When**:
- Verifying feature completion
- Validating requirements
- Checking acceptance criteria
- Ensuring implementation completeness

### ultrathink-debugger
**Purpose**: Complex debugging, root cause analysis, performance issues, race conditions

**Use When**:
- Debugging complex issues
- Root cause analysis
- Performance problem investigation
- Race condition detection

## Project Management Subagents

### jenny
**Purpose**: Requirements vs implementation verification, spec compliance, feature completeness

**Use When**:
- Verifying implementation matches specs
- Checking feature completeness
- Validating requirements
- Ensuring specification compliance

### karen
**Purpose**: Reality check on completion status, honest assessment, false claim detection

**Use When**:
- Verifying actual vs claimed completion
- Reality checking implementation status
- Detecting false completion claims
- Honest assessment of work quality

### multiagent-organizer
**Purpose**: Workflow orchestration, parallel task coordination, agent territory management

**Use When**:
- Coordinating multiple agents
- Orchestrating parallel workflows
- Managing agent territories
- Complex multi-agent coordination

## When to Use Which Subagent

### Planning Phase

1. **backend-designer** → System architecture
2. **api-designer** → API contracts
3. **database-admin** → Schema design
4. **jenny** → Requirements validation

### Development Phase

1. **electron-pro** → IPC handlers, desktop features
2. **backend-designer** → Service implementation
3. **database-admin** → Migrations, queries
4. **ui-comprehensive-tester** → Test coverage

### Debugging Phase

1. **ultrathink-debugger** → Complex issues, root cause analysis

### Verification Phase

1. **code-reviewer** → Security audits, code quality, vulnerabilities
2. **task-completion-validator** → Feature validation, acceptance criteria
3. **karen** → Reality check, honest assessment
4. **jenny** → Requirement compliance, spec verification
5. **claude-md-compliance-checker** → Standards compliance

## Sequential Orchestration Framework

### Phase Sequence

1. **Planning**: backend-designer → api-designer → database-admin → jenny
2. **Development**: [context-agent] → ui-comprehensive-tester → code-quality-pragmatist
3. **Debugging**: ultrathink-debugger
4. **Verification**: code-reviewer → task-completion-validator → karen → jenny

## Best Practices

1. **Use phase-appropriate subagents**: Match subagent to development phase
2. **Follow sequential order**: Complete planning before development
3. **Always verify**: Use validation subagents before claiming completion
4. **Reality check**: Use karen subagent for honest assessment
5. **Document decisions**: Use appropriate subagents for documentation

## Subagent vs MCP

**Use Subagents When**:
- Complex multi-step workflows
- Phase-specific expertise needed
- Validation and verification required
- Documentation generation

**Use MCP When**:
- Code navigation and reading
- Quick symbol lookups
- File searching
- Simple queries

## Related Documentation

- **[Multi-Agent Orchestration](../../claude-framework/docs/multiagent.md)** - Complete orchestration guide
- **[Planning Enforcement](./planning-enforcement.md)** - Planning workflow requirements
- **[MCP Query Optimization](./mcp-query-optimization.md)** - MCP usage patterns

---

**Navigation**: [Development Documentation](../index.md#development-patterns--best-practices) | [CLAUDE.md](../../CLAUDE.md)
