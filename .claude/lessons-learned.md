# Lessons Learned

Accumulated wisdom from project sessions. Read this at session start to avoid repeating mistakes.

---

## Bun & Hono

### 1. USE `export default app` FOR BUN SERVE

Bun's built-in server expects a default export:

```typescript
// Good - works with `bun run src/index.ts`
export default app

// Also good - explicit port
export default {
  port: 3000,
  fetch: app.fetch,
}
```

### 2. HONO CONTEXT TYPING

Always type your Hono app for better inference:

```typescript
// Good - typed context
const app = new Hono<{ Variables: { userId: string } }>()

// Access typed variables
app.use(async (c, next) => {
  c.set('userId', '123')
  await next()
})

app.get('/me', (c) => {
  const userId = c.get('userId') // typed as string
})
```

### 3. BUN SQLITE IS SYNCHRONOUS

Bun's built-in SQLite is synchronous, not async:

```typescript
// Good - synchronous
const db = new Database('todo.db')
const todos = db.query('SELECT * FROM todos').all()

// Bad - unnecessary async
const todos = await db.query('SELECT * FROM todos').all() // won't work
```

---

## Testing

### 1. USE `describe` AND `test` FROM BUN

Bun has built-in test runner:

```typescript
import { describe, test, expect, beforeEach } from 'bun:test'

describe('todos', () => {
  beforeEach(() => {
    // setup
  })

  test('should create todo', () => {
    expect(result).toBeDefined()
  })
})
```

### 2. TEST DATABASE ISOLATION

Use separate test database or reset between tests:

```typescript
import { Database } from 'bun:sqlite'

let db: Database

beforeEach(() => {
  db = new Database(':memory:') // fresh in-memory DB each test
  // run migrations
})
```

---

## API Design

### 1. CONSISTENT ERROR RESPONSES

Always return errors in the same format:

```typescript
// Good - consistent structure
return c.json({
  error: {
    code: 'NOT_FOUND',
    message: 'Todo with id 123 not found'
  }
}, 404)

// Bad - inconsistent
return c.json({ message: 'Not found' }, 404)
return c.json({ error: 'Not found' }, 404)
```

### 2. VALIDATE EARLY, FAIL FAST

Check input at route entry, not in business logic:

```typescript
app.post('/todos', async (c) => {
  const body = await c.req.json()

  // Validate immediately
  const validation = validateCreateTodo(body)
  if (!validation.success) {
    return c.json({ error: { code: 'VALIDATION_ERROR', message: validation.error } }, 400)
  }

  // Now safe to use
  const todo = createTodo(validation.data)
  return c.json({ data: todo }, 201)
})
```

---

## TypeScript

### 1. STRICT MODE CATCHES BUGS

Always use strict mode in tsconfig.json:

```json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true
  }
}
```

### 2. INFER RETURN TYPES

Let TypeScript infer return types for simple functions:

```typescript
// Good - inferred
function getTodo(id: number) {
  return db.query('SELECT * FROM todos WHERE id = ?').get(id) as Todo | null
}

// Only annotate when complex or for documentation
function processTodos(todos: Todo[]): ProcessedTodo[] {
  // complex logic
}
```

---

## Common Mistakes to Avoid

1. **Forgetting to export types** - Always `export interface` or `export type`
2. **Not handling null from DB** - `.get()` returns `undefined` if not found
3. **Forgetting Content-Type** - Hono's `c.json()` handles this, but raw responses don't
4. **Test pollution** - Always reset state between tests
5. **Sync vs Async confusion** - Bun SQLite is sync, network calls are async

---

## Add New Lessons

When you discover something important:

1. Add it here with a clear title
2. Include code examples (good vs bad)
3. Note when/where it was discovered
4. Keep it concise but complete

Format:
```markdown
### N. LESSON TITLE IN CAPS

Brief explanation of the issue.

```typescript
// Good - the right way
code example

// Bad - the wrong way
code example
```

Discovered in FEATURE-ID when [what happened].
```

---

*This file grows with the project. Check it at session start. Add to it when you learn something.*
