# **Ladder Config**

---

## **Ladder Configuration**

**Ladders** (Battle Categories) define the competitive and casual matchmaking formats in **Cobblemon BattleHUB**. They control everything: team size, ban rules, level adjustments, and which battle mechanics (gimmicks) are allowed in the arena.

Each ladder is one `.json` file. Which ones are actually offered in the queue is decided by `activeRankedLadders` / `activeCasualLadders` in [`server_config.json`](Server Config.md) — a ladder file that isn't listed there still loads but is never queueable.

---

### **Directory Path**

The mod organizes ladders into two directories to keep defaults and custom creations separate:

* **Root Directory (Default Ladders):** `config/cobblemon_battlehub/ladders/`
* **Custom Directory (Player/Admin Creations):** `config/cobblemon_battlehub/ladders/custom_ladders/`

---

## **Auto-Generated Ladders**

If the root folder is empty, the server generates a complete set of standard formats on first boot:

| Generated IDs | Type | Notes |
| :--- | :--- | :--- |
| `singles_50_casual`, `singles_100_casual`, `singles_ranked` | Singles | Casual at Lv. 50 and Lv. 100; ranked at Lv. 50 |
| `doubles_50_casual`, `doubles_100_casual`, `doubles_ranked` | Doubles | same pattern |
| `triples_50_casual`, `triples_100_casual`, `triples_ranked` | Triples | same pattern |
| `monotype_50_casual`, `monotype_100_casual` | Monotype (Singles) | Same Type Clause enforced by the `monotype` in the ID |
| `tourney_solo`, `tourney_duplas`, `tourney_trios`, `tourney_monotype` | Tournament | used by the tournament system, not the normal queue |

Every generated file is written with **class defaults** for the rules, and the ranked / tournament ones additionally get `"banPresets": ["ou"]` (monotype tournament gets `["monotype"]`). So a freshly generated `singles_ranked.json` still has `allowDynamax: true`, `allowRestrictedLegendary: true`, `maxSubLegendary: 6`, etc. — **if you want a strict ranked meta you must tighten those yourself.**

---

## **Configuration Template**

Below is a full JSON template for a Ladder, showing every field from the `Ladder.java` class. The values here are an illustrative **strict ranked Singles** example — not a copy of the auto-generated file. For reference, the class defaults (what you get if you omit a field) are: `enforce*Clause: true`, every `allow*: true`, every `max*: 6`, `requiredTeamSize: 6`, `adjustLevel: 50`.

```json
{
  "id": "singles_ranked",
  "queueLabel": "Ranked Queue",
  "displayName": "Ranked Singles Lv. 50",
  "description": "Official Ranked Singles format.",
  "ranked": true,

  "battleTypeId": "singles",
  "requiredTeamSize": 6,
  "adjustLevel": 50,

  "enforceSpeciesClause": true,
  "enforceItemClause": true,

  "banPresets": ["ou"],
  "bannedSpeciesKeys": [],
  "bannedItemKeys": [],
  "bannedAbilityKeys": [],
  "bannedMoveKeys": [],

  "allowRestrictedLegendary": false,
  "allowMythical": false,
  "allowParadox": false,
  "allowMega": true,
  "allowZMove": true,
  "allowDynamax": false,
  "allowTera": true,

  "maxSubLegendary": 1,
  "maxRestricted": 1,
  "maxMythical": 1,
  "maxParadox": 1,
  "maxCombinedSpecial": 1
}
```

!!! warning "Valid JSON"
    Every line except the last inside an object needs a trailing comma, and there is **no** comma after the closing `}`. Copy the block above exactly — a missing comma (a common mistake right after `allowTera`) makes the whole file fail to load and the ladder falls back to defaults.

---

## **Detailed Parameter Explanation**

### **1. Identification and Display (Matchmaking UI)**

* **`id`**: The category's unique internal ID (must match the filename exactly without the .json extension).  
* **`queueLabel`**: The queue name shown in network packets and interfaces (e.g. Ranked Queue, Quick Queue, Tournament).  
* **`displayName`**: Friendly title displayed to players inside the mod menus.  
* **`description`**: Brief description explaining the queue rules or purpose in the interface.  
* **`ranked`** (true/false): Determines whether the category awards ranking points (Elo/Glicko-2) and updates the global leaderboard.

