# Component Specification: FarmhouseControl

The `FarmhouseControl` represents the interactive farmhouse structure on the farm screen. It provides the sleep/end-day action trigger for transitioning to the next in-game day.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `dayCount`**: `number` – Current in-game day number.
- **1.1.2 `currentActions`**: `number` – Remaining daily action points.
- **1.1.3 `maxActionsPerDay`**: `number` – Daily action point cap.

### 1.2 Event Handlers
- **1.2.1 `onTriggerSleep`**: `() => void` – Triggered when the player taps the farmhouse to go to sleep.

## 2 Contained Elements & Sub-Components

### 2.1 Farmhouse Graphic Structure
- **2.1.1 Cozy Farmhouse Visual**: Illustrated red-roof cottage with chimney and glowing windows.
- **2.1.2 Day Number Banner**: Plaque above the door displaying `Day ${dayCount}`.

### 2.2 Bedtime Prompt & Indicators
- **2.2.1 Action Points Depleted State (`currentActions == 0`)**:
  - Chimney emits gentle sleep "Zzz" particles.
  - Windows glow with warm golden bedtime light.
  - Prominent bouncing badge: "Tap House to Sleep & Start Day ${dayCount + 1}!".
- **2.2.2 Action Points Remaining State (`currentActions > 0`)**:
  - Subdued day mode with subtle smoke puff.
  - Tapping displays a confirmation dialog: "You still have ${currentActions} energy points left today! Go to sleep anyway?".

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Default**: Warm illustrated cottage asset.
- **3.1.2 Hover / Touch Down**: Scale transition `1.03` over `150ms`.
- **3.1.3 Bedtime Glow**: Golden outline pulse (`box-shadow: 0 0 16px var(--color-coin-gold)`) when energy is depleted.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/farmhouse-control.html](mocks/farmhouse-control.html) demonstrates the farmhouse in both Day Mode (active AP remaining) and Bedtime Night Mode (energy depleted with Zzz particles).
