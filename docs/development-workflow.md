# Development Workflow & Task Management

**Claude Framework** development methodology combining task management patterns with comprehensive TDD integration for structured development workflows.

## Task Management Patterns

### Task Status Legend
- ⬜ Not Started
- 🟨 In Progress
- ✅ Completed
- 🔄 Needs Review
- ❌ Blocked

### Task Management Workflow

#### Project-Level Task Management
- **Global Task File**: `docs/project/task-status.md` - Contains overall project vision and completed tasks
- **Priority Planning**: Phase-based development with clear milestones
- **Progress Tracking**: Real-time status updates with completion percentages
- **Milestone Management**: Clear deliverable goals with success criteria

#### Agent-Specific Task Management (Multi-Agent Mode)
- **Agent-Specific Task Files**:
  - `docs/tasks/backend-api-tasks.md`
  - `docs/tasks/frontend-components-tasks.md`
  - `docs/tasks/ai-pipeline-tasks.md`
  - `docs/tasks/database-layer-tasks.md`
  - `docs/tasks/testing-suite-tasks.md`
  - `docs/tasks/devops-automation-tasks.md`

**Agent Task Workflow**:
1. **Agent Initialization**: Each agent MUST read both global task status and their specific tasks
2. **Current Focus**: Agent-specific task files contain only active/current tasks for immediate focus
3. **Task Completion**: When task completed, move from agent-specific to global task status with completion details
4. **Cross-Reference**: All agents should understand global context while focusing on their specific responsibilities

### Task Categories & Prioritization

#### Priority Levels
1. **🔴 CRITICAL**: Blocking issues, security vulnerabilities, data corruption
2. **🟠 HIGH**: Core functionality, user-facing features, performance issues
3. **🟡 MEDIUM**: Enhancements, optimizations, code quality improvements
4. **🟢 LOW**: Nice-to-have features, documentation, future planning

#### Development Phases
1. **Phase 1**: Core Infrastructure & Foundation
2. **Phase 2**: Primary Features & User Interface
3. **Phase 3**: Advanced Features & Integrations
4. **Phase 4**: Polish, Testing & Deployment

### Task Documentation Standards

#### Task Definition Format
```markdown
### Task Title
- **Priority**: 🔴/🟠/🟡/🟢
- **Status**: ⬜/🟨/✅/🔄/❌
- **Assigned Agent**: agent-name (in multi-agent mode)
- **Dependencies**: List of prerequisite tasks
- **Acceptance Criteria**: Clear success definition
- **Estimated Effort**: Hours/Days
- **Implementation Notes**: Technical details
```

#### Completion Verification
- Use task-completion-validator agent to verify completions before marking done
- Include verification steps in task definition
- Document actual vs estimated effort for future planning
- Update related documentation when tasks are completed

## TDD Integration & Workflow

### TDD Architecture Components

#### Core Services
1. **TestRunnerService** (`src/backend/services/TestRunnerService.ts`)
   - Core test execution engine
   - Jest integration with real-time output parsing
   - Agent-specific test running
   - Metrics and performance tracking

2. **TDD Debug Panel** (`src/frontend/components/DebugPanel/TDDDebugPanel.tsx`)
   - Real-time test results display
   - Coverage monitoring
   - Test execution controls
   - Performance metrics

3. **TDD Workflow Hook** (`src/frontend/hooks/useTDDWorkflow.ts`)
   - React hook for TDD state management
   - Real-time test result processing
   - Phase transition logic
   - Recommendation generation

4. **IPC Handlers** (`src/backend/ipc/testRunnerHandlers.ts`)
   - Bridge between frontend and backend
   - Real-time event streaming
   - Test execution coordination

### TDD Workflow Phases

#### 1. RED Phase - Write Failing Test

```typescript
// Example: Write a failing test first
describe('UserService', () => {
  it('should create a new user', async () => {
    const userService = new UserService();
    const userData = { name: 'John Doe', email: 'john@example.com' };

    const result = await userService.createUser(userData);

    expect(result.success).toBe(true);
    expect(result.user.id).toBeDefined();
    expect(result.user.name).toBe('John Doe');
  });
});
```

**Guidelines**:
- Write the smallest possible failing test
- Test should fail for the right reason
- Focus on the interface, not implementation
- One test at a time

#### 2. GREEN Phase - Make Test Pass

```typescript
// Example: Minimal implementation
class UserService {
  async createUser(userData: any): Promise<any> {
    // Minimal implementation to make test pass
    return {
      success: true,
      user: {
        id: Date.now().toString(),
        name: userData.name,
        email: userData.email
      }
    };
  }
}
```

