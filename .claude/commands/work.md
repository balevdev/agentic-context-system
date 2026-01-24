# Planning Mode

## Quick Start

```bash
# 1. Get session context (lightweight - always read)
cat features/index.json | jq '.meta.next_priority, .meta.session_plan'

# 2. Get full feature definition (from active/ directory)
cat features/active/FEATURE_ID.json

# 3. Read patterns from CLAUDE.md
```

## 1. Problem

```
User problem:
Success state:
```

## 2. Design

```
FLOW: Entry → Steps → Success/Error

API: What endpoints?

STATE: Database schema? In-memory?
```

## 3. Approach

```
Solution:
Why this:
Risk:
```

## Principles

- Correctness > Readability > Simplicity
- Duplication < Wrong abstraction
- Boring > Clever

## Session Context

Before starting work, check `features/index.json` for:
1. **Current session** - `status_map[FEATURE_ID].session`
2. **Session features** - `meta.session_plan[SESSION_NUMBER].features`
3. **Token budget** - `meta.session_plan[SESSION_NUMBER].token_budget`
4. **Dependency status** - All deps in `status_map` must have `passes: true`

## Execution Phases

Each feature follows a standard execution:

1. **Schema & Types** - Interfaces, database schema
2. **Validation** - Input validation + tests
3. **Core Logic** - Business logic, database operations + tests
4. **Routes** - API endpoints + integration tests
5. **Integration** - E2E testing, manual verification

## 4. Implementation Hints

### Files to Create/Modify

```
- `src/routes/example.ts` (create)
- `src/types/example.ts` (modify: add ExampleInput interface)
```

### Pattern References

```
- Route pattern: `src/routes/todos.ts` lines 1-30
- Validator pattern: `src/validators/todo.ts` lines 1-25
```

### Test Cases (write first)

```
1. Valid input returns success response
2. Invalid input returns validation error
3. Not found returns 404
```

### Code Snippets

```typescript
// Include key interfaces/types to implement
export interface ExampleInput {
  field: string;
  optionalField?: number;
}
```

---

Read @Prompt.md and plan the best implementation for the task.
