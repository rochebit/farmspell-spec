# Game Rules: Crop Growth and Withering

This document defines the rules for farm plots, crop growth requirements, daily watering logic, and withering penalties for **FarmSpell**.

## 1 Farm Grid & Plot States

### 1.1 Farm Grid Dimensions
- **1.1.1**: The farm consists of a fixed **5x5 grid** containing 25 Farm Plots.
- **1.1.2**: All 25 plots are unlocked and available to the player from the start of the game.
- **1.1.3**: The data architecture MUST support future grid size expansion (e.g., to 6x6 or 7x7) without breaking existing save file schemas.

### 1.2 Plot Lifecycle States
- **1.2.1**: Each Farm Plot exists in exactly one of the following states:
  - **Empty**: Unplanted soil ready for seed planting.
  - **Planted**: Seed has been sown today; requires initial watering.
  - **Growing**: Crop is actively growing; tracks `wateredDaysCount` and `consecutiveUnwateredDays`.
  - **ReadyToHarvest**: Crop has reached full maturity (`wateredDaysCount == requiredDaysToGrow`); ready for harvest.
  - **Withered**: Crop has died due to neglect (2 consecutive unwatered days); must be cleared before replanting.

## 2 Daily Watering & Growth Progress

### 2.1 Daily Growth Rules
- **2.1.1**: Every crop type defines a required number of growth days (`requiredDaysToGrow`, e.g., Carrot = 2 days, Wheat = 3 days, Pumpkin = 5 days).
- **2.1.2**: A crop ONLY gains 1 day of growth progress on an in-game day IF it was watered on that day.
- **2.1.3**: Watering a crop can only be done ONCE per crop per day. Attempting to water an already watered crop displays: *"Already watered today!"* without consuming an Action Point or word prompt.

### 2.2 End-Day Growth Evaluation Algorithm
- **2.2.1**: During end-day processing (when the player sleeps), every non-empty plot is evaluated:
  - **2.2.1.1**: **If Watered Today**:
    - Increment `wateredDaysCount` by 1.
    - Reset `consecutiveUnwateredDays` to 0.
    - If `wateredDaysCount >= requiredDaysToGrow`, transition plot state to `ReadyToHarvest`.
  - **2.2.1.2**: **If NOT Watered Today (1 Day Missed)**:
    - `wateredDaysCount` remains unchanged (growth pauses for that day).
    - Increment `consecutiveUnwateredDays` by 1.
    - Plot state remains `Growing`.
  - **2.2.1.3**: **If NOT Watered Today (2 Consecutive Days Missed)**:
    - If `consecutiveUnwateredDays >= 2`, transition plot state to `Withered`.
    - Growth progress is lost.

## 3 Clearing Withered Crops

### 3.1 Clearing Rules
- **3.1.1**: Tapping a `Withered` plot prompts the player: *"Clear withered crop?"*.
- **3.1.2**: Confirming clearing costs **1 Action Point** and resets the plot state to `Empty`.
- **3.1.3**: Clearing a withered crop requires **1 Spelling Challenge**.
