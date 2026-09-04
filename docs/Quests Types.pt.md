# **Quests Types**

---

## **Tipos de Missões e Gatilhos de Progresso**

O comportamento de cada missão vem do campo `type`, que precisa ser uma das 15 chaves abaixo (grafia exata, do `QuestType.java`). O progresso é checado uma vez no **fim de cada partida**.

`targetAmount` é quanto de progresso completa a missão. A maioria dos tipos soma **+1** por partida que se qualifica; as exceções estão marcadas abaixo.

---

## **Tabela de Gatilhos**

| `type` | `typeFilter` | O que faz no fim da partida |
| :--- | :--- | :--- |
| **`PLAY_MATCHES`** | — | +1 por partida completada (ranqueada ou casual, vitória ou derrota). |
| **`WIN_MATCHES`** | — | +1 quando o jogador venceu. |
| **`WIN_RANKED`** | palavra-chave de formato | +1 quando o jogador venceu uma partida **ranqueada** cujo ID da ladder bate com o filtro. |
| **`PLAY_RANKED`** | palavra-chave de formato | +1 quando o jogador completou uma partida **ranqueada** que bate com o filtro (vitória ou derrota). |
| **`PLAY_CASUAL`** | — | +1 quando a partida foi numa fila casual. |
| **`WIN_STREAK`** | — | +1 na vitória. **Na derrota, o progresso volta pra 0.** |
| **`USE_TYPE`** | tipo elemental (`fire`, `ghost`, …) | +1 se pelo menos um Pokémon que o jogador trouxe tinha esse tipo. O filtro é **obrigatório** — sem filtro, sem progresso. |
| **`WIN_WITH_ALIVE`** | — | +1 quando o jogador venceu. *(Hoje é idêntico ao `WIN_MATCHES` — não verifica quantos Pokémon sobreviveram.)* |
| **`WIN_FAST`** | — | +1 se o jogador venceu **e** a partida acabou em `targetAmount` turnos ou menos. Aqui o `targetAmount` é o limite de turnos, então a missão completa numa única vitória rápida. |
| **`PLAY_LONG`** | — | +1 se a partida durou pelo menos `targetAmount` turnos (ou passou de 20 turnos). |
| **`WIN_BY_FORFEIT`** | — | +1 quando o jogador venceu porque o oponente desistiu / desconectou. |
| **`PLAY_NO_FORFEIT`** | — | +1 quando a partida terminou sem ninguém desistir. |
| **`PLAY_MONOTYPE`** | — | +1 se a equipe do jogador tinha **2 ou menos tipos distintos no total** somando todos os Pokémon. |
| **`KNOCKOUT_TOTAL`** | — | **+3 na vitória, +1 na derrota** — uma "pontuação de agressividade" aproximada, sempre soma algo. |
| **`MASTER_MONOTYPE`** | tipo elemental | Não é um contador por partida. Ele **sincroniza** o progresso da missão até o total de vitórias monotype daquele tipo no perfil de estatísticas do jogador. O filtro é **obrigatório**. |

---

## **Como o filtro de formato funciona (`WIN_RANKED` / `PLAY_RANKED`)**

O filtro é uma checagem simples de **substring, sem diferenciar maiúsculas**, contra o ID da ladder que foi jogada.

* `typeFilter: "doubles"` → bate com `doubles_ranked`, `doubles_50_casual`. Não bate com `tourney_duplas` (esse ID não contém "doubles"). Bate com qualquer ID de ladder que contenha literalmente `doubles`.
* `typeFilter: null` ou `""` → bate com qualquer formato.

Então filtre pelos pedaços de ID que as suas ladders realmente usam (`singles`, `doubles`, `triples`, `monotype`, `ranked`, `_50_`, …).

---

## **Observações**

* O `targetAmount` do `WIN_FAST` é um **limite de turnos, não uma contagem de repetições** — a missão fica completa na primeira vez que o jogador vence dentro daquele número de turnos.
* O progresso do `MASTER_MONOTYPE` pode pular mais de 1 de uma vez (ele "alcança" o total do perfil), e nunca diminui.
* As chaves de `type` diferenciam maiúsculas de minúsculas e precisam bater com o `QuestType.java` exatamente — um erro de digitação faz a missão inteira deixar de carregar.
