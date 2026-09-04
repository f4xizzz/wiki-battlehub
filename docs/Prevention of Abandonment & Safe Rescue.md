# **Prevention of Abandonment & Safe Rescue**

---

## **Prevention of Abandonment and Safe Rescue**

To prevent competitive abuse (such as griefing, Alt+F4 to avoid losing points, or spying) and to ensure players never become trapped in arena dimensions due to connection issues or crashes, **Cobblemon BattleHUB** implements a robust security infrastructure and the *Leaver Buster* punishment system.

## **1. Disconnect Manager**

The system constantly monitors the life cycle of each player’s connection from the moment the arena is instantiated.

### **State Save Flow**

Once a match is paired (exactly when the BattleManager creates a Session), the mod performs the following safety steps:

1. **Location Snapshot:** The mod records the player’s exact location in the overworld (X, Y, Z), the dimension, camera rotation (Yaw/Pitch), and original game mode.  
2. **Offline/Fallback Registration:** If the player suffers an abrupt disconnection (Alt+F4, power loss, or client crash) during combat, the server prevents them from logging back into the instantiated arena.  
3. **Emergency Rescue:** When attempting to reconnect, the mod intercepts the player’s login and restores their original game mode and inventory, then safely teleports them to the coordinates configured in `config/cobblemon_battlehub/fallbackarenadisconnect.json` (usually the server spawn or lobby).

```json
{
  "dimension": "minecraft:overworld",
  "x": 0.5,
  "y": 100.0,
  "z": 0.5,
  "yaw": 0.0,
  "pitch": 0.0
}
```

Set this to your lobby / spawn so a player who crashes mid-battle lands somewhere sensible instead of the default `0, 100, 0`.

## **2. Leaver Buster System (Desertion Punishment)**

To keep the competitive ecosystem healthy, abandoning ranked matches (whether by timed-out turns or by intentionally leaving the server) is severely punished.

### **What Counts as Abandonment?**

A forced surrender loss (W.O) is declared in the following scenarios:

* **Disconnection:** The player closes the game or drops connection and their entity is removed from the server.  
* **Prolonged Inactivity (AFK):** The player exceeds the consecutive turn limit where autopilot is activated (as configured by `maxAfkAutopilotRounds` in `server_config.json`).

### **Consequences for the Leaver**

1. The active opponent receives an automatic W.O. victory with a congratulatory chat message.  
2. The leaver receives an automatic loss in their stats profile and loses Rating (`R`) as usual.  
3. The player is applied a **temporary ranked ban**. The duration of this ban is cumulative and prevents the user from re-entering the competitive queue for a set period.

**Queue Ban Warning:** When attempting to enter matchmaking with an active ban, the player receives a formatted remaining time warning (e.g. `"You are banned from the competitive queue for another 01h 15m"`).

## **3. Safe Cancellation**

In rare cases of extreme desynchronization where the match becomes stuck due to an unexpected battle engine response or an internal tick error, players can use the safety command `/fixbattle`.

### **How Does Safe Cancellation Memory Work?**

To prevent a player from abusing the command to escape an imminent loss, the system requires a **Mutual Agreement**:

1. When a player types the command, they send a cancellation request.  
2. The opponent receives a visual alert that a technical safety cancellation has been requested.  
3. If the opponent agrees and also types the command within an acceptable timeframe, the mod cleanly ends the arena:  
   * Neither player loses or gains Rating (`R`).  
   * Both players and their teams are safely teleported back to their original locations by safeRescue.  
   * The instantiated arena is released and cleared from the server immediately.

---