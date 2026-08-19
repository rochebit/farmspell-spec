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
    "seedCost",
    "requiredGrowthDays",
    "harvestSellPrice",
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
      "description": "Progression tier rating"
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
    "iconIdentifier": {
      "type": "string",
      "description": "Sprite / asset identifier for rendering plot and inventory icons"
    }
  }
}
```

## 2 Default System Crop Catalog

### 2.1 Complete 20-Crop Progression Registry

| Crop ID | Crop Name | Tier | Seed Cost | Growth Days Required | Harvest Sell Price | Net Profit per Crop | Profit Per Day |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Tier 1 Crops (3 Days Growth)** | | | | | | | |
| `crop_wheat` | Wheat | Tier 1 | 1 Coin | 3 Days | 2 Coins | +1 Coin | +0.33 Coins/Day |
| `crop_oat` | Oat | Tier 1 | 2 Coins | 3 Days | 4 Coins | +2 Coins | +0.67 Coins/Day |
| `crop_barley` | Barley | Tier 1 | 3 Coins | 3 Days | 5 Coins | +2 Coins | +0.67 Coins/Day |
| `crop_radish` | Radish | Tier 1 | 3 Coins | 3 Days | 6 Coins | +3 Coins | +1.00 Coins/Day |
| **Tier 2 Crops (4 Days Growth)** | | | | | | | |
| `crop_carrot` | Carrot | Tier 2 | 4 Coins | 4 Days | 8 Coins | +4 Coins | +1.00 Coins/Day |
| `crop_beet` | Beet | Tier 2 | 5 Coins | 4 Days | 10 Coins | +5 Coins | +1.25 Coins/Day |
| `crop_potato` | Potato | Tier 2 | 6 Coins | 4 Days | 12 Coins | +6 Coins | +1.50 Coins/Day |
| `crop_onion` | Onion | Tier 2 | 7 Coins | 4 Days | 14 Coins | +7 Coins | +1.75 Coins/Day |
| **Tier 3 Crops (5 Days Growth)** | | | | | | | |
| `crop_corn` | Corn | Tier 3 | 9 Coins | 5 Days | 18 Coins | +9 Coins | +1.80 Coins/Day |
| `crop_peas` | Peas | Tier 3 | 10 Coins | 5 Days | 20 Coins | +10 Coins | +2.00 Coins/Day |
| `crop_tomato` | Tomato | Tier 3 | 12 Coins | 5 Days | 24 Coins | +12 Coins | +2.40 Coins/Day |
| `crop_eggplant` | Eggplant | Tier 3 | 14 Coins | 5 Days | 28 Coins | +14 Coins | +2.80 Coins/Day |
| **Tier 4 Crops (6 Days Growth)** | | | | | | | |
| `crop_pumpkin` | Pumpkin | Tier 4 | 17 Coins | 6 Days | 34 Coins | +17 Coins | +2.83 Coins/Day |
| `crop_squash` | Squash | Tier 4 | 19 Coins | 6 Days | 38 Coins | +19 Coins | +3.17 Coins/Day |
| `crop_sunflower` | Sunflower | Tier 4 | 22 Coins | 6 Days | 44 Coins | +22 Coins | +3.67 Coins/Day |
| `crop_cabbage` | Cabbage | Tier 4 | 25 Coins | 6 Days | 50 Coins | +25 Coins | +4.17 Coins/Day |
| **Tier 5 Crops (7 Days Growth)** | | | | | | | |
| `crop_strawberry` | Strawberry | Tier 5 | 30 Coins | 7 Days | 60 Coins | +30 Coins | +4.29 Coins/Day |
| `crop_blueberry` | Blueberry | Tier 5 | 35 Coins | 7 Days | 70 Coins | +35 Coins | +5.00 Coins/Day |
| `crop_watermelon` | Watermelon | Tier 5 | 42 Coins | 7 Days | 84 Coins | +42 Coins | +6.00 Coins/Day |
| `crop_pineapple` | Pineapple | Tier 5 | 50 Coins | 7 Days | 100 Coins | +50 Coins | +7.14 Coins/Day |

### 2.2 Starter Profile Seed Allocation
- **2.2.1**: When a new player profile is created, the system allocates initial starter assets into `inventory`:
  - **Starter Coins**: `5 Coins`.
  - **Starter Seeds**: `3x Seed Packets of Wheat` (`crop_wheat`).
