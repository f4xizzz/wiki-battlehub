# **Spectate**

---

## **Spectator System**

**Cobblemon BattleHUB** has a robust real-time broadcast system that allows other players to watch ongoing matches directly from the arena’s physical stands.

This system ensures immersion for spectators and full competitive safety for combatants.

---

## **How Does the Spectator System Work?**

When entering spectator mode (either through a menu or mod command), the server logic performs the following steps:

1. **State Saving:** The mod saves the spectator’s original location (X, Y, Z), dimension, camera angles (Yaw/Pitch), and current GameMode.  
2. **State Change:** The player is switched to **Spectator** mode (`SPECTATOR`) and becomes invisible to other players.  
3. **Seat Mapping:** The system scans around the arena center for specific blocks to place the spectator (read more below).  
4. **Teleport and Broadcast:** The spectator is teleported to the seat, oriented toward the fight center, and begins receiving the mod’s combat messages and arena data stream.

---

## **The Stand Mechanism (Seats)**

To prevent spectators from floating chaotically around the stadium, the mod uses an intelligent **Seat Scanning** algorithm.

### **Default Seat Block**

The system actively searches for **Smooth Stone Slab** blocks (`SMOOTH_STONE_SLAB`) to serve as seats.

### **Scan Area**

The algorithm checks blocks within a radius around the arena center:

* **Horizontal (X/Z):** Up to 30 blocks away.  
* **Vertical (Y):** From 2 blocks below the arena floor up to 8 blocks above.

### **Usable Seat Validation**

For a spectator to be assigned to a slab, the block must be free:

1. The slab block must have a solid block below it.  
2. The block directly above the slab (where the player will sit) and the next block above must be air.  
3. No other player may be occupying that seat’s collision box.

**Note:** If the algorithm cannot find any usable smooth stone slab in the entire battle arena, the spectator is safely teleported 2 blocks above the exact arena center.

---

## **Safety Cylinder and Ceiling (Anti-Cheat)**

Since Minecraft’s spectator mode allows free movement through walls and ceilings, the mod constantly monitors each spectator’s position every server tick to prevent abuse (such as spying on bases, ghosting, or chunk-loading exploits).

There is an invisible safety cylinder configured with the following rules relative to the arena center:

* **Maximum Horizontal Distance:** 30 blocks. If the player moves more than 30 horizontal blocks from the center, they are intercepted.  
* **Maximum Height (Y):** 12 blocks above the arena floor level.  
* **Minimum Depth (Y):** 2 blocks below the arena floor level.

          [Limit: +12 Blocks Height]  
         ┌─────────────────────────────────┐  
         │        ALLOWED BROADCAST        │  30 Blocks  
         │         AREA (Spectator)        │ ◄───────►  
         │                                 │  Radius X/Z  
         │            Arena [X]            │  
         └─────────────────────────────────┘  
          [Limit: -2 Blocks Depth]

### **What Happens if the Spectator Exceeds the Limits?**

If the player crosses the invisible cylinder boundaries, the server will:

1. Block the movement and automatically find a new valid empty slab seat in the stands.  
2. Teleport the player immediately back to the stands and rotate their camera back toward the fight.  
3. Display a visual warning on the player’s action bar: `spectator.status.out_of_bounds`.

---

## **Exiting Spectator Mode**

When the monitored battle ends or the spectator chooses to leave:

* The player is restored to their previous GameMode (e.g., Survival or Creative).  
* All invisibility and temporary flight permissions are recalculated and cleared.  
* They are teleported back precisely to the exact coordinates and dimension where they were before watching.

---
