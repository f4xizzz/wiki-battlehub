# **Licença & Ativação**

---

O Cobblemon BattleHUB é um mod pago. Num **servidor dedicado** ele fica travado até você ativar uma chave de licença, que é então vinculada permanentemente ao IP do servidor por um backend assinado.

**Singleplayer e mundos LAN integrados estão sempre ativos** — sem chave, sem checagem de internet.

---

## **Ativando**

1. Compre uma chave no nosso [Discord](https://discord.gg/aDCgBbvRe5) — você recebe a chave assim que o pagamento via Stripe for confirmado.
2. Entre no seu servidor como operador de verdade (nível de permissão 4 / console).
3. Rode:

    `/bh activation [CHAVE-DE-LICENCA]`

Ao dar certo o servidor grava `config/cobblemon_battlehub/license.json` e destrava tudo. Esse arquivo é verificado **offline** (assinatura RSA) a cada boot, e a chave é revalidada contra o backend a cada **4 horas**.

!!! warning "Um IP por chave"
    O backend vincula a chave ao **primeiro IP** que a ativar. Você não pode mover uma chave pra um IP novo nem compartilhar entre servidores. Abra um ticket no Discord se você legitimamente precisa migrar uma chave.

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
| `BATTLEHUB-XXXX-XXXX` | Chave normal. Travada por IP, integridade do jar checada. Vitalícia, salvo se emitida como temporária. |
| `BATTLEHUB-XXXX-XXXX` *(temporária)* | Igual, mas expira numa data definida; o mod se trava quando a data passa (`/bh` mostra um aviso de "temporary license expired"). |
| `BATTLEHUB-DEV-XXXX-XXXX` | Chave de desenvolvedor. **Sem trava de IP, sem checagem de hash do jar.** Só pros seus próprios ambientes de teste. |

---

## **`license.json`**

```json
{
  "license_key": "BATTLEHUB-XXXX-XXXX",
  "expires_at": -1,
  "signature": "assinatura-RSA-em-base64"
}
```

**Não** edite. A `signature` é verificada contra a chave pública embutida no mod a cada startup; uma assinatura adulterada trava o mod. Esse arquivo é por servidor — não coloque no controle de versão nem copie entre servidores.

---

## **Anti-adulteração**

O BattleHUB inspeciona a própria pilha de execução quando a licença é checada. Se detectar outro mod tentando injetar nas classes de licenciamento (via Mixin), ele **trava o mod e loga um alerta** — o servidor Minecraft continua rodando normalmente, só o BattleHUB fica inerte:

```
[BattleHUB] SECURITY ALERT: possible illegal mixin injection into the license system.
[BattleHUB] Locking the mod (server keeps running).
```

Se isso disparar num setup limpo, é um falso positivo de uma interação incomum de mod. Você pode desligar a inspeção de pilha: coloque `ENABLE_STACK_INSPECTION = false` no `ActivationManager.java` e rebuilde, ou fale com o suporte.

---

## **Resolvendo problemas**

| Sintoma | Causa / solução |
| :--- | :--- |
| "Activation failed. Invalid key, or key is already bound to another IP." | Chave já usada em outro IP, revogada, ou digitada errado. |
| A ativação trava e falha na primeira tentativa | O backend de licença estava dormindo (cold start pode levar até um minuto). Rode o comando de novo. |
| "Integrity check failed. Adulterated JAR." | O hash do seu jar não está registrado pra essa release. Use uma chave `-DEV-` ou peça ao suporte pra registrar o hash da release. |
| Funciona, depois trava algumas horas depois | Chave temporária expirou, ou a revalidação de 4h falhou (chave revogada, ou o servidor não conseguiu alcançar o backend). |
| Log diz "LICENSE TAMPERED" no boot | O `license.json` foi editado (ou corrompido). Apague ele e rode `/bh activation` de novo. |
