# **Prevention of Abandonment & Safe Rescue**

---

## **Prevenção de Abandono e Resgate Seguro**

Para evitar abusos competitivos (como *griefing*, "Alt+F4" para não perder pontos ou espionagem) e garantir que os jogadores nunca fiquem presos em dimensões de arena devido a falhas de conexão ou crashes, o **Cobblemon BattleHUB** implementa uma robusta infraestrutura de segurança e o sistema de punições *Leaver Buster*.

## **1\. Gerenciador de Desconexão**

O sistema monitoriza constantemente o ciclo de vida da ligação de cada jogador desde o momento em que a arena é instanciada.

### **O Fluxo de Salvamento de Estado**

Assim que uma partida é emparelhada (no exato instante em que o BattleManager cria uma Session), o mod executa as seguintes etapas de segurança:

1. **Snapshot de Localização:** O mod regista no servidor a localização exata do jogador no mundo principal (X, Y, Z), a dimensão, a rotação da câmara (Yaw/Pitch) e o modo de jogo original.  
2. **Registro Offline / Fallback:** Caso o jogador sofra uma desconexão abrupta (Alt+F4, queda de energia ou crash do cliente) durante o combate, o servidor impede que ele faça login novamente dentro da arena instanciada.  
3. **Resgate de Emergência:** Ao tentar reconectar, o mod intercepta a entrada do jogador, devolve o modo de jogo e o inventário original, e teleporta ele em segurança para as coordenadas configuradas em `config/cobblemon_battlehub/fallbackarenadisconnect.json` (geralmente o spawn ou lobby do servidor).

```json
{
  "dimension": "minecraft:overworld",
  "x": 0.5,
  "y": 100.0,
  "z": 0.5,
  "yaw": 0.0,
  "pitch": 0.0
}
```

Ajuste isso para o seu lobby / spawn, senão quem crashar no meio da batalha cai no padrão `0, 100, 0`.

## **2\. Sistema Leaver Buster (Punição por Deserção)**

Para manter o ecossistema competitivo saudável, o abandono de partidas ranqueadas (seja por turnos estourados ou por sair do servidor de propósito) é severamente punido.

### **O que é considerado Abandono?**

Uma derrota por desistência forçada (W.O) é declarada nos seguintes cenários:

* **Desconexão:** O jogador fecha o jogo ou cai e o seu boneco é removido do servidor.  
* **Inatividade Prolongada (AFK):** O jogador estoura o limite de turnos em que o piloto automático é acionado de forma consecutiva (conforme configurado em `maxAfkAutopilotRounds` no `server_config.json`).

### **Consequências para o Desertor**

1. O oponente ativo recebe uma vitória automática por W.O. com uma mensagem de congratulações no chat.  
2. O desertor recebe uma derrota automática no perfil de estatísticas e perde Rating (`R`) normalmente.  
3. É aplicado um **bloqueio temporário (Ranked Ban)** ao jogador. A duração desse ban é cumulativa e impede a pessoa de entrar de novo na fila competitiva por um tempo determinado.

**Aviso de Fila Bloqueada:** Ao tentar entrar na fila de matchmaking com o banimento ativo, o jogador recebe o aviso do tempo restante formatado (`ex: "Você está banido da fila competitiva por mais 01h 15m"`).

## **3\. Cancelamento Seguro**

Em situações raras de dessincronização extrema onde a partida fica travada devido a uma resposta inesperada do motor de combates ou um erro interno de tick, os jogadores dispõem do comando de segurança `/fixbattle`.

### **Como Funciona a Memória de Cancelamento Seguro?**

Para evitar que um jogador abuse do comando para fugir de uma derrota iminente, o sistema exige um **Acordo Mútuo**:

1. Ao digitar o comando, o jogador envia uma solicitação de cancelamento.  
2. O oponente recebe um alerta visual de que foi solicitado um cancelamento técnico de segurança.  
3. Se o oponente concordar e também digitar o comando dentro de um período aceitável, o mod encerra a arena de forma limpa:  
   * Nenhum dos jogadores perde ou ganha Rating (`R`).  
   * Os dois jogadores e as equipes são teleportados de volta em segurança para os locais de origem pelo safeRescue.  
   * A arena instanciada é liberada e limpa imediatamente do servidor.

---