---

### **Server Requirements**

Before starting the installation process, make sure your infrastructure meets all the required dependencies listed below:

* **Minecraft:** 1.21.1  
* **Fabric Loader:** 0.17.3 or higher  
* **Fabric API:** 0.116.6+1.21.1 or higher  
* **Java:** 21 or higher  
* **Cobblemon:** 1.7.3  
* **Fabric Language Kotlin:** 1.13.0+kotlin.2.1.0 or higher

---

# **Installation:**

The installation process for **Cobblemon BattleHUB** is simple and straightforward. Follow the steps below to prepare your server correctly.

### **Step 1: Initial Installation**
1. Transfer the mod `.jar` file to your server's `mods` directory.  
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
    This license is valid exclusively for a single active instance. The security system will permanently bind your activation key to the first IP that validates it. Multiple activations or key sharing are not possible.

!!! info "Don't have an activation key yet?"
    Your license key (*License Key*) is automatically generated and sent to your email as soon as your Stripe payment is confirmed. To acquire yours, join our [Official Discord](https://discord.gg/aDCgBbvRe5) and open a support ticket.

---

### **Step 4: Finalization**
After configuring the JSON files and activating your key, run the following command on the server to apply the changes:                 
`/bh reload`

---

### Done! The mod is configured, activated, and ready to be used on your server.

---