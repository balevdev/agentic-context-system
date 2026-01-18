# CLAUDE.md - Technical Reference

> This file contains the technical specifications for your project.
> Update this file with your project's architecture, patterns, and standards.
> The AI reads this file at the start of each session.

---

## PROJECT OVERVIEW

### What We're Building

<!-- Describe your project here -->

```
Project Name: [Your Project Name]
Description: [What does this project do?]
Tech Stack: [Languages, frameworks, tools]
```

### Core Principles

<!-- What principles guide development? -->

1. **Principle 1**: Description
2. **Principle 2**: Description
3. **Principle 3**: Description

---

## ARCHITECTURE

### High-Level Structure

<!-- Describe your project's architecture -->

```
[Add an ASCII diagram or description of your architecture]
```

### Directory Structure

```
project/
├── src/                    # Source code
├── tests/                  # Test files
├── docs/                   # Documentation
└── ...                     # Other directories
```

---

## TYPE SYSTEM

<!-- If using TypeScript, define core types here -->

```typescript
// Example types - replace with your own

interface ExampleEntity {
  id: string;
  name: string;
  createdAt: Date;
}
```

---

## API DESIGN

<!-- If building an API, document endpoints here -->

```
Base URL: /api/v1

GET    /resources          # List resources
POST   /resources          # Create resource
GET    /resources/:id      # Get resource
PUT    /resources/:id      # Update resource
DELETE /resources/:id      # Delete resource
```

---

## TESTING STRATEGY

### Test Types

- **Unit Tests**: Test individual functions/modules
- **Integration Tests**: Test component interactions
- **E2E Tests**: Test full user flows

### Running Tests

```bash
# Replace with your actual commands
npm test              # Run all tests
npm run test:unit     # Run unit tests only
npm run test:e2e      # Run E2E tests
```

---

## ERROR HANDLING

<!-- Define how errors should be handled -->

```typescript
// Example error pattern - customize for your project

class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number
  ) {
    super(message);
  }
}
```

---

## CODING STANDARDS

### General Rules

1. **Naming**: Use descriptive names (camelCase for variables, PascalCase for types)
2. **Functions**: Keep functions small and focused
3. **Comments**: Document "why" not "what"
4. **Errors**: Always handle errors explicitly

### File Organization

```typescript
// Order of imports
// 1. Built-in modules
// 2. External packages
// 3. Internal modules
// 4. Relative imports

// Order within file
// 1. Types/interfaces
// 2. Constants
// 3. Helper functions
// 4. Main exports
```

---

## ENVIRONMENT

### Required Tools

<!-- List tools needed to work on this project -->

- Tool 1 (version)
- Tool 2 (version)

### Setup Commands

```bash
# Clone and setup
git clone [repo-url]
cd [project-name]

# Install dependencies
[your install command]

# Run development server
[your dev command]
```

---

## REMEMBER

When working on this project:

1. **Check the types**: Domain types are the source of truth
2. **Follow the patterns**: Consistency over cleverness
3. **Test first**: Write tests before implementation when possible
4. **Commit often**: Small, focused commits
5. **Ask for help**: Update CURRENT_TASK.md with blockers

---

## HOW TO CUSTOMIZE THIS FILE

1. Replace all placeholder text with your project specifics
2. Add sections relevant to your project
3. Remove sections that don't apply
4. Keep it concise but comprehensive
5. Update as the project evolves

This file should answer: "What do I need to know to work on this project?"
