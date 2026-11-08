# **Tournament Commands**

---

## **Comandos de Torneio**

O sistema de torneios do **Cobblemon BattleHUB** é comandado por uma árvore de comandos de admin (`TournamentCommand`). Com eles você fecha inscrições, sorteia e reseta chaveamentos, roda bots de teste e intervém em partidas específicas. Os torneios em si são definidos por arquivos de perfil — veja [Como funciona?](How does it work.md) e [Tournament Example](Tournament Example.md).

---

!!! info "Comandos marcados com *(desde a 1.4)*"
    Existem no lançamento normal 1.4 em diante. O build separado do Cobblemon 1.7.3 ainda não os tem.

## **Comando Principal e Aliases**

Todos os comandos de gerenciamento de torneio requerem a permissão base **`battlehub.tournament.base`** (ou permissão OP Nível 2\) no servidor para serem executados.

* **Comando Base:** `/bhubtournament`  
* **Aliases Suportados:**  
  * `/bht`  
  * `/cbht`  
  * `/bhunt`

---

## **Comandos Administrativos e Globais**

Esses comandos afetam as configurações do mod e a comunicação com canais externos.

### **1\. Recarregar Configurações (reload)**

* **Sintaxe:** `/bhubtournament reload`  
* **Permissão:** `battlehub.tournament.reload`  
* **Funcionamento:** Recarrega todos os perfis de torneios salvos na pasta `/tournaments/` para a memória ativa do servidor, salvando também os estados atuais e atualizando a interface gráfica (GUI) de todos os jogadores online.

### **2\. Sincronizar Chaves no Discord (webhook send)**

* **Sintaxe:** `/bhubtournament webhook send`  
* **Permissão:** `battlehub.tournament.webhook`  
* **Funcionamento:** Força o Mod a processar e reenviar imediatamente a imagem do chaveamento atualizada no canal configurado do Discord, caso o torneio esteja ativo ou finalizado.

---

## **Controle do Ciclo de Vida do Torneio**

Esses comandos dependem do **ID do Torneio** (definido no arquivo .json do perfil) e gerenciam as inscrições e o chaveamento geral.

### **1\. Limpar Inscrições (clear)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> clear`  
* **Permissão:** `battlehub.tournament.clear`  
* **Funcionamento:** Remove instantaneamente todos os jogadores inscritos na competição selecionada (disponível apenas se o torneio estiver nos estados UPCOMING ou REGISTRATION).

### **2\. Sorteio de Chaves (roll)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> roll`  
* **Permissão:** `battlehub.tournament.roll`  
* **Funcionamento:** Encerra as inscrições e gera matematicamente a árvore de eliminação única. Dispara o webhook de início de torneio com o anexo do chaveamento e altera o status do torneio para `IN_PROGRESS`.

### **3\. Cancelar / Resetar Torneio (cancel)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> cancel`  
* **Permissão:** `battlehub.tournament.cancel`  
* **Funcionamento:** Cancela o torneio ativo, limpa todas as partidas em andamento, limpa a Boss Bar do topo e redefine o estado da competição de volta para a fase de inscrições `(REGISTRATION)`.

---

## **Gerenciamento de Bots de Teste**

Utilize essas ferramentas para testar o chaveamento em servidores de desenvolvimento sem a necessidade de múltiplos jogadores reais online.

### **1\. Adicionar Bot Individual (playerlist add)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> playerlist add <Nickname>`  
* **Permissão:** `battlehub.tournament.playerlist.add`  
* **Funcionamento:** Inscreve um "jogador fantasma" com o nome desejado no torneio. O sistema gera automaticamente um UUID aleatório interno para ele, tratando-o como um participante comum.

### **2\. Preencher Vagas Restantes (playerlist fill)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> playerlist fill`  
* **Permissão:** `battlehub.tournament.playerlist.fill`  
* **Funcionamento:** Identifica a capacidade máxima de participantes do torneio (`maxParticipants`), calcula quantas vagas restam e preenche todas de forma instantânea com bots nomeados sequencialmente (ex: Teste_1, Teste_2), permitindo testes rápidos de bracket ideal.

