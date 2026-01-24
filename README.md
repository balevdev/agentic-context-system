# Agentic Context System

A template for AI-assisted software development with persistent context, phase gates, and attempt tracking.

> **New here?** Start with the [Intro.md](./Intro.md) tutorial for a beginner-friendly walkthrough with examples.

## The Problem

Traditional AI coding assistants are **stateless by default**. Each conversation starts fresh, losing:

- Architectural decisions and design rationale
- Progress tracking and task status
- Lessons learned from previous sessions
- Quality standards and verification procedures

For complex projects spanning multiple sessions, this creates repeated context-building, inconsistent quality, and lost progress.

## The Solution

The **Agentic Context System** externalizes context, rules, and progress into persistent files that the AI reads at session start and updates at session end.

```
┌─────────────────────────────────────────────────────────────────┐
│                    AGENTIC CONTEXT SYSTEM                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Context Files          Feature Registry      Execution         │
│   ─────────────          ────────────────      ─────────         │
│   • CLAUDE.md            • features/index.json • Prompt.md       │
│   • rules.md             • features/active/    • CURRENT_TASK.md │
│   • lessons-learned.md   • features/backlog.json                 │
│   • checklists.md                                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## Quick Start

### 1. Copy this template

```bash
git clone https://github.com/balevdev/agentic-context-system.git todo-api
cd todo-api
rm -rf .git && git init
```

### 2. Initialize git

```bash
git add .
git commit -m "chore: initialize agentic context system"
```

### 3. Customize for your project

1. **Edit `CLAUDE.md`** - Add your project's technical specifications
2. **Edit `features/index.json`** - Define your session plan
3. **Create `features/active/FEATURE_ID.json`** - Add your first feature
4. **Edit `.claude/static/checklists.md`** - Add your test/lint commands
5. **Start working** - The first task is already set up in `CURRENT_TASK.md`

## File Structure

```
your-project/
├── CLAUDE.md                     # Project bible (tech specs, architecture)
├── CURRENT_TASK.md               # Active task with phase tracking
├── Prompt.md                     # Session execution protocol
├── progress.txt                  # Append-only session log
├── features/
│   ├── index.json                # Lightweight status map + session plan
│   ├── backlog.json              # Slim list (id, name, deps only)
│   └── active/                   # Full definitions for active features
│       └── SETUP-001.json
└── .claude/
    ├── lessons-learned.md        # Accumulated wisdom from sessions
    ├── commands/
    │   └── work.md               # Planning mode with implementation hints
    ├── templates/
    │   └── HANDOFF.md            # Context spawn template
    └── static/
        ├── rules.md              # Critical operating rules
        └── checklists.md         # Phase-specific verification
```

## File Purposes

### Context Files (Read at session start)

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Technical specifications, architecture, coding standards |
| `Prompt.md` | Execution protocol: phase gates, attempt tracking, blocking |
| `.claude/static/rules.md` | Critical rules that must never be violated |
| `.claude/lessons-learned.md` | Patterns and wisdom from previous sessions |

### Feature Registry (Status tracking)

| File | Purpose |
|------|---------|
| `features/index.json` | Lightweight status map + session planning |
| `features/backlog.json` | Slim list of all features (id, name, deps) |
| `features/active/` | Full feature definitions for current work |

### Execution Files (Updated during sessions)

| File | Purpose |
|------|---------|
| `CURRENT_TASK.md` | Active task with phase table, attempts, blockers |
| `.claude/templates/HANDOFF.md` | Context transfer template for new agents |
| `progress.txt` | Append-only session audit trail |

## Key Concepts

### Phase Gates

Every feature is broken into phases. You can't move to Phase N+1 until Phase N passes tests and is committed.

```
Phase N → Test → Pass → Commit → Phase N+1
              ↓
           Fail → Fix (counts as attempt) → Retry
```

### Attempt Tracking

Each test failure increments an attempt counter. At 3 attempts, the task is marked BLOCKED.

```
Attempt 1/3 → Fail → Fix
Attempt 2/3 → Fail → Fix
Attempt 3/3 → Fail → BLOCKED → Wait for human
```

### Session Planning

Features are grouped into sessions with token budgets:

```json
"session_plan": {
  "1": {
    "name": "Foundation",
    "features": ["SETUP-001", "SETUP-002"],
    "token_budget": 50000
  }
}
```

## Session Workflow

### Starting a Session

1. Read `CURRENT_TASK.md` to see the active task and phase
2. Read `Prompt.md` for the execution protocol
3. Read `.claude/static/rules.md` for operating constraints
4. Read `.claude/lessons-learned.md` for project wisdom
5. Read `CLAUDE.md` for technical context

### During a Session (for each phase)

1. Build the phase deliverables
2. Run tests: `bun test && bun run lint`
3. If pass: Commit `[FEATURE_ID] Phase N: Description`
4. If fail: Fix (increment attempt counter), retry
5. At 3 failures: BLOCKED, stop
6. Update CURRENT_TASK.md phase table
7. Move to next phase

### Ending a Session

1. Run verification: `bun test && bun run lint`
2. Commit all changes
3. Update CURRENT_TASK.md status
4. If task complete: update features/index.json (`passes: true`)
5. Append session summary to progress.txt
6. Verify `git status` shows clean working tree

## Example: Bun + Hono Todo API

The template comes pre-configured with a Bun + Hono example project.

**CLAUDE.md** defines a REST API:
- Bun runtime with Hono framework
- SQLite database (Bun's built-in)
- TypeScript with strict mode

**features/active/SETUP-001.json** defines the first task:
- Initialize package.json
- Configure TypeScript
- Create basic Hono app with health check

**Verification commands** (in checklists.md):
```bash
bun test              # Run tests
bun run typecheck     # Type check
bun run lint          # Lint
```

## Customization

### For Different Tech Stacks

Update `.claude/static/checklists.md` with your commands:

**Node.js/TypeScript:**
```bash
npm test && npm run typecheck && npm run lint
```

**Python:**
```bash
pytest && mypy . && ruff check .
```

**Go:**
```bash
go test ./... && go vet ./... && golangci-lint run
```

### For Different Project Types

- **Web App**: Add API endpoints, component patterns, state management
- **CLI Tool**: Add command structure, argument parsing
- **Library**: Add public API docs, versioning strategy
- **Microservice**: Add service boundaries, communication patterns

## Benefits

| Benefit | How It's Achieved |
|---------|-------------------|
| **Session Independence** | Context loaded from files, not memory |
| **Phase Accountability** | Commits after each phase prove progress |
| **Safe Failure** | 3-attempt limit prevents spinning |
| **Context Handoff** | HANDOFF.md preserves state for new agents |
| **Knowledge Accumulation** | Lessons learned persist and grow |
| **Audit Trail** | progress.txt documents all sessions |

## Inspiration

This template is based on practices from:

- **[Ralph: The AI Agent Whisperer](https://ghuntley.com/ralph/)** - Geoffrey Huntley's exploration of effective AI agent collaboration
- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** - Anthropic's engineering guide on building systems for AI agents

## Contributing

This template is designed to be forked and customized. If you have improvements that would benefit everyone, feel free to submit a PR.

## License

MIT - Use this template for any project.
