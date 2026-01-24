# CLAUDE.md - Technical Reference

> This file contains the technical specifications for your project.
> Update this file with your project's architecture, patterns, and standards.
> The AI reads this file at the start of each session.

---

## PROJECT OVERVIEW

### What We're Building

<!-- Describe your project here -->

```
Project Name: todo-api
Description: A REST API for managing tasks
Tech Stack: Bun, Hono, SQLite (via Bun's built-in sqlite)
```

### Core Principles

1. **Type Safety**: TypeScript everywhere, no `any` types
2. **Test First**: Write tests before implementation when possible
3. **Simple APIs**: RESTful design, clear error messages

---

## ARCHITECTURE

### High-Level Structure

```
┌─────────────────────────────────────────────────────────────────┐
│                        TODO API                                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   Client ──► Hono Routes ──► Handlers ──► Database              │
│                   │              │                               │
│                   ▼              ▼                               │
│              Validation     Business Logic                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Directory Structure

```
todo-api/
├── src/
│   ├── index.ts          # Entry point, Hono app setup
│   ├── routes/
│   │   └── todos.ts      # Todo CRUD routes
│   ├── db/
│   │   ├── index.ts      # Database connection
│   │   └── schema.ts     # Table definitions
│   ├── validators/
│   │   └── todo.ts       # Input validation
│   └── types/
│       └── todo.ts       # TypeScript interfaces
├── tests/
│   ├── routes/
│   │   └── todos.test.ts
│   └── setup.ts          # Test utilities
├── package.json
├── tsconfig.json
└── bunfig.toml
```

---

## TYPE SYSTEM

```typescript
// Core domain types

interface Todo {
  id: number;
  title: string;
  description: string | null;
  completed: boolean;
  createdAt: string;
  updatedAt: string;
}

interface CreateTodoInput {
  title: string;
  description?: string;
}

interface UpdateTodoInput {
  title?: string;
  description?: string;
  completed?: boolean;
}

// API response types

interface ApiResponse<T> {
  data: T;
}

interface ApiError {
  error: {
    code: string;
    message: string;
  };
}
```

---

## API DESIGN

```
Base URL: /api/v1

GET    /todos              # List all todos
POST   /todos              # Create a todo
GET    /todos/:id          # Get a single todo
PATCH  /todos/:id          # Update a todo
DELETE /todos/:id          # Delete a todo
```

### Response Format

**Success:**
```json
{
  "data": { ... }
}
```

**Error:**
```json
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Todo with id 123 not found"
  }
}
```

---

## TESTING STRATEGY

### Test Types

- **Unit Tests**: Validators, utility functions
- **Integration Tests**: API routes with test database

### Running Tests

```bash
bun test              # Run all tests
bun test --watch      # Watch mode
bun test tests/routes # Run specific tests
```

---

## ERROR HANDLING

```typescript
// Standard error codes
const ErrorCodes = {
  NOT_FOUND: "NOT_FOUND",
  VALIDATION_ERROR: "VALIDATION_ERROR",
  INTERNAL_ERROR: "INTERNAL_ERROR",
} as const;

// Error helper
function apiError(c: Context, status: number, code: string, message: string) {
  return c.json({ error: { code, message } }, status);
}
```

---

## CODING STANDARDS

### General Rules

1. **Naming**: camelCase for variables/functions, PascalCase for types
2. **Functions**: Keep functions small and focused
3. **Errors**: Always return proper error responses, never throw unhandled
4. **Types**: Export types from dedicated files, import where needed

### File Organization

```typescript
// Order of imports
// 1. External packages (hono, etc.)
// 2. Internal modules (@/db, @/types, etc.)
// 3. Relative imports

// Order within file
// 1. Types/interfaces
// 2. Constants
// 3. Helper functions
// 4. Main exports (routes, handlers)
```

---

## ENVIRONMENT

### Required Tools

- Bun 1.0+

### Setup Commands

```bash
# Clone and setup
git clone [repo-url]
cd todo-api

# Install dependencies
bun install

# Run development server
bun run dev

# Run tests
bun test

# Type check
bun run typecheck

# Lint
bun run lint
```

---

## REMEMBER

When working on this project:

1. **Check the types**: Domain types in `src/types/` are the source of truth
2. **Follow the patterns**: Look at existing routes for examples
3. **Test first**: Write the test, then the implementation
4. **Validate input**: All POST/PATCH bodies go through validators
5. **Handle errors**: Return proper ApiError responses

---

## HOW TO CUSTOMIZE THIS FILE

1. Replace all placeholder text with your project specifics
2. Add sections relevant to your project
3. Remove sections that don't apply
4. Keep it concise but comprehensive
5. Update as the project evolves

This file should answer: "What do I need to know to work on this project?"
