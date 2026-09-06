# **Dependencies**

---

BattleHUB runs on **Fabric** or **NeoForge** for Minecraft `1.21.1` (Java `21+`). Download the jar that matches your loader.

## **Hard requirements**

The mod will not load without these:

* **Minecraft** `1.21.1`, **Java** `21`
* [**Cobblemon**](https://modrinth.com/mod/cobblemon) `>= 1.7.3` (Fabric or NeoForge)
* [**Architectury API**](https://modrinth.com/mod/architectury-api) `>= 13.0` (Fabric or NeoForge) — usually already installed by Cobblemon
* **Fabric:** Fabric Loader `>= 0.16` + Fabric API + Fabric Language Kotlin
* **NeoForge:** NeoForge `21.1.133+` + Kotlin for Forge `5.7.0+`

## **Needed for full functionality**

BattleHUB loads without these, but the matching features stay inactive until they're installed:

| Mod / Plugin | Enables | Without it |
| :--- | :--- | :--- |
| [Impactor](https://modrinth.com/mod/impactor) `5.3.5` (Fabric / NeoForge) | The shop economy (buying products, bundles, rotation Pokémon) | Shop purchases fail — this is the only economy supported right now |
| [LuckPerms](https://luckperms.net/) `5.4+` (Fabric / NeoForge) | Granular `battlehub.*` permission nodes | Permissions fall back to vanilla OP levels |
| [CarbonChat](https://modrinth.com/plugin/carbon) `3.0.0-beta.x` (**Fabric only**) | The in-menu Global / Local chat tabs | The chat tabs won't relay messages — see [Chat Config](Chat Config.md). Carbon has no NeoForge build, so this feature is Fabric-only. |

## **Optional integrations**

* [Cobblemon Mega Showdown](https://modrinth.com/mod/cobblemon-mega-showdown/version/Y6di9Ram) `1.8.4`
* [Cobblemon Battle Extras](https://modrinth.com/mod/cobblemon-battle-extras/version/1.13.45) `1.13.45`

!!! note "Using a different economy or chat mod?"
    Impactor and CarbonChat are the only ones wired in today. If your server runs something else, open a ticket on our [Discord](https://discord.gg/aDCgBbvRe5) and we'll look at adding the integration.
