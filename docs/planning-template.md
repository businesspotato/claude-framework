# Planning Template

**Last Updated**: 2025-01-15

## Overview

Structured 7-step planning template for complex development tasks. Use this format before starting work on tasks flagged as COMPLEX by `/planning-checklist`.

## When to Use This Template

**MANDATORY for**:
- Multi-file changes (3+ files)
- Multi-service integration
- Database schema changes/migrations
- Architectural decisions
- Performance optimization tasks (>1000 ops/sec)
- Algorithmic challenges
- Scalability planning

**Optional for**:
- Medium complexity tasks (2 indicators)
- Refactoring work spanning multiple components
- Feature implementations requiring coordination

## 7-Step Planning Format

```markdown
# [Feature/Task Name] - Planning Document

**Date**: YYYY-MM-DD
**Complexity**: SIMPLE | MEDIUM | COMPLEX
**Estimated Effort**: [hours/days]
**Author**: [Instance/Agent]

---

## 1. Problem Summary

[2-4 lines describing the problem to solve or feature to implement]

**Context**:
- Why is this needed?
- What problem does it solve?
- Who requested it?

**Current State**:
- What exists today?
- What's not working?
- What constraints exist?

**Desired State**:
- What should exist after completion?
- What are the success criteria?
- What metrics will improve?

---

## 2. Artifacts to Read

**Required Reading** (must read before implementation):
- [ ] `path/to/file1.ts` - Description of why this matters
- [ ] `path/to/file2.ts` - Description of why this matters
- [ ] `docs/related-doc.md` - Existing documentation

**Optional Reading** (context, not critical):
- [ ] `path/to/reference.ts` - Additional context
- [ ] External docs: [Link with description]

**Research Needed**:
- [ ] API documentation review
- [ ] Database schema investigation
- [ ] Performance baseline measurement

---

## 3. Proposed Changes

### Files to Modify

**Backend Changes**:
- `src/backend/services/ServiceName.ts`
  - Add new method: `processData(params)`
  - Modify existing: `getData()` to support new parameter
  - Remove deprecated: `oldMethod()`

**Frontend Changes**:
- `src/frontend/hooks/useServiceHook.ts`
  - Add new hook: `useProcessData()`
  - Update cache invalidation keys

**Database Changes**:
- Migration: `migrations/YYYY-MM-DD-add-column.sql`
  - Add column: `new_field TEXT`
  - Create index: `idx_new_field`

**Configuration Changes**:
- `.env.example` - Add `NEW_CONFIG_KEY`
- `package.json` - Add dependency: `new-library@1.0.0`

**Documentation Changes**:
- `docs/api/service-api.md` - Document new endpoint
- `docs/architecture.md` - Update data flow diagram

### Files to Create

- `src/backend/services/NewService.ts` - New service for [purpose]
- `test/unit/NewService.test.ts` - Unit tests
- `docs/planning/[task-name]-implementation.md` - Implementation details

---

## 4. Commands to Run

**Exact commands with flags**:

```bash
# Install new dependencies
npm install new-library@1.0.0

# Run database migration
npm run migrate:up

# Generate TypeScript types
npm run types:generate

# Run unit tests
npm test -- --testPathPattern=NewService

# Run integration tests
npm run test:integration -- --grep="new feature"

# Build project
npm run build

# Start application
npm start
```

**Command Order**:
1. Install dependencies
2. Run migrations
3. Generate types
4. Implement changes
5. Run tests (unit → integration → e2e)
6. Build
7. Manual testing

---

## 5. Rollback Plan

**If things go wrong, follow this sequence**:

```bash
# 1. Revert code changes
git reset --hard HEAD

# 2. Rollback database migration
npm run migrate:down

# 3. Restore dependencies
npm ci

# 4. Clear cache
rm -rf node_modules/.cache

