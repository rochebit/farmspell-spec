# Component Specification: ShopModal

The `ShopModal` provides the marketplace interface where players purchase new seed packets with earned coins and sell their harvested crops for instant coin payouts.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `coins`**: `number` – Current coin balance.
- **1.1.3 `seedInventory`**: `Record<CropType, number>` – Current seed quantities.
- **1.1.4 `harvestedCropInventory`**: `Record<CropType, number>` – Quantities of harvested crops held.
- **1.1.5 `cropCatalog`**: `Record<CropType, CropMetadata>` – Prices, growth durations, and sell values.

### 1.2 Event Handlers
- **1.2.1 `onBuySeed`**: `(cropType: CropType) => void` – Purchase 1 seed packet (0 AP cost, 0 spelling).
- **1.2.2 `onSellCrop`**: `(cropType: CropType) => void` – Sell all units of one crop type (0 AP cost, 0 spelling).
- **1.2.3 `onSellAllCrops`**: `() => void` – Bulk sell all harvested crops in inventory.
- **1.2.4 `onClose`**: `() => void` – Closes the Shop modal.

## 2 Contained Elements & Sub-Components

### 2.1 Modal Header & Tab Navigation
- **2.1.1 Shop Title**: "Farm Market & Shop" (font: `Heading1`).
- **2.1.2 Live Coin Display**: Coin counter badge with `--color-coin-gold` background.
- **2.1.3 Tab Switcher**: Segmented toggle buttons ("Buy Seeds" vs "Sell Crops").
- **2.1.4 Dismiss Button**: Top-right `X` close button.

### 2.2 Seed Storefront Tab (`Buy Seeds`)
- **2.2.1 Seed Shelf Grid**: Grid of seed cards for all 5 crops:
  - **Seed Packet Graphic & Name**: Crop icon and title.
  - **Cost Badge**: Coin icon + `${seedCost} Coins`.
  - **Stats Pill**: `${growthDays} Days Growth` | `${sellPrice} Coin Value`.
  - **Owned Badge**: `You own: ${seedInventory[crop]}`.
  - **Buy 1 Button**: Green 3D button (disabled with gray styling if `coins < seedCost`).

### 2.3 Crop Selling Shelf Tab (`Sell Crops`)
- **2.3.1 Inventory Crop List**: Cards for each crop held in `harvestedCropInventory`:
  - **Crop Graphic & Name**: Harvest icon and title.
  - **Quantity Held**: `x${count}` harvested crops.
  - **Payout Calculation**: `${count} × ${unitPrice} = ${count * unitPrice} Coins`.
  - **Sell Single Crop Button**: Instant sell action button.
- **2.3.2 Sell All Footer**:
  - Total calculation banner: "Total Harvest Value: ${totalValue} Coins".
  - "Sell All Crops" large gold action button (`--color-coin-gold`).
  - Empty Inventory Notice if player holds 0 harvested crops.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Buy Button Affordability**: Enabled green button when `coins >= cost`; disabled gray button when `coins < cost`.
- **3.1.2 Sell Success Animation**: Coin ding audio effect and coin particle counter increment.
- **3.1.3 Modal Container**: Glassmorphic overlay with `--radius-lg` and `--color-surface-card` body.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/shop-modal.html](mocks/shop-modal.html) demonstrates the marketplace modal with Buy Seeds / Sell Crops tab bar, seed catalog cards, and bulk sell-all calculation bar.
