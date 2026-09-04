# **Dependencies**

---

## **Hard requirements**

The mod will not load without these:

* **Fabric Loader** `>= 0.17.3` and **Fabric API**
* **Minecraft** `1.21.1`, **Java** `21`
* [**Cobblemon**](https://modrinth.com/mod/cobblemon/version/kF7CvxTo) `>= 1.7.3`

## **Needed for full functionality**

BattleHUB loads without these, but the matching features stay inactive until they're installed:

| Mod / Plugin | Enables | Without it |
| :--- | :--- | :--- |
| [Impactor](https://modrinth.com/mod/impactor/version/KwNU9SQW) `5.3.5` | The shop economy (buying products, bundles, rotation Pokémon) | Shop purchases fail — this is the only economy supported right now |
| [LuckPerms](https://modrinth.com/plugin/luckperms/version/l47d4ZWk) `5.4.140` | Granular `battlehub.*` permission nodes | Permissions fall back to vanilla OP levels |
| [CarbonChat](https://modrinth.com/plugin/carbon/version/314t2qDy) `3.0.0-beta.32` | The in-menu Global / Local chat tabs | The chat tabs won't relay messages — see [Chat Config](Chat Config.md) |

## **Optional integrations**

* [Cobblemon Mega Showdown](https://modrinth.com/mod/cobblemon-mega-showdown/version/Y6di9Ram) `1.8.4`
* [Cobblemon Battle Extras](https://modrinth.com/mod/cobblemon-battle-extras/version/1.13.45) `1.13.45`

!!! note "Using a different economy or chat mod?"
    Impactor and CarbonChat are the only ones wired in today. If your server runs something else, open a ticket on our [Discord](https://discord.gg/aDCgBbvRe5) and we'll look at adding the integration.
