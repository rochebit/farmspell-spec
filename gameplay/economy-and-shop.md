# Game Rules: Economy and Shop

This document defines the rules for crop harvesting, inventory storage, selling mechanics, seed purchasing, and economy scaling for **FarmSpell**.

## 1 Inventory & Crop Harvesting

### 1.1 Crop Inventory Rules
- **1.1.1**: Harvesting a mature crop (`ReadyToHarvest` plot) adds 1 unit of that crop to the player's **Crop Inventory**.
- **1.1.2**: Crop Inventory is stored permanently in the active Profile save state and has no storage capacity limits.

## 2 Crop Selling Mechanics

### 2.1 Transaction Rules
- **2.1.1 Zero Action Point Cost**: Conducting marketplace transactions (selling crops, unlocking seeds, purchasing seeds) costs **0 Action Points** and does NOT require a spelling challenge.
- **2.1.2 Instant Crop Selling**: Players can sell any harvested crops held in `inventory.crops`. Selling immediately removes the crops from inventory and adds their designated total Coin value to `inventory.coins`.

## 3 Seed Unlocking Mechanics

### 3.1 Unlock Requirements & Eligibility
- **3.1.1 Locked Seeds**: All crops except `crop_wheat` start in a locked state upon profile creation (`unlockedCropIds: ["crop_wheat"]`).
- **3.1.2 Prerequisites**: To unlock a seed packet, the player must hold the required quantities of specific harvested crops in `inventory.crops` as defined in [crops-and-seeds.md](../content/crops-and-seeds.md).
- **3.1.3 Unlock Eligibility**: A seed is eligible to unlock once all prerequisite crop quantities are present in inventory.

### 3.2 Unlock Transactions & Inventory Consumption
- **3.2.1 Crop Deduction**: Executing an unlock immediately deducts the required crop counts from `inventory.crops`.
- **3.2.2 Permanent Unlock State**: The unlocked `cropId` is appended to `unlockedCropIds` in the player profile document in Firestore, permanently enabling that seed for Coin purchases.
- **3.2.3 Idempotency & Safety**: If a player profile already contains a `cropId` in `unlockedCropIds`, no crops may be deducted again.

## 4 Seed Purchasing & Progression

### 4.1 Seed Purchasing Rules
- **4.1.1 Unlock Gate**: Players may only purchase seeds for crops present in `unlockedCropIds`. Locked seeds cannot be purchased with Coins until unlocked.
- **4.1.2 Coin Purchase**: Players spend **Coins** to purchase Seed Packets for any unlocked crop.
- **4.1.3 Seed Inventory**: Purchasing seeds increments the corresponding count in `inventory.seeds`.
- **4.1.4 Affordability Guard**: A seed purchase is only valid if `inventory.coins` is greater than or equal to the crop's `seedCost`.

### 4.2 Starter Assets & Progression
- **4.2.1 Starter Profile Assets**: Every new player profile starts with initial starter coins (`5 Coins`), starter seeds (`3x Wheat Seeds`), and initial unlocked crops (`["crop_wheat"]`).
- **4.2.2 Scaling Progression**: Crops scale in seed cost, required growth days, and harvest sell value to allow continuous player progression as defined in [crops-and-seeds.md](../content/crops-and-seeds.md).