### **2. Combat Structural Rules**

* **`battleTypeId`**: The battle field format. Supported values:  
  * `"singles"` (1v1)  
  * `"doubles"` (2v2)  
  * `"triples"` (3v3)  
* **`requiredTeamSize`**: Number of Pokémon the player must bring to enter the queue / accept a duel (Default: 6). Set it lower (e.g. `3`) for a bring-3-pick-3 style format.  
* **`adjustLevel`**: Level all team Pokémon are temporarily set to for the battle (Default: 50). Use **`0`** to disable adjustment and fight at each Pokémon's real level.

### **3. Competitive Clauses**

* **`enforceSpeciesClause`** (true/false): Prevents the player from using two or more Pokémon of the same species on the same team.  
* **`enforceItemClause`** (true/false): Prevents two or more Pokémon from holding the same equipped item.

### **4. Meta Filters (Legendary, Mythical, and Paradox Pokémon)**

BattleHUB reads Cobblemon's own labels for each species, so these filters stay accurate as Cobblemon adds new Pokémon — you never maintain a manual list.

**Allow toggles** — an `allow* : false` bans that whole category outright:

* **`allowRestrictedLegendary`** (true/false): *Restricted* legendaries — the box-art / cover legendaries (Mewtwo, Rayquaza, Koraidon…) that official VGC restricts.
* **`allowMythical`** (true/false): Mythical Pokémon (Mew, Celebi, Jirachi, …).
* **`allowParadox`** (true/false): Paradox Pokémon (Great Tusk, Iron Valiant, …).

**Max counts** — used when the matching `allow*` is `true`, to cap how many of that category a team may bring (class default `6` = effectively unlimited):

* **`maxSubLegendary`**: Max Pokémon carrying Cobblemon's `legendary` / `sub-legendary` label that are **not** on the restricted list (Articuno, Zapdos, Regirock, the genies…). There is no `allowSubLegendary` toggle; set this to `0` to ban them entirely.
* **`maxRestricted`**: Max restricted legendaries (only relevant if `allowRestrictedLegendary` is `true`).
* **`maxMythical`**: Max mythical Pokémon.
* **`maxParadox`**: Max paradox Pokémon.
* **`maxCombinedSpecial`**: Max **combined** total across all of the special categories above — e.g. `1` means the team may bring one legendary *or* one mythical *or* one paradox, not one of each.

### **5. Arena Mechanics (Gimmicks)**

* **`allowMega`** (true/false): Allows or bans Mega Evolution in battle.  
* **`allowZMove`** (true/false): Allows or bans the use of Z-Moves.  
* **`allowDynamax`** (true/false): Allows or bans Dynamax and Gigantamax mechanics.  
* **`allowTera`** (true/false): Allows or bans Terastal (Paldea phenomenon).

### **6. Ban Lists & Custom Rules (Showdown)**

* **`banPresets`**: A list containing the IDs of ban preset JSON files you want to inherit (e.g. ["ou"], ["lc"], ["legendaries"]).  
* **`bannedSpeciesKeys`**: Manual list to ban specific Pokémon by ID (e.g. ["charizard", "mewtwo"]).  
* **`bannedItemKeys`**: Manual list to ban specific items.  
* **`bannedAbilityKeys`**: Manual list to ban specific abilities.  
* **`bannedMoveKeys`**: Manual list to ban specific moves.  
---

## **Dynamic Suffix Logic (Custom Duels)**

The `Ladder.java` class includes an intelligent **Dynamic Suffix** system for when two players challenge each other to a direct duel and decide to disable certain mechanics on the fly (using the mod's invite UI).

If players choose to disable gimmicks in the menu, the mod generates an in-memory modified copy of the Ladder by adding the following invisible suffixes to the ID:

* `_nomega` (Disables Mega Evolution)  
* `_noz` (Disables Z-Moves)  
* `_nodyna` (Disables Dynamax)  
* `_notera` (Disables Terastal)

**Example:** If your base Ladder is `singles_ranked` and the player disables Tera on the invite screen, the mod temporarily creates a copy under the ID `singles_ranked_notera` with the option `"allowTera": false` applied automatically, ensuring the battle proceeds fairly and without rule desynchronization.

---