# 5. Restart application
npm start
```

**Checkpoints**:
- Create git checkpoint before starting: `./scripts/checkpoint.sh "pre-[task-name]"`
- Backup database: `cp data/database/cards.db data/database/cards.db.backup`
- Document current state in `docs/rollback/YYYY-MM-DD-[task].md`

**Recovery Time Objective (RTO)**: <5 minutes

---

## 6. Test Plan

### Unit Tests

**New Tests** (TDD - write before implementation):
```typescript
// test/unit/NewService.test.ts
describe('NewService', () => {
  describe('processData', () => {
    it('should process valid data', () => {
      // Arrange
      const input = { ... };
      
      // Act
      const result = service.processData(input);
      
      // Assert
      expect(result).toEqual({ ... });
    });
    
    it('should handle null input gracefully', () => { ... });
    it('should throw error for invalid data', () => { ... });
  });
});
```

**Coverage Target**: ≥80% for new code

### Integration Tests

**Test Scenarios**:
1. **Happy Path**: Full data flow from frontend → backend → database → frontend
2. **Error Handling**: Invalid input, network errors, database failures
3. **Edge Cases**: Empty data, maximum data size, concurrent requests

**Test Data**:
- Use known test cards: "Sanji OP12-041", "Luffy OP01-001"
- Create test fixtures in `test/fixtures/`

### E2E Tests

**User Flows to Test**:
1. User uploads image → Card recognized → Data displayed
2. User searches collection → Results returned → Filters applied
3. User views pricing → Market data loaded → Charts rendered

**Manual Testing Checklist**:
- [ ] UI renders correctly
- [ ] No console errors
- [ ] Performance within budget (<200ms p95)
- [ ] Works in production mode
- [ ] Hot-reload works correctly

### Benchmark Tests

**Performance Validation**:
```bash
# Run benchmark
npm run bench:action -- --name="new-feature"

# Check results
npm run bench:check-budgets
```

**Expected Results**:
- p50: <100ms
- p95: <200ms
- No memory leaks
- No CPU spikes

---

## 7. Risks & Mitigations

### Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Database migration fails | Medium | High | Test migration on copy first, have rollback script ready |
| Breaking change to API | Low | High | Use feature flags, deprecation warnings, version API |
| Performance regression | Medium | Medium | Run benchmarks before/after, profile if needed |
| Cache invalidation issues | High | Medium | Test with known data, verify cache keys updated |

### Timeline Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Task takes longer than estimated | Medium | Low | Break into smaller PRs, ship incrementally |
| Blocked by dependency | Low | Medium | Identify dependencies early, have alternatives |
| Testing reveals major issues | Medium | High | Allocate buffer time, have rollback ready |

### Integration Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Conflicts with other instance work | High | Medium | Check multi-instance status, coordinate handoffs |
| Breaking changes for other layers | Medium | High | Update IPC contracts, verify all layers synchronized |
| Cache desynchronization | High | Medium | Test cache invalidation at each layer transition |

### Mitigation Strategies

**General Approach**:
1. **Start small** - Implement MVP first, iterate
2. **Test early** - TDD approach, test each component
3. **Checkpoint often** - Git checkpoints before risky operations
4. **Monitor closely** - Watch logs, metrics, performance
5. **Communicate** - Update status files, coordinate with other instances

**Specific Mitigations**:
- Database changes: Use transactions, test on copy first
- API changes: Version endpoints, maintain backward compatibility
- Performance: Benchmark before/after, profile if issues
- Cache: Verify invalidation keys, test with real data

---

## Decision Log

**Key Decisions Made**:

| Date | Decision | Rationale | Alternatives Considered |
|------|----------|-----------|-------------------------|
| 2025-01-15 | Use SQLite for tracking | Simple, no extra dependencies | Redis (overkill), JSON files (no queries) |
| 2025-01-15 | Client-side caching | Reduce API calls, improve UX | Server-side only (slower) |

**Trade-offs Accepted**:
- **Decision**: Use eager loading instead of lazy loading
- **Trade-off**: Higher initial load time, but simpler code
- **Reason**: Application is local, 100ms extra load time acceptable

---

## Approval & Sign-off

**Planning Approval**:
- [ ] Reviewed by: [Name/Instance]
- [ ] Approved: [Date]
- [ ] Ready to implement: YES / NO

**Implementation Sign-off**:
- [ ] Code complete: [Date]
- [ ] Tests passing: YES / NO
- [ ] Performance verified: YES / NO
- [ ] Documentation updated: YES / NO
- [ ] Ready for review: YES / NO

---

## Notes & Updates

**[Date]**: Initial plan created  
**[Date]**: Updated after discovering [issue]  
**[Date]**: Approved and ready for implementation  

---

## Related Documentation

- **[Planning Enforcement](./planning-enforcement.md)** - When to use this template
- **[Subagent Guide](./subagent-guide.md)** - Which subagents to use per phase
- **[TDD Requirements](../testing/tdd-requirements.md)** - Test-driven development standards

---

**Navigation**: [Development Documentation](../index.md#development-patterns--best-practices) | [CLAUDE.md](../../CLAUDE.md)
