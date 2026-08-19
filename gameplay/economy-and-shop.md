# Game Rules: Economy and Shop

This document defines the rules for crop harvesting, inventory storage, selling mechanics, seed purchasing, and economy scaling for **FarmSpell**.

## 1 Inventory & Crop Harvesting

### 1.1 Crop Inventory Rules
- **1.1.1**: Harvesting a mature crop (`ReadyToHarvest` plot) adds 1 unit of that crop to the player's **Crop Inventory**.
- **1.1.2**: Crop Inventory is stored permanently in the active Profile save state and has no storage capacity limits.

## 2 Shop & Instant Selling Mechanics

### 2.1 Shop Access
- **2.1.1**: The Shop is accessible at any time from the main farm UI via a **Shop** button.
- **2.1.2**: Accessing the Shop and conducting transactions costs **0 Action Points** and does NOT require a spelling challenge.

### 2.2 Instant Crop Selling
- **2.2.1**: In the Shop interface, players can sell any harvested crops from their inventory.
- **2.2.2**: Selling is an instant transaction: tapping "Sell" immediately removes the crop from inventory and awards the crop's designated Coin value to `playerCoins`.

## 3 Seed Purchasing & Crop Tier Scaling

### 3.1 Seed Purchasing Rules
- **3.1.1**: Players spend **Coins** to purchase Seed Packets in the Shop.
- **3.1.2**: Purchasing seeds adds them to `seedInventory`.
- **3.1.3**: If `playerCoins` is less than the seed price, the purchase button is disabled.

### 3.2 Crop Scaling Progression
- **3.2.1**: Crops scale in seed cost, required growth days, and harvest sell value to allow continuous player progression.

### 3.3 Default Starter Assets
- **3.3.1**: Every new player profile starts with initial starter coins and seed packets in their inventory.

