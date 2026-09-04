# **Quests Types**

---

## **Quest Types and Progress Triggers**

Every quest's behavior comes from its `type` field, which must be one of the 15 keys below (exact spelling, from `QuestType.java`). Progress is checked once at the **end of every match**.

`targetAmount` is how much progress completes the quest. Most types add **+1** progress per qualifying match; the exceptions are called out below.

---

## **Quest Trigger Table**

| `type` | `typeFilter` | What it does at end of match |
| :--- | :--- | :--- |
| **`PLAY_MATCHES`** | — | +1 for every completed match (ranked or casual, win or lose). |
| **`WIN_MATCHES`** | — | +1 when the player won. |
| **`WIN_RANKED`** | format keyword | +1 when the player won a **ranked** match whose ladder ID matches the filter. |
| **`PLAY_RANKED`** | format keyword | +1 when the player completed a **ranked** match matching the filter (win or lose). |
| **`PLAY_CASUAL`** | — | +1 when the match was in a casual queue. |
| **`WIN_STREAK`** | — | +1 on a win. **On a loss, progress resets to 0.** |
| **`USE_TYPE`** | elemental type (`fire`, `ghost`, …) | +1 if at least one Pokémon the player brought had that type. Filter is **required** — no filter, no progress. |
| **`WIN_WITH_ALIVE`** | — | +1 when the player won. *(Currently identical to `WIN_MATCHES` — it does not check how many Pokémon survived.)* |
| **`WIN_FAST`** | — | +1 if the player won **and** the match ended in `targetAmount` turns or fewer. Here `targetAmount` is the turn limit, so this quest completes in a single fast win. |
| **`PLAY_LONG`** | — | +1 if the match lasted at least `targetAmount` turns (or ran past 20 turns). |
| **`WIN_BY_FORFEIT`** | — | +1 when the player won because the opponent forfeited / disconnected. |
| **`PLAY_NO_FORFEIT`** | — | +1 when the match finished with nobody forfeiting. |
| **`PLAY_MONOTYPE`** | — | +1 if the player's team had **2 or fewer distinct types total** across all its Pokémon. |
| **`KNOCKOUT_TOTAL`** | — | **+3 on a win, +1 on a loss** — a rough "aggression" score, always adds something. |
| **`MASTER_MONOTYPE`** | elemental type | Not a per-match counter. It **syncs** the quest's progress up to the player's lifetime monotype-win count for that type from their stats profile. Filter is **required**. |

---

## **How the format filter works (`WIN_RANKED` / `PLAY_RANKED`)**

The filter is a plain **substring check, case-insensitive**, against the ladder ID that was played.

* `typeFilter: "doubles"` → matches `doubles_ranked`, `doubles_50_casual`, `tourney_duplas`? → no (that ID is `tourney_duplas`, which does **not** contain "doubles"). It matches any ladder ID that literally contains `doubles`.
* `typeFilter: null` or `""` → matches every format.

So filter on the actual ID fragments your ladders use (`singles`, `doubles`, `triples`, `monotype`, `ranked`, `_50_`, …).

---

## **Notes**

* `WIN_FAST`'s `targetAmount` is a **turn cap, not a repeat count** — the quest is done the first time the player wins within that many turns. Pair it with a permanent quest if you want it repeatable-feeling, or accept it as a one-and-done.
* `MASTER_MONOTYPE` progress can jump by more than 1 at once (it catches up to the profile total), and it never goes down.
* Quest `type` keys are case-sensitive and must match `QuestType.java` exactly — a typo makes the whole quest fail to load.
