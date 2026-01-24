# Verification Checklists

Use these checklists to verify quality at each stage.

---

## Phase Completion Checklist

Before committing any phase:

```bash
# 1. Run tests - must show all passing
bun test                      # Exit code 0, all tests pass

# 2. Type check - must pass (if configured)
bun run typecheck             # Exit code 0, no errors

# 3. Lint check - must pass
bun run lint                  # Exit code 0, no errors

# 4. Verify the specific phase deliverables work
# (depends on the phase - see below)
```

---

## Phase-Specific Checks

### Phase 1: Schema & Types
- [ ] All interfaces defined in `src/types/`
- [ ] Types are exported and importable
- [ ] No `any` types
- [ ] TypeScript compiles: `bun run typecheck`

### Phase 2: Validation
- [ ] Validator functions exist in `src/validators/`
- [ ] Validation tests exist and pass
- [ ] Edge cases covered (empty, invalid, boundary)
- [ ] Error messages are clear

### Phase 3: Core Logic / Database
- [ ] CRUD operations implemented
- [ ] Database operations have tests
- [ ] Error handling for not-found cases
- [ ] Tests use isolated test database

### Phase 4: Routes
- [ ] All endpoints respond correctly
- [ ] Input validation wired up
- [ ] Error responses follow ApiError format
- [ ] Integration tests exist and pass

### Phase 5: Integration
- [ ] End-to-end flow works
- [ ] Manual testing completed
- [ ] Edge cases handled
- [ ] Documentation updated (if needed)

---

## Session End Checklist

Before ending any session:

```bash
# 1. Run all verification
bun test                      # All pass
bun run typecheck             # No errors (if configured)
bun run lint                  # No errors

# 2. Check git status
git status                    # "nothing to commit, working tree clean"

# 3. Verify CURRENT_TASK.md is accurate
# - Phase table shows correct status
# - Attempt counter is accurate
# - Blockers documented (if any)
```

---

## Feature Completion Checklist

Before marking a feature as `passes: true`:

- [ ] ALL acceptance criteria met
- [ ] ALL phases complete with commits
- [ ] ALL tests passing
- [ ] No lint/type errors
- [ ] Code committed with proper message
- [ ] features/index.json updated
- [ ] CURRENT_TASK.md updated to next feature
- [ ] Session summary appended to progress.txt

---

## Blocked Task Checklist

When marking a task as BLOCKED:

- [ ] 3 attempts logged with timestamps
- [ ] Each attempt describes what was tried
- [ ] "What Was Tried" section filled out
- [ ] "What's Needed" section explains the blocker
- [ ] Status set to BLOCKED in CURRENT_TASK.md
- [ ] No further work attempted on this task

---

## New Session Checklist

When starting a new session:

1. [ ] Read CURRENT_TASK.md - understand current state
2. [ ] Read features/index.json - check dependencies
3. [ ] Read Prompt.md - follow the protocol
4. [ ] Read CLAUDE.md - understand patterns
5. [ ] Read .claude/lessons-learned.md - avoid past mistakes
6. [ ] If handoff.md exists, read it first
7. [ ] Run `bun test` - verify clean starting state

---

## Quick Reference Commands

```bash
# Development
bun run dev               # Start dev server

# Testing
bun test                  # Run all tests
bun test --watch          # Watch mode
bun test tests/routes     # Specific tests

# Quality
bun run typecheck         # Type checking
bun run lint              # Linting
bun run lint --fix        # Auto-fix lint issues

# Git
git status                # Check working tree
git log --oneline -5      # Recent commits
```

---

*Run these checklists mechanically. Don't skip steps. Quality comes from consistency.*
