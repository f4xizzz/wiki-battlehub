# **Ladder Custom Example**

---

# **Practical Guide: Creating an Event Ladder (Little Cup)**

**Cobblemon BattleHUB** lets you create fully custom battle formats for temporary events, special tournaments, or new permanent queues.

For this guide, we will create a category based on Smogon’s classic **`Little Cup (LC)`** format: only baby Pokémon or their first evolution stage, adjusted to **Level 5**, with all power mechanics `(Mega, Z-Move, Dynamax, and Tera)` completely disabled.

---

### **Custom Directory Path**

All custom categories should be created inside the customization subfolder to avoid mixing them with the mod’s default queues:

`config/cobblemon_battlehub/ladders/custom_ladders/`

## **Step 1: Creating the Ladder JSON File**

Go to the `custom_ladders/` folder, create a file named `little_cup_event.json`, and add the following structure:

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
          "banPresets": [  
            "lc"  
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
          "allowMega": false,  
          "allowZMove": false,  
          "allowDynamax": false,  
          "allowTera": false  
        }

### **What are we configuring here?**

1. **id**: Set to `little_cup_event` (must exactly match the JSON file name).  
2. **adjustLevel**: Set to `5`. All Pokémon will be temporarily lowered or raised to level 5 for the battle.  
3. **banPresets**: We load the `lc` preset that ships with the mod. This will automatically ban strong stage 1 Pokémon for the format (like Scyther, Gligar, etc.).  
4. **Disabled Mechanics**: We set all gimmick properties (`allowMega`, `allowTera`, etc.) to `false` to ensure a pure early-gen strategic battle.

## **Step 2: Registering the Queue on the Server**

Creating the JSON file in the custom folder makes the mod load it into memory, but it will **not yet appear in the matchmaking menu**. To make the queue available to players, you must enable it in the main server config file.

1. Open the main config file at:  
   `config/cobblemon_battlehub/server_config.json` 
2. Add your new Ladder ID (`little_cup_event`) to the appropriate list. Since we set `"ranked": false` in our JSON, add it to the casual list:

          "activeCasualLadders": [  
            "singles_50_casual",  
            "doubles_50_casual",  
            "little_cup_event"  
          ],

!!! tip "Organization Tip"
    If you want this queue to award points and ranking on the server, simply change `"ranked": true` in your JSON file and register it under `"activeRankedLadders"` in `server_config.json`.

## **Step 3: Syncing the Changes**

After saving the files, you do not need to restart the physical server. Open the server console or run the command in-game as an administrator:

`/bh reload`

Done! The **Little Cup Event** queue will appear instantly in players’ menus with the rules, level adjustments, and Pokémon restrictions you just defined.

---