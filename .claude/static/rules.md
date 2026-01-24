# Critical Operating Rules

These rules must NEVER be violated. They exist to prevent common failure modes.

---

## 1. PHASE GATE RULE

**Never move to Phase N+1 until Phase N passes tests and is committed.**

```
Phase N Complete
    │
    ▼
Run: bun test && bun run lint
    │
    ├── PASS → Commit → Phase N+1
    │
    └── FAIL → Fix (counts as attempt) → Retry
```

- Every phase completion requires a commit
- Failed tests count as attempts toward the 3-attempt limit
- Commit message format: `[FEATURE_ID] Phase N: Description`

---

## 2. ONE TASK AT A TIME

**Never work on more than one feature simultaneously.**

- Complete the current task before starting another
- If blocked, mark as BLOCKED rather than switching tasks
- If you notice something else needs fixing, add it to backlog.json

---

## 3. ATTEMPT TRACKING

**Log every failure. Stop at 3 attempts.**

On test/lint failure:
1. Increment attempt counter (e.g., 1/3 → 2/3)
2. Log in Attempt Log: `[YYYY-MM-DD HH:MM] - Error description`
3. Try a different approach

At 3/3:
1. Set status to BLOCKED
2. Document what was tried in "What Was Tried" section
3. Add "What's Needed" section
4. **STOP** - Do not continue

---

## 4. COMMIT FREQUENTLY

**Small, focused commits after each phase.**

- Each phase = one commit
- Never end a session with uncommitted changes
- Commit message format: `[FEATURE_ID] Phase N: Description`

Example commits for CORE-001:
```
[CORE-001] Phase 1: Schema & Types
[CORE-001] Phase 2: Validation with tests
[CORE-001] Phase 3: Database operations with tests
```

---

## 5. VERIFY BEFORE COMPLETING

**No task is complete until ALL checks pass.**

Before marking any phase complete:
```bash
bun test              # All tests pass
bun run lint          # No lint errors
bun run typecheck     # No type errors (if configured)
git status            # Changes committed
```

---

## 6. UPDATE TRACKING FILES

**Files must reflect reality.**

After each phase:
- [ ] CURRENT_TASK.md phase table updated
- [ ] Attempt counter reset to 0/3

After task complete:
- [ ] features/index.json status_map updated (`passes: true`)
- [ ] CURRENT_TASK.md updated to next feature
- [ ] progress.txt session summary appended

---

## 7. CONTEXT HANDOFF

**Before spawning new agent or ending long session:**

1. Create `.claude/handoff.md` from template
2. Include: current phase, last commit, key files, test command
3. New agent reads handoff.md first

---

## 8. WHEN STUCK, BLOCK

**After 3 failed attempts, STOP immediately.**

Do NOT:
- Keep trying the same approach
- Make risky changes without understanding
- Move on to another task

Do:
- Mark BLOCKED in CURRENT_TASK.md
- Document all attempts
- Explain what's needed
- Wait for human input

---

## GIT RULES

**Commit Messages:**
```
[FEATURE_ID] Phase N: Description

Example: [CORE-001] Phase 2: Validation with tests
```

**Branch Strategy:**
- Work on main for greenfield projects
- Use feature branches for established projects
- Never force push
- Never rebase published commits

---

## ERROR RECOVERY

**When things go wrong:**

1. **Read the error message completely**
   - Don't assume you know what's wrong
   - Copy the full error for reference

2. **Check recent changes**
   - `git log --oneline -5` - see recent commits
   - `git diff` - see uncommitted changes

3. **Three-attempt protocol**
   - Try 3 distinct approaches to fix
   - Log each attempt in CURRENT_TASK.md
   - After 3 failures, mark as BLOCKED

---

## Summary Checklist

Before ending any session:

```
[ ] All tests pass (bun test)
[ ] No lint errors (bun run lint)
[ ] All changes committed
[ ] CURRENT_TASK.md is accurate
[ ] git status shows clean working tree
```

---

*These rules exist because AI agents fail in predictable ways. Following them prevents wasted time and broken code.*
