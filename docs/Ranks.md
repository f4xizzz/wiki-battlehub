# **Ranks**

---

## **Ranks and Seasons System**

**Cobblemon BattleHUB** has an integrated competitive system for ranking and pairing players. It keeps matches fair, discourages intentional abandonment, and automates season transitions.

---

## **The Rating Algorithm (Glicko-2)**

Instead of classic Elo (a single fixed number), BattleHUB uses **Glicko-2**, which tracks two values **per Ladder** for each player:

| Value | Meaning | Starting default |
| :--- | :--- | :--- |
| **Rating** (`R`) | The player's estimated skill score. | `1500` |
| **Rating Deviation** (`RD`) | How *uncertain* the system is about that skill. High = unsure, low = confident. | `350` |

The system is ~95% confident the player's real skill sits within roughly `R ± 2 × RD`.

### **How uncertainty (`RD`) affects matches**

* **New / inactive players (`RD` high):** the system doesn't know your level yet, so each win or loss causes a **large `R` swing** to place you quickly.
* **Regular players (`RD` low):** as you play, `RD` shrinks toward a floor (around `50`). The system trusts your rating, so `R` changes become **smaller and more stable**.

### **Scale conversion (internal)**

For its variance and volatility (`σ`) math, Glicko-2 works on a compressed internal scale: it divides `(R − 1500)` and `RD` by the constant **`173.7178`**, runs the update, then multiplies back. You never see or configure this — it's purely how the algorithm works under the hood.

---

## **Desertion Penalty System (Leaver Buster)**

To stop players ruining ranked by rage-quitting a losing battle, the `CobblemonBattleHandler` watches for intentional abandons (Alt+F4, freeze, crash) and turn timeouts from being AFK.

### What happens on abandonment

When a player disconnects or is punished for inactivity mid-ranked-match, `recordForfeitOrAFK(loser)` runs:

1. **Automatic loss:** the leaver takes an immediate loss and drops `R` as normal.
2. **W.O. win for the opponent:** the remaining player is told the opponent left and receives the win.
3. **Leaver Ban:** the leaver is barred from the ranked queue for a **cumulative** period (each fresh offence stacks more time).

Re-queuing during an active ban shows a formatted notice, e.g.:

> *"You are banned from the competitive queue for another 01h 15m due to recent abandons."*

Clear a ban manually with `/bh clearban <player>`.

---

## **Profile Data**

Stored in the `MySQL` / `SQLite` table (see [Storage](Storage.md)):

| Key | Meaning |
| :--- | :--- |
| `TOTAL_BATTLES` | Matches played (Ranked + Casual) |
| `RANKED_WINS` / `RANKED_LOSSES` | Competitive wins / losses |
| `RANKED_MATCHES` | Completed competitive matches |
| `QUICK_WINS` / `QUICK_MATCHES` | Casual matchmaking stats |
| `RANKED_BAN_EXPIRATION` | Millisecond timestamp when a penalized player may return to ranked |

Reset a single player's rating and rank with `/bh resetrank <player>`.

---

## **Season Management**

Seasons are configured in [`server_config.json`](Server Config.md) (`currentSeasonNumber`, `currentSeasonName`, the reward blocks…) and rolled over with a command.

### Ending a season

`/bh season rollover`

This runs, in order:

1. **Determine winners** — scans each ranked Ladder and generates the Top 1 / Top 3 / Top 10 rewards from `seasonEndRewards.placementRewards` in `server_config.json`.
2. **Filter inactive players** — only players who met `minimumGames` for the season are eligible for rewards and the final rankings.
3. **Archive** — the finished season is appended to `completedRankedSeasons`.
4. **Soft reset** — every active player's `R` is pulled back toward `1500` and `RD` is reset to `350` for the new season (progress isn't wiped, just softened).

Check the current season any time with `/bh season` (info) or `/bh season stats` (admin detail).
