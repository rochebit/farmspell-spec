# Game Rules: Day Cycle and Daily Actions

This document defines the rules governing the in-game Day Cycle and Daily Action Point system for **FarmSpell**.

## 1 In-Game Day Cycle

### 1.1 Day Progression Rules
- **1.1.1**: The game advances on an in-game calendar measured in discrete integer **Days** (Day 1, Day 2, Day 3, ...).
- **1.1.2**: Time does NOT progress automatically based on real-world clock time. The Day changes strictly when the player explicitly triggers the **End Day / Go to Sleep** action.
- **1.1.3**: The **End Day / Go to Sleep** action is accessible from the Farmhouse interface on the farm view.

### 1.2 End-Day Processing Sequence
- **1.2.1**: When the player triggers **End Day / Go to Sleep**, the system executes the following steps in sequence:
  - **1.2.1.1**: Evaluate all planted crops on the farm grid for watering status (see [crop-growth-and-withering.md](crop-growth-and-withering.md)).
  - **1.2.1.2**: Increment the global `currentDay` integer counter by 1.
  - **1.2.1.3**: Reset the player's `dailyActionsRemaining` counter to `maxDailyActions`.
  - **1.2.1.4**: Render a "Good Morning! Day X" transition screen displaying a summary of crops ready for harvest.

## 2 Daily Action Points

### 2.1 Action Allocation Rules
- **2.1.1**: The player is allocated a fixed number of **Action Points** per in-game day (default `maxDailyActions = 10`).
- **2.1.2**: Action Points represent the physical stamina available to perform farm work on that day.
- **2.1.3**: Unused Action Points at the end of a day DO NOT roll over to the next day.

### 2.2 Action Costs & Spelling Requirements
- **2.2.1**: Planting a seed on an empty plot costs **1 Action Point** + **1 Spelling Challenge**.
- **2.2.2**: Watering a planted crop costs **1 Action Point** + **1 Spelling Challenge**.
- **2.2.3**: Harvesting a mature crop costs **1 Action Point** + **1 Spelling Challenge**.
- **2.2.4**: Clearing a withered crop costs **1 Action Point** + **1 Spelling Challenge**.
- **2.2.5**: Accessing the Shop to buy seeds or sell crops costs **0 Action Points** and does NOT require a spelling challenge.

### 2.3 Zero-Action Exhaustion State
- **2.3.1**: When `dailyActionsRemaining` reaches 0, the player CANNOT perform further planting, watering, or harvesting actions.
- **2.3.2**: When at 0 Action Points, attempting to perform a farm action displays a friendly visual prompt pointing to the Farmhouse: *"Out of energy for today! Tap the Farmhouse to go to sleep."*
