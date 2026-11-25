---
name: seatwifi-lookup
description: Look up in-flight WiFi for a flight, airline, aircraft tail number or satellite WiFi rollout with the SeatWiFi tools. Use when someone asks about WiFi, internet, satellite WiFi, streaming or working online on a flight.
---

# SeatWiFi lookup

SeatWiFi tracks in-flight WiFi: which onboard WiFi system is fitted to which aircraft, whether it is free, how fast it is, and how far each airline's satellite WiFi rollout has progressed.

## Pick a tool

Use the SeatWiFi MCP tools. Pick the tool by what the user gave you:

| User gave you | Tool |
|---|---|
| A flight number (for example `UA1234`) | `get_wifi_facts`. Add the flight date when you know it. |
| An airline | `get_airline_wifi`, then `get_rollouts` for rollout timelines |
| A tail number (for example `N12345`) | `get_tail_info` |
| A question about speed, or "is it fast enough for calls" | `get_speed_stats` |
| A name or a partial term | `search_wifi` |
| No details yet | `open_seatwifi` (home view) or `list_airlines` |

## Rules for the answer

1. The answers are predictions from fleet data. They are not live readings from the aircraft.
2. Always give the confidence or the probability that the tool returns. Do not say that WiFi is certain.
3. For a flight, quote the `answer` line from `get_wifi_facts`.
4. Link to the SeatWiFi page in the result (the `url` field) so the user can see the full data.

## Without the MCP server

If the tools are not available, read the pages as Markdown. Send `Accept: text/markdown` or add `.md` to the URL:

- Airline: `https://seatwifi.com/airlines/{iata}.md`
- All hub pages: `https://seatwifi.com/llms.txt`
