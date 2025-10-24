# CI/CD Setup Guide

**Version:** 1.0.0
**Last Updated:** 2025-10-24

## Overview

This guide explains how to set up CI/CD pipelines for projects using the Claude Code framework. The provided templates support GitHub Actions with multi-agent parallel testing, cross-platform builds, and automated deployment.

## Table of Contents

1. [Quick Start](#quick-start)
2. [Template Structure](#template-structure)
3. [Customization](#customization)
4. [Required npm Scripts](#required-npm-scripts)
5. [Environment Variables & Secrets](#environment-variables--secrets)
6. [Testing Strategies](#testing-strategies)
7. [Deployment Configuration](#deployment-configuration)
8. [Troubleshooting](#troubleshooting)

---

## Quick Start

### 1. Copy Template to Your Project

```bash
# Create workflows directory
mkdir -p .github/workflows

# Copy the CI/CD template
cp claude-framework/templates/ci-cd/github-actions-template.yml \
   .github/workflows/ci.yml
```

### 2. Customize for Your Project

Open `.github/workflows/ci.yml` and update:

```yaml
env:
  NODE_VERSION: '20.x'  # Match your Node.js version
```

### 3. Add Required npm Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit",
    "lint": "eslint 'src/**/*.{ts,tsx}' --max-warnings 0",
    "format:check": "prettier --check 'src/**/*.{ts,tsx,json,md}'",
    "test": "jest",
    "test:backend": "jest src/backend/**/*.test.ts",
    "test:frontend": "jest src/frontend/**/*.test.ts",
    "test:integration": "jest __tests__/integration/**/*.test.ts",
    "test:e2e": "playwright test",
    "build": "webpack --mode production",
    "build:production": "NODE_ENV=production npm run build"
  }
}
```

### 4. Commit and Push

```bash
git add .github/workflows/ci.yml package.json
git commit -m "ci: add GitHub Actions CI/CD pipeline"
git push
```

Your CI/CD pipeline will now run on every push and pull request! 🎉

---

## Template Structure

The CI/CD template includes these stages:

### 1. Security Scanning

- **npm audit**: Checks for vulnerable dependencies
- **Anchore scan**: Deep dependency vulnerability analysis
- **Secret detection**: Scans commit history for leaked credentials

### 2. Code Quality

- **TypeScript compilation**: Ensures type safety across codebase
- **Linting**: Enforces code style and catches errors
- **Formatting**: Validates consistent code formatting

### 3. Multi-Agent Testing

- **Backend tests**: API, services, database layers
- **Frontend tests**: Components, hooks, utilities
- **Integration tests**: Cross-layer data flow
- **E2E tests**: Full user workflows with Playwright

### 4. Performance Benchmarking

- **Load tests**: API endpoint performance
- **Memory profiling**: Memory leak detection
- **Bundle analysis**: Build size tracking

### 5. Cross-Platform Builds

- **Ubuntu**: Linux builds
- **Windows**: Windows builds
- **macOS**: macOS builds

### 6. Deployment

- **Production deployment**: Main branch only
- **Release creation**: Automated artifact generation
- **GitHub releases**: Tag-based releases

### 7. Validation Reporting

- **Status summary**: All job results
- **Deployment readiness**: Pass/fail determination
- **Artifact uploads**: Test results, coverage, builds

---

## Customization

### Framework-Specific Adjustments

#### React/Next.js

```yaml
- name: Build application
  run: npm run build
  env:
    NEXT_PUBLIC_API_URL: ${{ secrets.API_URL }}
```

#### Vue/Nuxt

```yaml
- name: Build application
  run: npm run build
  env:
    NUXT_PUBLIC_API_URL: ${{ secrets.API_URL }}
```

#### Angular

```yaml
- name: Build application
  run: npm run build -- --configuration production
```

#### Electron

```yaml
env:
  ELECTRON_CACHE: ${{ github.workspace }}/.cache/electron
  ELECTRON_BUILDER_CACHE: ${{ github.workspace }}/.cache/electron-builder

- name: Setup build cache
  uses: actions/cache@v3
  with:
    path: |
      ${{ env.ELECTRON_CACHE }}
      ${{ env.ELECTRON_BUILDER_CACHE }}
    key: ${{ runner.os }}-electron-${{ hashFiles('package-lock.json') }}
```

### Database-Specific Setup

#### SQLite

```yaml
- name: Setup test database
  run: |
    npm run db:migrate:test
```

#### PostgreSQL

```yaml
services:
  postgres:
    image: postgres:15
    env:
      POSTGRES_PASSWORD: test
      POSTGRES_DB: test_db
    options: >-
      --health-cmd pg_isready
      --health-interval 10s
      --health-timeout 5s
      --health-retries 5

steps:
  - name: Run migrations
    run: npm run db:migrate
    env:
      DATABASE_URL: postgresql://postgres:test@localhost:5432/test_db
```

#### MongoDB

```yaml
services:
  mongodb:
    image: mongo:6
    ports:
      - 27017:27017

steps:
  - name: Run tests
    env:
      MONGODB_URI: mongodb://localhost:27017/test_db
```

---

## Required npm Scripts

### Essential Scripts

```json
{
  "scripts": {
    // Build & Type Checking
    "build": "Build your application",
    "typecheck": "tsc --noEmit",

    // Code Quality
    "lint": "eslint 'src/**/*.{ts,tsx}'",
    "format:check": "prettier --check 'src/**/*.{ts,tsx,json,md}'",

    // Testing
    "test": "jest",
    "test:coverage": "jest --coverage",

    // Optional but Recommended
    "test:backend": "jest src/backend",
    "test:frontend": "jest src/frontend",
    "test:integration": "jest __tests__/integration",
    "test:e2e": "playwright test",
    "test:performance": "jest __tests__/performance",

    // Deployment
    "build:production": "NODE_ENV=production npm run build",
    "release:create": "node scripts/create-release-artifacts.js"
  }
}
```

### Multi-Agent Testing Scripts

For projects using multi-agent development patterns:

```json
{
  "scripts": {
    "test:agent:backend": "jest src/backend/**/__tests__/**/*.test.ts",
    "test:agent:frontend": "jest src/frontend/**/__tests__/**/*.test.ts",
    "test:agent:ai": "jest src/services/ai/**/__tests__/**/*.test.ts",
    "test:agent:database": "jest src/database/**/__tests__/**/*.test.ts"
  }
}
```

---

## Environment Variables & Secrets

### GitHub Repository Secrets

Add these secrets in **Settings → Secrets and variables → Actions**:

#### Essential Secrets

```
CODECOV_TOKEN          # Code coverage reporting (codecov.io)
GITHUB_TOKEN           # Automatically provided by GitHub
```

#### Deployment Secrets

```
# Cloud Providers
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AZURE_CREDENTIALS
GCP_SERVICE_ACCOUNT_KEY

# Hosting Platforms
VERCEL_TOKEN
NETLIFY_AUTH_TOKEN
HEROKU_API_KEY

# App Stores
APPLE_ID
APPLE_ID_PASSWORD
GOOGLE_PLAY_SERVICE_ACCOUNT_JSON
```

#### External Services

```
# AI/ML Services
AZURE_CUSTOM_VISION_ENDPOINT
AZURE_CUSTOM_VISION_KEY
AZURE_OCR_ENDPOINT
AZURE_OCR_KEY

# Cloud Storage
CLOUDINARY_CLOUD_NAME
CLOUDINARY_API_KEY
CLOUDINARY_API_SECRET
```

### Environment Variables in Workflows

```yaml
- name: Run tests
  run: npm test
  env:
    NODE_ENV: test
    API_URL: ${{ secrets.API_URL }}
    DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

---

## Testing Strategies

### Unit Tests

Focus on isolated function testing:

```javascript
// Example: src/utils/__tests__/formatPrice.test.ts
describe('formatPrice', () => {
  it('formats USD prices correctly', () => {
    expect(formatPrice(1234.56, 'USD')).toBe('$1,234.56');
  });
});
```

### Integration Tests

Test cross-layer interactions:

```javascript
// Example: __tests__/integration/api.test.ts
describe('Card API Integration', () => {
  it('creates card and stores in database', async () => {
    const card = await api.createCard({ name: 'Luffy' });
    const stored = await db.getCard(card.id);
    expect(stored).toEqual(card);
  });
});
```

### E2E Tests

Test complete user workflows:

```javascript
// Example: e2e/card-upload.spec.ts
test('user can upload and recognize card', async ({ page }) => {
  await page.goto('/upload');
  await page.setInputFiles('input[type="file"]', 'test-card.jpg');
  await page.click('button:text("Analyze")');
  await expect(page.locator('.card-result')).toContainText('Monkey D. Luffy');
});
```

### Performance Tests

Benchmark critical operations:

```javascript
// Example: __tests__/performance/search.test.ts
describe('Card Search Performance', () => {
  it('searches 10k cards in under 100ms', async () => {
    const start = Date.now();
    await searchCards('Luffy');
    const duration = Date.now() - start;
    expect(duration).toBeLessThan(100);
  });
});
```

---

## Deployment Configuration

### GitHub Releases

Automated release creation on version tags:

```yaml
deploy:
  if: startsWith(github.ref, 'refs/tags/v')
  steps:
    - name: Create GitHub Release
      uses: softprops/action-gh-release@v1
      with:
        files: |
          dist/releases/*
        generate_release_notes: true
```

Usage:

```bash
git tag v1.0.0
git push origin v1.0.0  # Triggers deployment
```

### Vercel Deployment

```yaml
deploy-vercel:
  steps:
    - name: Deploy to Vercel
      uses: amondnet/vercel-action@v20
      with:
        vercel-token: ${{ secrets.VERCEL_TOKEN }}
        vercel-org-id: ${{ secrets.VERCEL_ORG_ID }}
        vercel-project-id: ${{ secrets.VERCEL_PROJECT_ID }}
        vercel-args: '--prod'
```

### AWS S3 + CloudFront

```yaml
deploy-aws:
  steps:
    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Deploy to S3
      run: |
        aws s3 sync dist/ s3://my-bucket/ --delete

    - name: Invalidate CloudFront
      run: |
        aws cloudfront create-invalidation \
          --distribution-id ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }} \
          --paths "/*"
```

---

## Troubleshooting

### Common Issues

#### 1. Tests Fail in CI but Pass Locally

**Cause:** Environment differences

**Solution:**

```yaml
- name: Set test environment
  run: |
    cp .env.example .env.test
    echo "CI=true" >> .env.test
```

#### 2. Build Times Too Long

**Cause:** Missing cache configuration

**Solution:**

```yaml
- name: Setup cache
  uses: actions/cache@v3
  with:
    path: |
      ~/.npm
      node_modules/.cache
    key: ${{ runner.os }}-${{ hashFiles('package-lock.json') }}
```

#### 3. Flaky E2E Tests

**Cause:** Race conditions, network timeouts

**Solution:**

```yaml
- name: Run E2E tests with retries
  run: npm run test:e2e
  env:
    PLAYWRIGHT_RETRIES: 2
    PLAYWRIGHT_TIMEOUT: 30000
```

#### 4. Out of Memory Errors

**Cause:** Large builds, memory-intensive tests

**Solution:**

```yaml
- name: Increase Node memory
  run: NODE_OPTIONS="--max_old_space_size=4096" npm run build
```

#### 5. Permission Errors

**Cause:** Missing execute permissions

**Solution:**

```bash
chmod +x scripts/*.sh
git add scripts/
git commit -m "fix: add execute permissions to scripts"
```

### Debug Mode

Enable detailed logging:

```yaml
- name: Debug info
  run: |
    echo "Node version: $(node --version)"
    echo "npm version: $(npm --version)"
    echo "Working directory: $(pwd)"
    echo "Files: $(ls -la)"
    npm run test -- --verbose
```

---

## Best Practices

### 1. Keep Workflows Fast

- Use caching aggressively
- Run tests in parallel
- Skip unnecessary steps (e.g., build on lint-only PRs)

### 2. Fail Fast

- Run quick checks first (lint, typecheck)
- Stop pipeline on critical failures
- Use `needs:` to create dependencies

### 3. Security First

- Never commit secrets
- Use repository secrets for credentials
- Scan dependencies regularly
- Keep actions up to date

### 4. Monitor Performance

- Track build times
- Set performance budgets
- Alert on regressions

### 5. Documentation

- Document custom workflows
- Explain deployment process
- Keep runbooks updated

---

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Actions Marketplace](https://github.com/marketplace?type=actions)
- [Codecov Integration](https://docs.codecov.com/docs/github-actions)

---

**Next Steps:**

1. Review [Testing Requirements](./testing-requirements.md)
2. Configure [Deployment Automation](./deployment-guide.md)
3. Set up [Performance Monitoring](./performance-tracking.md)

---

_Claude Code Framework - Professional CI/CD for Multi-Agent Development_
