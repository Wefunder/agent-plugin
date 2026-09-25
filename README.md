# Wefunder for AI agents

Connect your AI assistant to [Wefunder](https://wefunder.com), the home of the Community Round, and let it browse the startups you can invest in, look up companies and their rounds, and, if you run a syndicate or company on Wefunder, read your investor and member data.

It is **read by default**. A connected agent can never invest, reserve, or move money. With a permission you grant explicitly on the sign-in page, it can do one small thing on your behalf: follow or unfollow companies. Wefunder does not give investment advice, and neither will your agent: it reports what Wefunder shows, with links back to the source.

The server is live at `https://wefunder.com/mcp/server`; this package is how the major agents install it. Feedback and issues welcome here.

## What's in this repo

| Path | For |
|---|---|
| `plugin.json`, `mcp.json` | The portable plugin manifest ([Agent Plugins](https://agent-plugins.org) schema) with OpenAI listing metadata. Source of truth. |
| `.codex-plugin/plugin.json`, `.mcp.json` | Codex CLI and ChatGPT desktop (generated) |
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude Code plugin + marketplace (generated manifest) |
| `.cursor-plugin/plugin.json` | Cursor / Grok Bot marketplace listing (generated; includes logo) |
| `.agents/plugins/marketplace.json` | Codex / ChatGPT marketplace index pointing at this repo root |
| `skills/` | Four skills that teach an agent how to use the tools well: `browse-deals`, `company-research-and-watchlist`, `founder-investor-insights`, `syndicate-manager` |
| `assets/` | Logo and icon for directory listings |

Edit `plugin.json` or `mcp.json`, then run `python3 bin/build-manifests`; CI fails if the generated copies are stale.

## Install

Every route ends on the same Wefunder sign-in page where you choose permissions. The only required one is **Browse Wefunder as you**, which is read-only; the one optional checkbox today is **Follow companies for you**. You can turn them on later or take them away at [wefunder.com/settings/apps](https://wefunder.com/settings/apps). Manage or disconnect any time at [wefunder.com/settings/apps](https://wefunder.com/settings/apps).

### ChatGPT

*Wefunder is not yet in the ChatGPT plugin directory. Until then:*

- **Whole plugin (skills included):** Plugins → **+** → **Upload plugin** and upload a ZIP of this repo. Or, in a Business/Enterprise workspace, Admin → Plugins → Add → **Import marketplace** with source `Wefunder/agent-plugin`.
- **Server only:** Settings → Plugins → Developer Mode → on. Plugins → **+** → **Create app** → **Create MCP app**, server URL `https://wefunder.com/mcp/server`, authentication OAuth.

Leaving an optional permission unchecked makes ChatGPT show an orange "not all permissions were granted" warning. That is expected. Reinstalling does not re-authorize; delete the plugin and add it again to start over.

### Codex CLI

```bash
codex plugin marketplace add Wefunder/agent-plugin
codex plugin add wefunder@wefunder
codex mcp login wefunder
```

### Claude Code

```bash
claude plugin marketplace add Wefunder/agent-plugin
claude plugin install wefunder@wefunder
```

Then `/mcp` inside Claude Code to sign in. Server only, without the skills: `claude mcp add --transport http --scope user wefunder https://wefunder.com/mcp/server`.

### Claude (claude.ai and Claude Desktop)

Customize → Connectors → **+** → **Add custom connector** → paste `https://wefunder.com/mcp/server` → Add → Connect. Remote servers are not configured in `claude_desktop_config.json`.

### Cursor and Grok Bot

Grok Bot installs connectors from the [Cursor Marketplace](https://cursor.com/marketplace). Once Wefunder is listed there, install **Wefunder** from the marketplace in Cursor or Grok Bot, then complete the Wefunder sign-in when prompted.

Until the listing is live, either:

- **Plugin (skills included):** copy this repo to `~/.cursor/plugins/local/wefunder`, reload Cursor, and confirm skills + the Wefunder MCP server appear under Customize. Or submit the public repo at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish) for review.
- **Server only:** add to `~/.cursor/mcp.json` (all projects) or `.cursor/mcp.json` (one project):

```json
{ "mcpServers": { "wefunder": { "url": "https://wefunder.com/mcp/server" } } }
```

Then sign in when Cursor prompts.

### Grok (grok.com) and Grok Build

- **grok.com connectors:** Connectors → **New Connector** → **Custom** → paste `https://wefunder.com/mcp/server` and complete sign-in.
- **Grok Build:** this repo is packaged for the [xAI plugin marketplace](https://github.com/xai-org/plugin-marketplace). After the catalog entry merges, install **wefunder** from that marketplace.

### Anything else that speaks MCP

Server URL `https://wefunder.com/mcp/server`, Streamable HTTP transport, OAuth 2.1 with PKCE and dynamic client registration. Discovery documents are at `/.well-known/oauth-protected-resource/mcp/server` and `/.well-known/oauth-authorization-server`. Full guide, written for humans and agents alike: [wefunder.com/mcp/README.md](https://wefunder.com/mcp/README.md). If your agent can read a URL, paste it that guide and ask it to walk you through connecting.

## What the agent can do

For every connected investor: browse offerings with server-side filters (exemption, security type, testing-the-waters, minimum, closing date, amount raised), show them as a card carousel where the host supports it, search companies by name, and read a company's page and any round as structured data.

For founders and team members: fundraising dashboard totals, round history, investor lists, semantic investor search, connections by employment history, and a specific investor's investments in your company.

For syndicate managers: syndicates, members, member investments, deals, deal investors, and recent activity.

With the optional permission: list, follow, and unfollow the companies you follow.

Authorization is enforced by the Wefunder server on every call, exactly as it is for any API client; a prompt cannot talk its way past a 403.

## License

MIT for everything here except the Wefunder name and mark in `assets/`, which remain trademarks of Wefunder Inc.
