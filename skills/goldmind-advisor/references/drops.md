# Drop rates

`get_drop_rates` takes one of `item`, `creature`, `object` or `zone`.

- **By item:** where it drops. Each source has `rate` (chance per attempt), `ci_low`–`ci_high`, `attempts`, `need` (attempts still needed to call it known) and `knowledge`.
- **By creature or object:** its loot table.
- **By zone:** what drops there. With a market, the best value per attempt comes first.

`coverage` counts known (exact), good (close enough to compare farms, within about half), partial (rough) and unknown rows; known and good are "measured". Lead with it when it's low: "Goldmind has measured 3 of 11 drops here so far."

**Methods:** `kill`, `skin`, `mine`, `herb`, `fish`, `open` (chests and containers) and `craft`.

**`per_attempt`** is expected units per attempt: rate × average stack. Use it for value, not `rate`.

**Gathering, skinning and fishing** (`skin`, `herb`, `mine`, `fish`) carry `bonus_note`: those yields depend on legacy traits and buffs (Bountiful Harvest, Luremaster, lures), and Goldmind pools them across every player's bonuses for now. Say "pooled across players" for those; never present them as this character's own rate. Kill rates are not affected.

## When there's a gap

- **Look it up** if you can: drop rate, source and Forever context. The farm tools spell out what to search for in `lookups_wanted` (and in each `gap.lookups`): item, source, zone, level range, what Goldmind has, and the figure wanted. Then pass it to the farm tools in `known_rates`:

  ```json
  { "item": "Large Fang", "source": "Plains Hyena", "rate": 0.12, "unit": "chance", "note": "community drop table, classic era" }
  ```

- **`rate`** is a chance per attempt (0–1, or a percent); for stackable drops pass units per attempt with `unit: "units"`. `get_drop_rates` takes `known_rates` too. A rate Goldmind has as known or good keeps Goldmind's number; partial and unknown rows take yours.
- **In the maths,** an unknown row is listed unrated and never counted (a farm's value only has what Goldmind can stand behind, or what you supplied); a partial row counts with its bracket; good and known count as numbers. So a camp whose loot is mostly unknown shows a low value with many `lookups_wanted`: supply the rates rather than reading the low value as the answer.
- **Supplied values** count for that one answer only. Goldmind doesn't keep them. That's deliberate, so say "from another source" whenever you use one.
- **If you can't look it up,** say so and use Goldmind's range. Don't ask the user.
