# CLAUDE.md - AI Assistant Guide for n8n Claude Builder

**Last Updated**: 2026-01-23
**Repository**: n8n-claude-builder
**Purpose**: A workflow automation project integrating n8n with Claude AI capabilities

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Repository Structure](#repository-structure)
3. [Development Workflow](#development-workflow)
4. [Git Conventions](#git-conventions)
5. [Code Conventions](#code-conventions)
6. [Testing Guidelines](#testing-guidelines)
7. [AI Assistant Guidelines](#ai-assistant-guidelines)
8. [Common Tasks](#common-tasks)
9. [Troubleshooting](#troubleshooting)

---

## 🎯 Project Overview

### What is n8n Claude Builder?

This project integrates Claude AI capabilities with n8n, a workflow automation platform. The goal is to enable AI-powered workflow automation using Claude's advanced reasoning and task execution abilities.

### Technology Stack

- **Platform**: n8n (workflow automation)
- **AI Integration**: Claude AI (Anthropic)
- **Expected Technologies**: Node.js, TypeScript, REST APIs
- **Version Control**: Git

### Key Objectives

1. Enable Claude AI to interact with n8n workflows
2. Automate complex tasks using AI-driven decision making
3. Provide seamless integration between n8n and Claude API
4. Support various workflow automation scenarios

---

## 📁 Repository Structure

```
n8n-claude-builder/
├── README.md              # Project introduction and setup
├── CLAUDE.md             # This file - AI assistant guide
├── .git/                 # Git version control
├── .gitignore            # Git ignore patterns
├── package.json          # Node.js dependencies and scripts
├── tsconfig.json         # TypeScript configuration
├── .env.example          # Environment variable template
├── src/                  # Source code
│   ├── nodes/           # Custom n8n nodes
│   │   └── ClaudeNode/  # Claude AI node implementation
│   ├── workflows/       # Workflow templates
│   ├── integrations/    # Claude API integrations
│   │   ├── client.ts    # Claude API client
│   │   └── types.ts     # Type definitions
│   ├── utils/           # Utility functions
│   │   ├── errors.ts    # Error handling utilities
│   │   ├── validators.ts # Input validation
│   │   └── logger.ts    # Logging utilities
│   └── index.ts         # Main entry point
├── tests/               # Test files
│   ├── unit/           # Unit tests
│   ├── integration/    # Integration tests
│   └── e2e/           # End-to-end tests
├── docs/               # Documentation
│   ├── api.md         # API documentation
│   ├── setup.md       # Setup guide
│   └── examples.md    # Usage examples
└── examples/           # Example workflows
    └── sample-workflows.json
```

**Note**: This structure represents the expected organization as the project develops.

---

## 🔄 Development Workflow

### Branch Strategy

This project uses feature-based development with specific branch naming conventions:

- **Feature Branches**: `claude/claude-md-<session-id>`
- **Current Branch**: `claude/claude-md-mkrauxqa5evpmp86-LHrwJ`
- **Branch Requirement**: All feature branches MUST start with `claude/` and end with matching session ID

### Development Process

1. **Start Work**
   - Ensure you're on the correct feature branch
   - Create the branch if it doesn't exist: `git checkout -b claude/claude-md-<session-id>`
   - Pull latest changes: `git fetch origin <branch-name>`

2. **Make Changes**
   - Follow code conventions (see below)
   - Write clear, descriptive commit messages
   - Test changes thoroughly before committing

3. **Commit Changes**
   - Stage files: `git add <files>`
   - Commit with descriptive message: `git commit -m "Description"`
   - Follow commit message conventions

4. **Push Changes**
   - Always use: `git push -u origin <branch-name>`
   - Branch MUST start with `claude/` (403 error otherwise)
   - Retry on network errors (up to 4 times with exponential backoff: 2s, 4s, 8s, 16s)

5. **Create Pull Request**
   - Use `gh pr create` with clear title and description
   - Include summary of changes
   - Provide test plan

---

## 🌿 Git Conventions

### Branch Naming

- **Pattern**: `claude/claude-md-<session-id>`
- **Example**: `claude/claude-md-mkrauxqa5evpmp86-LHrwJ`
- **Critical**: Branch name MUST match pattern or push will fail with 403

### Commit Messages

Follow conventional commits format:

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks
- `ci`: CI/CD changes

**Examples**:
```
feat(nodes): add Claude AI node for n8n
fix(api): handle rate limiting in Claude API calls
docs(readme): update installation instructions
refactor(utils): simplify error handling logic
test(integration): add Claude API integration tests
```

### Git Operations Best Practices

**Push Operations**:
```bash
git push -u origin claude/claude-md-<session-id>
```
- Retry up to 4 times on network failure
- Exponential backoff: 2s, 4s, 8s, 16s

**Fetch/Pull Operations**:
```bash
git fetch origin <branch-name>
git pull origin <branch-name>
```
- Same retry logic as push operations

**Never**:
- Push to branches not starting with `claude/`
- Force push without explicit user permission
- Skip git hooks (--no-verify)
- Amend commits that have been pushed
- Update git config without permission
- Commit files with secrets (.env, credentials, etc.)

---

## 💻 Code Conventions

### TypeScript/JavaScript Standards

1. **File Naming**
   - Use kebab-case: `claude-node.ts`, `workflow-utils.ts`
   - Test files: `*.test.ts` or `*.spec.ts`
   - Type definitions: `*.types.ts`

2. **Code Style**
   - Use TypeScript for all source files
   - Prefer `const` over `let`, avoid `var`
   - Use async/await over callbacks
   - Use arrow functions for inline callbacks
   - Use template literals for string interpolation

3. **Naming Conventions**
   - Classes: PascalCase (`ClaudeNode`, `WorkflowBuilder`)
   - Functions/Methods: camelCase (`executeWorkflow`, `handleError`)
   - Constants: UPPER_SNAKE_CASE (`API_ENDPOINT`, `MAX_RETRIES`)
   - Interfaces: PascalCase with `I` prefix (`IClaudeConfig`, `IWorkflowOptions`)
   - Types: PascalCase (`WorkflowType`, `NodeConfig`)

4. **Imports**
   - Group imports: external libraries, then internal modules
   - Use absolute imports when configured
   - Sort alphabetically within groups

   Example:
   ```typescript
   // External imports
   import axios from 'axios';
   import { Request, Response } from 'express';

   // Internal imports
   import { ClaudeClient } from '@/integrations/client';
   import { validateInput } from '@/utils/validators';
   ```

5. **Error Handling**
   - Always handle errors appropriately
   - Use custom error classes when needed
   - Log errors with context
   - Validate inputs at system boundaries (user input, external APIs)

6. **Comments**
   - Only add comments where logic isn't self-evident
   - Don't add docstrings to unchanged code
   - Use JSDoc for public APIs and complex functions

   Example:
   ```typescript
   /**
    * Executes a workflow with Claude AI integration
    * @param workflowId - The unique identifier for the workflow
    * @param config - Configuration options for execution
    * @returns Promise resolving to workflow execution result
    * @throws {WorkflowError} If workflow execution fails
    */
   async function executeWorkflow(workflowId: string, config: IWorkflowConfig): Promise<WorkflowResult>
   ```

### Security Considerations

**Critical**: Avoid security vulnerabilities
- Never commit secrets, API keys, or credentials
- Use `.env` files for sensitive configuration (add to .gitignore)
- Validate and sanitize all external inputs
- Prevent SQL injection, XSS, command injection
- Follow OWASP Top 10 guidelines
- Use environment variables for sensitive configuration
- Implement rate limiting for API endpoints
- Use HTTPS for all external communications

### Avoid Over-Engineering

- Don't add features beyond what's requested
- Keep solutions simple and focused
- Don't create abstractions for one-time operations
- Don't design for hypothetical future requirements
- Only validate at system boundaries
- No backwards-compatibility hacks unless necessary
- Three similar lines of code is better than a premature abstraction

---

## 🧪 Testing Guidelines

### Test Structure

```
tests/
├── unit/           # Unit tests for individual functions/classes
│   ├── nodes/
│   ├── integrations/
│   └── utils/
├── integration/    # Integration tests for API interactions
│   └── claude-api.test.ts
└── e2e/           # End-to-end workflow tests
    └── workflow-execution.test.ts
```

### Testing Best Practices

1. **Write Tests For**:
   - New features
   - Bug fixes
   - Critical business logic
   - API integrations
   - Error handling paths

2. **Test Naming**:
   - Descriptive: `should create workflow when valid config provided`
   - Pattern: `should [expected behavior] when [condition]`

   Example:
   ```typescript
   describe('ClaudeClient', () => {
     it('should successfully call Claude API when valid credentials provided', async () => {
       // Test implementation
     });

     it('should throw error when API key is invalid', async () => {
       // Test implementation
     });
   });
   ```

3. **Test Coverage**:
   - Aim for high coverage on critical paths
   - Don't skip tests for edge cases
   - Test error conditions
   - Test boundary conditions

4. **Running Tests**:
   ```bash
   npm test              # Run all tests
   npm run test:unit     # Unit tests only
   npm run test:integration  # Integration tests
   npm run test:watch    # Watch mode
   npm run test:coverage # Generate coverage report
   ```

---

## 🤖 AI Assistant Guidelines

### When Working on This Project

1. **Always Read First**
   - Read files before modifying them
   - Understand existing code before suggesting changes
   - Check for existing implementations
   - Never propose changes to unread code

2. **Use Task Management**
   - Use TodoWrite tool for multi-step tasks (3+ steps)
   - Mark tasks as in_progress before starting
   - Mark completed immediately after finishing
   - Only one task in_progress at a time
   - Don't use for trivial single-step tasks

3. **File Operations**
   - Prefer Edit over Write for existing files
   - Use Read instead of cat/head/tail
   - Use Grep for searching instead of grep/rg commands
   - Use Glob for file pattern matching
   - Never use bash for file operations when tools exist

4. **Code References**
   - Include `file_path:line_number` when referencing code
   - Example: "Error handling is in src/utils/errors.ts:42"

5. **Git Operations**
   - Always develop on: `claude/claude-md-<session-id>`
   - Commit with clear, descriptive messages
   - Push using: `git push -u origin <branch-name>`
   - Create PRs with summary and test plan
   - Never force push without permission
   - Never skip git hooks

6. **Communication**
   - Be concise and clear
   - No emojis unless requested
   - Output text directly, don't use bash echo
   - Focus on facts and problem-solving
   - Avoid superlatives and excessive praise
   - Be objective and honest

7. **Security**
   - Never commit credentials or secrets
   - Flag security vulnerabilities immediately
   - Fix security issues as soon as discovered
   - Validate inputs at boundaries
   - Check for OWASP Top 10 vulnerabilities

8. **Avoid**
   - Creating unnecessary files
   - Over-engineering solutions
   - Adding features not requested
   - Modifying code outside scope
   - Batch completing multiple todos
   - Creating documentation files unless requested

### Task Workflow Example

```
User: Add Claude API integration and test it

Assistant: I'll help you add Claude API integration. Let me create a todo list to track this task.

[Uses TodoWrite tool to create tasks:]
1. Read existing integration code (if any)
2. Create Claude API client
3. Add configuration handling
4. Write integration tests
5. Test the integration

[Marks first task as in_progress]
[Uses Read/Grep tools to search for existing code]
[Marks first task complete, moves to second task]
[Creates src/integrations/client.ts]
[Marks second task complete]
...
```

---

## 📝 Common Tasks

### Setting Up Development Environment

```bash
# Clone repository
git clone <repository-url>
cd n8n-claude-builder

# Install dependencies
npm install

# Copy environment template
cp .env.example .env

# Edit .env with your credentials
vim .env
```

### Creating a New Feature

1. Create feature branch:
   ```bash
   git checkout -b claude/claude-md-<session-id>
   ```

2. Implement feature following code conventions

3. Write tests:
   ```bash
   npm run test:watch
   ```

4. Commit changes:
   ```bash
   git add .
   git commit -m "feat(scope): description"
   ```

5. Push to remote:
   ```bash
   git push -u origin claude/claude-md-<session-id>
   ```

### Running n8n with Custom Node

```bash
# Link your custom node
npm run link:node

# Start n8n in development mode
npm run dev:n8n

# Access n8n at http://localhost:5678
```

### Testing Claude API Integration

```bash
# Run integration tests
npm run test:integration

# Test specific API endpoint
npm run test:integration -- claude-api.test.ts

# Manual testing with curl
curl -X POST http://localhost:3000/api/claude \
  -H "Content-Type: application/json" \
  -d '{'prompt': 'Hello Claude\!'}'
```

### Building for Production

```bash
# Run tests
npm test

# Build TypeScript
npm run build

# Verify build output
ls -la dist/
```

---

## 🔧 Troubleshooting

### Git Push Returns 403 Error

**Problem**: Push fails with HTTP 403

**Solution**:
- Verify branch name starts with `claude/`
- Check branch name matches required pattern: `claude/claude-md-<session-id>`
- Example correct branch: `claude/claude-md-mkrauxqa5evpmp86-LHrwJ`

### TypeScript Compilation Errors

**Problem**: `tsc` reports type errors

**Solution**:
```bash
# Check TypeScript configuration
cat tsconfig.json

# Install type definitions
npm install --save-dev @types/node @types/express

# Clear cache and rebuild
rm -rf dist/ node_modules/
npm install
npm run build
```

### Claude API Rate Limiting

**Problem**: API returns 429 Too Many Requests

**Solution**:
- Implement exponential backoff
- Add retry logic with delays
- Check rate limits in Claude API documentation
- Consider implementing request queuing

### n8n Node Not Appearing

**Problem**: Custom node doesn't show in n8n UI

**Solution**:
```bash
# Verify node is linked properly
npm run link:node

# Restart n8n
npm run dev:n8n

# Check node credentials and configuration
# Verify node follows n8n node structure
```

### Tests Failing

**Problem**: Tests fail unexpectedly

**Solution**:
```bash
# Run tests in verbose mode
npm test -- --verbose

# Run specific test file
npm test -- path/to/test.test.ts

# Clear Jest cache
npm test -- --clearCache

# Check for environment variable issues
cat .env.test
```

---

## 📚 Additional Resources

### n8n Documentation
- [n8n Documentation](https://docs.n8n.io/)
- [Creating Custom Nodes](https://docs.n8n.io/integrations/creating-nodes/)
- [n8n Node Development](https://docs.n8n.io/integrations/creating-nodes/build/)

### Claude API Documentation
- [Claude API Reference](https://docs.anthropic.com/)
- [API Best Practices](https://docs.anthropic.com/claude/docs/best-practices)
- [Rate Limits](https://docs.anthropic.com/claude/reference/rate-limits)

### Development Tools
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Jest Testing Framework](https://jestjs.io/)
- [Conventional Commits](https://www.conventionalcommits.org/)

---

## 🤝 Contributing

When contributing to this project:

1. Follow all conventions in this document
2. Write tests for new features
3. Update documentation as needed
4. Keep commits focused and atomic
5. Request review before merging

---

## 📄 Document Maintenance

**Update Frequency**: This document should be updated when:
- Project structure changes significantly
- New conventions are established
- New tools or frameworks are added
- Development workflow changes

**Last Review Date**: 2026-01-23

**Maintainers**: AI assistants should keep this document current by updating it whenever significant changes occur to the codebase structure or development practices.
