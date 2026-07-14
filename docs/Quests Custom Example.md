# **Quests Config**

---

## **Practical Guide: Creating Custom Quests**

The **Cobblemon BattleHUB** system lets you create an unlimited number of challenges for your players. You can encourage the use of underused types, reward aggressive playstyles, or create lasting prestige milestones.

Below, we will walk through how to structure and register three practical examples of custom quests directly in your `quests.json` file.

---

## **Example 1: Daily Type Quest**

In this example, we will create a simple daily quest that encourages the player to try a Grass-type Pokémon in their team.

* **Objective:** Participate in 1 battle using at least one Grass-type Pokémon in the team.  
* **Reward:** $100 in server currency.

### **JSON Structure:**

Add this block inside the `"daily_quests"` list in your `quests.json` file:

        {  
          "id": "daily_custom_grass",  
          "type": "USE_TYPE",  
          "title": "Strong Roots",  
          "description": "Complete 1 battle using at least one Grass-type Pokémon in your main team.",  
          "targetAmount": 1,  
          "typeFilter": "grass",  
          "rewardDescription": "100 Coins",  
          "rewardCommands": [  
            "eco deposit 100 dollar %player%"  
          ]  
        }

---

## **Example 2: Weekly Fast Win Quest**

In this weekly challenge, we reward players who finish matches extremely quickly and aggressively in ranked queues.

* **Objective:** Win 3 ranked matches in 10 turns or less.  
* **Reward:** 500 Battle Points (BP).

### **JSON Structure:**

Add this block inside the `"weekly_quests"` list in your `quests.json` file:

        {  
          "id": "weekly_custom_fast_win",  
          "type": "WIN_FAST",  
          "title": "Lightning Strike",  
          "description": "Win 3 arena matches with a maximum of 10 turns per match.",  
          "targetAmount": 3,  
          "typeFilter": null,  
          "rewardDescription": "500 Battle Points",  
          "rewardCommands":[  
            "eco deposit 500 battlepoint %player%"  
          ]  
        }

---

## **Example 3: Permanent Mastery Quest (Dragon Master)**

Permanent quests act like lifetime achievements. They do not reset on the daily or weekly cycle. In this example, we will create a milestone for players who reach 150 wins using a Dragon Monotype team.

* **Objective:** Reach 150 wins with a Dragon Monotype team.  
* **Reward:** Grant an exclusive Tag via LuckPerms and announce it globally.

### **JSON Structure:**

Add this block inside the `"permanent_quests"` list in your `quests.json` file:

    {  
      "id": "perm_custom_dragon_master",  
      "type": "MASTER_MONOTYPE",  
      "title": "Dragon Sovereign",  
      "description": "Reach the historic milestone of 150 competitive wins using a Dragon Monotype team.",  
      "targetAmount": 150,  
      "typeFilter": "dragon",  
      "rewardDescription": "Legendary Tag [Dragon Master]",  
      "rewardCommands": [  
        "lp user %player% parent add mestre_dragao",  
        "broadcast &6[BattleHUB] &e%player% has achieved ultimate mastery and is now a Dragon Master!"  
      ]  
    }

---

## **Best Practices When Creating New Quests**

To ensure system stability and avoid compatibility issues with saved database data, make sure to follow these three simple rules:

1. **Unique and Standardized IDs:** Never use the same ID for two different quests. We recommend using a clear prefix for each list, such as `daily_...`, `weekly_...`, or `perm_...`.  
2. **Filter Normalization:** The `typeFilter` parameter reads directly from Cobblemon elemental types and format IDs. Always enter values in **lowercase** (e.g. `fire` instead of `Fire`, `doubles` instead of `Doubles`).  
3. **Command Syntax:** You can chain as many console commands as you'd like in the `"rewardCommands"` list. The system will automatically replace the `%player%` and `{player}` variables with the player's current nickname.

---

## **How to Apply the New Quests**

After saving changes to `quests.json`, apply the updates instantly to active players on the server by running the reload command:

`/bh reload`

---