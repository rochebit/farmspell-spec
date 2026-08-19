# Component Specification: SeedPicker

The `SeedPicker` is a bottom-sheet drawer or modal that opens when the player taps an empty farm plot, allowing them to choose which seed from their inventory to plant.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Visibility state of the drawer.
- **1.1.2 `targetPlotIndex`**: `number` – Index of the empty plot selected for planting.
- **1.1.3 `seedInventory`**: `Record<CropType, number>` – Map of owned seed quantities per crop type.
- **1.1.4 `cropDefinitions`**: `Record<CropType, CropMetadata>` – Registry containing name, icon, growth days, and sell price.

### 1.2 Event Handlers
- **1.2.1 `onSelectSeed`**: `(cropType: CropType) => void` – Triggered when a seed packet is selected for planting.
- **1.2.2 `onOpenShop`**: `() => void` – Shortcut action opening the Shop modal if seeds are needed.
- **1.2.3 `onClose`**: `() => void` – Triggered when closing the picker without selecting a seed.

## 2 Contained Elements & Sub-Components

### 2.1 Header Section
- **2.1.1 Title**: "Choose a Seed to Plant" (font: `Heading2`).
- **2.1.2 Close Button**: Circular dismiss button (`44px × 44px`) with `X` icon.

### 2.2 Seed Card List
- **2.2.1 Seed Card**: For each crop type in catalog:
  - **Seed Packet Graphic**: Distinct color-coded seed pouch icon.
  - **Crop Name**: Text label (e.g., "Carrot", "Tomato").
  - **Owned Quantity Badge**: Count badge displaying `x${quantity}` owned.
  - **Growth Duration Info**: Clock icon + `${growthDays} Days`.
  - **Harvest Value Info**: Coin icon + `${sellPrice} Coins`.
  - **Plant Button**: Action button with 3D tactile press feel.
- **2.2.2 Empty State Notice**: If all seed quantities are `0`:
  - Visual empty basket illustration.
  - Text: "You're out of seeds!".
  - "Visit the Shop" primary action button.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Available Seed Card**: Border `--color-soil-border`, enabled Plant button with `--color-farm-green-main`.
- **3.1.2 Zero Quantity Seed Card**: Opacity `0.5`, Plant button disabled and replaced with "Buy in Shop" link.
- **3.1.3 Drawer Transition**: Smooth slide-up transition (`transform: translateY(0)` over `300ms`).

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/seed-picker.html](mocks/seed-picker.html) demonstrates the slide-up drawer with seed cards, owned quantity badges, growth duration pills, and shop links.
