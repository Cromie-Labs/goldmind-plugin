# Goldmind for Claude

Goldmind brings live WoW Forever auction-house prices into Claude, for your own characters. Ask what your bags are worth, which crafts pay on your server, where an item drops, what to farm at your level, what to buy and hold, or what's moving today. Answers use real scans from players running the Goldmind sync app, with the age of every number.

## What's inside

- **Goldmind connector:** read-only tools over Goldmind's data at `https://api.goldmind.gg/api/mcp`. You sign in with your Goldmind account.
- **`goldmind-advisor` skill:** teaches Claude to pick your character from what you ask, batch lookups, read drop-rate confidence honestly, and keep answers short.
- **`goldmind-market-watch` skill:** for recurring checks of your market.

## Access

The data tools work for accounts whose Goldmind sync app has synced in the last 7 days. `get_player_context` always answers, and says how access opens. Learn more at https://goldmind.gg/mcp.

## Privacy

Goldmind reads only your own account's characters and the shared market data. See https://goldmind.gg/privacy.

## Support

https://goldmind.gg/support
