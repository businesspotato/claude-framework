# MCP Usage Guide - One Piece TCG Manager

**Model Context Protocol (MCP)** integration guide for development workflow optimization.

## Active MCP Servers

Your project uses multiple specialized MCP servers for enhanced development capabilities:

### Primary MCP Servers

1. **mcp__zen__*** - Advanced reasoning and analysis tools
2. **mcp__serena__*** - Code analysis and project management
3. **mcp__knowledge-graph__*** - Memory and knowledge management
4. **mcp__sequential-thinking__*** - Multi-step problem solving
5. **mcp__fetch__*** - Web content and image retrieval
6. **mcp__Ref__*** - Documentation search and retrieval
7. **mcp__figma__*** - Design system integration
8. **mcp__ide__*** - IDE integration tools
9. **mcp__memory-bank__*** - Memory management
10. **mcp__mcp-compass__*** - MCP server discovery

## Documentation Strategy: Ref-First Approach

**Preferred Method**: Use `mcp__Ref__ref_search_documentation` to retrieve live documentation rather than maintaining static copies.

### Ref Usage Patterns

#### 1. Framework/Library Documentation
```typescript
// Search for specific technology docs
mcp__Ref__ref_search_documentation({
  query: "React TypeScript Material-UI components best practices"
})

// API-specific searches
mcp__Ref__ref_search_documentation({
  query: "Electron IPC communication patterns security"
})
```

#### 2. Project-Specific Research
```typescript
// AI/ML documentation
mcp__Ref__ref_search_documentation({
  query: "Azure Computer Vision API OCR text extraction Python"
})

// API integration docs
mcp__Ref__ref_search_documentation({
  query: "eBay API OAuth 2.0 authentication Node.js"
})
```

#### 3. Reading Retrieved Documentation
```typescript
// After search, read full content
mcp__Ref__ref_read_url({
  url: "https://docs.microsoft.com/azure/cognitive-services/computer-vision/"
})
```

## MCP Servers for One Piece TCG Manager

### Development Workflow Integration

#### Code Analysis & Quality
- **mcp__serena__*** - Project-aware code analysis
  - `find_symbol` - Locate classes/functions
  - `search_for_pattern` - Code pattern searches
  - `replace_symbol_body` - Precise code modifications

#### Knowledge Management
- **mcp__knowledge-graph__*** - Entity relationships
  - `aim_create_entities` - Card/collection entities
  - `aim_search_nodes` - Query relationships
  - `aim_read_graph` - Full knowledge view

#### Advanced Reasoning
- **mcp__zen__*** - Complex problem solving and strategic planning
  - `thinkdeep` - Multi-step analysis and root cause investigation
  - `planner` - Interactive, sequential planning with revision capabilities
  - `consensus` - Multi-model consensus through structured debate
  - `debug` - Systematic debugging and root cause analysis
  - `codereview` - Step-by-step code review with expert validation
  - `precommit` - Git changes validation before committing
  - `chat` - Collaborative thinking partner for brainstorming

#### Memory & Context
- **mcp__memory-bank__*** - Session memory
- **mcp__sequential-thinking__*** - Chain of reasoning

### Project-Specific MCP Usage

#### 1. Card Recognition Pipeline
```typescript
// Research Azure AI documentation
mcp__Ref__ref_search_documentation({
  query: "Azure Computer Vision Custom Vision API card detection tutorial"
})

// Create knowledge entities for card types
mcp__knowledge-graph__aim_create_entities({
  entities: [{
    name: "OP12-041_Sanji",
    entityType: "card",
    observations: ["Special variant pricing issue", "Cache invalidation needed"]
  }]
})
```

#### 2. eBay API Integration
```typescript
// Get latest eBay API docs
mcp__Ref__ref_search_documentation({
  query: "eBay OAuth 2.0 marketplace API Node.js SDK 2025"
})

// Plan integration approach
mcp__zen__planner({
  step: "Design eBay API integration architecture for One Piece TCG price tracking",
  model: "o3"
})
```

#### 3. Database Optimization
```typescript
// Research SQLite optimization
mcp__Ref__ref_search_documentation({
  query: "SQLite performance optimization indexing strategies Node.js"
})

// Analyze current database patterns
mcp__serena__search_for_pattern({
  substring_pattern: "CREATE TABLE|CREATE INDEX",
  relative_path: "data/database"
})
```

