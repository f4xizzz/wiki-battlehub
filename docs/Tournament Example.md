# **Tournament Examples**

---

## **Tournament Example and Setup**

The **Cobblemon BattleHUB** system lets you create multiple independent tournament profiles within the server. Each tournament is defined by an individual .json file and has its own rules, participant limits, battle format (Ladder), and visual description.

---

## **Quick Guide for Creating and Running Tournaments**

Follow this quick flow to create and launch a new event on your Minecraft server:

### **Step 1: Create the Profile File**

Go to the `/tournaments/` folder and create a file with the .json extension (e.g., `monthly_cup.json`). Paste the example template structure and modify the fields with your event information.

### **Step 2: Reload Tournaments on the Server**

With the file saved, sync all data by running the following command in-game or in the console:

`/bhubtournament reload`

### **Step 3: Open Registration**

Change the `"manualRegistrationOpen"` field to true in the JSON file and run `/bhubtournament reload`, or use the in-game admin interface to unlock the registration button for the community.

### **Step 4: Draw the Bracket and Start the Event**

Once the participant limit is reached (or at the scheduled time with players online), run the command to lock registration and generate the esports bracket:

`/bhubtournament <Tournament_ID> roll`

*(At this point, the mod will generate the bracket, send the initial table to Discord, and place the first Phase 1 matches on standby.)*

---

### **Profile Directory Path**

`config/cobblemon_battlehub/tournaments/`

---

## **Real Configuration Template**

On first startup, the mod automatically generates a sample test profile called `tournament_ex.json`. Below is the full structure for you to use as a creation base:

        {  
          "tournamentId": "weekly_cup_ou",  
          "name": "BattleHUB Weekly Cup",  
          "description": "The classic 6v6 OverUsed (OU) esports tournament! Prepare your best team, plan your switch strategies, and prove you are the arena master.",  
          "tournamentActive": true,  
          "manualRegistrationOpen": false,  
          "maxParticipants": 16,  
          "minParticipants": 4,  
          "testModeNoMinLimit": false,  
          "ladderName": "Singles OverUsed (OU)",  
          "ladderId": "tourney_solo",  
          "dateText": "Saturday, July 18 at 19:00",  
          "rewardsText": "1,500 Coins + Shiny Tinkaton with 6 Perfect IVs",  
          "showChampion": true,  
          "lastChampion": "F4xizzz"  
        }

---

## **Detailed Parameter Explanation**

### **1. Technical Identification**

* **`tournamentId`** (Default: "`default`"):  
  The technical and unique tournament ID. It is used internally in the database to save states and in the admin command manager (e.g., `/bhubtournament <tournamentId> roll`). **Use lowercase letters, numbers, and underscores.**

### **2. Visual Information**

* **`name`**: The friendly, stylized title displayed to all players in the tournament interface.  
* **`description`**: The synopsis or general rules that explain the tournament format in the player interface.  
* **`dateText`**: Informational text showing the scheduled date and time for the competition (e.g., *"Saturday, July 18 at 19:00").*  
* **`rewardsText`**: The display text highlighting the prize reserved for the first-place winner.

### **3. Registration Mechanics and Logic**


* **`tournamentActive`** (true/false):  
  Enables or disables the tournament display in the players’ graphical interface. If set to false, the tournament remains hidden in-game.  
* **`manualRegistrationOpen`** (true/false):  
  Determines whether tournament registration is open for normal players to sign up via the mod menu.  
* **`maxParticipants`** (Default: 16):  
  The maximum number of players who can register for the competition. The mod blocks new registrations immediately when this value is reached.  
* **`minParticipants`** (Default: 4):  
  The minimum number of players required for the bracket draw (`roll`) to be executed officially.  
* **`testModeNoMinLimit`** (true/false):  
  If enabled, ignores the `minParticipants` rule. It is ideal for development testing on local servers with bots or few players.

### **4. Format Integration**

* **`ladderName`**: The aesthetic and descriptive name of the format that players will read (e.g., *"OverUsed (OU)", "VGC 2026"*).  
* **`ladderId`** (Default: `"tourney_solo"`):  
  The technical ID of the rule Ladder that instantiated arenas will inherit (must match the IDs from .json files located in the `ladders/` folder). This ensures tournament duels rigorously follow the competitive clauses and Pokémon/item bans you defined.

### **5. Champion History**

* **`showChampion`** (true/false):  
  Enables or disables the display of the tournament’s last champion in the players’ graphical menu.  
* **`lastChampion`**:  
  The name (nickname) of the player who won the last edition of this tournament. This field is **updated automatically** by the `TournamentManager` when the final is completed, but it can also be manually modified here if you want to register a classic champion.

---
