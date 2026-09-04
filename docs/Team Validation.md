# **Team Validation**

---

## **Team Validation**

To ensure competitive and casual matches occur fairly and without battle engine errors, **Cobblemon BattleHUB** uses a strict team analysis system.

This validator checks the player’s party at the exact moment they try to enter a queue or accept a duel invitation, immediately blocking any irregularity.

---

## **1. Structural Survival Checks**

Before analyzing format-specific rules, the system performs two basic infrastructure checks:

* **Minimum Team Size:** The validator checks whether the player has the number of Pokémon required by the Ladder (e.g., 6 for standard Singles).  
* **Health Status:** The mod scans the player’s party to ensure they have **at least one healthy Pokémon** (not fainted). If all party Pokémon are fainted, queue entry is blocked with the warning: *"All of your Pokémon are fainted. Heal your team before entering battle."*

## **2. Competitive Clauses (Ranked Rules)**

If the active Ladder is competitive (`ranked: true`) or a tournament format (`tourney_`), the validator applies the following restrictions:

### **Species Clause**

Prevents the player from using two or more Pokémon of the same species on the team.

* The system normalizes species IDs (removing special characters and converting to lowercase) to prevent workarounds using alternate forms or names.

### **Item Clause**

Ensures that each held item in the party is unique.

* If two Pokémon are holding the same item (for example, two Leftovers), the system blocks entry.  
* Empty items (air or no item) are ignored by the filter.

### **Banned Items**

The validator checks whether any held item on the team is registered in the format’s global banned-item list (such as the official VGCRules bans).

## **3. Automatic Meta Filters (Mythicals and Paradoxes)**

Unlike other mods where you must manually register hundreds of Pokémon in ban lists, BattleHUB scans Cobblemon’s code directly for the game’s native labels.

This guarantees a 100% accurate block of any creature classified as mythical or paradox by Cobblemon itself, keeping your configuration always up to date with future mod additions.

## **4. Monotype Validation Algorithm**

If the Ladder format includes the monotype suffix or rule, the validator runs a **set-intersection** check on the team's types.

Every Pokémon has a set of types (1 or 2). The system intersects all of those sets together; the team passes Monotype **only if that intersection still contains at least one type** — i.e. there is a single type that *every* Pokémon on the team shares.

**Example:** Charizard `{Fire, Flying}` + Talonflame `{Fire, Flying}` + Arcanine `{Fire}` → the common type is `{Fire}` → **valid** (mono-Fire). Swap Arcanine for Gyarados `{Water, Flying}` and the shared type becomes `{Flying}` → still valid (mono-Flying). Add a pure `{Water}` Pokémon and the intersection is empty → **rejected**.

---
