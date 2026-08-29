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

## 3 Seed Unlocking Mechanics

### 3.1 Unlock Requirements & Eligibility
- **3.1.1 Locked Seeds**: All crops except `crop_wheat` start in a locked state upon profile creation (`unlockedCropIds: ["crop_wheat"]`).
- **3.1.2 Prerequisites**: To unlock a seed packet, the player must hold the required quantities of specific harvested crops in `inventory.crops` as defined in [crops-and-seeds.md](../content/crops-and-seeds.md).
- **3.1.3 Sequential Availability**: A crop's unlock button is only active when all prerequisite crop quantities are present in inventory.

### 3.2 Unlock Transactions & Inventory Consumption
- **3.2.1 Unlock Action**: Unlocking a seed is an instant marketplace action (0 Action Point cost, 0 spelling challenges).
- **3.2.2 Crop Deduction**: Tapping "Unlock" immediately deducts the required crop counts from `inventory.crops`.
- **3.2.3 Permanent Unlock State**: The unlocked `cropId` is appended to `unlockedCropIds` in the player profile document in Firestore, permanently enabling that seed for Coin purchases.
- **3.2.4 Idempotency & Safety**: If a player profile already contains a `cropId` in `unlockedCropIds`, no crops may be deducted again.

## 4 Seed Purchasing & Crop Tier Scaling

### 4.1 Seed Purchasing Rules
- **4.1.1 Unlock Gate**: Players may only purchase seeds for crops present in `unlockedCropIds`. Locked seeds cannot be purchased with Coins until unlocked.
- **4.1.2 Coin Purchase**: Players spend **Coins** to purchase Seed Packets in the Shop for any unlocked crop.
- **4.1.3 Seed Inventory**: Purchasing seeds adds them to `seedInventory` (`inventory.seeds`).
- **4.1.4 Affordability Guard**: If `playerCoins` is less than the seed price, the purchase button is disabled.

### 4.2 Crop Scaling Progression
- **4.2.1**: Crops scale in seed cost, required growth days, and harvest sell value to allow continuous player progression.

### 4.3 Default Starter Assets
- **4.3.1**: Every new player profile starts with initial starter coins (`5 Coins`), starter seeds (`3x Wheat Seeds`), and initial unlocked crops (`["crop_wheat"]`).

