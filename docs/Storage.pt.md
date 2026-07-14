# **Storage**

---

## **Configuração de Banco de Dados**

O sistema de armazenamento do **Cobblemon BattleHUB** permite salvar com segurança o progresso das missões, o histórico de compras de loja e as estatísticas competitivas (Elo/Glicko-2) de todos os jogadores. O mod oferece suporte nativo tanto a **SQLite** (banco local) quanto a **MySQL** (banco remoto para redes de servidores).

---

## **Como Configurar Passo a Passo**

### **Opção A: Utilizar SQLite (Sem Configurações adicionais)**

Não há necessidade de fazer absolutamente nada! Basta iniciar o seu servidor e o mod criará o arquivo local battlehub_database.db no caminho:

`config/cobblemon_battlehub/storage/battlehub_database.db`

### **Opção B: Utilizar Conexão MySQL (Recomendado para Redes)**

1. Acesse o gerenciador de bancos de dados do seu painel de hospedagem de servidores e crie uma nova database limpa (ex: `cobblemon_battlehub`).  
2. Desligue o seu servidor físico de Minecraft.  
3. Abra o arquivo `storage.json` do mod.  
4. Altere `"storageType"` para `"mysql"`.  
5. Preencha o `"host"`, `"port"`, `"database"`, `"username"` e `"password"` com as credenciais geradas pela sua hospedagem.  
6. **Inicie o servidor físico.**

!!! warning "Aviso Importante sobre Mudança de Driver"
    Mudar o driver de armazenamento (sqlite para mysql ou vice-versa) exige que o servidor passe por uma **inicialização/reinicialização completa** para que os drivers de conexão e as queries iniciais sejam processadas de forma segura. Não utilize apenas o comando /bh reload ao alternar os drivers de banco.

---

### **Caminho do Arquivo**

`config/cobblemon_battlehub/storage/storage.json`

---

## **Modelo Padrão de Configuração**

Abaixo está a estrutura gerada automaticamente na primeira inicialização do mod:

        {  
         "storageType": "sqlite",  
         "host": "localhost",  
         "port": 3306,  
         "database": "cobblemon_battlehub",  
         "username": "admin",  
         "password": "password",  
         "tablePrefix": "bhub_"  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Tipo de Armazenamento (storageType)**

* **`storageType`** (Valores aceitos: `"sqlite"` ou `"mysql"` | Padrão: `"sqlite"`):  
    * `sqlite`: Cria um banco de dados em arquivo local de forma totalmente automática. Ideal para servidores em fase de testes ou isolados. Não requer nenhuma configuração externa.  
    * `mysql`: Conecta a um servidor de banco de dados remoto (externo). É a opção recomendada se você utiliza redes de servidores (como Velocity ou BungeeCord) para permitir a sincronização de dados instantânea dos jogadores entre os mundos.

### **2\. Credenciais de Conexão (Apenas para MySQL)**

* **`host`** (Padrão: `"localhost"`): O endereço de IP ou domínio onde o seu servidor de banco de dados MySQL está hospedado.  
* **`port`** (Padrão: `3306`): A porta de conexão de rede do seu servidor MySQL (geralmente 3306).  
* **`database`** (Padrão: `"cobblemon_battlehub"`): O nome da database dentro do servidor SQL onde as tabelas do mod serão criadas. *(Você precisa criar este banco manualmente no seu painel antes do mod se conectar\!)*  
* **`username`** (Padrão: `"admin"`): O usuário de acesso do banco de dados MySQL que possui permissão de leitura e escrita.  
* **`password`** (Padrão: `"password"`): A senha de acesso correspondente ao usuário configurado.

### **3\. Organização de Tabelas**

* **`tablePrefix`** (Padrão: `"bhub_"`):  
  O prefixo adicionado antes do nome de cada tabela no banco. Isso é muito útil para evitar conflitos de nomes se você compartilhar o mesmo banco de dados com outros plugins ou mods.

---

## **Estrutura das Tabelas Criadas**

Assim que o banco de dados é conectado com sucesso, o mod executa automaticamente as queries de criação de tabelas (`CREATE TABLE IF NOT EXISTS`). A estrutura é composta por:

1. **`<prefixo>quest_progress:`** Salva o progresso individual e o histórico de conclusão das missões diárias e mensais de cada jogador.  
2. **`<prefixo>player_stats:`** Guarda todos os dados estatísticos competitivos, histórico de Elo, vitórias e derrotas.  
3. **`<prefixo>shop_history:`** Registra o histórico e limites de compra de cada usuário na loja.

---
