---

### **Requisitos do servidor**

Antes de iniciar o processo de instalação, certifique-se de que a sua infraestrutura atende a todas as dependências obrigatórias listadas abaixo:

* **Minecraft:** 1.21.1  
* **Fabric Loader:** 0.17.3 ou superior  
* **Fabric API:** 0.116.6+1.21.1 ou superior  
* **Java:** 21 ou superior  
* **Cobblemon:** 1.7.3  
* **Fabric Language Kotlin:** 1.13.0+kotlin.2.1.0 ou superior

---

# **Instalação:**

O processo de instalação do **Cobblemon BattleHUB** é simples e direto. Siga os passos abaixo para preparar o seu servidor corretamente.

### **Passo 1: Instalação Inicial**
1. Transfira o arquivo .jar do mod para o diretório mods do seu servidor Minecraft.  
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
    Esta licença é válida exclusivamente para uma única instância ativa. O sistema de segurança vinculará permanentemente a sua chave de ativação ao primeiro IP que validá-la. Não é possível realizar ativações múltiplas ou compartilhamento de chave.

!!! info "Ainda não possui uma chave de ativação?"
    A sua chave de licença (*License Key*) é gerada de forma automática e enviada ao seu e-mail assim que o pagamento via Stripe for confirmado. Para adquirir a sua, junte-se ao nosso [Discord Oficial](https://discord.gg/aDCgBbvRe5) e abra um ticket de atendimento.

---

###**Passo 4: Finalização**
Após realizar as configurações nos arquivos JSON e ativar a sua chave, execute o comando abaixo no servidor para aplicar as mudanças:                 
`/bh reload`

---

###Pronto! O mod está configurado, ativado e pronto para ser utilizado em seu servidor.

---