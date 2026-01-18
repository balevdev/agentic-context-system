# Lessons Learned

> This file accumulates wisdom from previous sessions.
> Add new lessons as they are discovered.
> Reference this file at the start of each session.

---

## Git Workflow

<!-- Add lessons about git workflow as you discover them -->
<!-- Example:
1. ALWAYS COMMIT BEFORE MARKING COMPLETE
   - Code must be committed before setting passes: true
   - Run: git status (must show clean)
-->

---

## Verification Sequence

<!-- Add the exact verification sequence that works for your project -->
<!-- Example:
Run this exact sequence before marking complete:
```bash
npm test && npm run typecheck && npm run lint
git add -A && git commit -m "type(scope): description"
git status  # must show clean
```
-->

---

## Testing

<!-- Add testing lessons specific to your project -->
<!-- Example:
1. Never delete tests to make them pass
2. Mock external dependencies in unit tests
3. Integration tests should use real database
-->

---

## Architecture

<!-- Add architecture patterns specific to your project -->
<!-- Example:
1. All domain types use Zod schemas
2. Ports define interfaces, adapters implement them
3. Use cases coordinate between ports
-->

---

## Common Mistakes to Avoid

<!-- Add mistakes that have been made and should be avoided -->
<!-- Example:
1. Not reading files before modifying them
2. Changes affecting more than 5 files unexpectedly
3. Uncommitted changes at session end
4. Marking complete without verification
-->

---

## Project-Specific Patterns

<!-- Add patterns unique to your project -->
<!-- Example:
1. Error types extend base AnakinError class
2. All API responses use SuccessResponse<T> wrapper
3. Components use specific naming convention
-->

---

## How to Add Lessons

When you discover something important:

1. **Identify the category** - Git, Testing, Architecture, etc.
2. **Write it clearly** - Future sessions should understand immediately
3. **Include context** - Why is this important? What went wrong?
4. **Keep it actionable** - What should be done differently?

Format:
```
## Category

N. LESSON TITLE
   - Explanation of the lesson
   - Why it matters
   - What to do instead
```
