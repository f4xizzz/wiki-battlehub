---

!!! info "Fabric **and** NeoForge"
    Cobblemon BattleHUB ships for both loaders — use `cobblemon_battlehub-fabric-<version>.jar` or `cobblemon_battlehub-neoforge-<version>.jar`. NeoForge support is a recent addition; if something behaves differently from Fabric, report it on [Discord](https://discord.gg/aDCgBbvRe5). Note that **CarbonChat has no NeoForge build**, so the in-menu chat tabs are Fabric-only.

### **Server Requirements**

Before starting the installation process, make sure your infrastructure meets all the required dependencies listed below:

**Common:** Minecraft `1.21.1` · Java `21+` · Cobblemon `1.7.3` · [Architectury API](https://modrinth.com/mod/architectury-api) `13.0+`

| | Fabric | NeoForge |
| :--- | :--- | :--- |
| Loader | Fabric Loader `0.16+` | NeoForge `21.1.133+` |
| API | Fabric API `0.116.6+1.21.1+` | — |
| Kotlin runtime | Fabric Language Kotlin `1.13.0+kotlin.2.1.0+` | Kotlin for Forge `5.7.0+` |

---

# **Installation:**

The installation process for **Cobblemon BattleHUB** is simple and straightforward. Follow the steps below to prepare your server correctly.

### **Step 1: Initial Installation**
1. Transfer the mod `.jar` for your loader to your server's `mods` directory, alongside Cobblemon, Architectury API and the Kotlin runtime (Fabric Language Kotlin on Fabric, Kotlin for Forge on NeoForge — plus Fabric API on Fabric).
2. Start (or restart) the server to load the mod into memory.  
3. The mod will automatically generate the `cobblemon_battlehub` folder inside your server's `config/` directory during startup.

### **Step 2: Configure the Files**
Navigate to the newly created `config/cobblemon_battlehub/` folder and perform the required basic adjustments in the following files so the mod knows where to operate:

#### **fallbackarenadisconnect.json**
This file defines where the player will be teleported when disconnecting or leaving the arena. Set your server lobby coordinates:

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

#### **arenas.json**
Here you should configure the dimension where the arena structures will be generated:

    {
      "dimension": "multiworld:battles",
      "arenas": [
        // Configure your arenas here
     ]
    }
---

### **Step 3: Mod Activation**
To unlock the mod, you need to activate your license inside the server:

* **1.** Join your server.
* **2.** Use the activation command: `/bh activation [LICENSE-KEY]`
    * (The *[KEY]* is sent to you immediately after payment confirmation via Stripe).

!!! warning "Attention: Single Server License"
    This license is valid exclusively for a single active instance. Your activation key is permanently bound to the first server that activates it. Multiple activations or key sharing are not possible.

!!! info "Don't have an activation key yet?"
    Your license key (*License Key*) is automatically generated and sent to your email as soon as your Stripe payment is confirmed. To acquire yours, join our [Official Discord](https://discord.gg/aDCgBbvRe5) and open a support ticket.

---

### **Step 4: Finalization**
After configuring the JSON files and activating your key, run the following command on the server to apply the changes:                 
`/bh reload`

---

### Done! The mod is configured, activated, and ready to be used on your server.

---