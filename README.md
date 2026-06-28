# n8n Claude Builder

AI-powered workflow automation integrating n8n with Claude AI capabilities.

## Overview

This project enables AI-driven workflow automation using Claude's advanced reasoning and task execution abilities within n8n workflows. Build custom AI agents, automate complex decision-making processes, and create intelligent workflows.

## Project Structure

```
n8n-claude-builder/
├── README.md              # This file
├── CLAUDE.md             # AI assistant development guide
├── prompts/              # System prompts and AI configurations
│   ├── frank-copilot-system-prompt.md
│   ├── frank-copilot-config.json
│   └── README.md
├── src/                  # Source code (coming soon)
├── tests/                # Test files (coming soon)
├── docs/                 # Documentation (coming soon)
└── examples/             # Example workflows (coming soon)
```

## Quick Start

### Prerequisites

- Node.js 18+ and npm
- n8n installed (`npm install -g n8n`)
- Claude API key from Anthropic

### Setup

```bash
# Clone the repository
git clone <repository-url>
cd n8n-claude-builder

# Install dependencies
npm install

# Configure environment variables
cp .env.example .env
# Edit .env and add your ANTHROPIC_API_KEY
```

## Available Prompts

### Frank's Clone Copilot

A comprehensive system prompt for creating an AI assistant that operates as Frank Carreon's personal operating system for career, business, and decision-making.

**Location:** `prompts/frank-copilot-system-prompt.md`

**Features:**
- Executive-level communication
- Decision framework for high-leverage actions
- Product management and AI expertise
- Faith-driven leadership principles

See `prompts/README.md` for usage examples and integration guides.

## Development

This project uses feature branches with specific naming conventions. See `CLAUDE.md` for:
- Development workflow and branching strategy
- Code conventions and standards
- Testing guidelines
- AI assistant collaboration guide

**Current branch:** `claude/claude-md-mkrauxqa5evpmp86-LHrwJ`

## Documentation

- **CLAUDE.md** - Comprehensive guide for AI assistants working on this codebase
- **prompts/README.md** - System prompts and configuration guide
- **docs/** - Additional documentation (coming soon)

## Contributing

1. Follow conventions in `CLAUDE.md`
2. Use conventional commits format
3. Write tests for new features
4. Submit PRs with clear descriptions

## Resources

- [n8n Documentation](https://docs.n8n.io/)
- [Claude API Reference](https://docs.anthropic.com/)
- [Creating Custom n8n Nodes](https://docs.n8n.io/integrations/creating-nodes/)

## License

TBD

## Contact

Frank Carreon - frankcarreon@gmail.com
