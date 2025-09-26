# Multi-Agent Architecture & Coordination Documentation

**Claude Framework** multi-agent development patterns, specialized subagent configurations, and coordination protocols.

## Available Subagents (16 Total)

Located in `claude-framework/.claude/agents/` directory. Each subagent has specialized expertise and can be invoked using the Task tool.

### Development & Architecture Subagents

#### ai-engineer
- **Purpose**: AI/ML system design and implementation
- **Expertise**: TensorFlow, PyTorch, Hugging Face, model deployment
- **Use Cases**: AI pipeline design, model optimization, ML infrastructure

#### api-designer
- **Purpose**: API architecture and design patterns
- **Expertise**: REST, GraphQL, OpenAPI specifications
- **Use Cases**: Endpoint design, contract definition, API documentation

#### backend-designer
- **Purpose**: Backend systems architecture
- **Expertise**: Microservices, scalability, performance optimization
- **Use Cases**: Service design, system architecture, backend patterns

#### database-admin
- **Purpose**: Database architecture and optimization
- **Expertise**: PostgreSQL, MySQL, MongoDB, Redis, performance tuning
- **Use Cases**: Schema design, query optimization, migration strategies

#### deployment-engineer
- **Purpose**: CI/CD pipelines and deployment strategies
- **Expertise**: Docker, Kubernetes, GitHub Actions, blue-green deployments
- **Use Cases**: Pipeline setup, deployment automation, infrastructure as code

#### electron-pro
- **Purpose**: Desktop application specialist
- **Expertise**: Electron, native OS integration, security, performance
- **Use Cases**: Main/renderer process, IPC communication, desktop features

### Documentation & Quality Subagents

#### api-documenter
- **Purpose**: Comprehensive API documentation
- **Expertise**: OpenAPI/Swagger, Postman, documentation automation
- **Use Cases**: API specs, interactive docs, developer guides

#### code-reviewer
- **Purpose**: Code quality and security review
- **Expertise**: Static analysis, design patterns, security vulnerabilities
- **Use Cases**: PR reviews, security audits, code quality assessment

#### code-quality-pragmatist
- **Purpose**: Pragmatic code quality assessment
- **Expertise**: Identifying over-engineering, simplification opportunities
- **Use Cases**: Code simplification, pragmatic refactoring, complexity reduction

#### claude-md-compliance-checker
- **Purpose**: CLAUDE.md compliance verification
- **Expertise**: Project standards, guideline adherence
- **Use Cases**: Automated compliance checks, standard enforcement

### Testing & Validation Subagents

#### ui-comprehensive-tester
- **Purpose**: Comprehensive UI testing across platforms
- **Expertise**: Puppeteer, Playwright, Mobile testing
- **Use Cases**: E2E testing, cross-browser testing, mobile app testing

#### task-completion-validator
- **Purpose**: Validates task completion claims
- **Expertise**: Verification, functional testing, requirement validation
- **Use Cases**: Feature verification, completion audits, acceptance testing

#### ultrathink-debugger
- **Purpose**: Deep debugging and root cause analysis
- **Expertise**: Complex bug resolution, performance issues, system failures
- **Use Cases**: Production issues, intermittent failures, performance debugging

### Project Management Subagents

#### jenny
- **Purpose**: Requirements vs implementation verification
- **Expertise**: Specification analysis, gap identification
- **Use Cases**: Compliance checking, feature completeness, spec validation

#### karen
- **Purpose**: Reality check on project completion status
- **Expertise**: Honest assessment, cutting through incomplete work
- **Use Cases**: Project audits, realistic planning, completion verification

#### multiagent-organizer
- **Purpose**: Coordinates multi-agent workflows
- **Expertise**: Parallel execution, task distribution, agent coordination
- **Use Cases**: Complex multi-step tasks, parallel development, workflow orchestration

## Multi-Agent Tree Architecture

### Applied Pattern: Parallel Processing for Maximum Efficiency

- **CRITICAL**: Use parallel agents during planning phase whenever possible
- **NEVER serialize agent work** when multiple agents can work simultaneously
- **ALWAYS launch multiple agents in single message** for independent tasks
- **MAXIMIZE development velocity** through coordinated parallel execution

