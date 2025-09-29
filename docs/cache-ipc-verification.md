# Cache & IPC Verification Guide

## Overview

Comprehensive protocols for preventing and resolving cache/IPC synchronization issues that create "invisible barriers" blocking data flow between architectural layers.

## The Cache/IPC Problem

### Invisible Barriers
Cache and IPC synchronization failures are particularly frustrating because:
- **No visible errors**: Application appears to work but data doesn't flow
- **Inconsistent behavior**: Works sometimes, fails other times
- **Silent failures**: No error messages, just stale or missing data
- **Layer isolation**: Each layer works independently but integration fails

### Common Manifestations
- Backend data changes but UI still shows old values
- IPC handlers updated but frontend still calls old handlers
- Cache invalidation doesn't trigger UI updates
- Data exists in database but doesn't appear in frontend

## Layer Synchronization Architecture

### Critical Data Flow
```
Database → Backend Services → IPC Channels → Frontend Hooks → UI Components → Cache
```

**EVERY change must be synchronized across ALL layers**

### Layer Dependencies
1. **Database**: Source of truth for all data
2. **Backend Services**: Business logic and data processing
3. **IPC Channels**: Communication bridge between main/renderer processes
4. **Frontend Hooks**: Data fetching and state management
5. **UI Components**: Data presentation and user interaction
6. **Cache**: Performance optimization layer

## Verification Protocols

### Full-Stack Layer Verification Checklist

#### Database Layer
- [ ] Data exists in correct tables
- [ ] Indexes support query patterns
- [ ] Constraints validate data integrity
- [ ] Migrations applied successfully

```sql
-- Example verification
SELECT * FROM cards WHERE id = 'OP12-041';
SELECT * FROM pricing WHERE card_id = 'OP12-041';
```

#### Backend Services Layer
- [ ] Service methods return expected data
- [ ] Business logic processes data correctly
- [ ] Error handling covers edge cases
- [ ] Performance meets requirements

```typescript
// Example verification
const card = await cardService.getById('OP12-041');
const pricing = await pricingService.getLatest('OP12-041');
console.log('Backend data:', { card, pricing });
```

#### IPC Channels Layer
- [ ] Handlers registered with correct names
- [ ] Parameter formats match frontend expectations
- [ ] Return formats match frontend processing
- [ ] Error responses properly formatted

```typescript
// Backend: Verify handler registration
ipcMain.handle('collection:get-card', async (event, cardId) => {
  return await cardService.getById(cardId);
});

// Frontend: Verify call matches
const card = await invoke('collection:get-card', 'OP12-041');
```

#### Frontend Hooks Layer
- [ ] Hook calls match IPC handler names
- [ ] Parameters passed correctly
- [ ] Response processing handles all cases
- [ ] Cache invalidation triggers correctly

```typescript
// Example verification
const { data, isLoading, error } = useCard('OP12-041');
console.log('Frontend hook data:', { data, isLoading, error });
```

#### UI Components Layer
- [ ] Components receive data correctly
- [ ] Display logic handles all data states
- [ ] User interactions trigger correct actions
- [ ] Error states displayed appropriately

```tsx
// Example verification
const CardDisplay = ({ cardId }) => {
  const { data: card } = useCard(cardId);
  console.log('UI component data:', card);
  return <CardView card={card} />;
};
```

#### Cache Layer
- [ ] Cache keys consistent across layers
- [ ] Invalidation triggers at correct times
- [ ] Cache miss triggers data refresh
- [ ] Cache hit returns current data

```typescript
// Example verification
const cacheKey = `card-${cardId}`;
const cached = await cache.get(cacheKey);
console.log('Cache data:', cached);
```

## Common Cache/IPC Issues & Solutions

### Issue 1: Stale Data Display

#### Symptoms
- Backend data updated but UI shows old values
- Database has correct data but frontend displays stale information
- Refresh fixes the issue temporarily

#### Root Causes
- Cache not invalidated after backend changes
- Cache keys inconsistent between backend and frontend
- Cache invalidation happens before data updates

#### Debugging Protocol
```typescript
// 1. Check database directly
const dbCard = await db.query('SELECT * FROM cards WHERE id = ?', [cardId]);

// 2. Check backend service
const serviceCard = await cardService.getById(cardId);

// 3. Check cache
const cachedCard = await cache.get(`card-${cardId}`);

// 4. Check frontend data
const frontendCard = await invoke('collection:get-card', cardId);

// 5. Compare all sources
console.log('Data comparison:', {
  database: dbCard,
  service: serviceCard,
  cache: cachedCard,
  frontend: frontendCard
});
```

