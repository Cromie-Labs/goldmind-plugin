# Crafts and appraisal

## Appraising items

`appraise_items` takes every item the user lists, with counts, in one call. For each one it returns the best outlet (post, vendor, disenchant or craft) and the copper per unit and in total. When crafting wins, it spells out the chain: "3 Ruined Leather Scraps → Light Leather → Light Leather Bracers". The value is spread over every unit the recipe uses, so it matches what the Goldmind tooltip shows in game. If the user's tooltip and your answer ever differ, say so plainly rather than explaining it away.

## Ranking crafts

`rank_profitable_crafts` ranks a profession's recipes, or the character's professions when none is named. It ranks two ways: by profit per craft, and by profit times sell speed.

- Lead with recipes that sell. A big profit on something that rarely sells is a caution, not a pick.
- Pass `include_shopping_list` when the user wants to act.
- `known_by_character` shows whether the character has the recipe, when Goldmind knows. Recipes the character lacks are ideas for training, not picks.
- Skill levels aren't recorded for recipes yet. So "level Blacksmithing from 250 to 300 cheaply" can't be answered from Goldmind alone. Say so, and offer the most profitable crafts instead.

## Pricing a recipe

`price_recipe` prices one recipe. For each reagent it gives the buy price. For a reagent that can itself be crafted, it also gives the cost to make it one step deep, and whether buying or making is cheaper. Use it for "should I make or buy the X for this", including crate and writ fill costs.

## Finding uses for a material

`find_recipes_using` answers "what can I make with this?". With a market it adds the value and sell speed of each output.
