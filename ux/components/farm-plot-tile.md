# Component Specification: FarmPlotTile

The `FarmPlotTile` represents an individual 1x1 interactive farming plot on the 5x5 farm grid. It displays soil moisture, crop growth stages, ready-to-harvest rewards, and withered conditions.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `plotIndex`**: `number` – Fixed index of the plot on the grid (`0` to `24`).
- **1.1.2 `state`**: `PlotState` – Enum: `'Empty' | 'Planted' | 'Growing' | 'ReadyToHarvest' | 'Withered'`.
- **1.1.3 `cropType`**: `CropType | null` – Type of crop occupying the plot (`'Carrot' | 'Tomato' | 'Corn' | 'Strawberry' | 'Pumpkin' | null`).
- **1.1.4 `growthStage`**: `number` – Current growth stage integer (e.g., `0` to `totalGrowthDays`).
- **1.1.5 `totalGrowthDays`**: `number` – Total days required for the crop to reach harvest maturity.
- **1.1.6 `isWateredToday`**: `boolean` – Whether the plot has been watered during the current in-game day.
- **1.1.7 `hasActionsRemaining`**: `boolean` – Whether `currentActions > 0`.

### 1.2 Event Handlers
- **1.2.1 `onPlotTap`**: `(plotIndex: number) => void` – Triggered when the player taps or clicks the plot tile.

## 2 Contained Elements & Sub-Components

### 2.1 Soil Base Layer
- **2.1.1 Dry Soil Surface**: Rendered with `--color-soil-dry` when `isWateredToday == false`.
- **2.1.2 Moist/Watered Soil Surface**: Rendered with `--color-soil-wet` when `isWateredToday == true`, accompanied by subtle water sparkle particles.
- **2.1.3 Plot Border**: High-contrast outline (`--color-soil-border`, radius `--radius-lg`).

### 2.2 Crop Visual Layer
- **2.2.1 Empty State**: Bare tilled soil with subtle seed depression placeholder.
- **2.2.2 Planted State**: Small green sprout poking through the soil surface.
- **2.2.3 Growing State**: Scaled crop asset matching `growthStage / totalGrowthDays` progression ratio.
- **2.2.4 ReadyToHarvest State**: Fully mature, vibrant crop asset with golden star glow (`--color-coin-gold`) and gentle pulsing scale animation.
- **2.2.5 Withered State**: Wilted, desaturated brown-gray crop graphic (`--color-withered-gray`) with cracked soil texture.

### 2.3 Status Badges & Overlays
- **2.3.1 Growth Stage Pill**: Sub-badge displaying `Stage ${growthStage}/${totalGrowthDays}` for growing crops.
- **2.3.2 Water Drop Prompt**: Floating droplet icon over unwatered growing crops.
- **2.3.3 Harvest Basket Prompt**: Bouncing basket icon over `ReadyToHarvest` crops.

## 3 Visual States & Styling Mappings

### 3.1 Interaction States
- **3.1.1 Default**: Static tile rendering current soil and crop layers.
- **3.1.2 Tap Down / Pressed**: Scale down to `0.95` (`transform: scale(0.95)`) over `100ms`.
- **3.1.3 Ready-to-Harvest**: Continuous gentle pulse animation (scale `1.0` -> `1.06` -> `1.0` over `1500ms`).
- **3.1.4 Already Watered Toast Trigger**: If tapped when `isWateredToday == true` on a growing crop, does not trigger spelling; triggers informational toast.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/farm-plot-tile.html](mocks/farm-plot-tile.html) demonstrates all 6 lifecycle states with CSS gradients, moisture badges, and harvest pulse effects.
