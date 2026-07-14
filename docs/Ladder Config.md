# **Ladder Config**

---

# **Ladder Configuration**

**Ladders** (Battle Categories) define the competitive and casual matchmaking formats in **Cobblemon BattleHUB**. They control everything: team size, ban rules, level adjustments, and which battle mechanics (gimmicks) are allowed in the arena.

---

### **Directory Path**

`config/cobblemon_battlehub/ladders/`

---

## **Default Configuration Template**

Below is the default JSON template to create or edit a Ladder. It is based directly on the fields in the `Ladder.java` class:

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
          "banPresets": [  
            "ou"  
          ],  
          "bannedSpeciesKeys": [],  
          "bannedItemKeys": [],  
          "bannedAbilityKeys": [],  
          "bannedMoveKeys": [],  
          "bannedTierKeys": [],  
          "showdownRules": [],  
          "allowRestrictedPokemon": false,  
          "allowMythical": false,  
          "allowParadox": false,  
          "allowMega": true,  
          "allowZMove": true,  
          "allowDynamax": false,  
          "allowTera": true  
        }

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
  * "singles" (1v1)  
  * "doubles" (2v2)  
  * "triples" (3v3)  
* **`requiredTeamSize`**: Minimum number of Pokémon the player must have in their party to enter the matchmaking queue (Default: 6).  
* **`adjustLevel`**: Level to which all team Pokémon will be temporarily adjusted during battle. Use 0 to disable level adjustment and fight at the Pokémon's actual level.

### **3. Competitive Clauses**

* **`enforceSpeciesClause`** (true/false): Prevents the player from using two or more Pokémon of the same species on the same team.  
* **`enforceItemClause`** (true/false): Prevents two or more Pokémon from holding the same equipped item.

### **4. Meta Filters (Restricted, Mythical, and Paradox Pokémon)**


* **`allowRestrictedPokemon`** (true/false): If disabled, bans restricted-tier legendary Pokémon (according to official VGC rules).  
* **`allowMythical`** (true/false): Enables or disables the use of Mythical Pokémon (e.g. Mew, Celebi, Jirachi).  
* **`allowParadox`** (true/false): Enables or disables the use of Paradox Pokémon (e.g. Great Tusk, Iron Valiant).

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
* **`showdownRules`**: Allows injecting additional rules interpreted directly by Showdown into Cobblemon's engine (e.g. ["Same Type Clause"] for Monotype).

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