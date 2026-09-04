# **Ranks**

---

## **Sistema de Ranks e Temporadas**

O **Cobblemon BattleHUB** tem um sistema competitivo integrado para classificar e parear jogadores. Ele mantém as partidas justas, desencoraja o abandono intencional e automatiza a troca de temporadas.

---

## **O Algoritmo de Classificação (Glicko-2)**

Em vez do Elo clássico (um único número fixo), o BattleHUB usa **Glicko-2**, que guarda dois valores **por Ladder** para cada jogador:

| Valor | Significado | Padrão inicial |
| :--- | :--- | :--- |
| **Rating** (`R`) | A pontuação de habilidade estimada do jogador. | `1500` |
| **Rating Deviation** (`RD`) | O quanto o sistema está *incerto* sobre essa habilidade. Alto = inseguro, baixo = confiante. | `350` |

O sistema tem ~95% de confiança de que a habilidade real do jogador está mais ou menos dentro de `R ± 2 × RD`.

### **Como a incerteza (`RD`) afeta as partidas**

* **Jogadores novos / inativos (`RD` alto):** o sistema ainda não sabe seu nível, então cada vitória ou derrota causa uma **grande variação no `R`** para te posicionar rápido.
* **Jogadores frequentes (`RD` baixo):** conforme você joga, o `RD` encolhe até um piso (perto de `50`). O sistema confia no seu rating, então as mudanças de `R` ficam **menores e mais estáveis**.

### **Conversão de escala (interno)**

Para as contas de variância e volatilidade (`σ`), o Glicko-2 trabalha numa escala interna comprimida: divide `(R − 1500)` e `RD` pela constante **`173.7178`**, roda a atualização e multiplica de volta. Você nunca vê nem configura isso — é só como o algoritmo funciona por baixo dos panos.

---

## **Sistema de Penalidade por Desistência (Leaver Buster)**

Para evitar que jogadores estraguem a ranqueada saindo de batalhas perdidas, o `CobblemonBattleHandler` monitora abandonos intencionais (Alt+F4, travamento, crash) e turnos estourados por tempo (AFK).

### O que acontece no abandono

Quando um jogador desconecta ou é punido por inatividade no meio de uma partida ranqueada, o `recordForfeitOrAFK(loser)` é acionado:

1. **Derrota automática:** quem abandonou leva uma derrota imediata e perde `R` normalmente.
2. **Vitória por W.O. para o oponente:** o jogador que ficou é avisado de que o oponente saiu e recebe a vitória.
3. **Leaver Ban:** o desistente fica barrado da fila ranqueada por um período **cumulativo** (cada nova infração acumula mais tempo).

Ao tentar entrar na fila com um ban ativo, aparece um aviso formatado, tipo:

> *"Você está banido da fila competitiva por mais 01h 15m devido a abandonos recentes."*

Remova um ban manualmente com `/bh clearban <jogador>`.

---

## **Dados de Perfil**

Guardados na tabela `MySQL` / `SQLite` (veja [Storage](Storage.md)):

| Chave | Significado |
| :--- | :--- |
| `TOTAL_BATTLES` | Partidas jogadas (Ranked + Casual) |
| `RANKED_WINS` / `RANKED_LOSSES` | Vitórias / derrotas competitivas |
| `RANKED_MATCHES` | Partidas competitivas completadas |
| `QUICK_WINS` / `QUICK_MATCHES` | Estatísticas do matchmaking casual |
| `RANKED_BAN_EXPIRATION` | Timestamp (em ms) de quando um jogador penalizado pode voltar à ranqueada |

Resete o rating e o rank de um jogador com `/bh resetrank <jogador>`.

---

## **Gerenciamento de Temporadas (Seasons)**

As temporadas são configuradas no [`server_config.json`](Server Config.md) (`currentSeasonNumber`, `currentSeasonName`, os blocos de recompensa…) e encerradas por comando.

### Finalizando uma temporada

`/bh season rollover`

Isso roda, nesta ordem:

1. **Apurar vencedores** — varre cada Ladder ranqueada e gera as recompensas de Top 1 / Top 3 / Top 10 a partir de `seasonEndRewards.placementRewards` no `server_config.json`.
2. **Filtrar inativos** — só jogadores que cumpriram o `minimumGames` da temporada entram nas recompensas e no ranking final.
3. **Arquivar** — a temporada encerrada é anexada a `completedRankedSeasons`.
4. **Soft reset** — o `R` de cada jogador ativo é puxado de volta na direção de `1500` e o `RD` volta para `350` para a nova temporada (o progresso não é apagado, só suavizado).

Veja a temporada atual a qualquer momento com `/bh season` (info) ou `/bh season stats` (detalhe admin).
