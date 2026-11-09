# Farms

A good farm answer is two or three options, each backed by the maths: the full loot table of the camp, what to do with every item, and what it comes to per hour and per level. Goldmind does the sums; you look up what it hasn't measured yet and present the result.

## The flow

1. **`recommend_farms`** with the user's words as `character` ("my level 34"). Pass `zone` only if they named one. It ranks the farms the character qualifies for by projected copper per 100 attempts (kills, nodes, casts or chests), from the targets' whole loot table plus coin, with `per_hour` and `per_level` on kill farms.
2. **Read `lookups_wanted`.** It lists the exact rates Goldmind can't measure yet ("drop chance of Wool Cloth from Syndicate Footpad in Hillsbrad Foothills per kill, or units per kill"). If you can search, look those up now (recent Forever sources first; classic-era tables are a fallback and may differ). Then call `recommend_farms` again with what you found in `known_rates`; Goldmind redoes the projection and marks those rows `supplied`. If you can't look them up, say so and use Goldmind's ranges.
3. **`get_farm_details`** for the top two or three. Each returns the full loot table with a `plan` per item (post, vendor, disenchant or craft, decided as the Goldmind tooltip decides it, with the craft chain), coin per kill, skinning, per hour, per level and the guides it came from.
4. **Present** the options as below. Three calls plus the lookups is the normal cost; don't call `get_farm_details` for every row.

### When the catalog is thin

`recommend_farms` adds `catalog_note` when Goldmind's catalog has fewer than three farms for the level, or none with kill data (common above level 25 while the catalog grows). Then the camps come from you and Goldmind does the maths:

1. Name two or three camps you know for this character: level-appropriate creatures that drop cloth, leather, meat, elemental reagents or coin, in zones for its faction. If you can search, check a recent Forever guide for the loot and rates.
2. Call **`project_farm`** for each, with `targets` (creatures as named in game), `zone`, and `loot` (every item you know it drops, with `rate` and `unit` where you have them, `note` on the source). Pass `coin_per_kill` if you know it. Goldmind matches the targets to its drop sources, adds every drop row it has (its own rate wins), prices the whole table with a plan per item, and returns value per 100 kills, `per_hour` from its own kill logs at the character's level band and `per_level`. `targets[].matched` says which creatures Goldmind has seen; `lookups_wanted` names what is still unrated.
3. Present exactly as for catalog farms: the numbers are Goldmind's projection on your loot table, so say "loot table from <where>, priced and paced by Goldmind".

A camp with no match and no `loot` gets nothing priced: always pass the loot table. Skinning counts only where Goldmind has skin rows for the matched targets.

Unmet requirements are left out. Pass `include_unmet: true` when the user wants to know what a profession or a few levels would unlock; `skinning.needs` and `excluded_unmet` say what.

## Presenting the plan: the invoice

Every farm answer carries `lines`, the invoice of what 100 kills bring. Present it as line items, so the projection is complete, not just the headline item:

| Line | What it is | Where the figures come from |
|---|---|---|
| **main loot** | the items the camp is for (Tiger Meat, Raptor Flesh) | Goldmind's rows, or your supplied rates |
| **skinning** | the skins and hides a skinner takes off the same kills, as units per skin | the targets' skin rows; when Goldmind has none, the pool of creatures at that level (`basis` says so) |
| **other drops** | grays to vendor, greens, the rest | the targets' rows; for a camp Goldmind hasn't seen, the level band's pool |
| **coin** | copper per kill | Goldmind's figure, or the one you passed |
| **gathering** | herbs or ore picked while grinding, per hour | the zone's nodes Goldmind has seen x the `nodes_per_hour` you passed |

`counted` says whether the line is in `value_per_100` (skinning needs the skill; gathering needs the profession and a node pace). Show each line with its items, units per kill (or per skin, per node), what each is worth and the plan, and the line's total. Then the total per 100 kills, gold per hour (plus the gathering line's per hour when it counts) and gold by the next level.

For each of the 2-3 options, then:

- **One line:** farm, zone, and the headline: "about 7g 80s per 100 kills, 6g 50s to 8g 50s per hour, about 12g by level 24".
- **The invoice** as above. Mark each rate: measured, pooled from the level band, from another source, or not known.
- **One line on the maths:** how many of the loot rates are measured (known or good), and what the per-hour range rests on ("Goldmind's own kill logs at levels 20-24, 10 sessions"). Per-level figures are approximate: they use the classic XP table and the camp's XP per kill.

For a skinner, always pass `skinnable: true` on beast camps you project yourself, and for a herbalist or miner pass `gathering` with a node pace from the guide (or a modest estimate, said as such), so those lines exist.

Then say which you'd pick and why, in one line. Keep the sources apart in every number: Goldmind measured, pooled, you supplied, nobody knows yet. Give the pick plainly; no "these are my picks, not Goldmind's".

## Recipes: the shuffle inside the farm

A `plan` that says craft (or `craft_would_pay`) now carries `recipes[].known` from the character's own profession windows, and a `recipe_tip` when the recipe is missing: "Recipe: Jungle Stew (Cooking) is not on this character yet; with it each unit is worth 34s instead of 4s 57c." That is the best kind of tip, so lead with it when the gap is big: say what the recipe turns the loot into, what it is worth with and without, and where to get the recipe if you can look it up (`recipe_lookups` names what to search; Goldmind doesn't record recipe sources yet). `known: null` means Goldmind hasn't seen that profession window on this character; say "open your Cooking window once with the addon on and Goldmind will know".

## Vendor calls and the next move

When the plan says vendor, say why in the player's terms: "Silk sells for barely more than the vendor pays, so take the cash now; at this stage the gold does more in a better buy than sitting in listings." A vendor call is a cash-flow call, not a loss. Then offer the next move once, as a question: "Want ideas for what to put that gold into?" or "Want me to price what's in your bags?" One offer, not a menu.

## When the data is thin

`data_note` appears when the answer leans on pooled, partial or supplied figures. Say it as Goldmind does: Goldmind is still collecting data for this level range, every synced session sharpens the numbers, and more players syncing gets there faster. One sentence, warm, no apology; never "Goldmind has only measured X".

## What the numbers are

- `value_per_100` = the targets' whole loot table (expected units per attempt of every item Goldmind has seen drop there, times its value after the auction cut and a sell-speed factor, or its vendor price when higher) plus `coin_per_100`, plus `skinning` when the character can skin the targets. Targets are weighted by the kills Goldmind saw of each.
- `per_hour` = value per kill times kills per hour, a low-high range from Goldmind's own kill logs at the character's level band. It is never a creator's claim; when the band is thin it pools every level and says so.
- `per_level` = value per kill times kills to the next level (XP to level over XP per kill from the camp's kills, else the band's).
- `loot.measured` says how much of the table is known / good / partial / unknown / pooled / supplied. The guide's own `outputs` are kept too, with `measured` for them.
- Every row is labelled `source: "research, uncurated"` until Goldmind's curated farm list is live. Treat the farm ideas as ideas, and Goldmind's projection as the number.

## Gathering and skinning yields

Node, herb, fishing and skinning yields carry `bonus_note`: they depend on legacy traits and buffs, which Goldmind doesn't read yet, so they are pooled across every player's bonuses. Say so in one line when a gathering farm or a skinning figure is in the answer ("yields pooled across players' bonuses"), and don't present them as this character's own rate. Kill loot is unaffected.

## Sell speed matters

An output that "rarely sells" is worth much less than its price, and a common vendor good is worth its vendor price only. The projection already discounts by sell speed; say so when it changes the ranking or the plan.
