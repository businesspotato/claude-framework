# Framework Injection Guide

**Last Updated**: 2025-01-15

## Overview

Step-by-step guide for injecting Claude Framework into a new project. This process sets up the optimized documentation structure, subagents, and development tools.

## Prerequisites

- Git repository initialized
- Node.js project (or equivalent for your stack)
- Claude Code installed

## Quick Injection (5 Minutes)

```bash
# 1. Clone framework into your project
cd your-project
git clone https://github.com/your-org/claude-framework.git

# 2. Run injection script
./claude-framework/scripts/inject-framework.sh

# 3. Configure your project
nano .claude-config.yaml

# 4. Remove framework source
rm -rf claude-framework/.git
```

## Manual Injection (Complete Control)

### Step 1: Copy Core Files

```bash
# Copy CLAUDE.md template
cp claude-framework/CLAUDE.template.md CLAUDE.md

# Copy documentation framework
cp -r claude-framework/docs/* docs/
mkdir -p docs/development docs/testing docs/troubleshooting

# Copy subagent configurations
cp -r claude-framework/.claude .claude

# Copy settings template
cp claude-framework/.claude/settings.template.json .claude/settings.local.json
```

### Step 2: Configure CLAUDE.md

**See [Template Variables Reference](./template-variables.md) for complete variable list and conditional handling**

Replace template variables in `CLAUDE.md`:

```bash
# Project identification
{{PROJECT_NAME}} → "Your Project Name"
{{PROJECT_IDENTIFIER}} → "your-project-2025"
{{PROJECT_DESCRIPTION}} → "Brief project description"
{{VERSION}} → "1.0.0"

# Commands
{{START_COMMAND}} → "npm start"
{{TEST_COMMAND}} → "npm test"

# Instance configuration
{{INSTANCE_1_ROLE}} → "Backend/Database"
{{INSTANCE_1_FILES}} → "src/backend/**, data/database/**"
{{INSTANCE_2_ROLE}} → "Frontend/UI"
{{INSTANCE_2_FILES}} → "src/frontend/**"

# Sprint and domain
{{SPRINT_FOCUS}} → "Current sprint objectives"
{{DOMAIN_SPECIFIC_SECTION}} → "Domain Reference"
{{DOMAIN_SPECIFIC_CONTENT}} → "Domain-specific guidelines"
{{DOMAIN_TERMS}} → "domain terminology"

# Cleanup patterns
{{CACHE_PATTERN}} → "data/cache/**/*.json"
{{TEMP_UPLOADS}} → "temp-uploads/"
{{CACHE_CLEANUP_COMMAND}} → "find data/cache/ -name '*.json' -delete"

# Test examples
{{TEST_CARD_EXAMPLE}} → "Representative test data"
```

### Step 3: Create Project-Specific Documentation

```bash
# Create docs/index.md
cat > docs/index.md << 'EOF'
# Documentation Index

**Your Project Name** - Brief description

## Quick Start

**Stack**: Your tech stack
**Port**: Your port
**Node.js**: Version

\`\`\`bash
npm start    # Start application
npm test     # Run tests
\`\`\`

## Environment Setup

Create `.env` file with configuration...

## Project Structure

\`\`\`
your-project/
├── src/
│   ├── backend/
│   └── frontend/
├── docs/
│   ├── index.md (this file)
│   └── development/
└── CLAUDE.md
\`\`\`
EOF
```

### Step 4: Set Up Development Tools

```bash
# Create scripts directory
mkdir -p scripts

# Copy git workflow scripts
cp claude-framework/scripts/checkpoint.sh scripts/
cp claude-framework/scripts/safe-merge.sh scripts/
chmod +x scripts/*.sh

# Create status tracking
mkdir -p status
echo '{}' > status/multi-instance-status.json

# Create multi-instance structure
mkdir -p docs/prompts
mkdir -p docs/instance-logs
mkdir -p docs/handoffs
```

