# Template Variables Reference

**Last Updated**: 2025-01-15

## Overview

This document lists all template variables in `CLAUDE.template.md` that must be replaced when injecting the framework into a new project. Variables make the framework architecture-agnostic and adaptable to any tech stack.

## Core Project Variables

### Required Variables

**`{{PROJECT_NAME}}`** - Project name
- Example: `"My Awesome App"`, `"E-Commerce Platform"`

**`{{PROJECT_IDENTIFIER}}`** - Unique canary phrase to verify CLAUDE.md loaded correctly
- Example: `"my-app-2025"`, `"ecommerce-platform-v2"`
- Purpose: Context verification

**`{{PROJECT_DESCRIPTION}}`** - Brief project description
- Example: `"Desktop application for inventory management with real-time sync"`
- Length: 1-2 sentences

**`{{VERSION}}`** - Project version
- Example: `"1.0.0"`, `"2.3.1"`
- Format: Semantic versioning

**`{{SPRINT_FOCUS}}`** - Current sprint or development focus
- Example: `"Authentication system implementation"`, `"Performance optimization sprint"`
- Update regularly

**`{{DOMAIN_SPECIFIC_SECTION}}`** - Domain-specific section header
- Example: `"E-Commerce Domain"`, `"Healthcare Compliance"`, `"Game Development"`

**`{{DOMAIN_SPECIFIC_CONTENT}}`** - Domain-specific rules and context
- Example: HIPAA compliance rules, game design patterns, trading card game rules

**`{{DOMAIN_TERMS}}`** - Domain terminology to track
- Example: `"trading card game rules"`, `"medical terminology"`, `"financial regulations"`

## Build & Development Variables

**`{{START_COMMAND}}`** - Command to start application
- Example: `"npm start"`, `"python manage.py runserver"`, `"cargo run"`

**`{{TEST_COMMAND}}`** - Command to run tests
- Example: `"npm test"`, `"pytest"`, `"cargo test"`

**`{{BUILD_TOOL}}`** - Build tool name (npm, yarn, pnpm, cargo, etc.)
- Example: `"npm"`, `"yarn"`, `"pnpm"`, `"cargo"`
- Used in: Clean restart commands

**`{{APP_LIFECYCLE_COMMANDS}}`** - Application lifecycle commands
- Example: `"npm start/stop/restart"`, `"docker-compose up/down"`, `"systemctl start/stop"`

## Conditional Variables (Tech Stack)

### IPC/Communication Layer

**`{{IF_IPC}}`...`{{ENDIF_IPC}}`** - If project uses IPC (Inter-Process Communication)
- Use when: Electron apps, desktop applications with IPC bridge
- Remove if: Web apps with REST APIs only

**`{{IF_REST_API}}`...`{{ENDIF_REST_API}}`** - If project uses REST APIs
- Use when: Web applications, mobile apps, microservices
- Remove if: Desktop apps with IPC only

**`{{IF_GRAPHQL}}`...`{{ENDIF_GRAPHQL}}`** - If project uses GraphQL
- Use when: Projects using GraphQL APIs
- Remove if: REST or IPC only

### Frontend Technologies

**`{{IF_HOT_RELOAD}}`...`{{ENDIF_HOT_RELOAD}}`** - If project supports hot-reload
- Use when: React, Vue, Angular with HMR
- Remove if: Static sites, compiled binaries

**`{{HOT_RELOAD_TOOL}}`** - Hot reload tool name
- Example: `"Webpack HMR"`, `"Vite HMR"`, `"Fast Refresh"`

**`{{IF_ELECTRON}}`...`{{ENDIF_ELECTRON}}`** - If project is Electron app
- Use when: Desktop applications built with Electron
- Remove if: Web apps, mobile apps, CLI tools

### Backend Technologies

**`{{BACKEND_CHANGE_TYPES}}`** - Types of backend changes requiring restart
- Example: `"IPC handlers, services, database schema"`, `"API routes, middleware, database"`

**`{{IF_CACHE}}`...`{{ENDIF_CACHE}}`** - If project uses caching layer
- Use when: Redis, in-memory cache, CDN caching
- Remove if: No caching strategy

**`{{IF_CACHE_AND_IPC}}`...`{{ENDIF_CACHE_AND_IPC}}`** - If project uses BOTH cache and IPC
- Use when: Both conditions are true
- Remove if: Only one or neither

**`{{IF_CACHE_OR_IPC}}`...`{{ENDIF_CACHE_OR_IPC}}`** - If project uses cache OR IPC
- Use when: At least one condition is true
- Remove if: Neither condition applies

**`{{SYNC_GUIDE_PATH}}`** - Path to synchronization guide
- Example: `"cache-ipc-synchronization-guide.md"`, `"api-cache-sync.md"`

### Logging & Monitoring

**`{{IF_LOGGING_SERVICE}}`...`{{ENDIF_LOGGING_SERVICE}}`** - If project has dedicated logging service
- Use when: Custom logging abstraction exists
- Remove if: Using console.log or basic logging

**`{{IF_NO_LOGGING_SERVICE}}`...`{{ENDIF_NO_LOGGING_SERVICE}}`** - If project has NO dedicated logging service
- Use when: No custom logging abstraction
- Remove if: Dedicated logging service exists

**`{{LOGGING_SERVICE}}`** - Logging service name
- Example: `"loggingService"`, `"Logger"`, `"winston"`

## Multi-Instance Variables

**`{{INSTANCE_1_ROLE}}`** - Role description for Instance 1
- Example: `"Backend/Database Specialist"`, `"API Development"`