### Agent Hierarchy Tree Structure

```
Main Coordinator (Root)
├── Backend Development Branch
│   ├── API Integration Agent (eBay, Azure AI)
│   ├── Database Architecture Agent
│   └── Electron Main Process Agent
├── Frontend Development Branch
│   ├── React Component Agent (Material-UI)
│   ├── TypeScript Interface Agent
│   └── State Management Agent (Redux)
├── Intelligence System Branch
│   ├── Card Recognition AI Agent
│   ├── Market Intelligence Agent
│   └── OCR Processing Agent
└── DevOps & Quality Branch
    ├── Testing Automation Agent (TDD)
    ├── CI/CD Pipeline Agent
    └── Performance Optimization Agent
```

### IMMEDIATE PARALLEL LAUNCH Protocol

```bash
Launch 6 parallel development agents immediately:

Agent 1 (Backend-API): Handle API integration and endpoints
Agent 2 (Frontend-Components): Create React components and UI integration
Agent 3 (AI-Pipeline): Implement AI processing system
Agent 4 (Database): Design and implement database schemas
Agent 5 (Testing): Write comprehensive TDD test suites
Agent 6 (DevOps): Set up CI/CD and deployment automation

Each agent works independently on assigned file territories with status coordination.
```

## Territory Assignment Protocol

- **Agent 1**: `src/backend/api/*`, `src/backend/services/external/*`
- **Agent 2**: `src/frontend/components/*`, `src/frontend/pages/*`
- **Agent 3**: `src/backend/services/ai/*`, `src/backend/services/processing/*`
- **Agent 4**: `data/database/*`, `src/backend/models/*`, migration scripts
- **Agent 5**: `__tests__/*`, `src/**/*.test.ts`
- **Agent 6**: `.github/workflows/*`, `scripts/deployment/*`, `scripts/devops/*`, application lifecycle management

## Git Workflow & Parallel Development

### Worktrees (Parallel Multi-Agent Development)

```bash
# Create agent-specific worktrees for parallel development
git worktree add .trees/backend-api -b parallel/backend-api
git worktree add .trees/frontend-ui -b parallel/frontend-ui
git worktree add .trees/ai-pipeline -b parallel/ai-pipeline
git worktree add .trees/database-layer -b parallel/database-layer
git worktree add .trees/testing-suite -b parallel/testing-suite
git worktree add .trees/devops-automation -b parallel/devops-automation

# Launch Claude instances in each worktree (separate terminals)
cd .trees/backend-api && claude      # Terminal 1: Backend Agent
cd .trees/frontend-ui && claude      # Terminal 2: Frontend Agent
# ... etc for each agent

# Cleanup after feature completion
git worktree remove .trees/feature-name
```

### Multi-Agent Coordination Protocol

**Agent Status Updates**: Update `status/status.json` before starting any task
**File Modifications**: Log immediately in status system
**Conflict Detection**: Report to coordination system
**Integration Testing**: Test cross-agent handoffs before completion

### Task Management System

**Task.md File Structure**:
- **Global Task File**: `docs/project/task-status.md` - Contains overall project vision and completed tasks
- **Agent-Specific Task Files**:
  - `docs/tasks/backend-api-tasks.md`
  - `docs/tasks/frontend-components-tasks.md`
  - `docs/tasks/ai-pipeline-tasks.md`
  - `docs/tasks/database-layer-tasks.md`
  - `docs/tasks/testing-suite-tasks.md`
  - `docs/tasks/devops-automation-tasks.md`

**Task Management Workflow**:
1. **Agent Initialization**: Each agent MUST read both global task status and their specific tasks
2. **Current Focus**: Agent-specific task files contain only active/current tasks for immediate focus
3. **Task Completion**: When task completed, move from agent-specific to global task status with completion details
4. **Cross-Reference**: All agents should understand global context while focusing on their specific responsibilities

## Application Execution Protocol

### DevOps Agent Application Management Rule

**CRITICAL**: The **DevOps Automation Agent** is the designated Application Process Manager for the multi-agent system.

