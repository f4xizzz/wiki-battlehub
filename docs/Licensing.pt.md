# **Licença & Ativação**

---

O Cobblemon BattleHUB é um mod pago. Num **servidor dedicado** ele fica travado até você ativar uma chave de licença. A chave é então vinculada ao servidor e validada contra o nosso backend.

**Singleplayer e mundos LAN integrados estão sempre ativos** — sem chave, sem checagem de internet.

!!! tip "Trial grátis"
    Pega uma chave `BATTLEHUB-TRIAL-XXXX-XXXX` grátis no nosso [Discord](https://discord.gg/GbbbNvQG3N) pra testar o mod no seu servidor. Uma chave de trial funciona em qualquer servidor; cada servidor tem sua própria contagem regressiva, que começa na primeira ativação dele, e o mod se trava quando o trial acaba.

---

## **Ativando**

1. Pega uma chave no nosso [Discord](https://discord.gg/GbbbNvQG3N) — uma `BATTLEHUB-TRIAL-XXXX-XXXX` grátis, ou uma completa depois da compra confirmada.
2. Entre no seu servidor como operador de verdade (nível de permissão 4 / console).
3. Rode:

    `/bh activation [CHAVE-DE-LICENCA]`

Ao dar certo o servidor grava `config/cobblemon_battlehub/license.json` e destrava tudo. A licença é revalidada contra o backend a cada **4 horas**.

!!! info "Um servidor por chave"
    Na primeira ativação a chave é vinculada àquela instância de servidor. O seu IP público pode mudar (IP dinâmico, troca de host) sem quebrar a ativação, mas a chave não funciona em um segundo servidor diferente ao mesmo tempo. Abra um ticket no Discord se precisar migrar a chave pra outra máquina.

!!! info "Queda do backend não te derruba"
    Se o backend de licença ficar temporariamente fora do ar, um servidor já ativado continua rodando por um período de tolerância enquanto tenta de novo em segundo plano. Você só perde o acesso se a chave for realmente revogada ou expirar.

---

## **Enquanto sem licença**

Num servidor dedicado sem licença válida:

* Todo comando `/bh` e todo menu é bloqueado, **exceto `/bh activation`**.
* Os jogadores veem: *"This server doesn't have a valid license! … Use: /battlehub activation <your_key>"*.
* Convites de duelo, matchmaking e torneios não fazem nada.

O mod ainda carrega — ele só fica inerte até você ativar.

---

## **Tipos de chave**

| Formato | Comportamento |
| :--- | :--- |
| `BATTLEHUB-XXXX-XXXX` | Chave completa. Vinculada ao seu servidor. Vitalícia, salvo se emitida como temporária. |
| `BATTLEHUB-XXXX-XXXX` *(temporária)* | Igual, mas expira numa data definida; o mod se trava quando a data passa (`/bh` mostra um aviso de "temporary license expired"). |
| `BATTLEHUB-TRIAL-XXXX-XXXX` | Trial grátis. Funciona em qualquer servidor; cada servidor tem sua contagem de N dias, começando na primeira ativação dele. Trava quando o trial acaba. |

---

## **`license.json`**

Gravado e gerenciado pelo mod. **Não** edite — um arquivo inválido simplesmente falha na verificação e o mod fica travado até você rodar `/bh activation` de novo. Esse arquivo é por servidor: não coloque no controle de versão nem copie entre servidores.

Se o BattleHUB ficar travado num setup limpo e licenciado, abra um ticket no Discord com o seu `latest.log`.

---

## **Resolvendo problemas**

| Sintoma | Causa / solução |
| :--- | :--- |
| "Activation failed. Invalid key, or key is already bound to another server." | Chave já vinculada a outro servidor, revogada, ou digitada errado. |
| A ativação trava e falha na primeira tentativa | O backend de licença estava dormindo (cold start pode levar até um minuto). Rode o comando de novo. |
| Funciona, depois trava | Chave temporária expirou, a chave foi revogada, ou o backend ficou fora do ar além do período de tolerância. |
