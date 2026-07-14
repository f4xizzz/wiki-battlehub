# **Tournament Commands**

---

## **Tournament Commands**

The **Cobblemon BattleHUB** tournament system is managed by a high-precision administrative command tree based on the class. These commands allow you to create, reset, monitor brackets, and even manually intervene in ongoing matches.

---

## **Main Command and Aliases**

All tournament management commands require the base permission **`battlehub.tournament.base`** (or OP Level 2 permission) on the server to execute.

* **Base Command:** `/bhubtournament`  
* **Supported Aliases:**  
  * `/bht`  
  * `/cbht`  
  * `/bhunt`

---

## **Administrative and Global Commands**

These commands affect mod settings and communication with external channels.

### **1. Reload Configurations (reload)**

* **Syntax:** `/bhubtournament reload`  
* **Permission:** `battlehub.tournament.reload`  
* **Functionality:** Reloads all tournament profiles saved in the `/tournaments/` folder into the server’s active memory, saving current states and updating the graphical interface (GUI) for all online players.

### **2. Sync Brackets to Discord (webhook send)**

* **Syntax:** `/bhubtournament webhook send`  
* **Permission:** `battlehub.tournament.webhook`  
* **Functionality:** Forces the mod to process and resend the updated bracket image immediately to the configured Discord channel if the tournament is active or finished.

---

## **Tournament Lifecycle Control**

These commands depend on the **Tournament ID** (set in the profile .json file) and manage registrations and the overall bracket.

### **1. Clear Registrations (clear)**

* **Syntax:** `/bhubtournament <Tournament_ID>` clear  
* **Permission:** `battlehub.tournament.clear`  
* **Functionality:** Instantly removes all players registered in the selected competition (available only if the tournament is in the UPCOMING or REGISTRATION states).

### **2. Draw Brackets (roll)**

* **Syntax:** `/bhubtournament <Tournament_ID> roll`  
* **Permission:** `battlehub.tournament.roll`  
* **Functionality:** Closes registration and mathematically generates the single-elimination bracket. Triggers the tournament start webhook with the bracket attachment and changes the tournament status to `IN_PROGRESS`.

### **3. Cancel / Reset Tournament (cancel)**

* **Syntax:** `/bhubtournament <Tournament_ID> cancel`  
* **Permission:** `battlehub.tournament.cancel`  
* **Functionality:** Cancels the active tournament, clears all ongoing matches, clears the top Boss Bar, and resets the competition state back to the registration phase `(REGISTRATION)`.

---

## **Test Bot Management**

Use these tools to test the bracket on development servers without needing multiple real players online.

### **1. Add Single Bot (playerlist add)**

* **Syntax:** `/bhubtournament <Tournament_ID> playerlist add <Nickname>`  
* **Permission:** `battlehub.tournament.playerlist.add`  
* **Functionality:** Registers a “ghost player” with the desired name in the tournament. The system automatically generates an internal random UUID for them, treating them like a normal participant.

### **2. Fill Remaining Slots (playerlist fill)**

* **Syntax:** `/bhubtournament <Tournament_ID> playerlist fill`  
* **Permission:** `battlehub.tournament.playerlist.fill`  
* **Functionality:** Identifies the tournament’s maximum participant capacity (`maxParticipants`), calculates the remaining slots, and instantly fills them with sequentially named bots (e.g., Test_1, Test_2), enabling quick ideal bracket testing.

---

## **Individual Match Control (Blocks)**

These commands act directly on matches identified by a **Block ID** (e.g., `default_phase_1_3`, `default_final_1`).

### **1. Call Match (startmatch)**

* **Syntax:** `/bhubtournament startmatch <Block_ID>`  
* **Permission:** `battlehub.tournament.startmatch`  
* **Functionality:** Globally announces that the block match has been called, sending notifications to competitors’ screens and triggering the webhook. Puts the block under the standard preparation timer.


### **2. Force Preparation Time (prep)**

* **Syntax:** `/bhubtournament prep <Block_ID> <Time>`  
* **Permission:** `battlehub.tournament.prep`  
* **Functionality:** Changes the block status back to the preparation phase (PREPARING) and resets the timer based on the specified time.  
  * **Accepted Time Formats:** The command parses suffixes like m (minutes), h (hours), d (days).  
  * *Example:* `/bhubtournament prep block_1 5m` (Sets 5 minutes of tolerance.)


### **3. Force Arena Entry (forcearena)**

* **Syntax:** `/bhubtournament forcearena <Block_ID>`  
* **Permission:** `battlehub.tournament.forcearena`  
* **Functionality:** Bypasses the players’ readiness confirmation button and forces both participants to teleport to the instantiated physical arena, immediately starting the Cobblemon battle using the tournament ladder format.


### **4. Set Manual Winner (setwinner)**

* **Syntax:** `/bhubtournament setwinner <Block_ID> <_1 or _2>`  
* **Permission:** `battlehub.tournament.setwinner`  
* **Functionality:** Intervenes in the bracket and declares the winner arbitrarily. Use 1 to award victory to Player 1 of the block or 2 to award victory to Player 2. The mod will update the bracket, promote the winner, and trigger announcements automatically.

---