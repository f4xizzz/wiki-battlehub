# **Ladder Custom Example**

---

## **Practical Guide: Creating an Event Ladder (Little Cup)**

**Cobblemon BattleHUB** lets you create fully custom battle formats for temporary events, special tournaments, or new permanent queues.

For this guide we build a category based on Smogon's classic **Little Cup (LC)**: only unevolved Pokémon, everything set to **Level 5**, with all power mechanics (Mega, Z-Move, Dynamax, Tera) disabled.

---

### **Custom Directory Path**

Create custom categories inside the customization subfolder so they don't get mixed with the mod's default queues:

`config/cobblemon_battlehub/ladders/custom_ladders/`

## **Step 1: Create the Ladder JSON File**

In `custom_ladders/`, create `little_cup_event.json`:

```json
{
  "id": "little_cup_event",
  "queueLabel": "Event Queue",
  "displayName": "Little Cup Event",
  "description": "Only Level 5 Pokémon! No Megas, Z-Moves, Dynamax, or Terastal.",
  "ranked": false,

  "battleTypeId": "singles",
  "requiredTeamSize": 6,
  "adjustLevel": 5,

  "enforceSpeciesClause": true,
  "enforceItemClause": true,

  "banPresets": ["lc"],
  "bannedSpeciesKeys": ["ditto", "arceus"],
  "bannedItemKeys": ["choice_band"],
  "bannedAbilityKeys": ["sturdy"],
  "bannedMoveKeys": ["swords_dance"],

  "allowRestrictedLegendary": false,
  "allowMythical": false,
  "allowParadox": false,
  "allowMega": false,
  "allowZMove": false,
  "allowDynamax": false,
  "allowTera": false,

  "maxSubLegendary": 0,
  "maxRestricted": 0,
  "maxMythical": 0,
  "maxParadox": 0,
  "maxCombinedSpecial": 0
}
```

!!! warning "It has to be valid JSON"
    Every entry needs a trailing comma **except the last one before a `}`**, and there is no comma after the final `}`. One missing comma and the whole file fails to load. Note the field is **`allowRestrictedLegendary`** — `allowRestrictedPokemon` (the internal preset name) is silently ignored here.

### **What are we configuring here?**

1. **`id`** — `little_cup_event`, must match the file name exactly.
2. **`adjustLevel`** — `5`; every Pokémon is temporarily set to level 5 for the battle.
3. **`banPresets`** — loads the `lc` preset bundled with the mod, which bans the strong "LC-unviable" Pokémon for you (see [Ban Presets](Ban Presets.md)). Direct `bannedSpeciesKeys` / `bannedItemKeys` / `bannedAbilityKeys` / `bannedMoveKeys` are merged on top; use lowercase IDs with underscores (`swords_dance`), the `cobblemon:` prefix is optional.
4. **Disabled mechanics** — every `allow*` gimmick is `false`, and the `max*` caps are `0`, so no legendary / mythical / paradox Pokémon can sneak in.

## **Step 2: Register the Queue in the Server Config**

Dropping the JSON into the custom folder loads it into memory, but it will **not appear in the matchmaking menu** until you list it in the server config.

1. Open `config/cobblemon_battlehub/server_config.json`.
2. Add the Ladder ID to the right list. We set `"ranked": false`, so it goes in the casual list:

```json
"activeCasualLadders": [
  "singles_50_casual",
  "doubles_50_casual",
  "little_cup_event"
]
```

!!! tip "Make it ranked instead"
    To have this queue award rating and appear on the leaderboard, set `"ranked": true` in the Ladder file and list its ID under `"activeRankedLadders"` instead.

## **Step 3: Sync the Changes**

No server restart needed. From the console or in-game as an admin:

`/bh reload`

The **Little Cup Event** queue appears instantly in players' menus with the rules, level adjustment, and restrictions you defined.

---
