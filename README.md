# Telegram Email Agent

An AI assistant that lives in Telegram and manages your inbox for you. Message the bot in plain language to read, search, summarize, draft and send emails without opening Gmail.

## What It Can Do

- 📥 **Check your inbox**: "Any new emails today?"
- 🔎 **Search**: "Find emails from [sender] about invoices"
- 📝 **Summarize**: get short summaries of long threads
- ✍️ **Draft replies**: "Reply to John and say I'll send it Friday"
- 📤 **Send emails**: with a confirmation step before anything goes out
- 🔔 **Alerts** *(optional)*: push important new emails to Telegram

## How It Works

```
Telegram message
      │
      ▼
Telegram Trigger (n8n)
      │
      ▼
Auth check (only your Telegram chat ID is allowed)
      │
      ▼
AI Agent (Claude) + memory
      │   Tools: Gmail → Get Many / Get / Reply / Send / Create Draft
      ▼
Telegram reply with the result
```

## Tech Stack

- **n8n** (workflow engine)
- **Telegram Bot API** (via BotFather)
- **Gmail API** (OAuth2)
- **Anthropic Claude** (AI Agent node with window-buffer memory)

## Setup

### 1. Create the Telegram bot
1. Open Telegram and message **@BotFather**.
2. Run `/newbot` and follow the prompts.
3. Copy the **bot token**.

### 2. Get your chat ID
Message your bot once, then check the Telegram Trigger's output in n8n for `message.chat.id`.

### 3. Connect credentials in n8n
- Telegram API (bot token)
- Gmail OAuth2
- Anthropic API key

### 4. Import and configure
1. Import the workflow JSON.
2. In the **Auth check** node, set your Telegram chat ID so nobody else can use the bot.
3. Set the Gmail tools' scopes (read, send, drafts).
4. Activate the workflow.

## Example Commands

| You say | Agent does |
|---|---|
| "Summarize my unread emails" | Fetches unread mail and replies with a digest |
| "Show emails from Jumia this week" | Searches Gmail and lists results |
| "Draft a reply to the last email from my lecturer" | Creates a Gmail draft and shows it to you |
| "Send it" | Sends the draft after confirmation |

## Security

- **Chat ID whitelist**: the bot ignores everyone except you.
- **Confirm before sending**: the agent must ask for approval before sending any email.
- **Least-privilege OAuth**: only grant the Gmail scopes you need.
- **Keep secrets out of the repo**: tokens and keys live in n8n credentials, never in the workflow JSON.

## Limitations

- Long threads may be truncated to fit the AI's context window.
- The agent can misread ambiguous requests, so always review drafts before sending.
- Attachments are not processed [update if supported].

## Roadmap

- [ ] Scheduled morning inbox digest
- [ ] Priority and urgent-email alerts
- [ ] Attachment summaries
- [ ] Multi-account support

## Project Structure

```
/workflows
  telegram-email-agent.json
README.md
```

## License

[MIT / Private, choose one]

## Author

Built by **MONIEBEAST**
