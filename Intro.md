# Introduction to the Agentic Context System

A beginner's guide to AI-assisted software development that actually works.

---

## Table of Contents

1. [What Is This?](#what-is-this)
2. [The Problem We're Solving](#the-problem-were-solving)
3. [How It Works](#how-it-works)
4. [Tutorial: Building a Todo API](#tutorial-building-a-todo-api)
5. [Example Session Walkthrough](#example-session-walkthrough)
6. [Tips for Success](#tips-for-success)
7. [Inspiration & Further Reading](#inspiration--further-reading)

---

## What Is This?

Imagine having a coding assistant that:
- **Remembers** everything about your project between conversations
- **Follows** consistent quality standards every time
- **Tracks** what's done and what's next with phase-level detail
- **Learns** from mistakes and never repeats them
- **Stops** when stuck instead of making things worse

That's what the Agentic Context System enables.

**In simple terms:** It's a set of files that give AI assistants (like Claude, ChatGPT, or Cursor) a "memory", "rulebook", and "safety net" for your project.

---

## The Problem We're Solving

### Without the System

Every time you start a new conversation with an AI assistant:

```
You: "Continue working on the todo API feature"
AI:  "I'd be happy to help! Can you tell me about your project structure,
      what framework you're using, what you've already built, and where
      you left off?"
```

You spend 10-15 minutes re-explaining everything. Every. Single. Time.

And even after explaining:
- The AI might use different coding patterns than before
- It doesn't know about that bug you fixed last week
- It might redo work that's already done
- Quality varies wildly between sessions
- If stuck, it might spin for hours trying the same thing

### With the System

```
You: "Continue working on the todo API feature"
AI:  *reads project files*
     "I see from CURRENT_TASK.md that CORE-001 is in progress, Phase 2.
      You've completed the types and validation. Attempt 1/3 failed on
      a test. I'll follow the patterns from CLAUDE.md and the fix from
      lessons-learned.md. Let me continue from where the last attempt
      left off..."
```

The AI picks up exactly where you left off, follows your standards, and stops safely if stuck.

---

## How It Works

### The Core Idea

Instead of keeping context in your head (or the AI's temporary memory), you store it in files:

```
your-project/
├── CLAUDE.md              ← "Here's how this project works"
├── Prompt.md              ← "Here's the execution protocol"
├── CURRENT_TASK.md        ← "Here's what we're working on now"
├── progress.txt           ← "Here's what we've done"
├── features/
│   ├── index.json         ← "Here's the status of everything"
│   ├── backlog.json       ← "Here's what's planned"
│   └── active/            ← "Here's the current feature details"
└── .claude/
    ├── rules.md           ← "Here's what you must always do"
    ├── checklists.md      ← "Here's how to verify quality"
    ├── lessons-learned.md ← "Here's what we've learned"
    └── templates/
        └── HANDOFF.md     ← "Here's how to hand off to new agents"
```

### The Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                     THE AGENTIC LOOP                             │
│                                                                  │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐      │
│   │  READ   │───▶│  WORK   │───▶│ VERIFY  │───▶│ COMMIT  │      │
│   │ Context │    │ Phase N │    │ & Test  │    │ Phase N │      │
│   └─────────┘    └─────────┘    └─────────┘    └─────────┘      │
│        ▲                              │              │           │
│        │                         FAIL?│              ▼           │
│        │                              │        Update Task       │
│        │                              ▼              │           │
│        │                        [Attempt++]    Phase N+1?       │
│        │                              │              │           │
│        │                         3 attempts?         │           │
│        │                              │              │           │
│        │                              ▼              │           │
│        │                          BLOCKED            │           │
│        │                                             │           │
│        └─────────────────────────────────────────────┘           │
└─────────────────────────────────────────────────────────────────┘
```

1. **Read**: AI reads the context files to understand the project
2. **Work**: AI works on the current phase of the task
3. **Verify**: AI runs tests - if fail, try again (max 3 attempts)
4. **Commit**: AI commits the phase work
5. **Repeat**: Move to next phase, or next feature if complete

---

## Tutorial: Building a Todo API

Let's walk through setting up the Agentic Context System for a Bun + Hono REST API.

### Step 1: Copy the Template

```bash
git clone https://github.com/balevdev/agentic-context-system.git todo-api
cd todo-api
rm -rf .git && git init
```

### Step 2: Review the Pre-configured Setup

The template comes with a Bun + Hono example. Let's look at the key files:

**CLAUDE.md** - Technical specifications:
```markdown
## PROJECT OVERVIEW

Project Name: todo-api
Description: A REST API for managing tasks
Tech Stack: Bun, Hono, SQLite (via Bun's built-in sqlite)

## ARCHITECTURE

todo-api/
├── src/
│   ├── index.ts          # Entry point, Hono app setup
│   ├── routes/
│   │   └── todos.ts      # Todo CRUD routes
│   ├── db/
│   │   └── index.ts      # Database connection
│   └── types/
│       └── todo.ts       # TypeScript interfaces
├── tests/
└── package.json
```

**features/index.json** - Session planning:
```json
{
  "meta": {
    "project": "todo-api",
    "next_priority": "SETUP-001",
    "session_plan": {
      "1": {
        "name": "Foundation",
        "features": ["SETUP-001", "SETUP-002"],
        "token_budget": 50000
      }
    }
  },
  "status_map": {
    "SETUP-001": { "passes": false, "session": 1 }
  }
}
```

**CURRENT_TASK.md** - Active task with phases:
```markdown
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

## Acceptance Criteria

- [ ] package.json exists with bun as runtime
- [ ] Hono installed and configured
- [ ] Basic health check endpoint works
```

### Step 3: Customize the Checklists

Edit `.claude/static/checklists.md` to use Bun commands:

```bash
# Phase verification
bun test                      # All tests pass
bun run typecheck             # No type errors
bun run lint                  # No lint errors
```

### Step 4: Start Working

Now when you talk to your AI assistant, start with:

```
"Read my project files (CLAUDE.md, Prompt.md, CURRENT_TASK.md, and
features/) and continue working on the current task."
```

The AI will:
1. Read all the context
2. See that SETUP-001 Phase 1 is current
3. Know exactly what needs to be done
4. Follow your coding standards
5. Commit after each phase
6. Update tracking files

---

## Example Session Walkthrough

Here's what a real session looks like:

### Session 1: Project Setup

**You:** "Read my project files and work on the current task."

**AI does:**

1. Reads CLAUDE.md → understands it's a Bun + Hono API project
2. Reads Prompt.md → understands phase gate rules
3. Reads CURRENT_TASK.md → sees SETUP-001 Phase 1 is active
4. Reads rules.md → knows to commit after each phase

**Phase 1 - Schema & Types:**
- Creates package.json with Bun + Hono
- Creates tsconfig.json with strict mode
- Runs verification: `bun run typecheck` → passes
- Commits: `[SETUP-001] Phase 1: Schema & Types`
- Updates CURRENT_TASK.md phase table

**Phase 2 - Core Setup:**
- Creates src/index.ts with Hono app
- Adds health check endpoint
- Runs verification: `bun test` → passes
- Commits: `[SETUP-001] Phase 2: Core Setup`

**Phase 3 - Verification:**
- Verifies server starts
- Tests health endpoint
- Commits: `[SETUP-001] Phase 3: Verification`
- Updates features/index.json: `SETUP-001.passes: true`
- Updates CURRENT_TASK.md to SETUP-002

**The key:** Each phase has a commit. Progress is verifiable.

### Session 2: Next Day - With Failure

**You:** "Continue working on the project."

**AI does:**

1. Reads CURRENT_TASK.md → sees SETUP-002 Phase 2
2. Reads Prompt.md → knows the attempt rules
3. Working on linting setup...
4. Runs `bun run lint` → **FAILS** (missing config)

**What happens:**
- Increments: Attempts 0/3 → 1/3
- Logs: `[2024-01-16 10:30] - bun run lint failed: missing .eslintrc`
- Fixes the config
- Runs `bun run lint` → passes
- Commits and continues

**If it had failed 3 times:**
```markdown
**Status:** BLOCKED

### Attempt Log
[2024-01-16 10:30] - lint failed: missing config
[2024-01-16 10:45] - lint failed: parser error
[2024-01-16 11:00] - lint failed: plugin conflict

### What's Needed
Human review of ESLint + TypeScript-ESLint version compatibility
```

---

## Tips for Success

### 1. Be Specific in Acceptance Criteria

**Bad:**
```json
"acceptance_criteria": ["API works"]
```

**Good:**
```json
"acceptance_criteria": [
  "GET /todos returns array of todos",
  "POST /todos creates and returns new todo",
  "GET /todos/:id returns single todo or 404",
  "All endpoints have integration tests"
]
```

### 2. Start Sessions with Context Loading

Always begin with something like:
- "Read CLAUDE.md, Prompt.md, and CURRENT_TASK.md, then continue"
- "Check the project files and pick up where we left off"
- "Load the agentic context and work on the current task"

### 3. Don't Skip Phase Gates

The system only works if phases are committed. If you manually complete something, update the files yourself.

### 4. Add to Lessons Learned

When something goes wrong and you fix it:

```markdown
## Testing

### 1. BUN TEST NEEDS --preload FOR SETUP

When using test fixtures:

```typescript
// Good - preload setup
// bunfig.toml: preload = ["./tests/setup.ts"]

// Bad - import in each test
import './setup' // runs multiple times
```

Discovered in CORE-001 when tests polluted each other.
```

### 5. Keep Features Small

Each feature should be completable in one session. If bigger, split it:

**Too big:**
```
CORE-001: Build the entire API
```

**Just right:**
```
CORE-001: Database layer with SQLite
CORE-002: Todo CRUD routes
CORE-003: Error handling & validation
```

---

## Inspiration & Further Reading

### Ralph: The AI Agent Whisperer
**https://ghuntley.com/ralph/**

Geoffrey Huntley's exploration of how to effectively work with AI agents.

### Effective Harnesses for Long-Running Agents
**https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents**

Anthropic's engineering guide on building systems that help AI agents work effectively over extended periods.

---

## What's Next?

1. **Read the [README.md](./README.md)** for the complete file reference
2. **Clone this template** to your own project
3. **Customize the files** for your specific needs
4. **Start your first session** with an AI assistant

The more you use it, the better it gets. Your lessons-learned.md grows, your patterns solidify, and each session becomes more productive than the last.

---

## Quick Reference Card

```
┌────────────────────────────────────────────────────────────────┐
│                    AGENTIC CONTEXT CHEAT SHEET                  │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  START SESSION:                                                 │
│  1. AI reads: CLAUDE.md, Prompt.md, CURRENT_TASK.md, features/ │
│  2. AI identifies current phase from CURRENT_TASK.md            │
│  3. AI begins work                                              │
│                                                                 │
│  DURING SESSION (per phase):                                    │
│  1. Build the phase deliverables                                │
│  2. Run: bun test && bun run lint                               │
│  3. If pass: Commit [FEATURE_ID] Phase N: Description           │
│  4. If fail: Attempt++, fix, retry (max 3)                      │
│  5. Update CURRENT_TASK.md phase table                          │
│                                                                 │
│  END SESSION:                                                   │
│  1. All phases committed                                        │
│  2. CURRENT_TASK.md accurate                                    │
│  3. features/index.json updated (if complete)                   │
│  4. git status clean                                            │
│                                                                 │
│  IF STUCK (3 failures):                                         │
│  1. Mark BLOCKED in CURRENT_TASK.md                             │
│  2. Log all attempts                                            │
│  3. Explain what's needed                                       │
│  4. STOP - wait for human                                       │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

---

*Happy building!*