#### 4. Frontend Component Development
```typescript
// Get Material-UI documentation
mcp__Ref__ref_search_documentation({
  query: "Material-UI React TypeScript card components grid layout"
})

// Find existing component patterns
mcp__serena__find_symbol({
  name_path: "CollectionDisplay",
  relative_path: "src/frontend/components"
})
```

## 🧠 Zen MCP: Advanced Planning & Analysis

### Overview
Zen MCP provides sophisticated reasoning capabilities using ChatGPT-5 models for complex problem-solving, strategic planning, and deep analysis. It's designed for tasks requiring multi-step thinking, expert-level analysis, and collaborative reasoning.

### Core Zen Functions

#### 1. mcp__zen__planner
**Purpose**: Breaks down complex tasks through interactive, sequential planning with revision and branching capabilities.

**When to Use**:
- Complex project planning requiring multiple phases
- System design and architecture decisions
- Migration strategies and rollout planning
- Feature development with uncertain scope

**Key Parameters**:
- `step`: Current planning step content
- `step_number`: Current step in sequence (starts at 1)
- `total_steps`: Estimated total steps needed
- `next_step_required`: Whether another step is needed
- `model`: GPT model to use (o3, o3-mini, gpt-5, etc.)

**Example Usage**:
```typescript
// Initial planning step
mcp__zen__planner({
  step: "Design new real-time price tracking system for One Piece TCG Manager",
  step_number: 1,
  total_steps: 5,
  next_step_required: true,
  model: "o3"
})

// Follow-up step with revision
mcp__zen__planner({
  step: "Revised architecture based on performance constraints",
  step_number: 2,
  total_steps: 6,
  next_step_required: true,
  is_step_revision: true,
  revises_step_number: 1,
  model: "o3"
})
```

#### 2. mcp__zen__thinkdeep
**Purpose**: Performs multi-stage investigation and reasoning for complex problem analysis with systematic hypothesis testing.

**When to Use**:
- Architecture decisions requiring deep analysis
- Complex bugs or performance challenges
- Security analysis and vulnerability assessment
- Root cause analysis of system issues

**Key Parameters**:
- `step`: Investigation step content and findings
- `step_number`: Current step index (starts at 1)
- `findings`: Important discoveries and evidence
- `hypothesis`: Current theory about the issue/goal
- `confidence`: Confidence level (exploring → certain)
- `relevant_files`: Files relevant to the analysis

**Example Usage**:
```typescript
// Deep investigation of performance issue
mcp__zen__thinkdeep({
  step: "Analyze pricing system performance bottleneck affecting UI responsiveness",
  step_number: 1,
  total_steps: 4,
  next_step_required: true,
  findings: "Initial investigation shows 500ms+ query times on unified_prices table",
  hypothesis: "Missing database indexes on frequently queried columns",
  confidence: "medium",
  relevant_files: ["/src/backend/services/pricing/UnifiedPricingService.ts"],
  model: "o3"
})
```

#### 3. mcp__zen__consensus
**Purpose**: Builds multi-model consensus through systematic analysis and structured debate for complex decisions.

**When to Use**:
- Technology evaluation and selection
- Architectural approach comparison
- Feature design decisions with trade-offs
- Implementation strategy validation

**Key Parameters**:
- `step`: Core question for consensus evaluation
- `models`: Array of model configurations with different stances
- `findings`: Your analysis of the consensus topic
- `step_number`: Current step in consensus workflow

**Example Usage**:
```typescript
// Technology decision with multiple perspectives
mcp__zen__consensus({
  step: "Evaluate eBay API vs PriceCharting API for primary pricing source",
  models: [
    { model: "o3", stance: "for" },
    { model: "o3", stance: "against" },
    { model: "gpt-5", stance: "neutral" }
  ],
  findings: "Need to balance data accuracy, rate limits, and cost considerations",
  step_number: 1,
  total_steps: 2,
  next_step_required: true
})
```

#### 4. mcp__zen__debug
**Purpose**: Systematic debugging and root cause analysis for any type of issue with structured investigation.