### Step 5: Configure Settings

Edit `.claude/settings.local.json`:

```json
{
  "outputStyle": "Direct Objective",
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(printf \"rs\\n\")",
      "mcp__serena__*",
      "mcp__zen__*",
      "Task(*)",
      "Read({{PROJECT_PATH}}/**)"
    ]
  }
}
```

### Step 6: Initialize Documentation Structure

```bash
# Create development guides directory
mkdir -p docs/development
cp claude-framework/docs/mcp-query-optimization.md docs/development/
cp claude-framework/docs/subagent-guide.md docs/development/
cp claude-framework/docs/planning-enforcement.md docs/development/
cp claude-framework/docs/planning-template.md docs/development/

# Create project-specific guides (templates)
touch docs/development/multi-instance-guide.md
touch docs/development/repository-cleanup.md
touch docs/development/git-workflow-guide.md
touch docs/development/performance-tracking.md

# Create testing documentation
mkdir -p docs/testing
touch docs/testing/tdd-requirements.md
touch docs/testing/coverage-thresholds.md

# Create troubleshooting guides
mkdir -p docs/troubleshooting
touch docs/troubleshooting/known-issues.md
touch docs/troubleshooting/solutions.md
```

### Step 7: Create .gitignore

```bash
cat > .gitignore << 'EOF'
# Dependencies
node_modules/

# Build outputs
dist/
build/
.webpack/

# Temp files
tmp/
temp/
*.log

# IDE
.vscode/
.idea/
.warp/

# OS
.DS_Store
Thumbs.db

# Secrets
.env
.env.local

# Cache
.cache/
data/cache/**/*.json

# Framework source (after injection)
claude-framework/
EOF
```

## Configuration Templates

### Minimal Configuration

For simple projects, replace these key variables:

```bash
# In CLAUDE.md
PROJECT_NAME
START_COMMAND
TEST_COMMAND
PROJECT_IDENTIFIER
```

### Full Configuration

For complex projects with multi-instance coordination:

```bash
# All minimal variables plus:
INSTANCE_1_ROLE
INSTANCE_1_FILES
INSTANCE_2_ROLE
INSTANCE_2_FILES
SPRINT_FOCUS
DOMAIN_SPECIFIC_SECTION
DOMAIN_SPECIFIC_CONTENT
CACHE_PATTERN
TEMP_UPLOADS
CACHE_CLEANUP_COMMAND
```

## Verification

### Check Framework Installation

```bash
# Verify CLAUDE.md has no template variables
grep -E "{{.*}}" CLAUDE.md

# Verify subagents are present
ls .claude/agents/

# Verify documentation structure
tree docs/

# Verify settings configured
cat .claude/settings.local.json
```

### Test CLAUDE.md Loading

Start a new Claude Code conversation and check for:
1. Context verification canary phrase appears
2. No template variable errors
3. All documentation references resolve

## Post-Injection Tasks

### 1. Customize for Your Stack

**Web Application (React + Express)**:
```bash
# Update instance roles
INSTANCE_1: Backend/API Development
INSTANCE_2: Frontend/React Components
INSTANCE_3: Integration & DevOps

# Update file patterns
INSTANCE_1_FILES: src/api/**, src/backend/**
INSTANCE_2_FILES: src/components/**, src/pages/**
```

**Mobile Application (React Native)**:
```bash
INSTANCE_1: App Logic & State
INSTANCE_2: UI Components & Navigation
INSTANCE_3: Platform Integration (iOS/Android)

INSTANCE_1_FILES: src/store/**, src/hooks/**
INSTANCE_2_FILES: src/screens/**, src/components/**
```

**Microservices**:
```bash
INSTANCE_1: Service Development
INSTANCE_2: Gateway & Infrastructure
INSTANCE_3: Monitoring & Orchestration

INSTANCE_1_FILES: services/**
INSTANCE_2_FILES: gateway/**, infrastructure/**
```

