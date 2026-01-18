# Agentic Loop Starter

A template for AI-assisted software development with persistent context across sessions.

> **New here?** Start with the [Intro.md](./Intro.md) tutorial for a beginner-friendly walkthrough with examples.

## The Problem

Traditional AI coding assistants are **stateless by default**. Each conversation starts fresh, losing:

- Architectural decisions and design rationale
- Progress tracking and task status
- Lessons learned from previous sessions
- Quality standards and verification procedures

For complex projects spanning multiple sessions, this creates repeated context-building, inconsistent quality, and lost progress.

## The Solution

The **Agentic Loop** externalizes context, rules, and progress into persistent files that the AI reads at session start and updates at session end.

```
┌─────────────────────────────────────────────────────────────┐
│                    AGENTIC LOOP SYSTEM                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   Context Files          Task Management        Progress     │
│   ─────────────          ───────────────        ────────     │
│   • CLAUDE.md            • feature_list.json   • progress.txt│
│   • rules.md             • CURRENT_TASK.md                   │
│   • lessons-learned.md                                       │
│   • checklists.md                                            │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Quick Start

### 1. Copy this template

```bash
cp -r agentic-loop-starter your-project-name
cd your-project-name
```

### 2. Initialize git

```bash
git init
git add .
git commit -m "chore: initialize agentic loop template"
```

### 3. Customize for your project

1. **Edit `CLAUDE.md`** - Add your project's technical specifications
2. **Edit `feature_list.json`** - Define your project's phases and features
3. **Edit `.claude/static/checklists.md`** - Add your test/lint commands
4. **Start working** - The first task is already set up in `CURRENT_TASK.md`

## File Structure

```
your-project/
├── CLAUDE.md                 # Technical specifications (project-specific)
├── CURRENT_TASK.md           # Current task being worked on
├── feature_list.json         # Master list of all features
├── progress.txt              # Append-only session log
├── README.md                 # This file
└── .claude/
    ├── lessons-learned.md    # Accumulated wisdom from sessions
    ├── static/
    │   ├── rules.md          # Critical operating rules
    │   └── checklists.md     # Verification procedures
    └── templates/
        └── current-task.md   # Template for new tasks
```

## File Purposes

### Context Files (Read at session start)

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Technical specifications, architecture, coding standards |
| `.claude/static/rules.md` | Critical rules that must never be violated |
| `.claude/static/checklists.md` | Verification procedures for tasks and sessions |
| `.claude/lessons-learned.md` | Patterns and wisdom from previous sessions |

### Task Management (Updated during sessions)

| File | Purpose |
|------|---------|
| `feature_list.json` | Registry of all features with acceptance criteria |
| `CURRENT_TASK.md` | The active task with progress tracking |

### Progress Tracking (Append-only)

| File | Purpose |
|------|---------|
| `progress.txt` | Session-by-session audit trail |

## Session Workflow

### Starting a Session

1. Read `CURRENT_TASK.md` to see the active task
2. Read `.claude/static/rules.md` for operating constraints
3. Read `.claude/lessons-learned.md` for project wisdom
4. Read `CLAUDE.md` for technical context

### During a Session

1. Work on ONE task at a time
2. Make small, frequent commits
3. Update `CURRENT_TASK.md` with progress
4. Follow patterns from `CLAUDE.md`

### Ending a Session

1. Run the verification checklist from `.claude/static/checklists.md`
2. Commit all changes
3. Update `CURRENT_TASK.md` status
4. If task complete, update `feature_list.json` (`passes: true`)
5. Append session summary to `progress.txt`
6. Verify `git status` shows clean working tree

## Feature List Schema

```json
{
  "id": "PHASE-001",
  "phase": "PHASE",
  "name": "Feature Name",
  "description": "What this feature does",
  "dependencies": ["PREV-001"],
  "acceptanceCriteria": [
    "Criterion 1 that must be true",
    "Criterion 2 that must be true"
  ],
  "passes": false
}
```

**Rules:**
- Only change `passes` from `false` to `true`
- Never modify acceptance criteria
- Complete dependencies before starting a feature

## Key Principles

### 1. One Task at a Time
Never work on multiple features simultaneously. Complete or block the current task before moving to another.

### 2. Commit Frequently
Small, focused commits are better than large ones. Commit after every meaningful change.

### 3. Verify Before Completing
No task is complete until ALL acceptance criteria are met and ALL checks pass.

### 4. Document Progress
Update tracking files as you work. Future sessions depend on accurate documentation.

### 5. When Stuck, Block
If you can't proceed after 3 attempts, mark the task as BLOCKED with full context rather than making bad decisions.

## Customization Guide

### For Different Tech Stacks

Update `.claude/static/checklists.md` with your specific commands:

**Node.js/TypeScript:**
```bash
npm test
npm run typecheck
npm run lint
```

**Python:**
```bash
pytest
mypy .
ruff check .
```

**Go:**
```bash
go test ./...
go vet ./...
golangci-lint run
```

### For Different Project Types

- **Web App**: Add API endpoints to `CLAUDE.md`, component patterns, state management
- **CLI Tool**: Add command structure, argument parsing patterns
- **Library**: Add public API documentation, versioning strategy
- **Microservice**: Add service boundaries, communication patterns

## Benefits

| Benefit | How It's Achieved |
|---------|-------------------|
| **Session Independence** | Context loaded from files, not memory |
| **Quality Consistency** | Checklists enforce standards |
| **Progress Visibility** | Feature list and progress log track everything |
| **Knowledge Accumulation** | Lessons learned persist and grow |
| **Safe Failure** | Blocked state prevents bad autonomous decisions |
| **Audit Trail** | Append-only log documents all sessions |

## Inspiration

This template is based on practices from:

- **[Ralph: The AI Agent Whisperer](https://ghuntley.com/ralph/)** - Geoffrey Huntley's exploration of effective AI agent collaboration
- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** - Anthropic's engineering guide on building systems for AI agents

## Contributing

This template is designed to be forked and customized. If you have improvements that would benefit everyone, feel free to submit a PR.

## License

MIT - Use this template for any project.
