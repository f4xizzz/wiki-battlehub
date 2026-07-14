# **FAQs**

---

## **Frequently Asked Questions**

Welcome to the FAQ section of **Cobblemon BattleHUB**. Below, we have compiled answers to the most common questions from our community of server administrators and owners.

### **1. Where should I host images for the Shop?**

To ensure 2D image loading in the mod interface is fast and free of security blocks (CORS), we strongly recommend using [**Imgur**](https://imgur.com/).

* **How to do it:** Upload your image to Imgur, right-click it, and choose **"Copy Image Address"** (the link must end with `.png` or `.jpg`).  
* Paste that link directly into the `imageUrls` field in your `shop.json` or in the `webhook.json` settings.  
* **Technical Note:** The mod uses its own system that downloads the image only once on the player's machine and caches it in memory, ensuring high performance even with dozens of items in the shop!

---

### **2. How can I add custom arenas using my server's builds?**

Cobblemon BattleHUB instantiates physical arenas in the battle world using Minecraft's native **Structures** `(.nbt)` system. To customize arena appearances, you do not need to change the mod—just use a **Datapack**!

1. On our [Discord](https://discord.gg/aDCgBbvRe5), we provide the download for the mod's base *Structures* pack.  
2. Save your custom arena builds as `(.nbt)` files using Structure Blocks in-game.  
3. Replace the original .nbt files in the pack with your own, keeping the same filenames.  
4. Place the datapack folder inside `world/datapacks/` on your server and run `/reload`.

The next time a battle starts, the mod will build your custom arena!

---

### **3. How do I purchase the mod and the license for my server?**

The acquisition process is secure, fast, and 100% automated! To get your copy of **Cobblemon BattleHUB** and your activation key (*License Key*), follow the steps below:

1. Join our community on [Discord](https://discord.gg/aDCgBbvRe5).  
2. Navigate to the **Shop** channel/section.  
3. Select the Cobblemon BattleHUB package and add it to your **cart**.  
4. You will be redirected to complete the payment securely on the official **Stripe** website.  
5. Once the payment is approved, the system will instantly send you the mod `.jar` file and your exclusive **License Key**.  
6. Just place the mod in the server, run `/bh activation [YOUR_KEY]`, and enjoy!

---

!!! tip "Still have questions?"
    If your technical or configuration question was not answered here or on the previous documentation pages, don’t hesitate to open a **Support Ticket** on our Discord. Our team will be ready to help you set up the perfect eSports experience for your server!