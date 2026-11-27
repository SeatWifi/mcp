# SeatWiFi plugins

[SeatWiFi](https://seatwifi.com) tracks in-flight WiFi: which provider (Starlink, Viasat, Gogo,
Panasonic, Amazon Leo) is fitted to which aircraft, whether it is free, how fast it is, and how far
each airline's Starlink rollout has progressed.

This repository holds the SeatWiFi plugin for Cursor, Claude Code, Grok and OpenAI Codex. The plugin
connects your agent to the hosted, read-only SeatWiFi MCP server (`https://mcp.seatwifi.com/mcp`,
no account needed) and adds a `seatwifi-lookup` skill.

## Install

- **Claude Code:** `/plugin marketplace add SeatWifi/mcp` then `/plugin install seatwifi@seatwifi`
- **Cursor:** install SeatWiFi from the Cursor Marketplace, or use the
  [Add to Cursor](https://seatwifi.com/for-developers) button for the MCP server only.
- **Claude.ai, ChatGPT, Grok:** add `https://mcp.seatwifi.com/mcp` as a custom connector.

## Tools

`get_wifi_facts`, `get_flight_wifi`, `get_airline_wifi`, `list_airlines`, `search_wifi`,
`get_rollouts`, `get_speed_stats`, `get_tail_info`, `open_seatwifi`. All tools are read-only.

Answers are predictions from fleet data, not live readings, and each answer gives its confidence.

## Links

- Docs: <https://seatwifi.com/for-developers>
- Privacy: <https://seatwifi.com/privacy>
- Terms: <https://seatwifi.com/terms>
- Support: <https://seatwifi.com/contact>
