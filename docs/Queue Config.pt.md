# **Queue Config**

---

## **Configuração da Fila de Matchmaking**

O ficheiro queue\_config.json controla as regras de emparelhamento (matchmaking) do **Cobblemon BattleHUB**, focando principalmente na prevenção de manipulação de resultados (Win Trading) através de tempos de espera (cooldown) configuráveis entre os mesmos jogadores.

---

### **Caminho do Ficheiro**

`config/cobblemon\_battlehub/queue\_config.json`

## **Modelo Padrão de Configuração**

Abaixo está o modelo JSON padrão gerado de forma automática pelo mod:

        {  
         "QueueAntiTradingCasual": 30,  
         "QueueAntiTradingRanked": 60  
        }

---

## **Explicação Detalhada dos Parâmetros**

### **1\. Prevenção de Win Trading (Fila Casual)**

* **QueueAntiTradingCasual** (Padrão: 30):  
  O tempo de espera (em minutos) que dois jogadores específicos devem aguardar antes de poderem ser emparelhados novamente na fila **Casual** após terem lutado um contra o outro. Isto impede que amigos abusem do sistema de matchmaking rápido para jogar consecutivamente de forma intencional na fila pública.

### **2\. Prevenção de Win Trading (Fila Competitiva)**

* **QueueAntiTradingRanked** (Padrão: 60):  
  O tempo de espera (em minutos) que dois jogadores específicos devem aguardar antes de poderem ser emparelhados novamente na fila **Rankeada** após um duelo competitivo. Como as partidas rankeadas valem pontos de classificação e recompensas, este limite é superior (1 hora por padrão) para evitar qualquer tentativa de ganho artificial de pontos (Elo Boosting).

---

## **Como o Sistema de Matchmaking Funciona (Nos Bastidores)**

O algoritmo do Cobblemon BattleHUB executa várias verificações automáticas de segurança no momento em que um jogador tenta entrar na fila:

### **Verificação de Penalidades (Leaver Buster)**

Se um jogador tentar entrar na fila competitiva (Rankeada) mas possuir um histórico recente de desconexões intencionais (quits), o sistema bloqueia a entrada imediatamente:

1. O mod verifica se a expiração do banimento por desistência ainda está ativa.  
2. Caso esteja ativo, o jogador recebe um alerta no chat indicando o tempo exato restante de suspensão formatado em horas e minutos.

### **Cálculo de Rating (Glicko-2)**

Para as partidas competitivas, o sistema não faz buscas aleatórias. Ele calcula dinamicamente:

* **Rating:** A pontuação atual de habilidade do jogador (padrão inicial de 1500.0).  
* **RD (Rating Deviation):** A confiabilidade do nível do jogador (padrão inicial de 350.0). O matchmaking expande progressivamente os limites aceitáveis de diferença de Elo à medida que o tempo de espera na fila aumenta.

### **Validação de Arenas Disponíveis**

A fila só efetuará o emparelhamento se houver pelo menos **uma arena física vazia** disponível na dimensão de combates. Se todas as arenas estiverem ocupadas, os jogadores permanecerão na fila em modo de espera até que uma partida seja finalizada.

---

## **Como Recarregar as Alterações**

Se modificar os valores de minutos de anti-trading no ficheiro queue\_config.json, aplique as alterações ao servidor em tempo real utilizando o comando:

`/bh reload`

---