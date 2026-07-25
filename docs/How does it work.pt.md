# **How dows it work?**

---

## **Funcionamento do Sistema de Torneios**

O sistema de torneios do **Cobblemon BattleHUB** é integrado ao ecossistema de combates do servidor. Ele gerencia o ciclo de vida completo de uma competição — desde a abertura das inscrições até a coroação do grande campeão — sem que os administradores precisem controlar chaves externas de forma manual.

---
## **Como Configurar e Executar um Torneio na Prática**

### **1. Ativação e Preparação do Arquivo**
Para liberar um torneio, abra o arquivo JSON correspondente ao seu torneio na pasta do mod e ajuste as seguintes propriedades para `true`:

* **`tournamentActive`**: Define se o torneio está ativo no servidor.
* **`manualRegistrationOpen`**: Abre as inscrições para que os jogadores possam participar.

> 💡 **Dica de Testes:** Se você estiver apenas testando a mecânica de torneios sozinho ou com poucas pessoas, ative a opção `"testModeNoMinLimit": true` para ignorar a exigência do número mínimo de participantes.

### **2. Inscrição de Jogadores**

1. Os jogadores devem abrir a interface do mod pelo comando `/bh`, navegar até a **aba de Torneios** e clicar em se inscrever.
2. **Métodos para Testes de Inscrição:**
   * **Adicionar Bots:** Você pode preencher as vagas do torneio automaticamente usando o comando `/bht <Id do Torneio> playerlist fill` *(Nota: os bots preenchem as vagas na chave, mas não jogam as partidas)*.
   * **Testes Reduzidos:** Você pode definir um limite mínimo no arquivo do torneio para realizar testes com um amigo ou conta secundária:
       ```
       "maxParticipants": 2,
       "minParticipants": 2
        ```

### **3. Sortear o Chaveamento (Seeding)**
Com as inscrições encerradas, execute o comando de sorteio para gerar as chaves:

`/bht <block id> roll`
*(Exemplo: `/bht default roll`)*

> 📢 **Recomendação:** Certifique-se de estar com o **Webhook do Discord** configurado para receber o painel visual do chaveamento gerado diretamente no seu servidor.

### **4. Gerenciamento e Convocação de Partidas**

#### **Opção A: Iniciar a Partida Oficialmente para os Jogadores**
Para convocar os dois competidores de um bloco para a batalha, use o comando:

`/bht prep <blockid> <timelimit>`


* **Exemplo com segundos:** `/bht prep default_phase_1_1 15s`
* **Exemplo com minutos:** `/bht prep default_phase_1_1 15m`

**O que acontece a seguir:**

1. Um **overlay** aparecerá na tela dos dois jogadores alertando sobre o início da partida e o tempo restante para a preparação.
2. Os jogadores devem abrir o menu com `/bh`, ir até a aba de **Torneios** e clicar em **Ready (Pronto)**.
3. Se a equipe (party) do jogador estiver dentro de todas as regras e restrições configuradas para o torneio, a confirmação será aceita.
4. Assim que ambos confirmarem prontidão, a batalha é iniciada automaticamente.
5. Ao término do combate, o mod detecta o vencedor de forma autônoma, atualiza a chave e envia o resultado no Discord.

#### **Opção B: Definir um Vencedor Manualmente (Set Win)**
Caso ocorra algum imprevisto ou você precise avançar um jogador manualmente, utilize o comando:

`/bht setwinner <blockid> <winner>`
*(Exemplo: `/bht setwinner default_phase_1_1 1`)*

### 🔍 **Onde encontrar o ID dos Blocos?**
Os IDs de cada partida/bloco são gerados e salvos no arquivo `tournaments_state.json`, localizado no diretório:

`config/cobblemon_battlehub/tournaments_state.json`

**Padronização comum dos IDs dos blocos:**

* `default_phase_1_1` *(Fase 1 - Bloco 1)*
* `default_phase_2_1` *(Fase 2 - Bloco 1)*
* `default_final_1` *(Final)*

---

## **O Ciclo de Vida de um Torneio**

O fluxo de execução de um torneio passa por 6 estados lógicos bem definidos, controlados pela propriedade Status do gerenciador:

\[UPCOMING\] ➔ \[REGISTRATION\] ➔ \[SEEDING\] ➔ \[IN\_PROGRESS\] ➔ \[FINISHED\]

### **1\. Inicialização e Perfis (UPCOMING)**

O mod carrega todas as definições e regras estruturais dos torneios diretamente da pasta de perfis. O torneio fica em modo de espera, exibindo seus dados de recompensas, data agendada e regras de formato na interface visual dos jogadores.

### **2\. Abertura de Inscrições (REGISTRATION)**

Quando o parâmetro `manualRegistrationOpen` é definido como true (ou ativado via comando), os jogadores ganham acesso para se inscreverem no torneio através da interface in-game.

* O mod limita estritamente as inscrições ao limite máximo definido por `maxParticipants`.  
* Os dados de inscrição de cada participante são salvos associando sua UUID ao seu Nickname no banco de dados.

### **3\. Sorteio de Chaves e Emparelhamento (SEEDING)**

Quando o comando `/bh tournament <ID> roll` é executado, o sistema encerra as inscrições e inicia a montagem matemática da chave de eliminação única (*Single Elimination Bracket*):

### **4\. Convocação de Partidas (IN\_PROGRESS)**

Assim que o chaveamento é gerado, o torneio entra em andamento. O sistema de ticks monitora ativamente as partidas e gerencia os combates através de "Blocos de Luta":

1. **Chamada de Partida:** O administrador ou o sistema envia a ordem de início para um bloco. Os dois competidores recebem um alerta global e um temporizador de preparação é iniciado.  
2. **Boss Bar de Transmissão:** Uma Boss Bar global é exibida no topo da tela para todos os jogadores online do servidor que não estão lutando, indicando o progresso atual do torneio (ex: *"Torneio Mensal \- Fase 1"*).

### **5\. Execução Automática de Batalhas**

* Os jogadores convocados devem preparar seu time e clicar em "Pronto" na interface do mod.  
* Assim que ambos estiverem prontos (ou se o tempo de preparação for forçado), o mod inicia a partida de Cobblemon instantaneamente usando a Ladder configurada no perfil do torneio (ex: `tourney_solo`).  

### **6\. Avanço Dinâmico e Finalização (FINISHED)**

* Assim que a sessão de batalha termina, o mod intercepta o resultado in-game.  
* O vencedor é promovido na chave de forma instantânea para criar o bloco da próxima fase (ex: `_phase_2_1` ou `_final_1`).  
* O perdedor é eliminado da competição.  
* O processo se repete até que reste apenas 1 competidor ativo. Ao declarar o último vencedor, o status é alterado para FINISHED e o nome do campeão é gravado de forma vitalícia no histórico (`lastChampion`).

## **O Mecanismo Anti-W.O. por Tempo de Preparação**

Para evitar que jogadores ausentes (AFK) travem o andamento de todo o torneio, o Mod monitora rigorosamente o tempo limite de preparação:

* Cada partida convocada ganha uma data limite em milissegundos.  
* Se o cronômetro estourar e um dos competidores não tiver confirmado prontidão, o sistema executa a eliminação automática:  
  * Se o Player 2 marcou pronto mas o Player 1 não, o Player 2 é declarado vencedor por W.O.  
  * Se nenhum dos dois marcou pronto, o sistema elimina ambos ou promove o que estiver com a melhor semente/prontidão.  
  * O anúncio da desclassificação é enviado no chat e a chave é atualizada imediatamente no banco de dados.

---
