# CLAUDE.md - {{PROJECT_NAME}}

## ⚠️ CRITICAL RULES - Read First

- **ALWAYS check MCP server availability before planning or development** - Use `mcp__zen__version` or similar to verify MCP servers are functional before beginning any work phase
- **ALWAYS follow Sequential Orchestration Framework** - Use appropriate subagents in correct phase order (Planning → Development → Debugging → Verification) as defined in Automatic Subagent Orchestration section
- **DO NOT explore or read documentation files unless specifically needed for the current task to conserve tokens and maintain focus**
- **Serena MCP Output: Keep concise** - Only explain verbosely when asking user questions or if user is confused. Avoid repeating function names and parameters in output
- **Always verify UI data mapping matches backend response structure** - See `docs/project/api-implementation.md` for IPC patterns
- **Use loggingService, never console.log in production** - See `docs/project/technical-implementation.md` for logging standards
- **Check cache to ensure changes are actually taking place when updating features that use them**
- **Full-Stack Layer Synchronization Required** - Any change must be synchronized across ALL layers: Database → Backend Services → IPC Channels → Frontend Hooks → UI Components → Cache. Missing ANY layer causes runtime failures. See `docs/project/system-architecture.md` for complete data flow
- **Always include TDD in phase planning sessions** - See `claude-framework/docs/development-workflow.md` for guidelines
- **ALWAYS use Direct Objective communication style** - Maintain professional, objective tone that focuses on facts and solutions. Avoid excessive agreement or deference. Use measured language without sycophantic patterns.
- **ALWAYS use karen subagent for reality checks before claiming completion** - Before responding with status updates or completion claims, use Task tool with karen subagent to verify actual functionality and prevent false realities. Karen provides honest assessment of what actually works vs what should work.
- **NEVER claim application restart is successful without verification** - Always check actual application status via BashOutput, process logs, or explicit verification before reporting success. False success claims waste user time and break trust.
- **ALWAYS monitor application logs with BashOutput after startup** - After any application start/restart, use BashOutput tool to check for React errors, console warnings, unhandled promise rejections, and database errors. Check both stdout and stderr. Create automated feedback loop to catch issues immediately rather than waiting for user reports.
- **NEVER ask permission for application restart commands** - Execute `printf "rs
"` directly without asking user permission. This is a standard development command for hot-reloading and should be used freely during development cycles.
- **ALWAYS verify cache/IPC synchronization at each layer transition** - Check cache invalidation, IPC handler registration, and data flow between Database → Backend → IPC → Frontend → UI. Missing cache verification creates invisible barriers that block data from working properly. See `claude-framework/docs/cache-ipc-verification.md` for protocols.
- **Test cache invalidation after any backend data changes** - Specifically test with known examples (e.g., "{{TEST_CARD_EXAMPLE}}") to ensure UI reflects backend changes. Cache issues are invisible and cause frustrating data synchronization failures.
- **Verify IPC handler registration matches frontend hook calls** - When changing backend handlers, ensure frontend hooks use matching invoke calls and cache invalidation keys. Missing synchronization causes silent runtime failures.

## Development Cycle Management

**CRITICAL**: After completing each phase, update documentation and clear context:

1. **Planning Phase → Development Phase**: Update `docs/project/task-status.md`, sprint focus, and relevant .md files
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

**Primary Reference**: `docs/index.md` - Central documentation with:
- Complete environment setup guide
- All API configurations
- Development patterns and best practices
- Performance targets and benchmarks
- Links to all specialized documentation

**How to use**: Always check `docs/index.md` first for project information, environment setup, and navigation to specific documentation topics.

## Quick Start

See `docs/index.md#quick-start` for setup and commands

## Communication Style

**Direct Objective** - Professional, fact-focused communication style configured via `.claude/settings.local.json`
- See `claude-framework/docs/output-style-setup.md` for configuration details
- Maintains objective tone focused on facts and solutions
- Avoids excessive agreement patterns in favor of technical accuracy

## Available Subagents

**16 specialized subagents** configured in `claude-framework/.claude/agents/` - See `claude-framework/docs/multiagent.md` for detailed descriptions

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

**Usage**: Invoke via `Task({ subagent_type: "subagent-name", prompt: "task description" })`

## Safe Multi-Instance Development

**Current Approach**: Multiple Claude Code instances with sequential orchestration - See `claude-framework/docs/multi-instance-coordination.md` for comprehensive protocols

**Key Safety Principles:**
- **Sequential Execution**: Use multiple instances but coordinate changes sequentially
- **Instance Specialization**: Each instance maintains specialized context (backend, frontend, AI, database)
- **Handoff Coordination**: Clear documentation and status communication between instances
- **Cache/IPC Verification**: Comprehensive verification at each layer transition to prevent invisible barriers
- **Single Restart Authority**: Designate one instance for application lifecycle management

**Instance Roles:**
- **Instance 1**: Backend/API development and IPC handler management
- **Instance 2**: Frontend/UI development and component integration
- **Instance 3**: AI/ML pipeline and processing services
- **Instance 4**: Database/DevOps and cache management

## Multi-Agent Mode (Advanced)

