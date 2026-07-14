# **Storage**

---

## **Database Configuration**

The **Cobblemon BattleHUB** storage system securely saves mission progress, shop purchase history, and competitive statistics (Elo/Glicko-2) for all players. The mod natively supports both **SQLite** (local database) and **MySQL** (remote database for server networks).

---

## **Step-by-Step Setup**

### **Option A: Use SQLite (No Additional Setup Required)**

You do not need to do anything! Just start your server and the mod will create the local file `battlehub_database.db` at:

`config/cobblemon_battlehub/storage/battlehub_database.db`

### **Option B: Use MySQL Connection (Recommended for Networks)**

1. Open your hosting panel database manager and create a new clean database (e.g., `cobblemon_battlehub`).  
2. Shut down your physical Minecraft server.  
3. Open the mod’s `storage.json` file.  
4. Change `"storageType"` to `"mysql"`.  
5. Fill in `"host"`, `"port"`, `"database"`, `"username"`, and `"password"` with the credentials provided by your host.  
6. **Start the physical server.**

!!! warning "Important Warning About Driver Switching"
    Changing the storage driver (sqlite to mysql or vice versa) requires the server to go through a **full startup/restart** so the connection drivers and initial queries are processed safely. Do not use only the `/bh reload` command when switching database drivers.

---

### **File Path**

`config/cobblemon_battlehub/storage/storage.json`

---

## **Default Configuration Template**

Below is the structure automatically generated on the mod’s first startup:

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

## **Detailed Parameter Explanation**

### **1. Storage Type (`storageType`)**

* **`storageType`** (Accepted values: `"sqlite"` or `"mysql"` | Default: `"sqlite"`):  
    * `sqlite`: Creates a local file database automatically. Ideal for test or isolated servers. Does not require external configuration.  
    * `mysql`: Connects to a remote database server. Recommended if you use server networks (such as Velocity or BungeeCord) to allow instant player data synchronization across worlds.

### **2. Connection Credentials (MySQL Only)**

* **`host`** (Default: `"localhost"`): The IP address or domain where your MySQL database server is hosted.  
* **`port`** (Default: `3306`): The network port of your MySQL server (usually 3306).  
* **`database`** (Default: `"cobblemon_battlehub"`): The name of the database on the SQL server where the mod’s tables will be created. *(You must create this database manually in your panel before the mod connects!)*  
* **`username`** (Default: `"admin"`): The MySQL access user with read/write permission.  
* **`password`** (Default: `"password"`): The password for the configured user.

### **3. Table Organization**

* **`tablePrefix`** (Default: `"bhub_"`):  
  The prefix added before each table name in the database. This helps avoid name conflicts if you share the same database with other plugins or mods.

---

## **Created Table Structure**

Once the database is connected successfully, the mod automatically runs the table creation queries (`CREATE TABLE IF NOT EXISTS`). The structure includes:

1. **`<prefix>quest_progress:`** Saves each player’s individual progress and completion history for daily and monthly quests.  
2. **`<prefix>player_stats:`** Stores all competitive statistics, Elo history, wins, and losses.  
3. **`<prefix>shop_history:`** Records each user’s shop purchase history and purchase limits.

---
