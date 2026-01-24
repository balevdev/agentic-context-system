# SETUP-001: Initialize Bun Project

**Status:** NOT_STARTED | **Session:** 1

## Phase Progress

| Phase | Name           | Status  | Commit |
|-------|----------------|---------|--------|
| 1     | Schema & Types | pending | -      |
| 2     | Core Setup     | pending | -      |
| 3     | Verification   | pending | -      |

## Current Phase

**Phase:** 1 - Schema & Types
**Attempts:** 0/3

### Attempt Log
<!-- Add entries: [YYYY-MM-DD HH:MM] - Error description -->

## Acceptance Criteria

- [ ] package.json exists with bun as runtime
- [ ] Hono installed and configured
- [ ] Basic health check endpoint works
- [ ] bun run dev starts the server
- [ ] TypeScript configured with strict mode

## Dependencies

- None (first task)

## Implementation Hints

### Files to Create
- `package.json` - Project metadata and scripts
- `tsconfig.json` - TypeScript configuration
- `src/index.ts` - Hono app entry point

### Pattern Reference
```typescript
import { Hono } from 'hono'

const app = new Hono()

app.get('/health', (c) => c.json({ status: 'ok' }))

export default app
```

## Next

After completing SETUP-001, proceed to:
- **SETUP-002** - Configure Development Tools

---

## Notes

This is the first task when using this template. Customize the agentic loop files for your specific Bun + Hono project.
