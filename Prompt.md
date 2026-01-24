# Session Protocol

## Startup

```
1. CURRENT_TASK.md → Active feature
2. features/index.json → Check status_map for dependency passes
3. features/active/FEATURE_ID.json → Get full feature definition
4. Read by task type (CLAUDE.md for patterns)
```

## Phase Gate Rule

After completing Phase N:
1. Run: `bun test && bun run lint`
2. If pass: Commit `[FEATURE_ID] Phase N: Description`
3. If fail: Fix before Phase N+1 (counts toward 3-attempt limit)
4. Update CURRENT_TASK.md:
   - Set Phase N status to `complete` with commit hash
   - Increment Current Phase to N+1
   - Reset Attempts to 0/3

## Attempt Tracking

On any test/lint failure:
1. Increment attempt counter in CURRENT_TASK.md (e.g., 1/3 → 2/3)
2. Log error in Attempt Log: `[YYYY-MM-DD HH:MM] - Error description`
3. At 3/3: Set status to BLOCKED, document all attempts, STOP work

## Execution Loop

```
FOR each phase:
  1. Design (flow, components)
  2. Build (follow patterns from CLAUDE.md)
  3. Test
  4. Verify: bun test && bun run lint
  5. If fail: increment attempt, fix, retry
  6. If pass: COMMIT [FEATURE_ID] Phase N: Description
  7. Update CURRENT_TASK.md phase table
```

## Phase Completion Tracking

After each phase:
- [ ] Phase N tasks complete
- [ ] Tests for Phase N passing
- [ ] Ready to start Phase N+1

## Blocking

After 3 failed attempts:
1. CURRENT_TASK.md → Status: BLOCKED
2. Document all attempts + errors in Attempt Log
3. Add "What's Needed" section explaining the blocker
4. STOP, wait for human

## Context Handoff

Before spawning new context or handing off to another agent:
1. Create `.claude/handoff.md` from `.claude/templates/HANDOFF.md`
2. Fill in: current phase, last commit, key files, test command
3. New agent reads handoff.md first before CURRENT_TASK.md

## End Session

```bash
git status    # Clean
bun test      # Pass
bun run lint  # Pass
```

### Update Tracking Files

1. **features/index.json:**
   - Set `status_map[FEATURE_ID].passes: true`
   - Update `meta.completed_count`
   - Update `meta.next_priority`

2. **CURRENT_TASK.md:**
   - Update to next feature

3. **progress.txt:**
   - Append session summary