**When to Use**:
- Complex bugs with unclear causes
- Mysterious errors or race conditions
- Performance issues and memory leaks
- Integration problems between services

**Key Parameters**:
- `step`: Investigation plan and findings
- `hypothesis`: Concrete root cause theory
- `confidence`: Level of certainty in the hypothesis
- `relevant_files`: Files directly relevant to the issue

**Example Usage**:
```typescript
// Debug Sanji pricing inconsistency
mcp__zen__debug({
  step: "Investigate why Sanji OP12-041 shows $0.38 instead of $22.50 across different pages",
  step_number: 1,
  total_steps: 3,
  next_step_required: true,
  findings: "Price inconsistency appears related to cache synchronization issues",
  hypothesis: "Pricing data scattered across multiple tables causing stale cache reads",
  confidence: "high",
  relevant_files: [
    "/src/backend/services/pricing/UnifiedPricingService.ts",
    "/src/frontend/components/Upload/VariantSelector.tsx"
  ],
  model: "o3"
})
```

#### 5. mcp__zen__codereview
**Purpose**: Systematic code review covering quality, security, performance, and architecture with expert validation.

**When to Use**:
- Pre-merge code quality assessment
- Security vulnerability scanning
- Performance optimization review
- Architecture compliance validation

**Example Usage**:
```typescript
mcp__zen__codereview({
  step: "Review eBay API integration implementation for security and performance",
  step_number: 1,
  total_steps: 2,
  next_step_required: true,
  findings: "Need to assess authentication handling and rate limiting implementation",
  relevant_files: ["/src/backend/services/api/eBayService.ts"],
  review_type: "full",
  model: "o3"
})
```

#### 6. mcp__zen__precommit
**Purpose**: Validates git changes and repository state before committing with systematic analysis.

**When to Use**:
- Before committing significant changes
- Multi-repository validation workflows
- Security review of changes
- Change impact assessment

**Example Usage**:
```typescript
mcp__zen__precommit({
  step: "Validate eBay integration changes before commit",
  step_number: 1,
  total_steps: 3,
  next_step_required: true,
  findings: "Changes include new API service, database migrations, and IPC handlers",
  path: "/Users/user/Projects/OP TCG/one-piece-tcg-manager",
  include_staged: true,
  include_unstaged: false,
  model: "o3"
})
```

#### 7. mcp__zen__chat
**Purpose**: General chat and collaborative thinking partner for brainstorming and development discussion.

**When to Use**:
- Brainstorming solutions and approaches
- Getting second opinions on technical decisions
- Exploring ideas and validating approaches
- General development consultation

**Example Usage**:
```typescript
mcp__zen__chat({
  prompt: "I'm implementing a pricing matrix that needs to handle 8,972 entries across multiple APIs. What are the key performance considerations I should think about?",
  model: "o3",
  files: ["/src/backend/services/pricing/UnifiedPricingService.ts"],
  use_websearch: true
})
```

### Zen Planning Integration Patterns

#### Pattern 1: Architecture Decision Records (ADR)
```typescript
// 1. Use Zen to analyze options
mcp__zen__consensus({
  step: "Evaluate database schemas for pricing matrix expansion",
  models: [
    { model: "o3", stance: "for" },
    { model: "gpt-5", stance: "against" }
  ]
})

// 2. Capture decision in ADR
// Save output to docs/adr/YYYY-MM-DD-pricing-schema.md
```

#### Pattern 2: Complex Feature Development
```typescript
// 1. Initial planning with Zen
mcp__zen__planner({
  step: "Plan eBay API integration for pricing system",
  model: "o3"
})

// 2. Deep analysis of implementation
mcp__zen__thinkdeep({
  step: "Analyze technical challenges and constraints",
  model: "o3"
})

// 3. Implementation validation
mcp__zen__codereview({
  step: "Review implementation quality and security",
  model: "o3"
})
```

#### Pattern 3: Performance Optimization
```typescript
// 1. Identify bottlenecks
mcp__zen__debug({
  step: "Investigate slow pricing queries affecting UI",
  model: "o3"
})

// 2. Analyze optimization approaches
mcp__zen__thinkdeep({
  step: "Evaluate caching vs indexing vs query optimization",
  model: "o3"
})

// 3. Validate solution
mcp__zen__precommit({
  step: "Verify performance improvements don't break functionality",
  model: "o3"
})
```

