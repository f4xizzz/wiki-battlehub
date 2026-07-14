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
3. **Resgate de Emergência:** Ao tentar reconectar, o mod interceta a entrada do jogador e devolve o seu modo de jogo e o inventário original, e teleporta-o em segurança para as coordenadas globais configuradas no arquivo `fallbackarenadisconnect.json` (geralmente o Spawn ou Lobby do servidor).

## **2\. Sistema Leaver Buster (Punição por Deserção)**

Para manter o ecossistema competitivo saudável, o abandono de partidas ranqueadas (seja por turnos estourados ou por sair do servidor de propósito) é severamente punido.

### **O que é considerado Abandono?**

Uma derrota por desistência forçada (W.O) é declarada nos seguintes cenários:

* **Desconexão:** O jogador fecha o jogo ou cai e o seu boneco é removido do servidor.  
* **Inatividade Prolongada (AFK):** O jogador estoura o limite de turnos em que o piloto automático é acionado de forma consecutiva (conforme configurado em `maxAfkAutopilotRounds` no `server_config.json`).

### **Consequências para o Desertor**

1. O oponente ativo recebe uma vitória automática por W.O. com uma mensagem de congratulações no chat.  
2. O desertor recebe uma derrota automática no seu perfil estatístico e perde Rating (![][image2]) normalmente.  
3. E aplicado um **bloqueio temporário (Ranked Ban)** ao jogador. A duração deste banimento é cumulativa e impede o utilizador de entrar novamente na fila competitiva por um tempo determinado.

**Aviso de Fila Bloqueada:** Ao tentar entrar na fila de matchmaking com o banimento ativo, o jogador recebe o aviso do tempo restante formatado (`ex: "Você está banido da fila competitiva por mais 01h 15m"`).

## **3\. Cancelamento Seguro**

Em situações raras de dessincronização extrema onde a partida fica travada devido a uma resposta inesperada do motor de combates ou um erro interno de tick, os jogadores dispõem do comando de segurança `/fixbattle`.

### **Como Funciona a Memória de Cancelamento Seguro?**

Para evitar que um jogador abuse do comando para fugir de uma derrota iminente, o sistema exige um **Acordo Mútuo**:

1. Ao digitar o comando, o jogador envia uma solicitação de cancelamento.  
2. O oponente recebe um alerta visual de que foi solicitado um cancelamento técnico de segurança.  
3. Se o oponente concordar e também digitar o comando dentro de um período aceitável, o mod encerra a arena de forma limpa:  
   * Nenhum dos jogadores perde ou ganha Rating (![][image2]).  
   * Ambos os jogadores e as suas equipas são teleportados de volta para os seus locais de origem em segurança pelo safeRescue.  
   * A arena instanciada é libertada e limpa imediatamente do servidor.

---

[image1]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACoAAAAZCAYAAABHLbxYAAABoklEQVR4Xu2Wv0rDUByFKyIoCiJSg23S/KngqkTwBcRBUAQdRF/AwbmCkyAOuuno5uAjONmh4NZOLi4OLqK4uDlK/X54o+GatEbvUDAHDiSn5977JW1yWyjkyvWP5LrurOM4i3reSb7vTzPuGJ/hTdu2h/SOEXmeN1+pVGos0sJtjnf1Tpror+Fb5pgpFosjHB/gqyAIRvXun6VAl1lgCb/+FLRUKjn07/BWlDF2TF3wTrxrVCwSZgEVQNUPY3Ef2QVuyB2O5eb0C9DTBFD5hs7Jn8iDeG5MWUEVUBrot9yYsoCqB6eRBNQRNAzDAV4TFoXJbq5WqxMM6dfnyAJqWdYw3XoSUEdQeT24H++xrqZ7gj19jiygojSgtNyYsoLSPUwCUqAP5XLZjufG1A1UflosPh6ds4Ot0H+jvxBlQA6SXYrlOMqNKgLFe/pnapt8ljuFfckEGpgm3o96bJ9T0iHb+BxsSky6zeSPuB3zC75WD55A2Zzf4Hp8e+SuzpHdc5E1vM5xi/mO5OH+WqFHJG8A+SMD4Kpsq/rnuXLlytXjegelH4vGIJY1hQAAAABJRU5ErkJggg==>

[image2]: <data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAaCAYAAAC+aNwHAAABSElEQVR4XmNgGAUoQF5e3hGInwPxfyT8Coh/AfFfOTm5k0A6GKiUGV0vCgAqmgPEv4EabJCEmYH8NKhBZUA+I5IcAqirq/MCFR0G4ruKioriyHJAMUkgfohNDg6AkppA/BaI1wC5LMhysrKypkDxb0B8VUpKSgRZDg6AivyACv4rKCiko8sBxRpAckBcjC4HB0DJSfJo/jc2NmYFiiVDXVYK4iPrgQNRUVEeoIID8pBQPwZlXwfZCjRwurS0tDC6HhQgj93/jECnV8pDQt8VSTkmgPkfiIuQxYEajYFiX4F4DrI4BpDH4n+oeDTU4FZkcRRAIP5BBoPCoRxZHAUAna8DVPReHjP+WYBhsArZACC7Gsh2gWm0lYekLpATYfgVKDxgJgD5wUD8F2QQUGMskD1bRkaGEyZPFAB5C6jZFxQTJGseBcMeAACyIWKOy8AaQgAAAABJRU5ErkJggg==>