### 2. Add Project-Specific Rules

Add to CRITICAL RULES section in CLAUDE.md:

```markdown
- **Your Rule**: Description of project-specific requirement
- **Performance Target**: Your specific budgets
- **Domain Knowledge**: Links to domain-specific documentation
```

### 3. Configure Performance Budgets

Edit `docs/development/performance-tracking.md`:

```markdown
## Performance Budgets

| Metric | Budget | Measurement Point |
|--------|--------|-------------------|
| Cold Start | <YOUR_TARGET>s | Application launch |
| API Response | <YOUR_TARGET>ms | p95 endpoint latency |
| Page Load | <YOUR_TARGET>ms | First contentful paint |
```

### 4. Set Up Multi-Instance Coordination

If using multi-instance mode:

```bash
# Create instance onboarding
cp claude-framework/templates/instance-onboarding-template.md docs/prompts/multi-instance-onboarding.md

# Configure status tracking
nano status/multi-instance-status.json
```

### 5. Document Your Architecture

Create `docs/project/architecture.md`:

```markdown
# Architecture

## System Overview
[Your system architecture]

## Component Relationships
[Your components and their interactions]

## Data Flow
[How data flows through your system]
```

## Maintenance

### Updating Framework

```bash
# Pull latest framework changes
git subtree pull --prefix claude-framework https://github.com/your-org/claude-framework.git main --squash

# Review and merge documentation updates
git diff claude-framework/docs/
```

### Framework Version Tracking

Add to CLAUDE.md bottom:

```markdown
---

**Framework Version**: 2.0.0
**Project Version**: 1.0.0
**Last Framework Update**: 2025-01-15
```

## Troubleshooting

### Template Variables Still Present

```bash
# Find remaining variables
grep -r "{{.*}}" CLAUDE.md docs/

# Replace globally
sed -i 's/{{PROJECT_NAME}}/YourProject/g' CLAUDE.md
```

### Missing Documentation Files

```bash
# Check required files
ls docs/index.md
ls docs/development/subagent-guide.md
ls .claude/agents/

# Recreate if missing
cp claude-framework/docs/subagent-guide.md docs/development/
```

### Subagents Not Working

```bash
# Verify .claude directory
ls -la .claude/agents/

# Check settings.local.json
cat .claude/settings.local.json | jq '.permissions.allow'
```

## Migration from Old Projects

### From Old CLAUDE.md (10,000+ lines)

```bash
# 1. Backup existing
cp CLAUDE.md CLAUDE.md.backup

# 2. Extract project-specific content
grep -v "^- \*\*" CLAUDE.md > project-specific.txt

# 3. Use new template
cp claude-framework/CLAUDE.template.md CLAUDE.md

# 4. Merge project-specific content
# Edit CLAUDE.md and add content to DOMAIN_SPECIFIC_SECTION
```

### From No Framework

```bash
# Full injection process
git clone https://github.com/your-org/claude-framework.git
./claude-framework/scripts/inject-framework.sh --full
```

## Best Practices

1. **Start Minimal**: Begin with basic template, add complexity as needed
2. **Document Early**: Fill in project-specific docs before first sprint
3. **Version Framework**: Track framework version for updates
4. **Test Loading**: Always verify CLAUDE.md loads correctly in new conversations
5. **Progressive Enhancement**: Add features (multi-instance, performance tracking) incrementally

## Related Documentation

- **[CLAUDE.template.md](../CLAUDE.template.md)** - Template with all variables
- **[Framework Structure](../README.md#framework-structure)** - Complete framework organization
- **[Quick Setup Guide](../templates/quick-setup-guide.md)** - Automated setup
- **[Multi-Instance Coordination](./multi-instance-coordination.md)** - Multi-instance setup

---

**Navigation**: [Claude Framework](../README.md) | [Documentation Index](./README.md)
