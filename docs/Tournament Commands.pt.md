# **Tournament Commands**

---

## **Comandos de Torneio**

O sistema de torneios do **Cobblemon BattleHUB** é gerenciado por uma árvore de comandos administrativos de alta precisão baseados na classe. Esses comandos permitem criar, resetar, monitorar chaves e até intervir manualmente nas lutas em andamento.

---

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

* **Sintaxe:** `/bhubtournament <ID_do_Torneio>` clear  
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

* **Sintaxe:** `/bhubtournament startmatch <ID_do_Bloco>`  
* **Permissão:** `battlehub.tournament.startmatch`  
* **Funcionamento:** Alerta globalmente que a partida do bloco foi convocada, enviando notificações na tela dos competidores e acionando o webhook. Coloca o bloco sob o temporizador padrão de preparação.


### **2\. Forçar Tempo de Preparação (prep)**

* **Sintaxe:** `/bhubtournament prep <ID_do_Bloco> <Tempo>`  
* **Permissão:** battlehub.tournament.prep  
* **Funcionamento:** Altera o status do bloco de volta para a fase de preparação (PREPARING) e redefine o cronômetro com base no tempo especificado.  
  * **Formatos de Tempo Aceitos:** O comando analisa sufixos como m (minutos), h (horas), d (dias).  
  * *Exemplo:* `/bhubtournament prep bloco_1 5m (Define 5 minutos de tolerância).`


### **3\. Forçar Entrada na Arena (forcearena)**

* **Sintaxe:** `/bhubtournament forcearena <ID_do_Bloco>`  
* **Permissão:** `battlehub.tournament.forcearena`  
* **Funcionamento:** Ignora o botão de confirmação de prontidão dos jogadores e força ambos a serem teleportados para a arena física instanciada, iniciando o combate do Cobblemon de forma imediata usando o formato da Ladder do torneio.


### **4\. Definir Ganhador Manual (setwinner)**

* **Sintaxe:** `/bhubtournament setwinner <ID_do_Bloco> <_1 ou _2>`  
* **Permissão:** `battlehub.tournament.setwinner`  
* **Funcionamento:** Intervém na chave e declara o vencedor de forma arbitrária. Use 1 para dar a vitória ao Player 1 do bloco ou 2 para o Player 2. O mod atualizará a chave, promoverá o vencedor e acionará os anúncios automaticamente.

---