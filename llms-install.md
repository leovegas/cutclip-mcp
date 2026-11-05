# Installing the CutClip.one MCP server (instructions for AI agents such as Cline)

CutClip.one is a **hosted remote MCP server**. There is nothing to clone, build or install:
do not run `npm install`, `pip install` or `git clone`. You only add a remote server entry with a token.

## 1. Get a token from the user

Ask the user for their CutClip.one MCP token. It starts with `sfs_`.
If they don't have one:
1. Sign up at https://www.cutclip.one. New accounts get free credits.
2. Open **Settings → Connect an AI client (MCP)** in the app and create a token.

## 2. Add the server to the MCP settings

Add this entry to `mcpServers` in Cline's MCP settings file (`cline_mcp_settings.json`). Keep any existing servers.

```json
{
  "mcpServers": {
    "cutclip": {
      "type": "streamableHttp",
      "url": "https://cutclip.one/api/mcp",
      "headers": {
        "Authorization": "Bearer sfs_YOUR_TOKEN"
      },
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Replace `sfs_YOUR_TOKEN` with the user's token.

## 3. Verify

Call the `credits.get_balance` tool (some clients show it as `credits_get_balance`). It is free, and it returns the
user's credit balance.
- **401 / "Missing or invalid API token"**: the token is wrong, revoked, or missing the `Bearer ` prefix.
- **Tools are listed but calls fail**: the `Authorization` header isn't being sent. Check the `headers` block.

## Notes for the agent

- Start with `project.list_for_owner` or `project.create`, then `project.get_summary` to see a project.
- Editing and reading tools are free. Renders and AI generation (keyframes, video, music, voice) cost credits.
- `render.get_result` returns a signed download link that is valid for 24 hours.
- Every change can be reverted with `project.undo`.