#### Solution Protocol
```typescript
// 1. Ensure proper cache invalidation
await cardService.update(cardId, updates);
await cache.invalidate([`card-${cardId}`, 'collection', 'pricing']);

// 2. Verify cache keys are consistent
const CACHE_KEYS = {
  card: (id) => `card-${id}`,
  collection: 'collection',
  pricing: (id) => `pricing-${id}`
};

// 3. Test with specific examples
await testCacheInvalidation('OP12-041', 'Sanji');
```

### Issue 2: IPC Handler Mismatches

#### Symptoms
- Frontend calls fail silently
- IPC handlers not responding
- Methods not found errors

#### Root Causes
- Frontend hooks call old handler names
- Backend handlers registered with different names
- Parameter format mismatches

#### Debugging Protocol
```typescript
// 1. List all registered handlers
console.log('Registered IPC handlers:', ipcMain.eventNames());

// 2. Check frontend calls
const frontendCalls = [
  'collection:get-card',
  'collection:remove-card',
  'pricing:get-latest'
];

// 3. Verify handler registration
frontendCalls.forEach(handler => {
  const isRegistered = ipcMain.listenerCount(handler) > 0;
  console.log(`Handler ${handler}: ${isRegistered ? 'REGISTERED' : 'MISSING'}`);
});
```

#### Solution Protocol
```typescript
// 1. Standardize handler naming
const IPC_HANDLERS = {
  'collection:get-card': handleGetCard,
  'collection:remove-card': handleRemoveCard,
  'pricing:get-latest': handleGetLatestPricing
};

// 2. Register all handlers
Object.entries(IPC_HANDLERS).forEach(([name, handler]) => {
  ipcMain.handle(name, handler);
});

// 3. Verify frontend hooks match
const useCard = (cardId) => {
  return useIPC('collection:get-card', cardId);
};
```

### Issue 3: Cache Invalidation Race Conditions

#### Symptoms
- Intermittent cache issues
- Sometimes shows new data, sometimes old
- Cache state unpredictable

#### Root Causes
- Cache invalidated before data saved
- Multiple cache updates conflict
- Async operations race conditions

#### Solution Protocol
```typescript
// 1. Ensure proper async sequencing
const updateCard = async (cardId, updates) => {
  // Save data first
  const result = await cardService.update(cardId, updates);
  
  // Then invalidate cache
  await cache.invalidate([`card-${cardId}`]);
  
  // Return updated data
  return result;
};

// 2. Use cache versioning
const updateCardWithVersion = async (cardId, updates) => {
  const version = await cache.incrementVersion(cardId);
  const result = await cardService.update(cardId, { ...updates, version });
  await cache.set(`card-${cardId}-${version}`, result);
  return result;
};
```

## Specific Testing Protocols

### Example-Based Testing
Use specific, known examples for testing:

#### Sanji OP12-041 Pricing Test
```typescript
const testSanjiPricing = async () => {
  const cardId = 'OP12-041';
  const expectedPrice = 22.50;
  
  // 1. Database check
  const dbPrice = await db.query(
    'SELECT price1 FROM pricing WHERE card_id = ?', 
    [cardId]
  );
  
  // 2. Backend service check
  const servicePrice = await pricingService.getPrice(cardId);
  
  // 3. Frontend hook check
  const { data: hookData } = usePricing(cardId);
  
  // 4. UI display check
  const displayedPrice = document.querySelector(`[data-card="${cardId}"] .price`).textContent;
  
  // 5. Compare all values
  console.log('Sanji OP12-041 Pricing Test:', {
    expected: expectedPrice,
    database: dbPrice?.price1,
    service: servicePrice,
    hook: hookData?.price,
    displayed: parseFloat(displayedPrice?.replace('$', ''))
  });
  
  // 6. Identify discrepancies
  const issues = [];
  if (dbPrice?.price1 !== expectedPrice) issues.push('Database price incorrect');
  if (servicePrice !== expectedPrice) issues.push('Service price incorrect');
  if (hookData?.price !== expectedPrice) issues.push('Hook price incorrect');
  
  return issues.length === 0 ? 'PASS' : { status: 'FAIL', issues };
};
```

### Cache Verification with Known Examples

#### Collection Data Test
```typescript
const testCollectionData = async () => {
  const testCards = ['OP12-041', 'OP12-027', 'OP12-010'];
  
  for (const cardId of testCards) {
    // Clear cache
    await cache.invalidate([`card-${cardId}`]);
    
    // Fetch data (should hit database)
    const freshData = await cardService.getById(cardId);
    
    // Fetch again (should hit cache)
    const cachedData = await cardService.getById(cardId);
    
    // Verify cache hit
    console.log(`Card ${cardId}:`, {
      fresh: freshData,
      cached: cachedData,
      equal: JSON.stringify(freshData) === JSON.stringify(cachedData)
    });
  }
};
```

