# **Dependências**

---

O lançamento atual do BattleHUB roda em **Fabric** no Minecraft `1.21.1` (Java `21+`). Um build de NeoForge está em testes.

## **Requisitos obrigatórios**

O mod não carrega sem estes:

* **Minecraft** `1.21.1`, **Java** `21`
* [**Cobblemon**](https://modrinth.com/mod/cobblemon) `>= 1.8.0` (Fabric ou NeoForge)
* [**Architectury API**](https://modrinth.com/mod/architectury-api) `>= 13.0` (Fabric ou NeoForge) — normalmente o Cobblemon já instala
* **Fabric:** Fabric Loader `>= 0.16` + Fabric API + Fabric Language Kotlin
* **NeoForge:** NeoForge `21.1.133+` + Kotlin for Forge `5.7.0+`

## **Necessários para funcionar por completo**

O BattleHUB carrega sem estes, mas as funcionalidades correspondentes ficam inativas até instalá-los:

| Mod / Plugin | Habilita | Sem ele |
| :--- | :--- | :--- |
| [Impactor](https://modrinth.com/mod/impactor) `5.3.5` (Fabric / NeoForge) | A economia da loja (comprar produtos, bundles, Pokémon da rotação) | As compras da loja falham — é a única economia suportada por enquanto |
| [LuckPerms](https://luckperms.net/) `5.4+` (Fabric / NeoForge) | Os nós de permissão `battlehub.*` granulares | As permissões caem para os níveis de OP do vanilla |
| [CarbonChat](https://modrinth.com/plugin/carbon) `3.0.0-beta.x` (**só Fabric**) | As abas de chat Global / Local dentro do menu | As abas de chat não repassam mensagens — veja [Chat Config](Chat Config.md). O Carbon não tem build pra NeoForge, então essa feature é só do Fabric. |

## **Integrações opcionais**

* [Cobblemon Mega Showdown](https://modrinth.com/mod/cobblemon-mega-showdown/version/Y6di9Ram) `1.8.4`
* [Cobblemon Battle Extras](https://modrinth.com/mod/cobblemon-battle-extras/version/1.13.45) `1.13.45`

!!! note "Usa outra economia ou mod de chat?"
    Impactor e CarbonChat são os únicos integrados hoje. Se o seu servidor roda outra coisa, abra um ticket no nosso [Discord](https://discord.gg/YgM4Ng4QGu) que a gente avalia adicionar a integração.
