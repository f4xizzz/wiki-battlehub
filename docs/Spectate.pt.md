# **Spectate**

---

## **Sistema de Espectador**

O **Cobblemon BattleHUB** possui um robusto sistema de transmissão em tempo real que permite que outros jogadores assistam a partidas em andamento diretamente das arquibancadas físicas das arenas.

Este sistema garante imersão aos espectadores e total segurança competitiva aos combatentes.

---

## **Como Funciona o Sistema de Espectador?**

Ao entrar no modo espectador (seja por um menu ou comando do mod), o fluxo lógico do servidor executa as seguintes etapas:

1. **Salvamento de Estado:** O mod salva a localização original (X, Y, Z), a dimensão, o ângulo da câmera (Yaw/Pitch) e o Modo de Jogo (*GameMode*) atual do espectador.  
2. **Alteração de Estado:** O jogador é colocado no modo de jogo **Espectador** (SPECTATOR) e fica invisível para outros jogadores.  
3. **Mapeamento de Bancos:** O sistema varre as proximidades do centro da arena à procura de blocos específicos para acomodar o espectador (leia mais abaixo).  
4. **Teleporte e Transmissão:** O espectador é teleportado para o assento, virado de frente para o centro do combate, e passa a receber o fluxo de mensagens de combate e dados da arena do mod.

!!! note "Entrando antes da batalha começar"
    Você pode assistir a uma partida enquanto os jogadores ainda estão escolhendo o Pokémon líder. O espectador espera na arquibancada e a transmissão da batalha começa assim que a luta inicia (a partir da 1.4 você não é mais tirado da arquibancada nessa fase).

---

## **O Mecanismo das Arquibancadas (Seats)**

Para evitar que os espectadores fiquem flutuando de forma desorganizada pelo estádio, o mod possui um algoritmo inteligente de **Varredura de Assentos**.

### **O Bloco de Assento Padrão**

O sistema busca ativamente por blocos de **Laje de Pedra Polida** `(SMOOTH_STONE_SLAB)` para servir de banco.

### **Área de Varredura**

O algoritmo procura blocos em um raio em torno do centro da arena:

* **Horizontal (X/Z):** Até 30 blocos de distância.  
* **Vertical (Y):** 2 blocos abaixo do chão da arena até 8 blocos de altura.

### **Validação de Assento Usável**

Para que o espectador seja enviado a uma laje, o bloco precisa estar desocupado:

1. O bloco de laje precisa ter um bloco sólido abaixo.  
2. O bloco logo acima da laje (onde o jogador ficará sentado) e o subsequente devem ser ar (Air).  
3. Não pode haver nenhum outro jogador ocupando a caixa de colisão daquele assento.

**Nota:** Se o algoritmo não encontrar nenhuma laje de pedra polida usável em toda a arena de batalha, o espectador será teleportado por segurança para 2 blocos acima do centro exato da arena .

---


## **O Cilindro e Teto de Segurança (Anti-Cheat)**

Como o modo espectador do Minecraft garante movimentação livre (através de paredes e teto), o mod monitora constantemente a posição de cada espectador a cada tick do servidor para evitar abusos (como espionar bases, "Ghosting" ou travamento de chunks).

Existe um **Cilindro Invisível** de segurança configurado com as seguintes regras em relação ao centro da arena:

* **Distância Horizontal Máxima:** cerca de 46 blocos (o raio de varredura de 30 blocos × √2 + 4). O limite é largo o bastante para que todo assento da arquibancada — inclusive os dos cantos — fique dentro dele; se o jogador se afastar mais que isso do centro, ele é interceptado.  
* **Altura Máxima (Y):** 12 blocos acima do nível do chão da arena.  
* **Profundidade Mínima (Y):** 4 blocos abaixo do nível do chão da arena.

          [Limite: +12 Blocos de Altura]  
         ┌─────────────────────────────────┐  
         │         ÁREA PERMITIDA          │  ~46 Blocs  
         │         DE TRANSMISSÃO          │ ◄───────►  
         │          (Espectador)           │  Raio X/Z  
         │                                 │  
         │            Arena [X]            │  
         └─────────────────────────────────┘  
        [Limite: -4 Blocos de Profundidade]

### **O que acontece se o espectador ultrapassar os limites?**

Caso o jogador atravesse as barreiras invisíveis do cilindro, o servidor irá:

1. Bloquear o avanço e encontrar automaticamente um novo assento de laje válido e vazio na arquibancada.  
2. Teleportar o jogador imediatamente de volta para a arquibancada e rotacionar a sua câmera de volta para a luta.  
3. Exibir um aviso visual na barra de ações `(Action Bar)` do jogador: `spectator.status.out_of_bounds` (no máximo uma vez a cada 2 segundos, então não inunda mais a tela).  
4. O jogador sempre volta para a **dimensão da própria arena**, mesmo que tenha pulado para um jogador de outro mundo pelo menu de espectador.

---


## **Saindo do Modo Espectador**

Quando a batalha monitorada chega ao fim ou o espectador decide sair:

* O jogador é alterado de volta para o seu Modo de Jogo anterior (ex: Survival ou Creative).  
* Toda a invisibilidade e as permissões de voo temporárias são recalculadas e limpas.  
* Ele é teleportado de volta com precisão milimétrica para as coordenadas e dimensão exatas onde estava antes de começar a assistir.

---
