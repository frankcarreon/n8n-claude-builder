# Prompts Directory

This directory contains system prompts and prompt templates for AI agents and workflows in the n8n Claude Builder project.

## Available Prompts

### eliza-system-prompt.md
Master system prompt for Eliza, Frank Carreon's AI Clone Copilot. This prompt configures an AI assistant to operate as Frank's personal operating system for career, business, faith, and decision-making.

**Use Cases:**
- Configure AI agents in n8n workflows
- Set system prompts for Claude API integrations
- Template for creating specialized AI assistants
- Training material for understanding Frank's decision-making framework

**Key Features:**
- Executive-level communication style
- Decision framework optimized for leverage
- Faith-driven leadership principles
- Product management and AI expertise
- Structured, actionable output format

## Usage

### In n8n Workflows

```json
{
  "systemPrompt": "{{ $('HTTP Request').item.json.prompt }}",
  "model": "claude-sonnet-4-5",
  "temperature": 0.7
}
```

### With Claude API

```javascript
import Anthropic from '@anthropic-ai/sdk';
import fs from 'fs';

const systemPrompt = fs.readFileSync('./prompts/frank-copilot-system-prompt.md', 'utf-8');

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

const message = await anthropic.messages.create({
  model: 'claude-sonnet-4-5',
  max_tokens: 4096,
  system: systemPrompt,
  messages: [
    { role: 'user', content: 'Your question here' }
  ],
});
```

### As n8n Custom Node Parameter

Reference the prompt file in your n8n custom node configuration to ensure consistent behavior across workflows.

## Creating New Prompts

When creating new prompt templates:

1. Use clear, structured markdown format
2. Include a version number in the title
3. Define the AI's identity, role, and boundaries
4. Specify communication style and output format
5. Document decision frameworks and priorities
6. Include usage examples

## Prompt Versioning

Prompts should be versioned to track improvements:
- `v1.0` - Initial version
- `v1.1` - Minor updates, clarifications
- `v2.0` - Major structural changes

Create new files for major versions (e.g., `frank-copilot-system-prompt-v2.md`) while keeping old versions for backward compatibility.

## Security Notes

- Never include API keys or credentials in prompt files
- Prompts may contain business logic and strategy - protect accordingly
- Review prompts before sharing externally
- Use environment variables for sensitive configuration

## Contributing

When adding new prompts:
1. Create the prompt file in this directory
2. Update this README with description and usage
3. Add examples to `/examples` if applicable
4. Test the prompt with actual API calls
5. Document any specific requirements or dependencies
