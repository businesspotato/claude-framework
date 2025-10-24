# CI/CD Templates

Quick-start CI/CD templates for Claude Code projects.

## 📁 Files

- `github-actions-template.yml` - Full-featured GitHub Actions workflow
- `package-scripts-reference.json` - Required npm scripts examples

## 🚀 Quick Start

### 1. Copy Template

```bash
mkdir -p .github/workflows
cp claude-framework/templates/ci-cd/github-actions-template.yml .github/workflows/ci.yml
```

### 2. Add Required Scripts to package.json

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "lint": "eslint 'src/**/*.{ts,tsx}'",
    "format:check": "prettier --check 'src/**/*.{ts,tsx,json,md}'",
    "test": "jest",
    "test:backend": "jest src/backend",
    "test:frontend": "jest src/frontend",
    "test:integration": "jest __tests__/integration",
    "test:e2e": "playwright test",
    "build": "webpack --mode production",
    "build:production": "NODE_ENV=production npm run build"
  }
}
```

### 3. Customize Workflow

Open `.github/workflows/ci.yml` and update:

- `NODE_VERSION` environment variable
- Branch triggers (`main`, `develop`, etc.)
- Test commands for your project structure
- Build steps for your framework
- Deployment configuration

### 4. Add GitHub Secrets

Go to **Settings → Secrets and variables → Actions** and add:

- `CODECOV_TOKEN` (optional, for code coverage)
- Deployment credentials (AWS, Vercel, etc.)
- External API keys (if needed for tests)

### 5. Push and Watch

```bash
git add .github/workflows/ci.yml package.json
git commit -m "ci: add GitHub Actions pipeline"
git push
```

Visit **Actions** tab to see your pipeline run! 🎉

## 📊 Pipeline Overview

```
┌─────────────────────────────────────────────────────┐
│                Security & Quality                    │
│  • npm audit    • TypeScript    • Linting           │
└────────────────┬────────────────────────────────────┘
                 │
        ┌────────┴────────┐
        │                 │
┌───────▼──────┐  ┌──────▼────────┐
│   Backend    │  │   Frontend    │
│    Tests     │  │     Tests     │
└───────┬──────┘  └──────┬────────┘
        │                │
        └────────┬───────┘
                 │
        ┌────────▼────────┐
        │  Integration &  │
        │   E2E Tests     │
        └────────┬────────┘
                 │
        ┌────────▼────────┐
        │  Cross-Platform │
        │     Builds      │
        │ Ubuntu/Win/Mac  │
        └────────┬────────┘
                 │
        ┌────────▼────────┐
        │   Deployment    │
        │  (main only)    │
        └─────────────────┘
```

## 🔧 Customization Guide

### For Different Frameworks

#### Next.js
```yaml
- name: Build
  run: npm run build
  env:
    NEXT_PUBLIC_API_URL: ${{ secrets.API_URL }}
```

#### Electron
```yaml
env:
  ELECTRON_CACHE: ${{ github.workspace }}/.cache/electron

- name: Package
  run: npm run package
```

#### React Native
```yaml
- name: Build Android
  run: cd android && ./gradlew assembleRelease

- name: Build iOS
  run: cd ios && xcodebuild -workspace App.xcworkspace -scheme App
```

### For Different Databases

#### PostgreSQL
```yaml
services:
  postgres:
    image: postgres:15
    env:
      POSTGRES_PASSWORD: test
```

#### MongoDB
```yaml
services:
  mongodb:
    image: mongo:6
    ports:
      - 27017:27017
```

#### Redis
```yaml
services:
  redis:
    image: redis:7
    ports:
      - 6379:6379
```

## 🎯 Features

✅ **Security Scanning**
- npm audit
- Dependency vulnerability checks
- Secret detection

✅ **Code Quality**
- TypeScript compilation
- ESLint checks
- Prettier formatting

✅ **Multi-Agent Testing**
- Backend tests
- Frontend tests
- Integration tests
- E2E tests with Playwright

✅ **Performance**
- Benchmark tracking
- Bundle size analysis
- Load testing

✅ **Cross-Platform Builds**
- Linux (Ubuntu)
- Windows
- macOS

✅ **Automated Deployment**
- Main branch deployment
- Tag-based releases
- Artifact generation

## 📚 Documentation

Full documentation: [ci-cd-setup-guide.md](../../docs/ci-cd-setup-guide.md)

### Key Topics

- [Required npm Scripts](../../docs/ci-cd-setup-guide.md#required-npm-scripts)
- [Environment Variables](../../docs/ci-cd-setup-guide.md#environment-variables--secrets)
- [Testing Strategies](../../docs/ci-cd-setup-guide.md#testing-strategies)
- [Deployment Config](../../docs/ci-cd-setup-guide.md#deployment-configuration)
- [Troubleshooting](../../docs/ci-cd-setup-guide.md#troubleshooting)

## 🆘 Common Issues

### Tests Pass Locally but Fail in CI

```yaml
# Add environment variables
- name: Run tests
  env:
    NODE_ENV: test
    CI: true
```

### Slow Build Times

```yaml
# Add caching
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('package-lock.json') }}
```

### Memory Issues

```yaml
# Increase Node memory
- run: NODE_OPTIONS="--max_old_space_size=4096" npm run build
```

## 💡 Best Practices

1. **Keep workflows fast** - Use caching, parallel jobs
2. **Fail fast** - Run quick checks first
3. **Secure secrets** - Use GitHub secrets, never commit
4. **Monitor performance** - Track build times, set budgets
5. **Document changes** - Explain custom workflows

## 🔗 Resources

- [GitHub Actions Docs](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Actions Marketplace](https://github.com/marketplace?type=actions)

---

**Ready to automate your development workflow!** 🚀
