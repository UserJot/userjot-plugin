# UserJot plugin for Claude

[UserJot](https://userjot.com) is a feedback board, roadmap, changelog, support inbox and help center for SaaS teams. This plugin connects Claude to your UserJot workspace and teaches it to install the UserJot widget in your app.

## What it adds

- **The UserJot MCP server.** Claude can read and change your workspace: list and triage feature requests, merge duplicates, change statuses, reply to comments, draft and publish changelog entries, answer support conversations, and write help articles.
- **A setup skill.** `/userjot:set-up-userjot` installs the widget in the app you have open, identifies your signed-in users, and adds Feedback, What's new and Help links to your own menus.

## Install

In Claude Code:

```
/plugin marketplace add UserJot/userjot-plugin
/plugin install userjot@userjot
```

Then run `/mcp`, choose `userjot`, and sign in. You pick a workspace and click Allow. There is no token to copy. Only owners and admins of a workspace can connect an app to it.

UserJot's API and MCP server are included on every plan, Free included.

## Use it

Ask in plain words:

- "Set up UserJot in this app."
- "What are the ten most requested features this month?"
- "Mark the dark mode request as completed and draft a changelog entry for it."
- "Summarise the support conversations that are waiting on us."

## What it sends to UserJot

The MCP server at `https://api.userjot.com/v1/mcp` receives the tool calls Claude makes and the arguments you or Claude supply, and nothing else from your machine. Every call acts on the one workspace you signed in to. Tools that only read are marked read-only; tools that create, change or delete data ask before they run.

The setup skill makes no network calls of its own. It edits files in your project.

You can disconnect the plugin at any time from `Settings` → `MCP & API` → Connected apps in the UserJot dashboard.

## Other clients

The same server works in claude.ai, ChatGPT, Cursor, Codex and Windsurf. See the [MCP guides](https://feedback.userjot.com/help/articles/mcp-overview).

## Links

- Documentation: https://feedback.userjot.com/help
- Privacy policy: https://userjot.com/privacy
- Terms: https://userjot.com/terms
- Support: support@userjot.com

## License

MIT