**`{{INSTANCE_1_FILES}}`** - File patterns Instance 1 owns
- Example: `"src/backend/**"`, `"src/api/**, src/database/**"`

**`{{INSTANCE_2_ROLE}}`** - Role description for Instance 2
- Example: `"Frontend/UI Developer"`, `"Mobile Application Development"`

**`{{INSTANCE_2_FILES}}`** - File patterns Instance 2 owns
- Example: `"src/frontend/**"`, `"src/mobile/**"`

## Architecture Variables

**`{{LAYER_SYNCHRONIZATION_PATTERN}}`** - Full-stack layer synchronization pattern
- Example IPC: `"Database → Backend Services → IPC Channels → Frontend Hooks → UI Components → Cache"`
- Example REST: `"Database → Backend Services → REST APIs → Frontend State → UI Components → Cache"`
- Example GraphQL: `"Database → Resolvers → GraphQL Schema → Apollo Client → React Components"`
- Example Microservices: `"Database → Domain Services → Message Queue → API Gateway → Clients"`

## Repository Management Variables

**`{{CACHE_PATTERN}}`** - Cache file pattern to ignore
- Example: `"data/cache/**"`, `".cache/**"`, `"tmp/cache/**"`

**`{{TEMP_UPLOADS}}`** - Temporary upload directory pattern
- Example: `"temp-uploads/"`, `"uploads/temp/"`, `".tmp/uploads/"`

**`{{CACHE_CLEANUP_COMMAND}}`** - Command to clean cache files
- Example: `"rm -rf data/cache/*"`, `"npm run cache:clean"`, `"redis-cli FLUSHALL"`

## Variable Replacement Strategy

### Step 1: Identify Tech Stack

Determine which conditional sections apply to your project:
- IPC vs REST API vs GraphQL
- Hot-reload support
- Caching layer
- Logging service
- Electron vs Web vs Mobile

### Step 2: Replace Core Variables

Replace all `{{VARIABLE}}` placeholders with project-specific values.

### Step 3: Handle Conditional Sections

**Option A: Keep Section** - Replace conditional tags with actual content
```markdown
# Before
{{IF_IPC}}See IPC guide{{ENDIF_IPC}}

# After (if using IPC)
See IPC guide
```

**Option B: Remove Section** - Delete entire conditional block
```markdown
# Before
{{IF_IPC}}See IPC guide{{ENDIF_IPC}}

# After (if NOT using IPC)
[removed entirely]
```

### Step 4: Remove Empty Bullets

After removing conditionals, clean up any resulting empty bullet points:
```markdown
# Before
- **Rule 1**
- {{IF_REMOVED}}**Rule 2**{{ENDIF_REMOVED}}
- **Rule 3**

# After
- **Rule 1**
- **Rule 3**
```

## Example Replacements

### Web Application (React + REST API)

```markdown
# Variables
PROJECT_NAME: "E-Commerce Platform"
PROJECT_IDENTIFIER: "ecommerce-2025"
START_COMMAND: "npm start"
BUILD_TOOL: "npm"
HOT_RELOAD_TOOL: "Webpack HMR"
BACKEND_CHANGE_TYPES: "API routes, middleware, database"
LAYER_SYNCHRONIZATION_PATTERN: "Database → Services → REST APIs → React State → Components → Cache"

# Conditionals
IF_REST_API: Keep
IF_HOT_RELOAD: Keep
IF_CACHE: Keep
IF_IPC: Remove
IF_ELECTRON: Remove
IF_LOGGING_SERVICE: Keep (custom logger exists)
```

### Desktop Application (Electron + IPC)

```markdown
# Variables
PROJECT_NAME: "Desktop Trading App"
PROJECT_IDENTIFIER: "trading-app-2025"
START_COMMAND: "npm start"
BUILD_TOOL: "npm"
HOT_RELOAD_TOOL: "Webpack HMR"
BACKEND_CHANGE_TYPES: "IPC handlers, services, database schema"
LAYER_SYNCHRONIZATION_PATTERN: "Database → Services → IPC → Frontend Hooks → Components → Cache"

# Conditionals
IF_IPC: Keep
IF_HOT_RELOAD: Keep
IF_ELECTRON: Keep
IF_CACHE: Keep
IF_REST_API: Remove
IF_LOGGING_SERVICE: Keep
```

### CLI Tool (Python + No Frontend)

```markdown
# Variables
PROJECT_NAME: "Data Processing CLI"
PROJECT_IDENTIFIER: "data-cli-2025"
START_COMMAND: "python main.py"
BUILD_TOOL: "pip"
BACKEND_CHANGE_TYPES: "modules, configuration"
LAYER_SYNCHRONIZATION_PATTERN: "Database → Processing Pipeline → Output"

# Conditionals
IF_IPC: Remove
IF_REST_API: Remove
IF_HOT_RELOAD: Remove
IF_ELECTRON: Remove
IF_CACHE: Remove
IF_LOGGING_SERVICE: Keep (using Python logging)
```

## Validation

After replacement, verify:
1. No `{{` or `}}` remain in CLAUDE.md
2. No empty bullet points or orphaned text
3. All commands are valid for your tech stack
4. Context verification canary phrase is unique
5. Layer synchronization pattern matches your architecture

## Related Documentation

- [Framework Injection Guide](./framework-injection-guide.md) - Complete injection process
- [Subagent Guide](./subagent-guide.md) - Subagent usage patterns
- [Planning Template](./planning-template.md) - Task planning format

---

**Navigation**: [Framework Documentation](../README.md) | [Injection Guide](./framework-injection-guide.md)
