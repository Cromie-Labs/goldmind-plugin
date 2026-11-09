---
name: goldmind-market-watch
description: Runs a recurring WoW Forever auction-house watch with the Goldmind connector - on a schedule, checks what moved on the player's market since the last run and reports only what is worth acting on. Use when the user asks to watch, monitor or check the auction house regularly, or to be told when prices of items they care about move.
---

# Goldmind market watch

For scheduled or repeated runs, such as a daily task, a loop or a cron agent. Keep each run small and quiet.

1. **First run:** call `get_player_context`. Write down the character, the market and the items the user cares about (a watchlist), and keep them in the task's own notes or memory for later runs.
2. **Each run:** call `get_market_movers` with `since` set to the previous run's time. If there's a watchlist, also call `get_auction_prices` with the whole list in one call.
3. **Report only** what crossed a line the user set, or a big move on something that actually sells (sell speed "sells" or better). If nothing did, say "Nothing worth acting on" in one line, or stay silent if the scheduler allows it.
4. **Never** post, buy or cancel anything, and never write macros that do. The user acts in game.
5. **Report format:** item, then and now, % change, sell speed, age. Five rows at most.
