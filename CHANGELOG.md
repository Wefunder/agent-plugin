# Changelog

Versions here are the package's: the manifest `version` in `plugin.json`, which
is also the version each directory listing (ChatGPT, Claude, Codex, Cursor,
xAI) is submitted under. The server at `https://wefunder.com/mcp/server` has
its own version, reported on every connection; each entry names the server
version it was written against. Server changes reach every connected agent
when they deploy, whatever version a directory shows.

## 1.0.1 — 2026-10-02

Built against server **0.4.0**.

- New permission on the sign-in page, unchecked until you tick it: **Read your
  investments**. With it the agent can list your own portfolio
  (`list_my_investments`): one position per offering, with status, cost and
  current value as Wefunder records them. **Follow companies for you** stays
  preselected on first connection, as before; clear it for browse-only access.
- The people tools (`list_members`, `list_deal_investors`,
  `list_company_investors`, `search_company_investors`) never return contact or
  identity details any more; there is no longer a switch to ask for them. The
  skills say so.
- Following a company: the skill now discloses that a follow clears any earlier
  "Not interested" dismissal of that company's posts, and that unfollowing does
  not restore it.
- Agents are told to copy ids exactly from previous results, and a mistyped or
  truncated company or offering id now gets a "Did you mean …?" answer.
- README: Wefunder is in the ChatGPT directory (approved 2026-09-28); install
  from [wefunder.com/mcp/chatgpt/install](https://wefunder.com/mcp/chatgpt/install).

## 1.1.0 — 2026-09-25 (manifest only, never submitted)

The manifest carried 1.1.0 when the fourth skill (company research and
watchlist) landed, while the ChatGPT listing was submitted as 1.0.0 the same
day. 1.0.1 resets the manifest to the listing's line; from here the manifest
is the number.

## 1.0.0 — 2026-09-25

First ChatGPT directory submission, against server **0.3.0**: four skills
(browse-deals, company-research-and-watchlist, founder-investor-insights,
syndicate-manager), permissions **Browse Wefunder as you** (required) and
**Follow companies for you** (optional). Approved 2026-09-28.
