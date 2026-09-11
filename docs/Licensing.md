# **Licensing & Activation**

---

Cobblemon BattleHUB is a paid mod. On a **dedicated server** it stays locked until you activate a license key. The key is then bound to that server and validated against our backend.

**Singleplayer and integrated LAN worlds are always active** — no key, no internet check.

!!! tip "Free trial"
    Grab a free `BATTLEHUB-TRIAL-XXXX-XXXX` key on our [Discord](https://discord.gg/GbbbNvQG3N) to try the mod on your server. One trial key works on any server; each server gets its own countdown that starts on its first activation, and the mod locks itself when that trial runs out.

---

## **Activating**

1. Get a key on our [Discord](https://discord.gg/GbbbNvQG3N) — a free `BATTLEHUB-TRIAL-XXXX-XXXX` trial key, or a full key after your purchase is confirmed.
2. Join your server as a real operator (permission level 4 / console).
3. Run:

    `/bh activation [LICENSE-KEY]`

On success the server writes `config/cobblemon_battlehub/license.json` and unlocks everything. The license is re-validated against the backend every **4 hours**.

!!! info "One server per key"
    On first activation the key is bound to that server instance. Your public IP can change (dynamic IP, host migration) without breaking activation, but the key will not work on a second, different server at the same time. Open a ticket on Discord if you need to move a key to a new machine.

!!! info "Backend outages don't take you down"
    If the licensing backend is temporarily unreachable, an already-activated server keeps running for a grace period while it retries in the background. You only lose access if the key is actually revoked or expires.

---

## **While unlicensed**

On a dedicated server without a valid license:

* Every `/bh` command and every menu is blocked, **except `/bh activation`**.
* Players see: *"This server doesn't have a valid license! … Use: /battlehub activation <your_key>"*.
* Duel invites, matchmaking and tournaments do nothing.

The mod still loads — it just stays inert until you activate it.

---

## **Key types**

| Format | Behaviour |
| :--- | :--- |
| `BATTLEHUB-XXXX-XXXX` | Full key. Bound to your server. Lifetime unless issued as temporary. |
| `BATTLEHUB-XXXX-XXXX` *(temporary)* | Same, but expires on a set date; the mod locks itself when the date passes (`/bh` shows a "temporary license expired" notice). |
| `BATTLEHUB-TRIAL-XXXX-XXXX` | Free trial. Works on any server; each server gets its own N-day countdown starting at its first activation. Locks when the trial ends. |

---

## **`license.json`**

Written and managed by the mod. Do **not** edit it — an invalid file simply fails verification and the mod stays locked until you run `/bh activation` again. This file is per-server: don't put it in version control or copy it between servers.

If BattleHUB stays locked on a clean, licensed setup, open a ticket on Discord with your `latest.log`.

---

## **Troubleshooting**

| Symptom | Cause / fix |
| :--- | :--- |
| "Activation failed. Invalid key, or key is already bound to another server." | Key already bound elsewhere, revoked, or mistyped. |
| Activation hangs then fails on the first try | The licensing backend was asleep (cold start can take up to a minute). Run the command again. |
| Works, then locks later | Temporary key expired, the key was revoked, or the backend was unreachable past the grace period. |
