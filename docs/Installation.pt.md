---

!!! info "Fabric é o lançamento atual"
    A versão pública roda em **Fabric** — use `cobblemon_battlehub-fabric-<versão>.jar`. Existe um build de **NeoForge** em testes; vai ser publicado assim que for verificado — fica de olho no [Discord](https://discord.gg/GbbbNvQG3N). O **CarbonChat não tem build pra NeoForge**, então as abas de chat do menu vão continuar só no Fabric.

### **Requisitos do servidor**

Antes de iniciar o processo de instalação, certifique-se de que a sua infraestrutura atende a todas as dependências obrigatórias listadas abaixo:

**Comum:** Minecraft `1.21.1` · Java `21+` · Cobblemon `1.8.0`/`1.8.1` (ou `1.7.3` — [veja qual jar baixar](Dependencies.pt.md)) · [Architectury API](https://modrinth.com/mod/architectury-api) `13.0+`

| | Fabric | NeoForge |
| :--- | :--- | :--- |
| Loader | Fabric Loader `0.16+` | NeoForge `21.1.133+` |
| API | Fabric API `0.116.6+1.21.1+` | — |
| Runtime Kotlin | Fabric Language Kotlin `1.13.0+kotlin.2.1.0+` | Kotlin for Forge `5.7.0+` |

---

# **Instalação:**

O processo de instalação do **Cobblemon BattleHUB** é simples e direto. Siga os passos abaixo para preparar o seu servidor corretamente.

### **Passo 1: Instalação Inicial**
1. Transfira o arquivo `.jar` do mod do seu loader para o diretório `mods` do servidor, junto com Cobblemon, Architectury API e o runtime do Kotlin (Fabric Language Kotlin no Fabric, Kotlin for Forge no NeoForge — mais a Fabric API no Fabric).
2. Inicie (ou reinicie) o servidor para carregar o mod na memória.  
3. O mod gerará de forma automática a pasta cobblemon\_battlehub dentro do diretório config/ do seu servidor durante o startup.

###**Passo 2: Configuração dos Arquivos**
Navegue até a pasta recém-criada em config/cobblemon_battlehub/ e realize o ajuste básico obrigatório nos seguintes arquivos para que o mod saiba onde operar:

####**fallbackarenadisconnect.json**
Este arquivo define para onde o jogador será teleportado ao desconectar ou sair da arena. Defina as coordenadas do lobby do seu servidor:

    json
    {
      "dimension": "multiworld:void",
      "x": 777.5,
      "y": 77.0,
      "z": 777.5,
      "yaw": 0.0,
      "pitch": 0.0
    }
---

####**arenas.json**
Aqui você deve configurar a dimensão onde as estruturas das arenas serão geradas:

    {
      "dimension": "multiworld:battles",
      "arenas": [
        // Configure suas arenas aqui
     ]
    }
---

###**Passo 3: Ativação do Mod**
Para desbloquear o mod, você precisa ativar a sua licença dentro do servidor:

* **1.** Entre no seu servidor.
* **2.** Utilize o comando de ativação: `/bh activation [LICENSE-KEY]`
    * (A *[KEY]* é enviada para você imediatamente após a confirmação do pagamento via Stripe).

!!! warning "Atenção: Licença Única por Servidor"
    Esta licença é válida exclusivamente para uma única instância ativa. A chave de ativação fica permanentemente vinculada ao primeiro servidor que a ativar. Não é possível realizar ativações múltiplas ou compartilhamento de chave.

!!! info "Ainda não possui uma chave de ativação?"
    A sua chave de licença (*License Key*) é gerada de forma automática e enviada ao seu e-mail assim que o pagamento via Stripe for confirmado. Para adquirir a sua, junte-se ao nosso [Discord Oficial](https://discord.gg/GbbbNvQG3N) e abra um ticket de atendimento.

---

###**Passo 4: Finalização**
Após realizar as configurações nos arquivos JSON e ativar a sua chave, execute o comando abaixo no servidor para aplicar as mudanças:                 
`/bh reload`

---

###Pronto! O mod está configurado, ativado e pronto para ser utilizado em seu servidor.

---