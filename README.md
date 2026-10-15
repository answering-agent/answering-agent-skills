# Answering Agent

Answering Agent answers phone calls, texts, website chats, and emails for car washes and other service businesses. This plugin connects Claude to your Answering Agent account and adds four skills for the work owners and managers do most.

## Skills

- Weekly front desk report gives last week's calls, texts, chats, and emails, the AI-resolved rate, the top reasons a person had to follow up, open tasks, and a chart.
- Morning task triage lists the open and overdue follow-up tasks, who owns each one, and what to do first.
- Cancellation review shows why members cancelled, which retention offers the AI pitched and which were accepted, and who to follow up with.
- Reply to a customer finds the conversation, drafts a short text or email, and shows you the exact message before it sends.

Ask in your own words, such as "How did the front desk do last week?" or "Text Jordan Hale back about his discount."

## Install

In Claude, open Settings, then Connectors, and add Answering Agent from the directory.

In Claude Code, run:

```
/plugin marketplace add answering-agent/answering-agent-skills
/plugin install answering-agent@answering-agent-skills
```

## Requirements

You need an Answering Agent account with the owner, admin, or manager role. Sign in to Answering Agent when Claude asks you to connect. Each connection works on one team, the one open in your dashboard when you connect.

The skills read your data and change nothing unless you ask. When you reply to a customer, the customer receives it, so read the exact message before you approve it.

## Links

- [Setup guide](https://docs.answeringagent.com/mcp-server)
- [Support](https://answeringagent.com/support)
- [Privacy policy](https://answeringagent.com/legal/privacy)
- [Terms](https://answeringagent.com/legal/terms)

## License

MIT
