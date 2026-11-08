# **Tournament Commands**

---

## **Tournament Commands**

The **Cobblemon BattleHUB** tournament system is driven by an admin command tree (`TournamentCommand`). These commands let you close registration, draw and reset brackets, run test bots, and step into individual matches. Tournaments themselves are defined by profile files — see [How does it work?](How does it work.md) and [Tournament Example](Tournament Example.md).

---

!!! info "Commands marked *(since 1.4)*"
    They exist in the regular release 1.4 and newer. The separate Cobblemon 1.7.3 build does not have them yet.

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

* **Syntax:** `/bhubtournament <Tournament_ID> clear`  
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

* **Syntax:** `/bhubtournament startmatch [Block_ID]`  
* **Permission:** `battlehub.tournament.startmatch`  
* **Functionality:** Globally announces that the block match has been called, sending notifications to competitors’ screens and triggering the webhook. Puts the block under the standard preparation timer. If the block doesn't exist or is not waiting (already preparing, playing or finished) the command says so instead of announcing a match. Without a `Block_ID` it calls the **next match in bracket order** — see *Next Match and Auto-Run* below *(since 1.4)*.


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

* **Syntax:** `/bhubtournament setwinner <Block_ID> <1|2|Nickname>`  
* **Permission:** `battlehub.tournament.setwinner`  
* **Functionality:** Intervenes in the bracket and declares the winner. Pass `1` to award the win to Player 1 of the block, or `2` for Player 2 — or type the exact nickname of one of the two players *(since 1.4; before that, any value other than `1` was treated as Player 2, so a nickname always gave the win to Player 2)*. Anything that doesn't match is rejected. The mod updates the bracket, promotes the winner, and fires the announcements automatically.

---

## **Overview and Phase Lists** *(since 1.4)*

You no longer need to open `tournaments_state.json` to find out which blocks exist.

### **1. List Tournaments (list)**

* **Syntax:** `/bhubtournament list`
* **Permission:** `battlehub.tournament.list`
* **Functionality:** One line per loaded tournament: ID, status, registered players / maximum, and the current state of registration, test mode, `tournamentActive` and auto-run. Each line has a clickable **[PHASES]** button.

### **2. Phase List (phases)**

* **Syntax:** `/bhubtournament phases` (every tournament that already has a bracket) or `/bhubtournament <Tournament_ID> phases`
* **Permission:** `battlehub.tournament.phases`
* **Functionality:** Prints the bracket grouped by phase (Phase 1, Phase 2... Final). Each line shows the Block ID, both players and the status: `WAITING`, `PREPARING`, `PLAYING` or `FINISHED` with the winner. Matches that can be called show a clickable **[START]**; matches in progress show **[WIN 1]** / **[WIN 2]**, which only *fill the chat* with the `setwinner` command so you confirm it with Enter.

Example:

    ═ Weekly Cup (weekly_cup_ou)  status: IN_PROGRESS | auto-run: ON
     ▸ Phase 1
       weekly_cup_ou_phase_1_1 Steve vs Alex [FINISHED - winner: Steve]
       weekly_cup_ou_phase_1_2 Notch vs Herobrine [WAITING] [START]

### **3. Tab-complete**

Pressing Tab suggests tournament IDs, Block IDs (only the ones that can be called, for `startmatch`) and the fields of `set`.

---

## **Next Match and Auto-Run** *(since 1.4)*

### **1. Next Match (startmatch / next)**

* **Syntax:** `/bhubtournament startmatch` (no Block_ID) or `/bhubtournament <Tournament_ID> next`
* **Permission:** `battlehub.tournament.startmatch`
* **Functionality:** Calls the next match of the bracket in creation order (Phase 1 block 1, block 2..., then Phase 2...). Only blocks that are `WAITING` with both players defined count; BYE blocks are skipped. `startmatch` without an ID only works when exactly one tournament is in progress — with several, use `<Tournament_ID> next`.

### **2. Auto-Run (autorun)**

* **Syntax:** `/bhubtournament <Tournament_ID> autorun` (shows the current state), `/bhubtournament <Tournament_ID> autorun on [parallel]` or `/bhubtournament <Tournament_ID> autorun off`
* **Permission:** `battlehub.tournament.autorun`
* **Functionality:** While on, the server calls the next waiting match **by itself**, in bracket order, as soon as there is room. `parallel` (1–8, default 1) is how many matches may be preparing or playing at the same time. It waits 5 seconds between calls and 10 seconds after a result, so players can see it. Matches called this way follow the normal rules: preparation timer, the **Ready** button and the anti-W.O. rule (see [How does it work?](How does it work.md)). A match between two test bots is resolved instantly (Player 1 advances), since nobody could play it. The state is saved and restored on restart; `cancel` turns it off.

---

## **Edit Tournament Settings In-Game (set)** *(since 1.4)*

* **Syntax:** `/bhubtournament <Tournament_ID> set <field> <value>`
* **Permission:** `battlehub.tournament.set`
* **Functionality:** Changes a setting of the tournament profile and **saves it to the tournament's `.json` file** — no manual edit or `reload` needed.

| Field | Value | Profile setting |
| :--- | :--- | :--- |
| `registration` | `on` / `off` | `manualRegistrationOpen`. While the tournament is still waiting for the draw, it also opens or closes registration right away |
| `testmode` | `on` / `off` | `testModeNoMinLimit` |
| `active` | `on` / `off` | `tournamentActive` |
| `showchampion` | `on` / `off` | `showChampion` |
| `min` / `max` | number from 2 to 1024 | `minParticipants` / `maxParticipants` |
| `name`, `description`, `date`, `rewards` | text | `name`, `description`, `dateText`, `rewardsText` |
| `ladder` | text | `ladderName` (the display name) |
| `ladderid` | ladder ID | `ladderId` — must be an existing ladder |

On/off values also accept `true`/`false` and `yes`/`no`.

*Examples:* `/bht weekly_cup_ou set registration on` · `/bht weekly_cup_ou set rewards 1,500 Coins + Shiny Tinkaton`

---