**Advanced parallel development** - See `claude-framework/docs/multiagent.md#multi-agent-coordination-protocol` for comprehensive coordination protocols

**Key Concepts:**
- **Parallel Execution**: Launch multiple agents simultaneously for independent tasks
- **Territory Assignment**: Each agent owns specific file paths and responsibilities
- **Status Broadcasting**: Coordinate through `status/status.json` updates
- **DevOps Agent**: Only DevOps agent manages application lifecycle (`npm start`, `rs`)

## 🤖 Automatic Subagent Orchestration (Single-Tab Mode)

**Sequential execution framework for structured development phases** - See `claude-framework/docs/multiagent.md` for parallel coordination patterns

### Sequential Execution Framework

#### PLANNING PHASE Sequence
1. **Initial Analysis** → backend-designer
   - System architecture implications
   - Service dependency mapping
   - Performance impact assessment

2. **API Design** (if endpoints affected) → api-designer
   - Endpoint structure validation
   - IPC channel planning - See `docs/project/api-implementation.md` for IPC patterns
   - Response format specification

3. **Data Layer** (if schema changes) → database-admin
   - Migration planning
   - Index optimization
   - Query performance projections

4. **Validation Gate** → jenny
   - Requirement alignment check
   - Specification completeness

#### DEVELOPMENT PHASE Sequence
1. **Primary Development** → [Context-Specific Agent]
   - electron-pro: UI/Renderer changes
   - backend-designer: Service architecture
   - ai-engineer: AI pipeline modifications
   - database-admin: Schema/query changes

2. **Test Generation** → ui-comprehensive-tester
   - Generate test cases for current changes
   - Update existing test suites - See `claude-framework/docs/development-workflow.md` for TDD guidelines
   - Coverage gap analysis

3. **Quality Check** → code-quality-pragmatist
   - Anti-pattern detection
   - Complexity analysis
   - Performance bottleneck identification

#### DEBUGGING PHASE Sequence
1. **Root Cause Analysis** → ultrathink-debugger
   - Stack trace analysis
   - Performance profiling
   - Memory leak detection

2. **Code Review** → code-reviewer
   - Security vulnerability scan
   - Design pattern validation
   - Best practice compliance - See `docs/project/technical-implementation.md` for standards

#### VERIFICATION PHASE Sequence
1. **Feature Validation** → task-completion-validator
   - Acceptance criteria verification
   - Integration test results
   - User story completion

2. **Reality Check** → karen
   - Actual functionality verification
   - "Does it really work?" assessment
   - Performance target achievement - See `docs/index.md` for benchmarks

3. **Specification Compliance** → jenny
   - Original requirements matching
   - Documentation accuracy - See `docs/index.md` for documentation structure
   - API contract validation

**Usage**: Execute phases sequentially using `Task({ subagent_type: "agent-name", prompt: "phase-specific task" })` according to above sequences

## MCP Server Integration

**10 active MCP servers** for enhanced development capabilities - See `claude-framework/docs/mcp-usage-guide.md` for comprehensive usage

**Key MCP Workflows:**
- **Documentation**: Use `mcp__Ref__ref_search_documentation` for live docs instead of static copies
- **Code Analysis**: `mcp__serena__*` tools for semantic code operations
- **Knowledge Graph**: `mcp__knowledge-graph__*` for project entity relationships
- **Advanced Reasoning**: `mcp__zen__*` for complex problem solving and planning

## Application Restart Verification Protocol

**When restarting the application:**

1. **Use kill/start instead of printf "rs
" if rs fails**
2. **Wait for complete startup** (check for "Window ready to show" and "📊 System Health: X/X services healthy")
3. **Monitor for errors in stderr** (UnhandledPromiseRejectionWarning, database schema errors)
4. **Verify core functionality** (expected data loads, no React crashes)
5. **Only claim success after comprehensive verification**

**Log Monitoring Requirements:**
- Use BashOutput tool to check both stdout and stderr after restart
- Create automated feedback loop to catch issues immediately
- Check for React errors, console warnings, unhandled promise rejections, database errors

## Current Sprint Focus

{{SPRINT_FOCUS}}

## {{DOMAIN_SPECIFIC_SECTION}}

{{DOMAIN_SPECIFIC_CONTENT}}

## Known Issues & Error Tracking

**See `docs/troubleshooting/` for comprehensive error documentation**

## Additional Development Notes

- **Scripts go in `/scripts` directory only** - See `docs/index.md#project-structure`
- **Update documentation with significant changes** - See `docs/index.md` for documentation structure

---

_Template Version 1.0.0 - Claude Framework Integration_
- Ensure that MCPs are fully functional before we begin planning phases, if it has not yet been checked
- New information about {{DOMAIN_SPECIFIC_TERMS}} that you did not know before, remember to add to the referenced .md file
- Don't ask for permission for this: printf "rs\n"

# important-instruction-reminders
Do what has been asked; nothing more, nothing less.
NEVER create files unless they're absolutely necessary for achieving your goal.
ALWAYS prefer editing an existing file to creating a new one.
NEVER proactively create documentation files (*.md) or README files. Only create documentation files if explicitly requested by the User.

IMPORTANT: this context may or may not be relevant to your tasks. You should not respond to this context unless it is highly relevant to your task.