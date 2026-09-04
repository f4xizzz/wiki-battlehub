# **Licensing & Activation**

---

Cobblemon BattleHUB is a paid mod. On a **dedicated server** it stays locked until you activate a license key, which is then permanently bound to that server's IP by a signed backend.

**Singleplayer and integrated LAN worlds are always active** — no key, no internet check.

---

## **Activating**

1. Buy a key on our [Discord](https://discord.gg/aDCgBbvRe5) — you receive a key immediately after your Stripe payment is confirmed.
2. Join your server as a real operator (permission level 4 / console).
3. Run:

    `/bh activation [LICENSE-KEY]`

On success the server writes `config/cobblemon_battlehub/license.json` and unlocks everything. That file is verified **offline** (RSA signature) on every boot, and the key is re-validated against the backend every **4 hours**.

!!! warning "One IP per key"
    The backend binds the key to the **first IP** that activates it. You cannot move a key to a new IP or share it between servers. Open a ticket on Discord if you legitimately need to migrate a key.

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
| `BATTLEHUB-XXXX-XXXX` | Normal key. IP-locked, jar-integrity checked. Lifetime unless issued as temporary. |
| `BATTLEHUB-XXXX-XXXX` *(temporary)* | Same, but expires on a set date; the mod locks itself when the date passes (`/bh` shows a "temporary license expired" notice). |
| `BATTLEHUB-DEV-XXXX-XXXX` | Developer key. **No IP-lock, no jar-hash check.** For your own test environments only. |

---

## **`license.json`**

```json
{
  "license_key": "BATTLEHUB-XXXX-XXXX",
  "expires_at": -1,
  "signature": "base64-RSA-signature"
}
```

Do **not** edit it. The `signature` is checked against the mod's embedded public key on every startup; a tampered signature locks the mod. This file is per-server — don't put it in version control or copy it between servers.

---

## **Anti-tamper**

BattleHUB inspects its own call stack when the license is checked. If it detects another mod trying to inject into the licensing classes (via Mixin), it **locks the mod and logs an alert** — the Minecraft server keeps running normally, only BattleHUB goes inert:

```
[BattleHUB] SECURITY ALERT: possible illegal mixin injection into the license system.
[BattleHUB] Locking the mod (server keeps running).
```

If this fires on a clean setup it's a false positive from an unusual mod interaction. You can disable the stack inspection: set `ENABLE_STACK_INSPECTION = false` in `ActivationManager.java` and rebuild, or contact support.

---

## **Troubleshooting**

| Symptom | Cause / fix |
| :--- | :--- |
| "Activation failed. Invalid key, or key is already bound to another IP." | Key already used on another IP, revoked, or mistyped. |
| Activation hangs then fails on the first try | The licensing backend was asleep (cold start can take up to a minute). Run the command again. |
| "Integrity check failed. Adulterated JAR." | Your jar's hash isn't registered for this release. Use a `-DEV-` key or ask support to register the release hash. |
| Works, then locks a few hours later | Temporary key expired, or the 4-hour re-validation failed (key revoked, or the server couldn't reach the backend). |
| Log says "LICENSE TAMPERED" on boot | `license.json` was edited (or corrupted). Delete it and re-run `/bh activation`. |
