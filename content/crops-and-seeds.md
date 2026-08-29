# Content Specification: Crops & Seeds Data Catalog

This document specifies the data schema, progression scaling, and default system registry for crops and seeds in **FarmSpell**.

## 1 Crop Data Schema

### 1.1 Crop Definition Schema
- **1.1.1 Schema Format**: All crop definitions follow a standardized JSON schema format:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CropDefinition",
  "type": "object",
  "required": [
    "cropId",
    "cropName",
    "tier",
    "tierPosition",
    "seedCost",
    "requiredGrowthDays",
    "harvestSellPrice",
    "isDefaultUnlocked",
    "unlockRequirements",
    "iconIdentifier"
  ],
  "properties": {
    "cropId": {
      "type": "string",
      "description": "Unique string key for the crop (e.g. crop_wheat, crop_carrot)"
    },
    "cropName": {
      "type": "string",
      "description": "Human-readable display name"
    },
    "tier": {
      "type": "integer",
      "minimum": 1,
      "maximum": 5,
      "description": "Progression tier rating (1 to 5)"
    },
    "tierPosition": {
      "type": "integer",
      "minimum": 1,
      "maximum": 4,
      "description": "Position within tier (1 to 4)"
    },
    "seedCost": {
      "type": "integer",
      "minimum": 1,
      "description": "Coin cost to purchase 1 seed packet in the Shop (minimum 1 Coin)"
    },
    "requiredGrowthDays": {
      "type": "integer",
      "minimum": 3,
      "description": "Number of watered in-game days required to reach full maturity (minimum 3 Days)"
    },
    "harvestSellPrice": {
      "type": "integer",
      "minimum": 1,
      "description": "Coin price earned when selling 1 harvested crop at the Shop"
    },
    "isDefaultUnlocked": {
      "type": "boolean",
      "description": "Whether the crop seed is unlocked by default on profile creation (true only for crop_wheat)"
    },
    "unlockRequirements": {
      "type": "object",
      "additionalProperties": {
        "type": "integer",
        "minimum": 1
      },
      "description": "Map of prerequisite cropId to required harvested quantity consumed to unlock this seed in the Shop"
    },
    "iconIdentifier": {
      "type": "string",
      "description": "Sprite / asset identifier for rendering plot and inventory icons"
    }
  }
}
```

## 2 Default System Crop Catalog

### 2.1 Seed Unlock Progression Formula & Rules
- **2.1.1 Default Unlocked Crop**: `crop_wheat` (Tier 1, Position 1) is unlocked by default on all profiles (`isDefaultUnlocked: true`, `unlockRequirements: {}`).
- **2.1.2 Prerequisite Crop Selection Rule**:
  - **Immediate Predecessor**: Every locked crop requires the crop of immediately lower value (the preceding crop in the 20-crop progression order).
  - **Tier-Column Predecessors**: For every tier lower than the current crop's tier ($T-1, T-2, \dots, 1$), the crop requires the crop that occupies the same `tierPosition` within that lower tier.
  - **Total Required Crop Types**: As a consequence, Tier 1 requires 1 crop type, Tier 2 requires 2 crop types, Tier 3 requires 3 crop types, Tier 4 requires 4 crop types, and Tier 5 requires 5 crop types.
- **2.1.3 Quantity Scaling Formula**:
  - **Highest-Value Required Crop**: Quantity $Q_{\text{base}} = 3 + (\text{Tier} + \text{tierPosition}) \times 2$.
  - **Subsequent Diminishing Crops**: For each required crop in descending value order, decrement the required quantity by 3: $Q_n = Q_{\text{base}} - (n \times 3)$, where $n \in \{0, 1, \dots, \text{Tier} - 1\}$.
- **2.1.4 Unlock Consumption Rule**: Unlocking a seed packet in the Shop permanently unlocks it for purchase and consumes the exact specified quantities of harvested crops from `inventory.crops`.

### 2.2 Complete 20-Crop Progression & Unlock Registry

| Crop ID | Crop Name | Tier | Pos | Seed Cost | Growth Days | Harvest Value | Unlock Requirements (Crops Consumed) | Total Crops |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Tier 1 (3 Days Growth)** | | | | | | | | |
| `crop_wheat` | Wheat | Tier 1 | 1 | 1 Coin | 3 Days | 2 Coins | **Default Unlocked** | 0 |
| `crop_oat` | Oat | Tier 1 | 2 | 2 Coins | 3 Days | 4 Coins | `crop_wheat: 9` | 9 |
| `crop_barley` | Barley | Tier 1 | 3 | 3 Coins | 3 Days | 5 Coins | `crop_oat: 11` | 11 |
| `crop_radish` | Radish | Tier 1 | 4 | 3 Coins | 3 Days | 6 Coins | `crop_barley: 13` | 13 |
| **Tier 2 (4 Days Growth)** | | | | | | | | |
| `crop_carrot` | Carrot | Tier 2 | 1 | 4 Coins | 4 Days | 8 Coins | `crop_radish: 9`, `crop_wheat: 6` | 15 |
| `crop_beet` | Beet | Tier 2 | 2 | 5 Coins | 4 Days | 10 Coins | `crop_carrot: 11`, `crop_oat: 8` | 19 |
| `crop_potato` | Potato | Tier 2 | 3 | 6 Coins | 4 Days | 12 Coins | `crop_beet: 13`, `crop_barley: 10` | 23 |
| `crop_onion` | Onion | Tier 2 | 4 | 7 Coins | 4 Days | 14 Coins | `crop_potato: 15`, `crop_radish: 12` | 27 |
| **Tier 3 (5 Days Growth)** | | | | | | | | |
| `crop_corn` | Corn | Tier 3 | 1 | 9 Coins | 5 Days | 18 Coins | `crop_onion: 11`, `crop_carrot: 8`, `crop_wheat: 5` | 24 |
| `crop_peas` | Peas | Tier 3 | 2 | 10 Coins | 5 Days | 20 Coins | `crop_corn: 13`, `crop_beet: 10`, `crop_oat: 7` | 30 |
| `crop_tomato` | Tomato | Tier 3 | 3 | 12 Coins | 5 Days | 24 Coins | `crop_peas: 15`, `crop_potato: 12`, `crop_barley: 9` | 36 |
| `crop_eggplant` | Eggplant | Tier 3 | 4 | 14 Coins | 5 Days | 28 Coins | `crop_tomato: 17`, `crop_onion: 14`, `crop_radish: 11` | 42 |
| **Tier 4 (6 Days Growth)** | | | | | | | | |
| `crop_pumpkin` | Pumpkin | Tier 4 | 1 | 17 Coins | 6 Days | 34 Coins | `crop_eggplant: 13`, `crop_corn: 10`, `crop_carrot: 7`, `crop_wheat: 4` | 34 |
| `crop_squash` | Squash | Tier 4 | 2 | 19 Coins | 6 Days | 38 Coins | `crop_pumpkin: 15`, `crop_peas: 12`, `crop_beet: 9`, `crop_oat: 6` | 42 |
| `crop_sunflower` | Sunflower | Tier 4 | 3 | 22 Coins | 6 Days | 44 Coins | `crop_squash: 17`, `crop_tomato: 14`, `crop_potato: 11`, `crop_barley: 8` | 50 |
| `crop_cabbage` | Cabbage | Tier 4 | 4 | 25 Coins | 6 Days | 50 Coins | `crop_sunflower: 19`, `crop_eggplant: 16`, `crop_onion: 13`, `crop_radish: 10` | 58 |
| **Tier 5 (7 Days Growth)** | | | | | | | | |
| `crop_strawberry` | Strawberry | Tier 5 | 1 | 30 Coins | 7 Days | 60 Coins | `crop_cabbage: 15`, `crop_pumpkin: 12`, `crop_corn: 9`, `crop_carrot: 6`, `crop_wheat: 3` | 45 |
| `crop_blueberry` | Blueberry | Tier 5 | 2 | 35 Coins | 7 Days | 70 Coins | `crop_strawberry: 17`, `crop_squash: 14`, `crop_peas: 11`, `crop_beet: 8`, `crop_oat: 5` | 55 |
| `crop_watermelon` | Watermelon | Tier 5 | 3 | 42 Coins | 7 Days | 84 Coins | `crop_blueberry: 19`, `crop_sunflower: 16`, `crop_tomato: 13`, `crop_potato: 10`, `crop_barley: 7` | 65 |
| `crop_pineapple` | Pineapple | Tier 5 | 4 | 50 Coins | 7 Days | 100 Coins | `crop_watermelon: 21`, `crop_cabbage: 18`, `crop_eggplant: 15`, `crop_onion: 12`, `crop_radish: 9` | 75 |

### 2.3 Starter Profile Seed & Unlock Allocation
- **2.3.1 Starter Assets**: When a new player profile is created, the system allocates initial starter assets:
  - **Starter Coins**: `5 Coins`.
  - **Starter Seeds**: `3x Seed Packets of Wheat` (`crop_wheat`).
  - **Initial Unlocked Crops**: `["crop_wheat"]` stored in `unlockedCropIds`.
