# Quick Setup Guide: Multi-Instance Coordination Framework

## Getting Started

This framework enables autonomous coordination between multiple Claude Code instances working on the same project. Follow these steps to set up multi-instance coordination for your project.

## Step 1: Choose Your Architecture Pattern

Select the template that best matches your project:

### Available Templates

1. **Web Application** (`architecture-patterns/web-application.yaml`)
   - Frontend + Backend API architecture
   - React/Vue/Angular + Express/FastAPI/Django
   - 3-instance setup: Backend, Frontend, Coordinator

2. **Microservices** (`architecture-patterns/microservices.yaml`)
   - Distributed microservices architecture
   - Service mesh, API gateway, container orchestration
   - 4-instance setup: Services, Gateway, Infrastructure, Coordinator

3. **Mobile Application** (`architecture-patterns/mobile-application.yaml`)
   - React Native, Flutter, or native development
   - Cross-platform mobile app development
   - 3-instance setup: Mobile UI, App Logic, Coordinator

4. **Custom** (`multi-instance-setup-template.yaml`)
   - Generic template for any architecture
   - Fully customizable instance roles and verification

## Step 2: Customize Your Configuration

### Option A: Use Architecture Template (Recommended)

1. Copy your chosen architecture template:
   ```bash
   cp claude-framework/templates/architecture-patterns/web-application.yaml your-project-config.yaml
   ```

2. Replace template variables with your project values:
   ```yaml
   project_config:
     name: "Your Project Name"  # Replace {{PROJECT_NAME}}
     architecture_type: "web_app"
   ```

### Option B: Create Custom Configuration

1. Copy the generic template:
   ```bash
   cp claude-framework/templates/multi-instance-setup-template.yaml your-project-config.yaml
   ```

2. Fill in all `{{VARIABLE}}` placeholders with your project-specific values

## Step 3: Set Up Project Structure

### Required Directories

Create these directories in your project root:

```bash
mkdir -p docs/verification-failures/failure-templates
mkdir -p docs/verification-failures/failure-history
mkdir -p scripts/verification
mkdir -p status
mkdir -p docs/instance-logs
```

### Copy Framework Files

1. **Copy verification script template**:
   ```bash
   cp claude-framework/templates/verification-script-template.js scripts/verification/project-verification.js
   ```

2. **Customize verification script** with your project's specific checks

3. **Create status tracking file**:
   ```bash
   echo '{}' > status/multi-instance-status.json
   ```

## Step 4: Initialize Status Tracking

### Create Initial Status File

Based on your configuration, create the status file structure:

```json
{
  "coordinationSession": {
    "sessionId": "session-your-project-$(date +%Y%m%d)",
    "coordinationMode": "THREE_INSTANCE_AUTONOMOUS",
    "totalInstances": 3,
    "currentPhase": "SETUP"
  },
  "autonomousCoordination": {
    "enabled": true,
    "coordinatorInstance": "instance-coordinator",
    "autoApprovalEnabled": true,
    "monitoringInterval": 30000
  },
  "instances": {},
  "handoffProtocol": {
    "currentHandoffPhase": "NONE"
  }
}
```

## Step 5: Configure Instance Roles

### Define Each Instance

For each instance in your setup, add to the status file:

```json
"instances": {
  "instance-backend": {
    "instanceId": "instance-backend",
    "role": "BACKEND_DEVELOPMENT",
    "status": "STANDBY",
    "specializations": ["API development", "Database operations"],
    "filesResponsible": ["src/api/**/*", "src/backend/**/*"],
    "readyForHandoff": false
  }
}
```

## Step 6: Create Verification Scripts

### Customize Verification Framework

1. **Edit** `scripts/verification/project-verification.js`
2. **Implement** your project-specific verification methods:
   - Database layer checks
   - API contract validation
   - UI component testing
   - Performance benchmarking

### Example Customization

```javascript
// Replace the template method with your actual verification
async checkApiLayer() {
  // Your API verification logic
  const response = await fetch('/api/health');
  return {
    success: response.ok,
    message: `API health check: ${response.status}`
  };
}
```

## Step 7: Test Your Setup

### Verify Configuration

1. **Test verification script**:
   ```bash
   node scripts/verification/project-verification.js
   ```

2. **Verify status file structure**:
   ```bash
   jq . status/multi-instance-status.json
   ```

### Run Initial Coordination Test

1. **Start with one instance** and verify it can update the status file
2. **Add second instance** and test handoff coordination
3. **Enable coordinator instance** for autonomous verification

## Step 8: Document Your Setup

### Create Project Documentation

1. **Copy coordination docs** to your project:
   ```bash
   cp claude-framework/docs/autonomous-multi-instance-framework.md docs/
   ```

2. **Document your instance roles** and responsibilities
3. **Create handoff checklists** specific to your project

### Update Project README

Add multi-instance coordination section to your README:

```markdown
## Multi-Instance Development

This project uses autonomous multi-instance coordination with:
- **Instance 1**: Backend development and API implementation
- **Instance 2**: Frontend development and UI components
- **Instance 3**: Integration coordination and verification

See `docs/autonomous-multi-instance-framework.md` for details.
```

## Common Patterns by Architecture

### Web Application Pattern

```yaml
instances:
  backend:     # Database, API, authentication
  frontend:    # Components, state, user interface
  coordinator: # Integration testing, performance
```

### Microservices Pattern

```yaml
instances:
  services:      # Individual microservice development
  gateway:       # API gateway, routing, security
  infrastructure: # Deployment, monitoring, orchestration
  coordinator:   # Cross-service integration
```

### Mobile Application Pattern

```yaml
instances:
  mobile_ui:   # Screens, components, navigation
  app_logic:   # State, business logic, API integration
  coordinator: # Cross-platform testing, app store compliance
```

## Best Practices

### Instance Coordination

1. **Clear boundaries**: No overlapping file responsibilities
2. **Regular status updates**: Update status every 30 minutes during active work
3. **Comprehensive handoffs**: Document all changes and verification requirements
4. **Emergency protocols**: Define escalation procedures for conflicts

### Verification Strategy

1. **Automate everything**: Prefer automated verification over manual checks
2. **Fast feedback**: Target <15 minute verification cycles
3. **Comprehensive coverage**: Test all integration points
4. **Clear remediation**: Provide specific fix guidance for failures

### Quality Assurance

1. **Fail fast**: Catch integration issues early
2. **Performance monitoring**: Include regression detection
3. **Security scanning**: Integrate security checks into verification
4. **Documentation**: Maintain clear audit trails

## Troubleshooting

### Common Issues

1. **Status file conflicts**: Use file locking or atomic updates
2. **Verification script failures**: Check dependencies and permissions
3. **Instance communication gaps**: Verify status file accessibility
4. **Performance bottlenecks**: Optimize verification script execution

### Getting Help

1. Check the framework documentation: `autonomous-multi-instance-framework.md`
2. Review architecture-specific templates for guidance
3. Examine verification failure logs in `docs/verification-failures/`
4. Test with minimal configuration first, then add complexity

---

*This framework provides the foundation for scalable, autonomous multi-instance development while maintaining quality through systematic verification.*