**Guidelines**:
- Write minimal code to make test pass
- Don't worry about code quality yet
- Hardcode values if needed
- Just make it work

#### 3. REFACTOR Phase - Improve Code

```typescript
// Example: Improved implementation
import { v4 as uuidv4 } from 'uuid';

interface User {
  id: string;
  name: string;
  email: string;
  createdAt: Date;
}

interface CreateUserResult {
  success: boolean;
  user: User;
}

class UserService {
  async createUser(userData: { name: string; email: string }): Promise<CreateUserResult> {
    // Validate input
    if (!userData.name || !userData.email) {
      throw new Error('Name and email are required');
    }

    // Create user with proper typing
    const user: User = {
      id: uuidv4(),
      name: userData.name,
      email: userData.email,
      createdAt: new Date()
    };

    return {
      success: true,
      user
    };
  }
}
```

**Guidelines**:
- Improve code while keeping tests green
- Remove duplication
- Improve names and structure
- Run tests frequently

#### 4. COMMIT Phase - Save Changes

```bash
git add .
git commit -m "feat(user-service): implement user creation with validation

- Add UserService class with createUser method
- Include input validation and proper typing
- Generate UUID for user IDs
- Add comprehensive test coverage

🤖 Generated with [Claude Code](https://claude.ai/code)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

**Guidelines**:
- Commit with descriptive message
- Include agent prefix in commit (in multi-agent mode)
- Document what was implemented
- Push changes regularly

### Test Execution & Management

#### Command Line Usage

```bash
# Start TDD workflow
npm run tdd

# Run tests for specific agent (multi-agent mode)
npm run tdd -- agent backend-api

# Start watch mode
npm run tdd -- watch

# Complete guided TDD cycle
npm run tdd:cycle

# Generate TDD report
npm run tdd:report

# Individual phase commands
npm run tdd:red      # Start RED phase
npm run tdd:green    # Start GREEN phase
npm run tdd:refactor # Start REFACTOR phase
npm run tdd:commit   # Start COMMIT phase
```

#### Agent-Specific Testing (Multi-Agent Mode)

```bash
# Test backend API agent
npm run test:agent:backend

# Test frontend UI agent
npm run test:agent:frontend

# Test AI pipeline agent
npm run test:agent:ai

# Test database layer agent
npm run test:agent:database

