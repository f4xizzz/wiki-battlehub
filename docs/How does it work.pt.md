# **How dows it work?**

---

## **Funcionamento do Sistema de Torneios**

O sistema de torneios do **Cobblemon BattleHUB** é totalmente automatizado e integrado ao ecossistema de combates do servidor. Ele gerencia o ciclo de vida completo de uma competição — desde a abertura das inscrições até a coroação do grande campeão — sem que os administradores precisem controlar chaves externas de forma manual.

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
