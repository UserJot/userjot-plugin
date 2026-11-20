# Installing the UserJot MCP server (instructions for AI agents)

UserJot runs one hosted MCP server at `https://api.userjot.com/v1/mcp`. This repo holds configuration and no server code, so do not clone it, build it, install packages, or start anything locally.

1. Add a remote MCP server with these settings:
   - Name: `userjot`
   - URL: `https://api.userjot.com/v1/mcp`
   - Transport: Streamable HTTP

   In Cline, add this entry to the `mcpServers` object in `cline_mcp_settings.json` and keep every server that is already there:

   ```json
   {
     "mcpServers": {
       "userjot": {
         "type": "streamableHttp",
         "url": "https://api.userjot.com/v1/mcp",
         "disabled": false,
         "autoApprove": []
       }
     }
   }
   ```

   Write `streamableHttp` exactly as shown. Without `type`, Cline uses the older SSE transport and the connection fails.

2. Do not ask the user for an API key, and do not add headers or environment variables. The server signs in with OAuth. When the `userjot` row in Cline's MCP Servers panel shows an **Authenticate** button, ask the user to click it. A browser window opens where they sign in to UserJot, pick a workspace, and click Allow. In the Cline CLI, run `cline mcp` and choose **Authorize OAuth**.

   The user needs a UserJot account and must be an owner or admin of the workspace they pick. A free account works, and they can create one at https://userjot.com.

3. Check the install by calling `getWorkspace`. It only reads, and it returns the name and URL of the workspace the user connected.

Notes:

- Every tool acts on the one workspace chosen at sign-in. To use a different workspace, authenticate again and pick it.
- Leave `autoApprove` empty, or list only tools whose names start with `get`, `list`, `search` or `lookup`. Those only read. The rest create, change or delete things in the workspace, and some reach customers: `createConversation` and `createConversationReply` send a message to a customer, and `updateRequestStatus` with an `actor` emails the people following that request. Cline should ask before each of those. That holds only while Cline's **Use MCP servers** auto-approve setting is off, so tell the user if it is on.
- If the **Authenticate** button comes back later, the sign-in has ended. Click it again.
- If the user's client cannot sign in with OAuth, they can create an API token in UserJot under `Settings` → `MCP & API` and send it as an `Authorization: Bearer` header. Use this only when OAuth is not available, and never write the token into a file that is committed.
- The server only runs at `https://api.userjot.com/v1/mcp`. Do not configure a localhost URL.
- Claude Code users can install this repo as a plugin, which adds two skills as well. The [README](README.md) has those commands.
