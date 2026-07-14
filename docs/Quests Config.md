# **Quests Config**

---

## **Quest System Configuration (quests.json)**

The quest system in **Cobblemon BattleHUB** rewards player activity and loyalty through daily, weekly, and permanent challenges. The `quests.json` file is the core where all quests, completion goals, and reward commands are stored and edited.

---

### **File Path**

`config/cobblemon\_battlehub/quests.json`

---

## **How the System Works**

The system behavior is orchestrated dynamically by the `QuestManager.java` class on every login and end of match:

1. **Automatic Generation:** On first startup, if `quests.json` is not found, the mod creates the file automatically and populates it with more than **90 pre-defined quests**.  
2. **Intelligent Random Selection:** For daily and weekly quests, the mod does not give the player the entire catalog. It randomly selects a subset of IDs based on a seed generated from the current date combined with the player’s UUID. This ensures each player receives different quests each cycle!  
3. **Safe Persistence:** The player’s current progress (how many steps they completed, what has already been redeemed, and reset times) is saved asynchronously to the database.

---

## **Configuration Structure Template**

Below is the simplified structure of `quests.json` that demonstrates how quests and global parameters are defined:

        {  
          "daily_quests_per_player": 2,  
          "weekly_quests_per_player": 5,  
          "daily_quests": [  
            {  
              "id": "daily_01",  
              "type": "PLAY_MATCHES",  
              "title": "First Step",  
              "description": "Play 1 match in any queue.",  
              "targetAmount": 1,  
              "typeFilter": null,  
              "rewardDescription": "50 Dollars",  
              "rewardCommands": [  
                "eco deposit 50 dollar %player%"  
              ]  
            }  
          ],  
          "weekly_quests": [  
            {  
              "id": "weekly_01",  
              "type": "WIN_RANKED",  
              "title": "Arena Veteran",  
              "description": "Win 15 Ranked matches.",  
              "targetAmount": 15,  
              "typeFilter": null,  
              "rewardDescription": "1000 Battle Points",  
              "rewardCommands": [  
                "eco deposit 1000 battlepoint %player%"  
              ]  
            }  
          ],  
          "permanent_quests": [  
            {  
              "id": "perm_mono_fire",  
              "type": "MASTER_MONOTYPE",  
              "title": "Master of Flames",  
              "description": "Reach 100 wins using Fire Monotype.",  
              "targetAmount": 100,  
              "typeFilter": "fire",  
              "rewardDescription": "Fire Master Tag",  
              "rewardCommands": [  
                "lp user %player% parent add master_fire"  
              ]  
            }  
          ]  
        }

---

## **Detailed Parameter Explanation**

### **1. Global Selection Parameters**

* **`daily_quests_per_player`** (Default: 2):  
  Determines how many daily quests are drawn from the global pool and assigned to each player every 24 hours.  
* **`weekly_quests_per_player`** (Default: 5):  
  Determines how many weekly quests are drawn from the pool and assigned to each player every 7 days.

### **2. Quest List Parameters (`daily_quests`, `weekly_quests`, `permanent_quests`)**

Each quest object in the lists has the following required properties:

* **`id`**:  
  The quest’s technical identifier. It is used by the database to save whether the player has completed or redeemed the reward. **Never duplicate IDs!**  
* **`type`**:  
  The mission action trigger that connects to the mod’s statistics. It must match the names listed in `QuestType.java` exactly (e.g. `PLAY_MATCHES`, `WIN_RANKED`, `USE_TYPE`, `MASTER_MONOTYPE`).  
* **`title`**:  
  The visual, eye-catching title displayed to the player in the quest menu and chat notifications.  
* **`description`**:  
  The text explanation that tells the player exactly what they need to do to complete the quest.  
* **`targetAmount`**:  
  The number of times the trigger must be activated for the quest to be considered complete (e.g. Win **15** matches, KO **50** Pokémon).  
* **`typeFilter`**:  
  A condition filter. It can limit the quest to a specific format (such as singles or doubles) or to a Pokémon type in the team (such as fire, electric, water). Set it to `null` if the quest is generic.  
* **`rewardDescription`**:  
  The visual reward description shown to the player when they complete or redeem the quest.  
* **`rewardCommands`**:  
  A list of commands that the server console executes immediately when the player redeems the reward. Supports the `%player%` or `{player}` variable to automatically inject the name of the player who completed the objective.

---

## **How to Reload New Quests**

If you add new quests, change descriptions, or modify rewards directly in `quests.json`, you can apply the changes in real time and sync the interface for all players using the command:

`/bh reload`

---