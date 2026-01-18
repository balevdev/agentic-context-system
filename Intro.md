# Introduction to the Agentic Loop

A beginner's guide to AI-assisted software development that actually works.

---

## Table of Contents

1. [What Is This?](#what-is-this)
2. [The Problem We're Solving](#the-problem-were-solving)
3. [How the Agentic Loop Works](#how-the-agentic-loop-works)
4. [Tutorial: Building a Todo CLI App](#tutorial-building-a-todo-cli-app)
5. [Example Session Walkthrough](#example-session-walkthrough)
6. [Tips for Success](#tips-for-success)
7. [Inspiration & Further Reading](#inspiration--further-reading)

---

## What Is This?

Imagine having a coding assistant that:
- **Remembers** everything about your project between conversations
- **Follows** consistent quality standards every time
- **Tracks** what's done and what's next automatically
- **Learns** from mistakes and never repeats them

That's what the Agentic Loop enables.

**In simple terms:** It's a set of files that give AI assistants (like Claude, ChatGPT, or Cursor) a "memory" and "rulebook" for your project.

---

## The Problem We're Solving

### Without the Agentic Loop

Every time you start a new conversation with an AI assistant:

```
You: "Continue working on the user authentication feature"
AI:  "I'd be happy to help! Can you tell me about your project structure,
      what framework you're using, what you've already built, what the
      requirements are, and where you left off?"
```

You spend 10-15 minutes re-explaining everything. Every. Single. Time.

And even after explaining:
- The AI might use different coding patterns than before
- It doesn't know about that bug you fixed last week
- It might redo work that's already done
- Quality varies wildly between sessions

### With the Agentic Loop

```
You: "Continue working on the user authentication feature"
AI:  *reads project files*
     "I see from CURRENT_TASK.md that AUTH-002 is in progress. You've
      completed the login endpoint and tests. Next up is the password
      reset flow. I'll follow the patterns from CLAUDE.md and avoid
      the bcrypt issue noted in lessons-learned.md. Let me continue..."
```

The AI picks up exactly where you left off, follows your standards, and avoids known pitfalls.

---

## How the Agentic Loop Works

### The Core Idea

Instead of keeping context in your head (or the AI's temporary memory), you store it in files:

```
your-project/
├── CLAUDE.md              ← "Here's how this project works"
├── feature_list.json      ← "Here's what we're building"
├── CURRENT_TASK.md        ← "Here's what we're working on now"
├── progress.txt           ← "Here's what we've done"
└── .claude/
    ├── rules.md           ← "Here's what you must always do"
    ├── checklists.md      ← "Here's how to verify quality"
    └── lessons-learned.md ← "Here's what we've learned"
```

### The Workflow

```
┌─────────────────────────────────────────────────────────────────┐
│                     THE AGENTIC LOOP                             │
│                                                                  │
│   ┌─────────┐      ┌─────────┐      ┌─────────┐      ┌───────┐  │
│   │  READ   │ ──── │  WORK   │ ──── │ VERIFY  │ ──── │UPDATE │  │
│   │ Context │      │ On Task │      │ Quality │      │ Files │  │
│   └─────────┘      └─────────┘      └─────────┘      └───────┘  │
│        │                                                  │      │
│        └──────────────────────────────────────────────────┘      │
│                          (repeat)                                │
└─────────────────────────────────────────────────────────────────┘
```

1. **Read**: AI reads the context files to understand the project
2. **Work**: AI works on the current task
3. **Verify**: AI runs tests and checks quality
4. **Update**: AI updates tracking files with progress
5. **Repeat**: Next session picks up where this one ended

---

## Tutorial: Building a Todo CLI App

Let's walk through setting up the Agentic Loop for a real project: a command-line todo list application in Python.

### Step 1: Copy the Template

```bash
cp -r agentic-loop-starter todo-cli
cd todo-cli
```

### Step 2: Define Your Project in CLAUDE.md

Replace the template content with your project specifics:

```markdown
# CLAUDE.md - Todo CLI Technical Reference

## PROJECT OVERVIEW

### What We're Building

A command-line todo list application in Python.

```
Project Name: todo-cli
Description: A simple but powerful CLI for managing tasks
Tech Stack: Python 3.11+, Click (CLI framework), SQLite (storage)
```

### Core Principles

1. **Simple First**: Start with basic features, add complexity later
2. **Test Everything**: Every feature has tests before it's complete
3. **User-Friendly**: Clear error messages, helpful --help text

## ARCHITECTURE

### Directory Structure

```
todo-cli/
├── src/
│   └── todo/
│       ├── __init__.py
│       ├── cli.py          # Click commands
│       ├── database.py     # SQLite operations
│       └── models.py       # Data classes
├── tests/
│   ├── test_cli.py
│   └── test_database.py
├── pyproject.toml
└── README.md
```

## CODING STANDARDS

- Use type hints everywhere
- Format with `black`
- Lint with `ruff`
- Test with `pytest`

## COMMANDS

```bash
# Development
pip install -e ".[dev]"    # Install in dev mode
pytest                      # Run tests
black .                     # Format code
ruff check .               # Lint code
```
```

### Step 3: Define Your Features in feature_list.json

Replace the example with your actual features:

```json
{
  "project": {
    "name": "todo-cli",
    "version": "0.1.0",
    "description": "Command-line todo list application"
  },
  "phases": [
    {
      "id": "SETUP",
      "name": "Project Setup",
      "description": "Initialize the Python project",
      "features": [
        {
          "id": "SETUP-001",
          "phase": "SETUP",
          "name": "Initialize Python Project",
          "description": "Create pyproject.toml and basic structure",
          "dependencies": [],
          "acceptanceCriteria": [
            "pyproject.toml exists with project metadata",
            "src/todo/__init__.py exists",
            "pytest runs (even with 0 tests)",
            "black and ruff are configured"
          ],
          "passes": false
        }
      ]
    },
    {
      "id": "CORE",
      "name": "Core Features",
      "description": "Basic todo functionality",
      "features": [
        {
          "id": "CORE-001",
          "phase": "CORE",
          "name": "Database Layer",
          "description": "SQLite database for storing todos",
          "dependencies": ["SETUP-001"],
          "acceptanceCriteria": [
            "Can create a new database file",
            "Can add a todo item",
            "Can list all todo items",
            "Can mark a todo as complete",
            "Can delete a todo",
            "All operations have unit tests"
          ],
          "passes": false
        },
        {
          "id": "CORE-002",
          "phase": "CORE",
          "name": "CLI Commands",
          "description": "Click commands for all operations",
          "dependencies": ["CORE-001"],
          "acceptanceCriteria": [
            "todo add 'task' works",
            "todo list shows all tasks",
            "todo done <id> marks complete",
            "todo remove <id> deletes task",
            "--help works for all commands",
            "CLI has integration tests"
          ],
          "passes": false
        }
      ]
    }
  ]
}
```

### Step 4: Update the Checklists

Edit `.claude/static/checklists.md` to use Python commands:

```markdown
## SESSION END CHECKLIST

### Required Commands

```bash
# 1. Run tests - must show all passing
pytest                        # Exit code 0

# 2. Format check - must pass
black --check .               # Exit code 0

# 3. Lint check - must pass
ruff check .                  # 0 errors

# 4. Git status - must be clean
git status                    # "nothing to commit"
```
```

### Step 5: Set Up the First Task

Your `CURRENT_TASK.md` should point to SETUP-001:

```markdown
# SETUP-001: Initialize Python Project

**Status:** NOT_STARTED | **Previous:** None (first task)

## Acceptance Criteria

- [ ] pyproject.toml exists with project metadata
- [ ] src/todo/__init__.py exists
- [ ] pytest runs (even with 0 tests)
- [ ] black and ruff are configured

## Progress

- (not started)

## Next

After setup is complete, proceed to CORE-001: Database Layer
```

### Step 6: Start Working

Now when you talk to your AI assistant, start with:

```
"Read my project files (CLAUDE.md, CURRENT_TASK.md, feature_list.json,
and the .claude/ directory) and continue working on the current task."
```

The AI will:
1. Read all the context
2. See that SETUP-001 is the current task
3. Know exactly what needs to be done
4. Follow your coding standards
5. Update the tracking files when done

---

## Example Session Walkthrough

Here's what a real session looks like:

### Session 1: Project Setup

**You:** "Read my project files and work on the current task."

**AI does:**
1. Reads CLAUDE.md → understands it's a Python CLI project
2. Reads CURRENT_TASK.md → sees SETUP-001 is active
3. Reads rules.md → knows to commit frequently, test everything
4. Creates pyproject.toml, src/todo/__init__.py
5. Configures black and ruff
6. Runs pytest (passes with 0 tests)
7. Commits the changes
8. Updates CURRENT_TASK.md:
   ```markdown
   **Status:** COMPLETED | **Previous:** None

   ## Acceptance Criteria
   - [x] pyproject.toml exists with project metadata
   - [x] src/todo/__init__.py exists
   - [x] pytest runs (even with 0 tests)
   - [x] black and ruff are configured
   ```
9. Updates feature_list.json: sets SETUP-001 `passes: true`
10. Appends to progress.txt:
    ```
    ================================================================================
    SESSION: 2024-01-15 - SETUP-001 COMPLETED
    ================================================================================

    TASK: SETUP-001 - Initialize Python Project
    STATUS: COMPLETED

    COMPLETED:
    - Created pyproject.toml with Click, pytest, black, ruff
    - Created src/todo/__init__.py
    - Verified pytest runs successfully
    - Committed as: abc1234

    NEXT SESSION SHOULD:
    - Start CORE-001: Database Layer
    ================================================================================
    ```

### Session 2: Database Layer (Next Day)

**You:** "Continue working on the project."

**AI does:**
1. Reads CURRENT_TASK.md → sees SETUP-001 is done
2. Reads feature_list.json → finds CORE-001 is next
3. Reads progress.txt → sees what was done yesterday
4. Updates CURRENT_TASK.md to show CORE-001
5. Implements the database layer with tests
6. Runs pytest, black, ruff
7. Commits and updates all tracking files

**The key:** The AI knew exactly where to start without you explaining anything.

---

## Tips for Success

### 1. Be Specific in Your Feature Criteria

**Bad:**
```json
"acceptanceCriteria": ["Database works"]
```

**Good:**
```json
"acceptanceCriteria": [
  "Can create a new database file",
  "Can add a todo item with title and optional description",
  "Can list all todo items sorted by creation date",
  "Returns empty list when no todos exist",
  "All operations have unit tests with >80% coverage"
]
```

### 2. Start Sessions with Context Loading

Always begin with something like:
- "Read CLAUDE.md and CURRENT_TASK.md, then continue the work"
- "Check the project files and pick up where we left off"
- "Load the agentic loop context and work on the current task"

### 3. Don't Skip the Tracking Updates

The system only works if files are kept up to date. If you manually complete something, update the files yourself.

### 4. Add to Lessons Learned

When something goes wrong and you fix it, add it to `.claude/lessons-learned.md`:

```markdown
## Database

1. ALWAYS USE CONTEXT MANAGERS FOR SQLITE
   - Bad: `conn = sqlite3.connect(db); conn.execute(...)`
   - Good: `with sqlite3.connect(db) as conn: conn.execute(...)`
   - Discovered in CORE-001 when database locked
```

### 5. Keep Features Small

Each feature should be completable in one session (roughly 1-2 hours of work). If it's bigger, split it:

**Too big:**
```
CORE-001: Build the entire backend
```

**Just right:**
```
CORE-001: Database schema and connection
CORE-002: CRUD operations for todos
CORE-003: CLI commands for todos
```

---

## Inspiration & Further Reading

This template is inspired by real-world practices for working with AI coding assistants:

### Ralph: The AI Agent Whisperer
**https://ghuntley.com/ralph/**

Geoffrey Huntley's exploration of how to effectively work with AI agents, including the importance of structured context and clear task definitions.

### Effective Harnesses for Long-Running Agents
**https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents**

Anthropic's engineering guide on building systems that help AI agents work effectively over extended periods. Key insights on context management, verification, and error recovery that directly influenced this template.

---

## What's Next?

1. **Read the [README.md](./README.md)** for the complete file reference
2. **Copy this template** to your own project
3. **Customize the files** for your specific needs
4. **Start your first session** with an AI assistant

The more you use it, the better it gets. Your `lessons-learned.md` grows, your patterns solidify, and each session becomes more productive than the last.

---

## Quick Reference Card

```
┌────────────────────────────────────────────────────────────────┐
│                    AGENTIC LOOP CHEAT SHEET                    │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  START SESSION:                                                │
│  1. AI reads: CLAUDE.md, CURRENT_TASK.md, rules.md, lessons    │
│  2. AI identifies current task from feature_list.json          │
│  3. AI begins work                                             │
│                                                                │
│  DURING SESSION:                                               │
│  • One task at a time                                          │
│  • Commit after each meaningful change                         │
│  • Update CURRENT_TASK.md with progress                        │
│                                                                │
│  END SESSION:                                                  │
│  1. Run all tests ─────────────────────── must pass            │
│  2. Run linter ────────────────────────── must pass            │
│  3. Commit all changes ────────────────── git status clean     │
│  4. Update CURRENT_TASK.md status                              │
│  5. Update feature_list.json (if complete)                     │
│  6. Append to progress.txt                                     │
│                                                                │
│  IF STUCK:                                                     │
│  • Try 3 different approaches                                  │
│  • If still stuck, mark as BLOCKED                             │
│  • Document what was tried and what's needed                   │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

---

*Happy building!*