### Model Selection Guidelines

**For Planning & Architecture:**
- `o3`: Complex architectural decisions, systematic analysis
- `gpt-5`: Advanced reasoning with large context windows
- `o3-mini`: Quick planning for moderate complexity

**For Deep Analysis:**
- `o3`: Root cause analysis, complex debugging
- `o3-pro`: Extremely complex problems (use sparingly - expensive)
- `gpt-5`: Large codebase analysis

**For Reviews & Validation:**
- `o3-mini`: Code review and validation
- `gpt-5`: Comprehensive security analysis

### Zen Best Practices

1. **Context Setting**: Always provide relevant files and project constraints
2. **Incremental Steps**: Use step-by-step progression for complex problems
3. **Model Selection**: Choose appropriate model based on complexity
4. **Documentation**: Capture Zen insights in project documentation
5. **Validation**: Use multiple Zen functions for critical decisions

## MCP Integration Best Practices

### 1. Documentation-First Development
- Always use `mcp__Ref__ref_search_documentation` before implementing
- Read official docs with `mcp__Ref__ref_read_url` for accuracy
- Avoid maintaining static documentation copies

### 2. Knowledge Persistence
- Use `mcp__knowledge-graph__aim_create_entities` for project insights
- Store debugging solutions for recurring issues
- Maintain entity relationships for card collections

### 3. Complex Problem Solving
- Use `mcp__zen__thinkdeep` for architecture decisions
- Apply `mcp__sequential-thinking__sequentialthinking` for multi-step processes
- Leverage `mcp__zen__consensus` for technology choices

### 4. Code Quality Workflows
- Use `mcp__serena__find_referencing_symbols` before refactoring
- Apply `mcp__serena__replace_symbol_body` for precise changes
- Validate with `mcp__ide__getDiagnostics` integration

## Common MCP Workflows

### Research → Plan → Implement → Validate

#### 1. Research Phase
```typescript
mcp__Ref__ref_search_documentation({
  query: "[specific technology] [specific problem] documentation"
})
```

#### 2. Planning Phase
```typescript
mcp__zen__planner({
  step: "Plan implementation of [feature]",
  model: "o3"
})
```

#### 3. Implementation Phase
```typescript
mcp__serena__find_symbol({ name_path: "[target]" })
mcp__serena__replace_symbol_body({ /* implementation */ })
```

#### 4. Validation Phase
```typescript
mcp__ide__getDiagnostics()  // Check for errors
mcp__zen__debug()           // If issues found
```

## Quick Reference Commands

### Documentation Lookup
- `mcp__Ref__ref_search_documentation` - Search across docs
- `mcp__Ref__ref_read_url` - Read specific documentation pages

### Code Analysis
- `mcp__serena__get_symbols_overview` - File structure overview
- `mcp__serena__find_symbol` - Locate specific code elements
- `mcp__serena__search_for_pattern` - Pattern-based searches

### Problem Solving & Planning
- `mcp__zen__planner` - Interactive sequential planning
- `mcp__zen__thinkdeep` - Deep multi-stage analysis
- `mcp__zen__debug` - Systematic debugging and root cause analysis
- `mcp__zen__consensus` - Multi-model consensus building
- `mcp__zen__codereview` - Comprehensive code review
- `mcp__zen__precommit` - Git changes validation
- `mcp__zen__chat` - Collaborative brainstorming
- `mcp__sequential-thinking__sequentialthinking` - Step-by-step reasoning

### Knowledge Management
- `mcp__knowledge-graph__aim_search_nodes` - Query stored knowledge
- `mcp__knowledge-graph__aim_create_entities` - Store new insights
- `mcp__memory-bank__process-memory-bank-request` - Memory operations

---

**Philosophy**: Use MCP servers to enhance development velocity while maintaining code quality. Prefer live documentation over static copies, and leverage specialized reasoning tools for complex decisions.

*See [MCP Official Docs](https://modelcontextprotocol.io/docs/getting-started/intro) via Ref for latest protocol updates.*