#### Primary Responsibilities:
- **ONLY the DevOps Automation Agent** should run `npm start` or `npm run dev`
- **ONLY the DevOps Automation Agent** should restart the application (`rs` command)
- **ONLY the DevOps Automation Agent** manages application lifecycle (start/stop/restart)
- **ONLY the DevOps Automation Agent** monitors application health and performance

#### Other Agent Protocols:
- **All other agents** MUST coordinate through DevOps agent for application restarts
- **All other agents** can request application restarts via status file updates
- **All other agents** should monitor application output via process monitoring tools
- **All other agents** NEVER directly execute `npm start`, `rs`, or application lifecycle commands

#### Status Communication for Application Management:

```typescript
// Request restart from other agents
interface RestartRequest {
  requestingAgent: string;
  reason: string;
  urgency: "low" | "normal" | "high" | "critical";
  filesChanged: string[];
  timestamp: string;
}

// DevOps agent response
interface RestartResponse {
  status: "acknowledged" | "restarting" | "completed" | "failed";
  restartId: string;
  estimatedDuration: number; // seconds
  timestamp: string;
}
```

## Agent Communication System

### Status Broadcasting Format

```typescript
interface AgentStatus {
  agentId: string;
  status:
    | "idle"
    | "planning"
    | "in_progress"
    | "testing"
    | "completed"
    | "blocked";
  currentTask: string;
  filesOwned: string[];
  filesModified: string[];
  dependencies: string[];
  estimatedCompletion: string;
}
```

### Conflict Resolution Protocol

1. Detect file ownership conflicts in status.json
2. Automatic escalation to conflict-resolver agent
3. Coordination meeting in shared status file
4. Resolution strategy documented
5. Implementation with all affected agents coordinated

## Multi-Agent Architecture

### Orchestrator

- **multiagent-organizer**: Coordinates domain experts in parallel

### Domain Experts (Ultrathinking Required)

- **api-designer**: API integration architecture
- **backend-designer**: Backend systems
- **database-admin**: Data layer architecture
- **deployment-engineer**: CI/CD & infrastructure
- **electron-pro**: Desktop application specifics
- **ai-engineer**: AI/ML oversight & intervention

### Documentation Requirements

Each expert produces:

1. Architecture diagrams
2. Function signatures
3. Data flow maps
4. Error scenarios
5. Integration points
6. Testing strategy
7. Performance targets

## Multi-Agent TDD Coordination

### Agent-Specific TDD Responsibilities:

**Backend API Agent:**
- **MUST** write unit tests for all service methods before implementation
- **MUST** write integration tests for all IPC handlers before implementation
- **MUST** write API endpoint tests before route implementation
- **MUST** test error conditions and edge cases

**Frontend Components Agent:**
- **MUST** write component tests before creating components
- **MUST** write hook tests before implementing custom hooks
- **MUST** write interaction tests before adding user interactions
- **MUST** test component states and props validation

**AI Pipeline Agent:**
- **MUST** write tests for AI service functions before implementation
- **MUST** write tests for card recognition logic before implementation
- **MUST** write tests for data transformation before implementation
- **MUST** test with mock data and error scenarios

**Database Layer Agent:**
- **MUST** write model tests before creating database models
- **MUST** write migration tests before writing migrations
- **MUST** write query tests before implementing complex queries
- **MUST** test data validation and constraints

**Testing Suite Agent:**
- **MUST** create test infrastructure before other agents need it
- **MUST** write test utilities before other agents implement features
- **MUST** write cross-agent integration tests
- **MUST** maintain test coverage reports

**DevOps Automation Agent:**
- **MUST** write tests for deployment scripts before implementation
- **MUST** write tests for monitoring functions before implementation
- **MUST** write tests for health checks before implementation
- **MUST** test CI/CD pipeline components

### Cross-Agent TDD Coordination

#### Integration Testing Requirements:
- **Backend API Agent** writes integration tests before implementing IPC handlers
- **Frontend Components Agent** writes component integration tests before implementing data consumption
- **AI Pipeline Agent** writes service integration tests before implementing pipeline connections
- **Database Layer Agent** writes data persistence tests before implementing schemas
- **Testing Suite Agent** writes cross-agent workflow tests before integration points are built

