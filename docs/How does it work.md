# **How does it work?**

---

## **Tournament System Overview**

The **Cobblemon BattleHUB** tournament system is integrated into the server's battle ecosystem. It manages the entire competition lifecycle — from opening registration to crowning the champion — without administrators needing to handle external brackets manually.

---
## **How to Configure and Run a Tournament**

### **1. Activate and Prepare the File**
To enable a tournament, open the tournament JSON file in the mod folder and set the following properties to `true`:

* **`tournamentActive`**: Determines whether the tournament is active on the server.
* **`manualRegistrationOpen`**: Opens registration so players can join.

> 💡 **Testing tip:** If you are only testing the tournament system alone or with a small group, enable `"testModeNoMinLimit": true` to ignore the minimum participant requirement.

### **2. Player Registration**

1. Players should open the mod interface with `/bh`, go to the **Tournaments tab**, and click to register.
2. **Registration testing methods:**
   * **Add bots:** You can fill tournament slots automatically using `/bht <tournamentId> playerlist fill` *(Note: bots fill bracket slots but do not play matches)*.
   * **Low-player tests:** You can set a minimum limit in the tournament file to test with a friend or alternate account:
       ```json
       "maxParticipants": 2,
       "minParticipants": 2
       ```

### **3. Draw the Bracket (Seeding)**
Once registration is closed, run the draw command to generate the bracket. `<tournament id>` is the `tournamentId` from the profile file (`"default"` in the shipped one):

`/bht <tournament id> roll`
*(Example: `/bht default roll`)*

> 📢 **Recommendation:** Make sure your **Discord webhook** is configured to receive the visual bracket panel directly on your server.

### **4. Match Management and Callouts**

#### **Option A: Start the Match Officially for Players**
To summon the two competitors in a block to battle, use:

`/bht prep <blockid> <timelimit>`

* **Example with seconds:** `/bht prep default_phase_1_1 15s`
* **Example with minutes:** `/bht prep default_phase_1_1 15m`

**What happens next:**

1. An **overlay** appears on both players' screens warning them about the match start and showing the remaining preparation time.
2. Players must open `/bh`, go to the **Tournaments** tab, and click **Ready**.
3. If the player's party complies with all tournament rules and restrictions, the confirmation is accepted.
4. Once both players confirm readiness, the battle starts automatically.
5. When the match ends, the mod detects the winner automatically, updates the bracket, and sends the result to Discord.

#### **Option B: Set a Winner Manually**
If something unexpected happens or you need to advance a player manually, use:

`/bht setwinner <blockid> <winner>`
*(Example: `/bht setwinner default_phase_1_1 1`)*

### 🔍 **Where to find block IDs?**
The IDs for each match/block are generated and saved in `tournaments_state.json`, located at:

`config/cobblemon_battlehub/tournaments_state.json`

**Common block ID pattern:**

* `default_phase_1_1` *(Phase 1 - Block 1)*
* `default_phase_2_1` *(Phase 2 - Block 1)*
* `default_final_1` *(Final)*

---

## **Tournament Lifecycle**

A tournament progresses through these states (the `Status` enum), controlled by the manager:

[UPCOMING] ➔ [REGISTRATION] ➔ [SEEDING] ➔ [IN_PROGRESS] ➔ [FINISHED]

(`/bht <id> cancel` sends it back to REGISTRATION at any point.)

### **1. Initialization and Profiles (UPCOMING)**

The mod loads all tournament definitions and structural rules directly from the profile folder. The tournament stays on standby, displaying reward details, scheduled date, and format rules in the players' UI.

### **2. Registration Opening (REGISTRATION)**

When the `manualRegistrationOpen` parameter is set to true (or activated via command), players gain access to register for the tournament through the in-game interface.

* The mod strictly limits registrations to the maximum defined by `maxParticipants`.
* Each participant's registration data is saved by associating their UUID with their nickname in the database.

### **3. Bracket Drawing and Pairing (SEEDING)**

When `/bht <tournament id> roll` is executed, the system closes registration and begins building the single-elimination bracket.

### **4. Match Callouts (IN_PROGRESS)**

Once the bracket is generated, the tournament is underway. The tick system actively monitors matches and manages battles through "Fight Blocks":

1. **Match call:** The administrator or system issues the start order for a block. Both competitors receive a global alert and a preparation timer starts.
2. **Broadcast Boss Bar:** A global boss bar is displayed at the top of the screen for all online players who are not currently fighting, showing the current tournament progress (for example: "Monthly Tournament - Phase 1").

### **5. Automatic Match Execution**

* Called players must prepare their team and click "Ready" in the mod interface.
* Once both are ready (or the preparation timer is forced), the mod starts the Cobblemon match instantly using the tournament profile's configured Ladder (for example: `tourney_solo`).

### **6. Dynamic Advancement and Finish (FINISHED)**

* When the battle session ends, the mod intercepts the in-game result.
* The winner advances instantly in the bracket to create the next phase block (for example: `_phase_2_1` or `_final_1`).
* The loser is eliminated from the competition.
* This repeats until only one active competitor remains. When the final winner is declared, the status changes to FINISHED and the champion's name is recorded permanently in `lastChampion`.

## **Anti-W.O. Preparation Timer Mechanism**

To prevent absent players (AFK) from stalling the entire tournament, the mod strictly monitors the preparation timer:

* Each summoned match receives a deadline in milliseconds.
* If the timer expires and one competitor has not confirmed readiness, the system performs an automatic elimination:
  * If Player 2 is ready but Player 1 is not, Player 2 is declared the winner by W.O.
  * If neither player is ready, the system eliminates both or advances the one with the better seed/readiness.
  * The disqualification announcement is sent in chat and the bracket is updated immediately in the database.

---
