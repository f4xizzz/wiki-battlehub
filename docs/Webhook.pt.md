# **Webhook**

---

## **Integração de Webhook do Discord**

O **Cobblemon BattleHUB** pode enviar eventos de **torneio** direto para um canal do Discord através de um webhook: quando um torneio começa, quando uma partida é convocada, quando uma partida termina e quando um campeão é coroado. O chaveamento atual é renderizado como imagem e anexado automaticamente nas mensagens de início / atualização / fim de partida.

!!! info "Só torneios"
    Este webhook cobre o sistema de torneios. Resultados de partidas ranqueadas / casuais e viradas de temporada **não** são enviados aqui.

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/webhook.json`

---

## **Modelo Padrão de Configuração**

Gerado pelo `WebhookConfig.java` na primeira inicialização:

```json
{
  "enabled": false,
  "webhookUrl": "WEBHOOK_URL",
  "msgTournamentStart": {
    "content": "@everyone",
    "title": "🏆 Tournament {name} has Started!",
    "description": "The bracket has been drawn for the **{participants}** participants.\n\n**INITIAL BRACKET:**\n{bracket}\n\n*Good luck to all trainers!*",
    "colorHex": "#8A2BE2",
    "thumbnailUrl": "",
    "imageUrl": ""
  },
  "msgMatchCalled": {
    "content": "",
    "title": "⚔️ New Match Called!",
    "description": "**{phase}** is about to begin!\n\n🔸 **{p1}** vs **{p2}**\n\nOpen your menus and prepare your teams!",
    "colorHex": "#8A2BE2",
    "thumbnailUrl": "",
    "imageUrl": ""
  },
  "msgMatchWinner": {
    "content": "",
    "title": "🔥 Battle Ended!",
    "description": "**{winner}** defeated **{loser}** in **{phase}** and advances in the tournament bracket!",
    "colorHex": "#8A2BE2",
    "thumbnailUrl": "",
    "imageUrl": ""
  },
  "msgTournamentWinner": {
    "content": "@everyone",
    "title": "👑 WE HAVE A CHAMPION!",
    "description": "The grand winner of the **{name}** tournament is **{winner}**!\n\nCongratulations on the victory and thanks to everyone who participated!",
    "colorHex": "#8A2BE2",
    "thumbnailUrl": "",
    "imageUrl": ""
  }
}
```

!!! danger "Não traduza os placeholders"
    Os textos das mensagens você pode reescrever à vontade (inclusive em português). Mas os termos entre chaves — `{name}`, `{phase}`, `{bracket}`, `{p1}`, etc. — são **literais em inglês**. Se você escrever `{fase}` ou `{chaveamento}`, o mod não substitui nada e o texto sai quebrado.

---

## **Ativando**

1. No Discord: **Configurações do canal → Integrações → Webhooks → Novo Webhook**, depois **Copiar URL do Webhook**.
2. Cole essa URL em `webhookUrl`.
3. Coloque `"enabled": true`.
4. Rode `/bh reload` (ou reinicie o servidor).

!!! warning "Coloque uma URL de verdade"
    Deixar `webhookUrl` no valor placeholder `WEBHOOK_URL` com `enabled` em `true` gera tentativas de envio com erro no log. Ou cole uma URL de webhook válida, ou deixe `enabled: false`.

---

## **Os quatro eventos de mensagem**

| Chave | Dispara quando | Anexa a imagem do chaveamento |
| :--- | :--- | :--- |
| `msgTournamentStart` | O chaveamento é sorteado e o torneio começa (reenviada também quando o chaveamento avança de rodada). | Sim |
| `msgMatchCalled` | Uma partida específica é convocada e os dois jogadores entram na janela de preparação de 3 minutos. | Não |
| `msgMatchWinner` | Uma partida do torneio termina. | Sim |
| `msgTournamentWinner` | A final termina e um campeão é decidido. | Não |

---

## **Campos dentro de cada mensagem**

| Campo | Para que serve |
| :--- | :--- |
| `content` | Texto puro **fora** do embed. É aqui que um `@everyone` / `<@&idDoCargo>` de verdade precisa ficar — pings dentro de `title`/`description` não notificam. Deixe `""` para não pingar. |
| `title` | Título do embed (linha de cabeçalho em negrito). |
| `description` | Corpo do embed. Markdown do Discord funciona: `**negrito**`, `*itálico*` e `\n` para quebra de linha. |
| `colorHex` | A cor da barra lateral esquerda do embed, em hexadecimal (`#8A2BE2`). Valores inválidos são ignorados. |
| `thumbnailUrl` | Imagem pequena no canto superior direito do embed. Precisa ser uma URL pública. |
| `imageUrl` | Imagem grande na parte de baixo do embed. **Ignorada** nas mensagens que anexam o chaveamento automaticamente (`msgTournamentStart`, `msgMatchWinner`) — o `bracket.png` gerado ocupa esse espaço. |

---

## **Variáveis e Placeholders Dinâmicos**

Termos entre chaves são substituídos antes do envio. Um placeholder sem valor para aquele evento vira uma string vazia. Podem ser usados em `content`, `title` e `description`:

| Placeholder | O que ele substitui |
| :--- | :--- |
| `{name}` | Nome de exibição do torneio, do perfil dele. |
| `{participants}` | Quantidade de jogadores inscritos (só na mensagem de início). |
| `{phase}` | O identificador do bloco da partida (ex: `Round 1 - Match 3`). |
| `{p1}` | Primeiro jogador da partida convocada. |
| `{p2}` | Segundo jogador da partida convocada. |
| `{winner}` | Jogador que venceu a partida, ou o campeão do torneio. |
| `{loser}` | Jogador que perdeu a partida. |
| `{bracket}` | Um texto/link curto para o chaveamento (a figura em si é anexada à parte como imagem). |

---

## **Reload**

Depois de editar o `webhook.json`:

`/bh reload`