## Prevention Strategies

### Development Workflow Integration

#### Pre-Change Checklist
- [ ] Identify all layers affected by change
- [ ] Plan cache invalidation strategy
- [ ] Document IPC handler changes required
- [ ] Prepare test cases with specific examples

#### Post-Change Verification
- [ ] Test with known examples (Sanji OP12-041, etc.)
- [ ] Verify cache invalidation works correctly
- [ ] Test IPC handlers respond correctly
- [ ] Confirm UI displays updated data

#### Handoff Requirements
- [ ] Document cache keys affected
- [ ] List IPC handlers changed
- [ ] Provide test cases for verification
- [ ] Confirm integration points tested

### Automated Testing

#### Cache Integration Tests
```typescript
describe('Cache Integration', () => {
  test('cache invalidation updates UI data', async () => {
    const cardId = 'OP12-041';
    
    // Initial data
    const initialData = await cardService.getById(cardId);
    
    // Update data
    await cardService.update(cardId, { name: 'Updated Name' });
    
    // Verify cache cleared
    const cachedData = await cache.get(`card-${cardId}`);
    expect(cachedData).toBeNull();
    
    // Verify new data available
    const updatedData = await cardService.getById(cardId);
    expect(updatedData.name).toBe('Updated Name');
  });
});
```

#### IPC Integration Tests
```typescript
describe('IPC Integration', () => {
  test('frontend hooks match backend handlers', async () => {
    const cardId = 'OP12-041';
    
    // Direct backend call
    const backendData = await cardService.getById(cardId);
    
    // IPC call
    const ipcData = await ipcRenderer.invoke('collection:get-card', cardId);
    
    // Should be identical
    expect(ipcData).toEqual(backendData);
  });
});
```

## Emergency Procedures

### When Cache/IPC Issues Persist

#### 1. Full System Reset
```bash
# Clear all caches
npm run cache:clear

# Restart application
printf "rs\n"

# Wait for full startup
# Test with known examples
```

#### 2. Layer-by-Layer Debugging
```typescript
// Test each layer independently
const debugLayers = async (cardId) => {
  console.log('=== LAYER DEBUGGING ===');
  
  // Layer 1: Database
  const dbResult = await testDatabase(cardId);
  console.log('Database:', dbResult);
  
  // Layer 2: Backend Service
  const serviceResult = await testBackendService(cardId);
  console.log('Backend Service:', serviceResult);
  
  // Layer 3: IPC
  const ipcResult = await testIPC(cardId);
  console.log('IPC:', ipcResult);
  
  // Layer 4: Frontend
  const frontendResult = await testFrontend(cardId);
  console.log('Frontend:', frontendResult);
  
  // Layer 5: Cache
  const cacheResult = await testCache(cardId);
  console.log('Cache:', cacheResult);
};
```

#### 3. Documentation Update
```markdown
## Issue Found: [Date]
**Problem**: [Description]
**Root Cause**: [Analysis]
**Solution**: [What fixed it]
**Prevention**: [How to avoid in future]
**Test Case**: [How to verify fix]
```

## Quick Reference

### Cache Key Conventions
```typescript
const CACHE_KEYS = {
  card: (id) => `card-${id}`,
  collection: 'collection',
  pricing: (id) => `pricing-${id}`,
  user: (id) => `user-${id}`,
  stats: 'stats'
};
```

### IPC Handler Conventions
```typescript
const IPC_HANDLERS = {
  // Collection operations
  'collection:get-card': handleGetCard,
  'collection:remove-card': handleRemoveCard,
  'collection:add-card': handleAddCard,
  
  // Pricing operations
  'pricing:get-latest': handleGetLatestPricing,
  'pricing:update': handleUpdatePricing,
  
  // AI operations
  'ai:recognize-card': handleRecognizeCard,
  'ai:process-image': handleProcessImage
};
```

### Test Examples
```typescript
const TEST_CASES = {
  // Pricing edge cases
  sanji: { id: 'OP12-041', expectedPrice: 22.50 },
  boa: { id: 'OP12-027', expectedPrice: 15.00 },
  
  // Collection edge cases
  promoCard: { id: 'P-001', type: 'promo' },
  variantCard: { id: 'OP12-041_AA', variant: true }
};
```

---

**Framework Version**: 1.0.0
**Last Updated**: 2025-09-27

*This guide provides comprehensive protocols for preventing and resolving cache/IPC synchronization issues that create invisible barriers in full-stack applications.*