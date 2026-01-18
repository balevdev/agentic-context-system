# Verification Checklists

> Use these checklists to ensure consistent quality.
> Never skip steps. Never mark complete until ALL checks pass.

---

## FEATURE VERIFICATION CHECKLIST

Run this checklist before marking any feature as complete.

### 1. ACCEPTANCE CRITERIA

```
[ ] Read each criterion from feature_list.json
[ ] Manually verify EACH criterion is met
[ ] Document verification in CURRENT_TASK.md
[ ] No criterion is partially met - all or nothing
```

### 2. AUTOMATED TESTS

```
[ ] Unit tests exist for new code
[ ] Tests cover happy path scenarios
[ ] Tests cover error/edge cases
[ ] All tests pass (exit code 0)
[ ] No tests were deleted or skipped
```

### 3. CODE QUALITY

```
[ ] Linting passes (no errors)
[ ] No debug code left (console.log, print, etc.)
[ ] No commented-out code blocks
[ ] Code follows project patterns
[ ] No hardcoded values that should be config
```

### 4. TYPE SAFETY (if applicable)

```
[ ] Type checking passes
[ ] No `any` types added
[ ] All exports have explicit types
[ ] No type assertions without justification
```

### 5. INTEGRATION

```
[ ] All imports resolve correctly
[ ] No circular dependencies introduced
[ ] Works with existing code
[ ] No breaking changes to public APIs
```

### 6. DOCUMENTATION

```
[ ] Complex logic has comments
[ ] Public functions have descriptions
[ ] CURRENT_TASK.md updated with progress
[ ] Any new patterns documented
```

---

## SESSION END CHECKLIST

Run this checklist before ending ANY session.

### Required Commands

Run each command and verify the expected output:

```bash
# 1. Run tests - must show all passing
[YOUR_TEST_COMMAND]              # Exit code 0

# 2. Type check (if applicable) - must pass
[YOUR_TYPECHECK_COMMAND]         # Exit code 0

# 3. Lint check - must pass
[YOUR_LINT_COMMAND]              # 0 errors

# 4. Git status - must be clean
git status                        # "nothing to commit, working tree clean"

# 5. Recent commits - verify your work
git log --oneline -3              # Shows your commits
```

### File Updates

```
[ ] CURRENT_TASK.md has accurate status
    - If complete: status = COMPLETED, shows commit hash
    - If in progress: status = IN_PROGRESS, shows progress
    - If blocked: status = BLOCKED, shows blockers

[ ] feature_list.json updated (if task complete)
    - Set "passes": true for completed feature

[ ] progress.txt has session summary
    - Task ID and name
    - What was completed
    - Verification results
    - What next session should do
```

### Final Verification

```
[ ] `git status` shows "nothing to commit"
[ ] All tests passing
[ ] No uncommitted changes anywhere
[ ] Next session has clear starting point
```

---

## QUICK REFERENCE

**Before starting work:**
1. Read CURRENT_TASK.md
2. Read rules.md
3. Read lessons-learned.md
4. Understand acceptance criteria

**During work:**
1. Make small changes
2. Test frequently
3. Commit working states
4. Update CURRENT_TASK.md

**Before ending session:**
1. Run full test suite
2. Commit all changes
3. Update tracking files
4. Verify clean git state

---

## COMMON MISTAKES TO AVOID

1. **Marking complete without committing**
   - Code must be committed before `passes: true`

2. **Skipping the test run**
   - Always run full test suite at session end

3. **Leaving progress.txt empty**
   - Every session needs a summary entry

4. **Forgetting to update CURRENT_TASK.md**
   - Status must reflect reality

5. **Starting new task before completing current**
   - One task at a time, always
