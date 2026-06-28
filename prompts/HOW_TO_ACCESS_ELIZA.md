# How to Access Eliza

Eliza is your AI Clone Copilot. Here are all the ways you can access and use it:

---

## Option 1: Claude.ai Projects (Easiest - Web Interface)

### Step 1: Go to Claude.ai
Visit: https://claude.ai

### Step 2: Create a New Project
1. Click "Projects" in the left sidebar
2. Click "+ New Project"
3. Name it: **"Eliza"**

### Step 3: Add the System Prompt
1. In your project, click "Project Knowledge"
2. Click "Add content"
3. Copy the entire content from `prompts/eliza-system-prompt.md`
4. Paste it into the project instructions/custom instructions field

### Step 4: Start Using
- Any chat in this project will now use Eliza's personality and framework
- Ask questions like:
  - "Help me prioritize my week"
  - "Review this product strategy"
  - "What's the highest-leverage action here?"

**Pro:** Easiest, no code required
**Con:** Only accessible through Claude.ai web interface

---

## Option 2: Claude Code CLI (What You're Using Now)

### Access Eliza Through Claude Code

1. **Copy the prompt:**
   ```bash
   cat /home/user/n8n-claude-builder/prompts/eliza-system-prompt.md
   ```

2. **Paste into conversation:**
   Start a new session with Claude Code and paste the prompt, then say:
   "From now on, act according to this system prompt."

3. **Or create a custom slash command:**
   Edit `~/.claude/settings.json`:
   ```json
   {
     "customCommands": {
       "/eliza": {
         "description": "Activate Eliza mode",
         "prompt": "{{readFile('/home/user/n8n-claude-builder/prompts/eliza-system-prompt.md')}}"
       }
     }
   }
   ```
   Then just type `/eliza` to activate.

---

## Option 3: Claude API (For Automation)

### Direct API Usage

```javascript
import Anthropic from '@anthropic-ai/sdk';
import fs from 'fs';

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

// Load Eliza's system prompt
const elizaPrompt = fs.readFileSync(
  './prompts/eliza-system-prompt.md', 
  'utf-8'
);

// Chat with Eliza
const message = await anthropic.messages.create({
  model: 'claude-sonnet-4-5',
  max_tokens: 4096,
  temperature: 0.7,
  system: elizaPrompt,
  messages: [
    { 
      role: 'user', 
      content: 'Help me prioritize my product roadmap for Q2' 
    }
  ],
});

console.log(message.content[0].text);
```

### With Streaming

```javascript
const stream = await anthropic.messages.stream({
  model: 'claude-sonnet-4-5',
  max_tokens: 4096,
  system: elizaPrompt,
  messages: [
    { role: 'user', content: 'Your question here' }
  ],
});

for await (const event of stream) {
  if (event.type === 'content_block_delta') {
    process.stdout.write(event.delta.text);
  }
}
```

---

## Option 4: n8n Workflows (For Automation)

### Create an "Ask Eliza" Workflow

1. **Add HTTP Request Node:**
   - Method: POST
   - URL: `https://api.anthropic.com/v1/messages`
   - Headers:
     ```json
     {
       "x-api-key": "{{$env.ANTHROPIC_API_KEY}}",
       "anthropic-version": "2023-06-01",
       "content-type": "application/json"
     }
     ```

2. **Body:**
   ```json
   {
     "model": "claude-sonnet-4-5",
     "max_tokens": 4096,
     "temperature": 0.7,
     "system": "{{$('ReadFile').item.json.elizaPrompt}}",
     "messages": [
       {
         "role": "user",
         "content": "{{$json.userQuestion}}"
       }
     ]
   }
   ```

3. **Add Read File Node (before HTTP):**
   - File Path: `/home/user/n8n-claude-builder/prompts/eliza-system-prompt.md`
   - Property Name: `elizaPrompt`

### Use Cases:
- Slack bot that responds as Eliza
- Email assistant that drafts replies in your voice
- Daily briefing automation
- Product decision framework assistant

---

## Option 5: ChatGPT Custom GPT (Alternative Platform)

1. Go to ChatGPT > "Create a GPT"
2. Name it: **Eliza**
3. Paste the system prompt from `eliza-system-prompt.md` into Instructions
4. Configure:
   - Tone: Professional, Direct, Strategic
   - Focus: Product Management, AI, Leadership
5. Save and use

---

## Option 6: Cursor / VS Code with AI Extensions

### Cursor IDE:
1. Open Cursor Settings
2. Go to "AI Rules" or "Custom Instructions"
3. Add Eliza's system prompt
4. Now Cursor's AI will respond as Eliza when you ask questions

### VS Code with Continue:
1. Install Continue extension
2. Edit `.continue/config.json`:
   ```json
   {
     "systemMessage": "{{readFile('prompts/eliza-system-prompt.md')}}"
   }
   ```

---

## Option 7: Mobile Access (Claude App)

### iOS/Android Claude App:
1. Open Claude app
2. Start a new conversation
3. In your first message, paste Eliza's system prompt
4. Add: "From now on in this chat, operate as Eliza"
5. Bookmark this conversation for future use

---

## Quick Access Scripts

### Create a Shell Script (`ask-eliza.sh`):

```bash
#!/bin/bash

PROMPT_FILE="/home/user/n8n-claude-builder/prompts/eliza-system-prompt.md"
SYSTEM_PROMPT=$(cat "$PROMPT_FILE")

curl https://api.anthropic.com/v1/messages \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d "{
    \"model\": \"claude-sonnet-4-5\",
    \"max_tokens\": 4096,
    \"system\": $(echo "$SYSTEM_PROMPT" | jq -Rs .),
    \"messages\": [{
      \"role\": \"user\",
      \"content\": \"$1\"
    }]
  }" | jq -r '.content[0].text'
```

**Usage:**
```bash
chmod +x ask-eliza.sh
./ask-eliza.sh "Help me prioritize my week"
```

---

## Recommended Setup for You

Based on your needs, I recommend:

### For Daily Use:
1. **Claude.ai Project** - Quick access from any browser
2. **ask-eliza.sh script** - Terminal access for fast questions

### For Automation:
3. **n8n Workflow** - Build Slack bot, email assistant, etc.

### For Development:
4. **Cursor IDE with Eliza** - Code with your AI copilot

---

## Testing Eliza

Once set up, test with these prompts:

1. **Decision Framework:**
   "I have 3 product opportunities. Help me choose using your decision framework."

2. **Communication:**
   "Review this email and make it more executive-level."

3. **Strategy:**
   "What's the highest-leverage action I should take this week?"

4. **Systems Thinking:**
   "I keep manually doing X. How can I automate it?"

---

## Environment Variables Needed

For API access, set:

```bash
export ANTHROPIC_API_KEY="your-api-key-here"
```

Get your API key: https://console.anthropic.com/settings/keys

---

## Cost Estimates

Claude Sonnet 4.5 pricing:
- Input: $3 per million tokens
- Output: $15 per million tokens

Typical Eliza conversation (500 tokens in, 1000 tokens out):
- Cost: ~$0.017 per conversation
- 100 conversations/day = ~$1.70/day

---

**Need help setting up any of these? Let me know which option you want to start with.**