---

## **Controle Individual de Partidas (Blocos)**

Esses comandos atuam diretamente sobre as partidas identificadas por um **ID de Bloco** (ex: `default_phase_1_3, default_final_1`).

### **1\. Convocar Partida (startmatch)**

* **Sintaxe:** `/bhubtournament startmatch [ID_do_Bloco]`  
* **Permissão:** `battlehub.tournament.startmatch`  
* **Funcionamento:** Alerta globalmente que a partida do bloco foi convocada, enviando notificações na tela dos competidores e acionando o webhook. Coloca o bloco sob o temporizador padrão de preparação. Se o bloco não existir ou não estiver aguardando (já em preparação, em jogo ou finalizado), o comando avisa em vez de anunciar uma partida. Sem `ID_do_Bloco`, convoca a **próxima partida na ordem do chaveamento** — veja *Próxima Partida e Auto-Run* abaixo *(desde a 1.4)*.


### **2\. Forçar Tempo de Preparação (prep)**

* **Sintaxe:** `/bhubtournament prep <ID_do_Bloco> <Tempo>`  
* **Permissão:** `battlehub.tournament.prep`  
* **Funcionamento:** Altera o status do bloco de volta para a fase de preparação (PREPARING) e redefine o cronômetro com base no tempo especificado.  
  * **Formatos de Tempo Aceitos:** O comando analisa sufixos como m (minutos), h (horas), d (dias).  
  * *Exemplo:* `/bhubtournament prep bloco_1 5m (Define 5 minutos de tolerância).`


### **3\. Forçar Entrada na Arena (forcearena)**

* **Sintaxe:** `/bhubtournament forcearena <ID_do_Bloco>`  
* **Permissão:** `battlehub.tournament.forcearena`  
* **Funcionamento:** Ignora o botão de confirmação de prontidão dos jogadores e força ambos a serem teleportados para a arena física instanciada, iniciando o combate do Cobblemon de forma imediata usando o formato da Ladder do torneio.


### **4\. Definir Ganhador Manual (setwinner)**

* **Sintaxe:** `/bhubtournament setwinner <ID_do_Bloco> <1|2|Nickname>`  
* **Permissão:** `battlehub.tournament.setwinner`  
* **Funcionamento:** Intervém na chave e declara o vencedor. Passe `1` para dar a vitória ao Player 1 do bloco, ou `2` para o Player 2 — ou digite o nick exato de um dos dois jogadores *(a partir da 1.4; antes, qualquer valor diferente de `1` era tratado como Player 2, então um nick sempre dava a vitória ao Player 2)*. O que não bater é rejeitado. O mod atualiza a chave, promove o vencedor e dispara os anúncios automaticamente.

---

## **Visão Geral e Listas de Fases** *(desde a 1.4)*

Você não precisa mais abrir o `tournaments_state.json` para descobrir quais blocos existem.

### **1. Listar Torneios (list)**

* **Sintaxe:** `/bhubtournament list`
* **Permissão:** `battlehub.tournament.list`
* **Funcionamento:** Uma linha por torneio carregado: ID, status, jogadores inscritos / máximo e o estado atual de inscrição, modo de teste, `tournamentActive` e auto-run. Cada linha tem um botão **[PHASES]** clicável.

### **2. Lista de Fases (phases)**

* **Sintaxe:** `/bhubtournament phases` (todo torneio que já tem chaveamento) ou `/bhubtournament <ID_do_Torneio> phases`
* **Permissão:** `battlehub.tournament.phases`
* **Funcionamento:** Mostra o chaveamento agrupado por fase (Fase 1, Fase 2... Final). Cada linha traz o ID do Bloco, os dois jogadores e o status: `WAITING`, `PREPARING`, `PLAYING` ou `FINISHED` com o vencedor. As partidas que podem ser convocadas mostram um **[START]** clicável; as em andamento mostram **[WIN 1]** / **[WIN 2]**, que apenas *preenchem o chat* com o comando `setwinner` para você confirmar com Enter.