# Test DevOps automation agent
npm run test:agent:devops
```

#### Coverage & Quality Gates

**Coverage Thresholds**:
```json
{
  "jest": {
    "coverageThreshold": {
      "global": {
        "branches": 80,
        "functions": 90,
        "lines": 90,
        "statements": 90
      }
    }
  }
}
```

**Quality Requirements**:
- 90% minimum test coverage
- <30 second test execution time
- Zero TypeScript compilation errors
- Zero ESLint warnings or errors

### Agent Test Patterns (Multi-Agent Mode)

- **Backend API**: `src/backend/**/*.test.ts`
- **Frontend UI**: `src/frontend/**/*.test.{ts,tsx}`
- **AI Pipeline**: `src/backend/services/ai/**/*.test.ts`
- **Database Layer**: `src/backend/models/**/*.test.ts`
- **Testing Suite**: `__tests__/**/*.test.{ts,tsx}`
- **DevOps**: `scripts/**/*.test.{js,ts}`

### Agent-Specific TDD Responsibilities (Multi-Agent Mode)

#### Backend API Agent:
- **MUST** write unit tests for all service methods before implementation
- **MUST** write integration tests for all IPC handlers before implementation
- **MUST** write API endpoint tests before route implementation
- **MUST** test error conditions and edge cases

#### Frontend Components Agent:
- **MUST** write component tests before creating components
- **MUST** write hook tests before implementing custom hooks
- **MUST** write interaction tests before adding user interactions
- **MUST** test component states and props validation

#### AI Pipeline Agent:
- **MUST** write tests for AI service functions before implementation
- **MUST** write tests for card recognition logic before implementation
- **MUST** write tests for data transformation before implementation
- **MUST** test with mock data and error scenarios

#### Database Layer Agent:
- **MUST** write model tests before creating database models
- **MUST** write migration tests before writing migrations
- **MUST** write query tests before implementing complex queries
- **MUST** test data validation and constraints

#### Testing Suite Agent:
- **MUST** create test infrastructure before other agents need it
- **MUST** write test utilities before other agents implement features
- **MUST** write cross-agent integration tests
- **MUST** maintain test coverage reports

#### DevOps Automation Agent:
- **MUST** write tests for deployment scripts before implementation
- **MUST** write tests for monitoring functions before implementation
- **MUST** write tests for health checks before implementation
- **MUST** test CI/CD pipeline components

## Development Best Practices

### Task Management Best Practices

1. **Clear Definition**: Every task must have clear acceptance criteria
2. **Dependency Tracking**: Document prerequisites and blockers
3. **Regular Updates**: Update status immediately after completion
4. **Verification**: Use validator agents to verify completions
5. **Documentation**: Update related documentation when tasks are completed

### TDD Best Practices

1. **Write Tests First**: Always write failing tests before implementation
2. **Small Steps**: Make incremental changes and test frequently
3. **Clear Names**: Use descriptive test and function names
4. **Single Responsibility**: One test should test one thing
5. **Fast Tests**: Keep tests quick to maintain feedback loop

### Code Quality Standards

1. **Coverage Goals**: Maintain 90%+ test coverage
2. **Type Safety**: Use TypeScript strictly
3. **Error Handling**: Test error conditions and edge cases
4. **Performance**: Monitor test execution time
5. **Maintainability**: Keep tests simple and readable

### Workflow Management

1. **Phase Discipline**: Complete each phase before moving to next
2. **Frequent Commits**: Commit after each successful cycle
3. **Continuous Integration**: Run tests before every commit
4. **Agent Coordination**: Test cross-agent interactions (multi-agent mode)
5. **Documentation**: Update docs with significant changes

## Integration with Multi-Agent System

### Multi-Agent Coordination

The TDD system integrates with the multi-agent development workflow:

1. **Agent Assignment**: Each agent has specific test responsibilities
2. **Territory Management**: Tests are organized by agent domains
3. **Integration Testing**: Cross-agent interaction testing
4. **Status Coordination**: Test results affect agent status

### TDD Communication Protocol (Multi-Agent Mode)

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

### Cross-Agent Integration Testing Requirements

- **Backend API Agent** writes integration tests before implementing IPC handlers
- **Frontend Components Agent** writes component integration tests before implementing data consumption
- **AI Pipeline Agent** writes service integration tests before implementing pipeline connections
- **Database Layer Agent** writes data persistence tests before implementing schemas
- **Testing Suite Agent** writes cross-agent workflow tests before integration points are built

## Metrics and Monitoring

### Performance Metrics

- **Test Execution Time**: Track test suite performance
- **Coverage Percentage**: Monitor code coverage trends
- **Failure Rate**: Track test stability over time
- **Agent Performance**: Compare test execution across agents (multi-agent mode)

### Quality Metrics

- **Test Count**: Total number of tests by agent
- **Coverage Distribution**: Coverage by file and agent
- **Failure Patterns**: Common failure points
- **Refactoring Impact**: Code quality improvements

### Task Management Metrics

- **Completion Velocity**: Tasks completed per sprint
- **Estimation Accuracy**: Actual vs estimated effort
- **Blocking Issues**: Frequency and resolution time
- **Phase Distribution**: Time spent in each development phase

## Troubleshooting

### Common TDD Issues

1. **Tests Not Running**
   - Check Node.js version (requires 20.19.0+)
   - Verify Jest installation: `npm install jest`
   - Check file permissions on test scripts

2. **Coverage Below Threshold**
   - Review uncovered lines in coverage report
   - Add tests for missing branches
   - Remove or ignore non-testable code

3. **Watch Mode Not Working**
   - Check file system permissions
   - Verify Git is initialized in project
   - Try manual test run first

### Debug Commands

```bash
# Check test runner status
npm run tdd -- --help

# Run with verbose output
npm run tdd -- run --verbose

# Generate detailed coverage report
npm run test:coverage

# Check specific agent tests (multi-agent mode)
npm run test:agent:backend --verbose

# Debug test parsing
npm run tdd:report
```

### Task Management Issues

1. **Unclear Requirements**: Break down tasks into smaller, more specific items
2. **Blocked Dependencies**: Identify and resolve prerequisite issues
3. **Estimation Inaccuracy**: Use historical data to improve future estimates
4. **Status Tracking**: Implement automated status updates where possible

## Future Enhancements

### Planned TDD Features

1. **AI-Powered Test Generation**: Automatic test creation
2. **Visual Test Results**: Enhanced UI for test visualization
3. **Performance Profiling**: Detailed test performance analysis
4. **Mutation Testing**: Code quality validation through mutations

### Task Management Enhancements

1. **Automated Estimation**: AI-powered effort estimation
2. **Dependency Analysis**: Visual dependency mapping
3. **Progress Analytics**: Advanced completion trend analysis
4. **Integration Hooks**: Automated task updates from code changes

---

**Version 1.0.0 - Integrated Development Workflow Framework**

*This comprehensive guide combines task management patterns with TDD integration for Claude Framework projects, supporting both single-developer and multi-agent development modes.*