#### TDD Communication Protocol:
```typescript
interface TDDStatusUpdate {
  agentId: string;
  feature: string;
  phase: "test-written" | "test-passing" | "refactored" | "integration-ready";
  testFiles: string[];
  dependencies: string[];
  readyForIntegration: boolean;
}
```

## Multi-Agent Test Commands

```bash
# Individual Agent Testing (Run before ANY commit)
npm run test:agent:backend        # Backend agent - MUST pass before commit
npm run test:agent:frontend       # Frontend agent - MUST pass before commit
npm run test:agent:ai             # AI pipeline agent - MUST pass before commit
npm run test:agent:database       # Database layer agent - MUST pass before commit
npm run test:agent:testing        # Testing suite agent - MUST pass before commit
npm run test:agent:devops         # DevOps agent - MUST pass before commit

# Cross-Agent Integration Testing (Run before merge)
npm run test:integration          # Cross-agent integration - MUST pass before merge
npm run test:e2e                  # End-to-end workflows - MUST pass before merge
npm run test:contracts            # API contracts - MUST pass before merge
```

## Invoking Subagents

### Using the Task Tool

```typescript
// Example: Launch code reviewer for recent changes
Task({
  subagent_type: "code-reviewer",
  description: "Review API changes",
  prompt: "Review the recent API integration for security and best practices"
})

// Example: Launch multiple agents in parallel
// Send single message with multiple Task tool calls
Task({ subagent_type: "api-designer", ... })
Task({ subagent_type: "backend-designer", ... })
Task({ subagent_type: "database-admin", ... })
```

### Common Multi-Agent Workflows

#### 1. Feature Implementation (Parallel)
- api-designer: Design API contracts
- backend-designer: Design service architecture
- database-admin: Design data models
- All work in parallel, then integrate

#### 2. Debugging Complex Issues
- ultrathink-debugger: Root cause analysis
- code-reviewer: Identify code issues
- task-completion-validator: Verify fixes

#### 3. Project Audit
- karen: Realistic assessment
- jenny: Spec compliance check
- claude-md-compliance-checker: Standards verification

## Multi-Agent Startup Protocol

```bash
# Agent-specific startup sequence
./scripts/start-multi-agent-session.sh

# Executes: environment cleanup, agent coordination init,
# territory validation, development server start
```

## Custom Commands (.claude/commands/)

**Multi-Agent Commands**:

- `/launch-agents`: Start multi-agent parallel session
- `/agent-status`: Check current agent coordination status
- `/resolve-conflicts`: Initiate agent conflict resolution
- `/sync-agents`: Synchronize all agent branches
- `/agent-territories`: Display agent file territory assignments

## Performance Targets (Multi-Agent)

- **Agent coordination overhead**: <50ms per status update
- **Cross-agent communication**: <200ms for handoffs
- **Multi-agent task completion**: Max 2x single-agent time
- **Parallel execution efficiency**: 40-60% time reduction vs sequential

## Multi-Agent Quick Start

```bash
# 1. Initialize multi-agent session
./scripts/init-multi-agent-session.sh

# 2. Launch all agents in parallel (6 terminals)
./scripts/launch-all-agents.sh

# 3. Monitor coordination status
watch -n 5 'cat status/status.json | jq .'

# 4. Integrate and deploy
./scripts/integrate-all-agents.sh
```

## Best Practices

1. **Always use parallel agents** when tasks are independent
2. **Define clear territories** to avoid conflicts
3. **Use status broadcasting** for coordination
4. **Test integration points** between agent work
5. **Document handoff requirements** clearly
6. **Verify with validator agents** before marking complete
7. **Respect DevOps agent application management** protocols
8. **Follow TDD practices** within each agent domain
9. **Communicate through status files** for coordination
10. **Test cross-agent integrations** before marking features complete

---

**Version 4.0.0 - Consolidated Multi-Agent Framework**

*This comprehensive guide covers both individual subagent usage and advanced multi-agent coordination protocols for Claude Framework projects.*