Exemplo:

    ═ Weekly Cup (weekly_cup_ou)  status: IN_PROGRESS | auto-run: ON
     ▸ Phase 1
       weekly_cup_ou_phase_1_1 Steve vs Alex [FINISHED - winner: Steve]
       weekly_cup_ou_phase_1_2 Notch vs Herobrine [WAITING] [START]

### **3. Autocompletar com Tab**

O Tab sugere IDs de torneio, IDs de Bloco (só os que podem ser convocados, no `startmatch`) e os campos do `set`.

---

## **Próxima Partida e Auto-Run** *(desde a 1.4)*

### **1. Próxima Partida (startmatch / next)**

* **Sintaxe:** `/bhubtournament startmatch` (sem ID_do_Bloco) ou `/bhubtournament <ID_do_Torneio> next`
* **Permissão:** `battlehub.tournament.startmatch`
* **Funcionamento:** Convoca a próxima partida do chaveamento na ordem de criação (Fase 1 bloco 1, bloco 2..., depois a Fase 2...). Só contam blocos `WAITING` com os dois jogadores definidos; blocos BYE são pulados. O `startmatch` sem ID só funciona quando há exatamente um torneio em andamento — com vários, use `<ID_do_Torneio> next`.

### **2. Auto-Run (autorun)**

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> autorun` (mostra o estado atual), `/bhubtournament <ID_do_Torneio> autorun on [parallel]` ou `/bhubtournament <ID_do_Torneio> autorun off`
* **Permissão:** `battlehub.tournament.autorun`
* **Funcionamento:** Ligado, o servidor convoca a próxima partida em espera **sozinho**, na ordem do chaveamento, assim que houver vaga. `parallel` (1–8, padrão 1) é quantas partidas podem estar em preparação ou em jogo ao mesmo tempo. Ele espera 5 segundos entre as convocações e 10 segundos depois de um resultado, para os jogadores conseguirem ver. As partidas convocadas assim seguem as regras normais: tempo de preparação, botão **Ready** e a regra anti-W.O. (veja [Como funciona?](How does it work.md)). Uma partida entre dois bots de teste é resolvida na hora (o Player 1 avança), já que ninguém poderia jogá-la. O estado é salvo e restaurado ao reiniciar; o `cancel` desliga o auto-run.

---

## **Editar Configurações do Torneio Dentro do Jogo (set)** *(desde a 1.4)*

* **Sintaxe:** `/bhubtournament <ID_do_Torneio> set <campo> <valor>`
* **Permissão:** `battlehub.tournament.set`
* **Funcionamento:** Altera uma configuração do perfil do torneio e **grava no arquivo `.json` do torneio** — sem editar à mão nem `reload`.

| Campo | Valor | Configuração do perfil |
| :--- | :--- | :--- |
| `registration` | `on` / `off` | `manualRegistrationOpen`. Enquanto o torneio ainda aguarda o sorteio, também abre ou fecha as inscrições na hora |
| `testmode` | `on` / `off` | `testModeNoMinLimit` |
| `active` | `on` / `off` | `tournamentActive` |
| `showchampion` | `on` / `off` | `showChampion` |
| `min` / `max` | número de 2 a 1024 | `minParticipants` / `maxParticipants` |
| `name`, `description`, `date`, `rewards` | texto | `name`, `description`, `dateText`, `rewardsText` |
| `ladder` | texto | `ladderName` (o nome exibido) |
| `ladderid` | ID da ladder | `ladderId` — precisa ser uma ladder existente |

Os valores on/off também aceitam `true`/`false` e `yes`/`no`.

*Exemplos:* `/bht weekly_cup_ou set registration on` · `/bht weekly_cup_ou set rewards 1.500 Moedas + Tinkaton Shiny`

---
