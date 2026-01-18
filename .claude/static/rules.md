# Rules Reference

> Critical rules that govern autonomous AI operation.
> These rules are NEVER to be violated.

---

## CRITICAL RULES - NEVER VIOLATE

1. **ONE TASK AT A TIME**
   - Never work on more than one feature per session
   - Complete the current task before starting another
   - If blocked, mark as BLOCKED rather than switching tasks

2. **COMMIT FREQUENTLY**
   - Commit after every meaningful change
   - Small, focused commits are better than large ones
   - Never end a session with uncommitted changes

3. **ALWAYS UPDATE PROGRESS**
   - Update CURRENT_TASK.md as you work
   - Append to progress.txt before ending session
   - Document what was done and what comes next

4. **NEVER BREAK EXISTING TESTS**
   - Run tests before and after changes
   - If tests fail, fix them before proceeding
   - Never delete or skip tests to make them pass

5. **LEAVE CODEBASE CLEAN**
   - `git status` must show "nothing to commit" at session end
   - All tests must pass
   - No linting errors

---

## WORKFLOW RULES

1. **READ BEFORE WRITING**
   - Always read a file before modifying it
   - Understand existing code before making changes
   - Check for existing patterns to follow

2. **MAKE SMALL INCREMENTAL CHANGES**
   - Prefer multiple small changes over one large change
   - Test after each change
   - Commit working states frequently

3. **TEST IMMEDIATELY**
   - Write tests for new functionality
   - Run tests after every change
   - Fix broken tests before moving on

4. **DOCUMENT AS YOU GO**
   - Update CURRENT_TASK.md with progress
   - Add comments for non-obvious code
   - Keep progress.txt up to date

---

## STOP CONDITIONS

**Mark task as BLOCKED if:**

- You need human input on design decisions
- A dependency doesn't exist or isn't clear
- Tests fail after 3 distinct fix attempts
- Changes would affect more than 5 files unexpectedly
- Requirements are ambiguous or contradictory
- You discover a security concern

**When blocked:**
- Document what was attempted (minimum 3 approaches)
- Explain what specifically is blocking progress
- Describe what input is needed
- Note any partial progress made

---

## COMPLETION CONDITIONS

**A task is ONLY complete when ALL of these are true:**

- [ ] All acceptance criteria from feature_list.json are met
- [ ] All tests pass (exit code 0)
- [ ] Type checking passes (if applicable)
- [ ] Linting passes (no errors)
- [ ] Code is committed to git
- [ ] CURRENT_TASK.md shows COMPLETED status
- [ ] progress.txt has session summary
- [ ] `git status` shows clean working tree

---

## GIT RULES

**Commit Messages:**
```
type(scope): short description

Types: feat, fix, docs, style, refactor, test, chore
Example: feat(auth): add login validation
```

**Branch Strategy:**
- Work on main for greenfield projects
- Use feature branches for established projects
- Never force push
- Never rebase published commits

**Reverting:**
- Use `git revert` to undo changes
- Never use `git reset --hard` on shared history

---

## ERROR RECOVERY

**When things go wrong:**

1. **Read the error message completely**
   - Don't assume you know what's wrong
   - Copy the full error for reference

2. **Check recent changes**
   - `git log --oneline -5` - see recent commits
   - `git diff` - see uncommitted changes
   - `git diff HEAD~1` - see last commit's changes

3. **Three-attempt protocol**
   - Try 3 distinct approaches to fix
   - Document each attempt
   - After 3 failures, mark as BLOCKED

4. **When stuck after 3 attempts**
   - Revert to last working state if needed
   - Document the issue thoroughly
   - Mark as BLOCKED with full context

---

## REMEMBER

- Quality over speed
- Working code over complete code
- Documented progress over silent progress
- Small commits over large commits
- Asking for help over making assumptions
