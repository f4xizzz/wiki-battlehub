# **Server Config**

---

## **Formatos de Batalha e Presets**

O **Cobblemon BattleHUB** possui suporte nativo para múltiplos formatos de combate competitivos e casuais, geridos pela lógica interna da classe `RankedPreset`. Estes formatos definem o comportamento das arenas, a quantidade de Pokémon que entram em campo em simultâneo e as regras estruturais da batalha.

---

## **Formatos Base Suportados**

O mod adapta-se dinamicamente às regras oficiais do ecossistema de eSports Pokémon, suportando três estruturas de combate principais:

* **Singles (Individual):** Combates tradicionais de 1v1.  
* **Doubles (Duplas):** Combates de 2v2 (formato padrão do VGC oficial).  
* **Triples (Trios):** Combates de 3v3 de alta complexidade estratégica.

---

## **Presets de Regras (`RankedPreset`)**

Para simplificar a gestão e garantir o equilíbrio competitivo das filas rankeadas, o mod vem equipado com **Presets de Regras** pré-configurados diretamente no código.

Abaixo encontras a especificação de cada Preset disponível no mod:

### **1\. Solo (`solo`)**

* **Estrutura de Combate:** Singles (1v1)  
* **Nível Ajustado:** 50  
* **Cláusula de Espécie:** Ativa (Proíbe Pokémon repetidos na mesma equipa)  
* **Cláusula de Item:** Ativa (Proíbe itens equipados repetidos)  
* **Regras Showdown:** `Standard` (Garante as validações competitivas base da Smogon)

### **2\. Doubles (`doubles`)**

* **Estrutura de Combate:** Doubles (2v2)  
* **Nível Ajustado:** 50  
* **Cláusula de Espécie:** Ativa  
* **Cláusula de Item:** Ativa  
* **Regras Showdown:** `Standard`

### **3\. Triples (`triples`)**

* **Estrutura de Combate:** Triples (3v3)  
* **Nível Ajustado:** 50  
* **Cláusula de Espécie:** Ativa  
* **Cláusula de Item:** Ativa  
* **Regras Showdown:** `Standard`

### **4\. Monotype (`monotype`)**

* **Estrutura de Combate:** Singles (1v1)  
* **Nível Ajustado:** 50  
* **Cláusula de Espécie:** Ativa  
* **Cláusula de Item:** Ativa  
* **Regras Showdown:** `Standard` e `Same Type Clause` (Obriga a que todos os Pokémon da equipa partilhem pelo menos um tipo elemental comum).

### **5\. Custom (`custom`)**

* **Estrutura de Combate:** Singles (1v1) por padrão.  
* **Nível Ajustado:** 50 por padrão.  
* **Cláusula de Espécie:** Desativada por padrão.  
* **Cláusula de Item:** Desativada por padrão.  
* **Descrição:** Permite aos administradores criar regras totalmente personalizadas nos ficheiros das Ladders, ignorando as restrições competitivas padrão do mod.

---

## **Gimmicks e Mecânicas de Arena**

Independentemente do Preset de regras selecionado, o mod permite ativar ou desativar de forma isolada as principais mecânicas de batalha introduzidas ao longo das gerações Pokémon nas configurações das Ladders:

* **Mega Evolução** (`allowMega`)  
* **Z-Moves** (`allowZMove`)  
* **Dynamax / Gigantamax** (`allowDynamax`)  
* **Terastal** (`allowTera`)

!!! tip "Otimização de Performance"
    Todas as mecânicas acima são aplicadas na criação do objeto `BattleFormat` gerado pelo mod, garantindo que o motor de batalhas do Cobblemon aplique e valide as mecânicas em tempo real sem causar dessincronização entre o cliente e o servidor.

---