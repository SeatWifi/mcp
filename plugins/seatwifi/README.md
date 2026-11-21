# SeatWiFi plugin

One plugin folder for Cursor, Claude Code and OpenAI (Codex and the ChatGPT directory). It connects
the agent to the hosted SeatWiFi MCP server at `https://mcp.seatwifi.com/mcp` and adds one skill,
`seatwifi-lookup`, that tells the agent which tool to use and how to give the answer.

The server is read-only and needs no account. A paid SeatWiFi API key is optional and goes in an
`Authorization: Bearer` header.

## What is in this folder

| File | Used by |
|---|---|
| `.cursor-plugin/plugin.json`, `mcp.json` | Cursor |
| `.claude-plugin/plugin.json`, `.mcp.json` | Claude Code and the Claude plugin directory |
| `.codex-plugin/plugin.json`, `.mcp.json` | OpenAI Codex and the ChatGPT plugin directory |
| `skills/seatwifi-lookup/SKILL.md` | All three |
| The logo image (512 x 512 PNG) | All three |

The marketplace files at the repository root (`.cursor-plugin/marketplace.json` and
`.claude-plugin/marketplace.json`) point here. Keep `version` the same in all three manifests and in
`mcp/package.json`.

Check it before you submit:

```bash
claude plugin validate ./plugins/seatwifi --profile directory
claude plugin validate .
```

## Install by hand (no directory listing)

| Client | How |
|---|---|
| Cursor | [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=seatwifi&config=eyJ1cmwiOiJodHRwczovL21jcC5zZWF0d2lmaS5jb20vbWNwIn0=), or symlink this folder into `~/.cursor/plugins/local/seatwifi` to test the full plugin |
| Claude Code | `/plugin marketplace add <this repo>` then `/plugin install seatwifi@seatwifi` (needs read access to the repo) |
| Claude.ai | Settings → Connectors → Add custom connector → `https://mcp.seatwifi.com/mcp` |
| ChatGPT | Developer mode in Settings → Apps, then add an app with the URL and no authentication |
| Grok | grok.com/connectors → New Connector → Custom → the URL. A paid Grok plan is necessary |
| Codex | `codex mcp add seatwifi --url https://mcp.seatwifi.com/mcp` |

## Submit to the directories

Each directory reviews by hand. Nothing here is submitted automatically.

### Cursor Marketplace

1. The marketplace reads the plugin from a **public** Git repository. This repository is private, so
   copy this folder and the root `.cursor-plugin/marketplace.json` into a public repository (for
   example `seatwifi-plugins`). Change `source` to the new path.
2. Sign in and submit the repository link at <https://cursor.com/marketplace/publish>.
3. Submit one time only. A second submission makes a duplicate in the review queue.

### Claude (two listings)

- **Connector** (Claude.ai, desktop and mobile): <https://claude.ai/directory/manage> → Submit new →
  MCP connector. Connection URL `https://mcp.seatwifi.com/mcp`, authentication `none`. Every tool
  has a `title`, `readOnlyHint: true` and `destructiveHint: false`. Privacy policy:
  `https://seatwifi.com/privacy`. Support: `https://seatwifi.com/contact`. A paid Claude plan is
  necessary to submit.
- **Plugin** (Claude Code): the same portal → Plugin bundle, with this folder.

### ChatGPT directory (OpenAI plugin)

1. In the OpenAI platform organization, complete individual or business verification. You need the
   Apps Management Write permission.
2. Make the ZIP from this folder:
   put the `.codex-plugin`, `.mcp.json`, `skills`, logo folder and `LICENSE` from this folder in one ZIP file.
3. <https://platform.openai.com/plugins> → Upload new or existing plugin → upload the ZIP.
4. MCPs → Connect. The portal gives a domain verification token. Set it on the MCP worker, deploy,
   then let the portal check it:
   `cd mcp && npx wrangler secret put OPENAI_APPS_CHALLENGE` (the worker serves it at
   `https://mcp.seatwifi.com/.well-known/openai-apps-challenge`).
5. Enter the test cases below, a demo video URL, and submit. Authentication: none.
6. If the portal rejects `.mcp.json` in the ZIP, remove it and the `mcpServers` line, and connect the
   URL in the portal only.

`interface.termsOfServiceURL` points to `https://seatwifi.com/terms` (`src/worker/pages/terms.ts`).

#### Test cases for the OpenAI review

OpenAI asks for 5 positive and 3 negative cases.

| # | Prompt | Tools | Expected |
|---|---|---|---|
| P1 | Will UA1234 have Starlink WiFi on its next flight? | `get_wifi_facts` | Gives the Starlink odds, the confidence level and a link to the SeatWiFi flight page |
| P2 | What WiFi does Qatar Airways have, and is it free? | `get_airline_wifi` | Names the providers, the free conditions and the cabins |
| P3 | Which airlines are installing Starlink right now? | `get_rollouts` | Lists airlines with an in-progress rollout and their progress |
| P4 | Is the WiFi on Delta fast enough for a video call? | `get_speed_stats` | Gives measured speeds and says if they are enough for video calls |
| P5 | What WiFi is fitted to aircraft N37502? | `get_tail_info` | Gives the provider history of that tail number |

| # | Prompt | Expected |
|---|---|---|
| N1 | Book me a flight from London to New York. | Does not call SeatWiFi. SeatWiFi cannot book flights |
| N2 | What is the WiFi password at my hotel? | Does not call SeatWiFi. It covers in-flight WiFi only |
| N3 | Give me a recipe for banana bread. | Does not call SeatWiFi |

### Grok

Grok has no public directory for third-party connectors at this time. Users add SeatWiFi as a
custom connector (see the table above). Grok Business and Enterprise admins can add the same URL for
their team.
