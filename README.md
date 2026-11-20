# UserJot MCP server and plugin

[UserJot](https://userjot.com) is customer support, a help center, feedback boards, a roadmap and a changelog for SaaS teams. Its MCP server lets an AI assistant work in your UserJot workspace: answer support conversations, write help articles, triage feature requests, and publish changelog entries.

It is one remote server with nothing to install locally:

```
https://api.userjot.com/v1/mcp
```

Sign in with your UserJot account, pick a workspace, and click Allow. There is no token to copy. The API and the MCP server are included on every plan, Free included.

## What you can ask

- "Which support conversations are waiting on us? Draft a reply to each."
- "Search our help center for how to export data. If nothing covers it, draft an article."
- "What are the ten most requested features this month, and which look like duplicates?"
- "Mark the dark mode request as completed and draft a changelog entry for it."

## What it covers

The server has 98 tools for one workspace:

- **Support conversations.** List the conversations waiting on your team, read the full timeline, reply to customers, leave internal notes, assign, resolve and reopen.
- **Help center.** Search, write, edit, organize, publish and unpublish articles and collections.
- **Feedback and feature requests.** Search by board, status, tag or author, see who voted, find similar posts, merge duplicates, comment, and change a request's status on the public roadmap.
- **Changelog.** Draft, edit, schedule and publish entries, and link each one to the requests it shipped.
- **Customers and companies.** Look up, list, sync and update the people and companies behind the feedback.

Tools that only read are marked read-only, and tools that create, change or delete are marked as writes, so your client can ask before it runs one.

## Install

### VS Code with GitHub Copilot

In UserJot, open `Settings` → `MCP & API` → `More` → `VS Code` and click **Add to VS Code**. Or add the server to `.vscode/mcp.json` yourself:

```json
{
  "servers": {
    "userjot": {
      "type": "http",
      "url": "https://api.userjot.com/v1/mcp"
    }
  }
}
```

### Claude Code

Install the plugin, which adds the server and two skills:

```
/plugin marketplace add UserJot/userjot-plugin
/plugin install userjot@userjot
```

Then run `/mcp`, choose `userjot`, and sign in.

- `/userjot:work-the-support-inbox` goes through the conversations waiting on your team, checks your help center, and drafts replies for you to approve.
- `/userjot:set-up-userjot` installs the UserJot widget in the app you have open, identifies your signed-in users, and adds Feedback, What's new and Help links to your own menus.

To add the server without the skills:

```
claude mcp add --transport http --scope user userjot https://api.userjot.com/v1/mcp
```

### Other clients

| Client | Guide |
| --- | --- |
| Claude (web, desktop, mobile) | [Common setup](https://feedback.userjot.com/help/articles/mcp-common-setup) |
| ChatGPT | [ChatGPT](https://feedback.userjot.com/help/articles/mcp-chatgpt) |
| Cursor | [Cursor](https://feedback.userjot.com/help/articles/mcp-cursor) |
| Codex | [Codex](https://feedback.userjot.com/help/articles/mcp-codex) |
| Gemini CLI | [Common setup](https://feedback.userjot.com/help/articles/mcp-common-setup) |
| Windsurf | [Windsurf](https://feedback.userjot.com/help/articles/mcp-windsurf) |

Any other client that speaks remote HTTP MCP works with the server URL above. Clients that cannot sign in with OAuth can send an API token from `Settings` → `MCP & API` as `Authorization: Bearer YOUR_TOKEN`.

## Who can connect, and how to disconnect

Only owners and admins of a workspace can connect an app to it. Every call acts on the one workspace you chose at sign-in.

Connected apps are listed in the UserJot dashboard under `Settings` → `MCP & API` → Connected apps, with the name of the person who connected each one. You can revoke any of them there.

## What it sends to UserJot

The server receives the tool calls your assistant makes and the arguments you or the assistant supply, and nothing else from your machine. The setup skill makes no network calls of its own. It edits files in your project.

## In this repo

- `.claude-plugin/`, `.mcp.json` and `skills/`: the Claude Code plugin.
- `plugin.json` and `mcp.json`: the ChatGPT plugin package.
- `server.json`: the entry in the [MCP registry](https://registry.modelcontextprotocol.io), published as `com.userjot/userjot`.
- `llms-install.md`: setup steps an agent such as Cline follows to add the server.

The server itself is part of UserJot and is not in this repo.

## Links

- MCP documentation: https://feedback.userjot.com/help/articles/mcp-overview
- Help center: https://feedback.userjot.com/help
- Privacy policy: https://userjot.com/privacy
- Terms: https://userjot.com/terms
- Support: support@userjot.com

## License

MIT
