# SeatWiFi plugin

One plugin folder for Cursor, Claude Code, Grok and OpenAI (Codex and the ChatGPT directory). It connects
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
| `.grok-plugin/plugin.json`, `mcp.json` | Grok |
| `skills/seatwifi-lookup/SKILL.md` | All four |
| `assets/logo.png` | All four (512 x 512 PNG on a solid `#0A0E14` background, fully opaque) |

The marketplace files at the repository root (`.cursor-plugin/marketplace.json` and
`.claude-plugin/marketplace.json`) point here. Keep `version` the same in all four manifests.

Validate the plugin:

```bash
claude plugin validate ./plugins/seatwifi --profile directory
claude plugin validate .
```

## Install by hand (no directory listing)

| Client | How |
|---|---|
| Cursor | [Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=seatwifi&config=eyJ1cmwiOiJodHRwczovL21jcC5zZWF0d2lmaS5jb20vbWNwIn0=), or symlink this folder into `~/.cursor/plugins/local/seatwifi` to test the full plugin |
| Claude Code | `/plugin marketplace add SeatWifi/mcp` then `/plugin install seatwifi@seatwifi` |
| Claude.ai | Settings → Connectors → Add custom connector → `https://mcp.seatwifi.com/mcp` |
| ChatGPT | Developer mode in Settings → Apps, then add an app with the URL and no authentication |
| Grok | grok.com/connectors → New Connector → Custom → the URL. A paid Grok plan is necessary |
| Codex | `codex mcp add seatwifi --url https://mcp.seatwifi.com/mcp` |

## Test cases

Five positive and three negative cases, also listed in `.codex-plugin/plugin.json`.

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
custom connector (see the table above), or install the plugin from `.grok-plugin/plugin.json`. Grok Business and Enterprise admins can add the same URL for
their team.
