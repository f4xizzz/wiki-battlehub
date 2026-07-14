# **Comandos:**

---

### **Importante**
* `/fixbattle`    
*(Cria um pedido para seu oponente, pedindo para cancelar a partida por bugs (nem voce nem ele perdem ponto))*
    * `/fixbattle confirm`   
    *(aceita o pedido de cancelar partida do oponente)*

---

### **Comandos Gerais**

| Comando | Descrição | Permissão |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Abre a interface principal do Cobblemon BattleHUB. | `battlehub.base` |
| `/bh status` | Mostra o status atual do servidor (ex: jogadores na fila, temporada, partidas em andamento, etc). | `battlehub.status` |
| `/bh stats` | Mostra suas estatísticas pessoais (ex: rank, pontos, partidas jogadas, etc). | `battlehub.stats.self` |
| `/bh stats [jogador]` | Mostra as estatísticas de um jogador específico. | `battlehub.stats.other` |
| `/bh leavequeue` | Retira você da fila de matchmaking. | `battlehub.leavequeue` |
| `/bh leave` | Retira você de uma partida em andamento ou do modo espectador. | `battlehub.leave` |
| `/bh spectate` | Abre a interface de espectador para assistir a partidas ativas. | `battlehub.spectate` |
| `/bh quests` | Abre a interface com suas missões (quests) diárias e mensais. | `battlehub.quests` |
| `/bh season` | Mostra informações sobre a temporada atual. | `battlehub.season.info` |
| `/bh season stats` | Mostra dados e estatísticas internas da temporada ativa. | `battlehub.season.status` |
| `/bh season rollover` | Encerra a temporada atual e inicia a próxima imediatamente. | `battlehub.season.rollover` |
| `/battlehub pokemonstats`<br>_Alias: `/bh pokemonstats`_ | Mostra o ranking dos Pokémon mais utilizados em partidas ranqueadas. | `battlehub.pokemonstats` |
{ .md-typeset__table }

---

### **Comandos de Administração e Gerenciamento**

| Comando | Descrição | Permissão |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Abre a interface principal do Cobblemon BattleHUB. | `battlehub.base` |
| `/bh serverstatus` | Exibe informações gerais e dados de desempenho do servidor. | `battlehub.serverstatus` |
| `/bh reload` | Recarrega instantaneamente todos os arquivos de configuração do mod. | `battlehub.reload` |
| `/bh clearqueue` | Limpa a fila global de matchmaking, removendo todos os jogadores dela. | `battlehub.clearqueue` |
| `/bh activation [LICENSE-KEY]` | Realiza a validação e ativação da licença do mod para o IP do servidor. | `battlehub.activation` |
| `/bh arenas` | Mostra o status e dados técnicos das arenas (estruturas) carregadas. | `battlehub.arenas` |
| `/bh battles` | Exibe uma lista com todos os duelos que estão acontecendo em tempo real. | `battlehub.battles` |
| `/bh end [jogador]` | Força o encerramento imediato da partida do jogador especificado. | `battlehub.end` |
| `/bh clearban [jogador]` | Remove todas as restrições e banimentos aplicados ao jogador. | `battlehub.clearban` |
| `/bh resetrank [jogador]` | Reseta completamente os pontos de classificação e o rank de um jogador. | `battlehub.resetladder` |
| `/bh adminstats [jogador]` | Mostra um relatório completo de informações internas de um jogador para moderação. | `battlehub.adminstats` |
| `/bh forcestart [jogador1] [jogador2] [LadderID]` | Força o início imediato de um duelo entre dois jogadores em uma categoria específica. | `battlehub.forcestart` |
| `/bh leaderboard [LadderID] [Limite]` | Mostra no chat o ranking dos melhores jogadores da categoria (Ladder) especificada. | `battlehub.leaderboard` |
{ .md-typeset__table }

---

### **Comandos Dono/Diretor**

| Comando | Descrição | Permissão |
| :--- | :--- | :--- |
| `/battlehub`<br>_Aliases: `/bh`, `/bhub`, `/cbh`_ | Abre a interface principal do Cobblemon BattleHUB. | `battlehub.base` |
| `/bh resetlimitshop [jogador]` | Renova e redefine todos os limites de compra da loja para o jogador especificado. | `battlehub.resetlimitshop` |
| `/bh rollpokemonshop` | Reseta e força uma nova rotação de Pokémon disponíveis para compra na loja. | `battlehub.rollpokemonshop` |
| `/bh structures list` | Exibe uma lista com todas as arenas (estruturas) geradas e ativas no momento. | `battlehub.structures.list` |
| `/bh structures clear` | Remove e destrói todas as arenas (estruturas) que estão spawnadas no momento. | `battlehub.structures.clear` |
| `/bh structures tp [ArenaID]` | Teletransporta você instantaneamente para as coordenadas da arena indicada pelo ID. | `battlehub.structures.tp` |
| `/bh structures build [all/ArenaID]` | Constrói uma arena específica (pelo ID) ou força a geração de todas as arenas de uma vez. | `battlehub.structures.build` |
{ .md-typeset__table }

---

  <a href="../Tournament%20Commands/" style="text-decoration: none; color: inherit; display: block;">
    <div style="border: 1px solid var(--md-default-fg-color--lightest); border-radius: 8px; padding: 20px; background-color: var(--md-code-bg-color); transition: border-color 0.25s, transform 0.25s, box-shadow 0.25s; height: 100%; display: flex; flex-direction: column; justify-content: space-between;" 
         onmouseover="this.style.borderColor='var(--md-accent-fg-color)'; this.style.boxShadow='0 4px 12px rgba(0,0,0,0.15)'; this.style.transform='translateY(-4px)';" 
         onmouseout="this.style.borderColor='var(--md-default-fg-color--lightest)'; this.style.boxShadow='none'; this.style.transform='none';">
      <div>
        <div style="font-weight: 700; font-size: 1.1rem; color: var(--md-default-fg-color); display: flex; align-items: center; justify-content: space-between; margin-bottom: 8px;">
          <span>🏆 Comandos dos Torneios</span>
          <span style="color: var(--md-accent-fg-color); font-size: 1.2rem;">&rarr;</span>
        </div>
        <div style="font-size: 0.9rem; color: var(--md-default-fg-color--light); line-height: 1.5;">
          Clique para ir para á página.
        </div>
      </div>
    </div>
  </a>