# Component Specification: MasteryMeter

The `MasteryMeter` is a tactile, vertical garden growth gauge docked along the side of the main farm view. It provides continuous, granular visual feedback on player spelling progress across their active word list without cluttering the screen with numbers or percentages.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `activeWordCount`**: `number` – Total number of words in the active spelling curriculum ($N$).
- **1.1.2 `spellingStats`**: `Record<string, WordStats>` – Map of active words to their stats (`correctCount`, `streak`, `incorrectCount`).
- **1.1.3 `position`**: `'left' | 'right'` – Screen dock position (default: `'right'`).

### 1.2 Derived Calculations
- **1.2.1 Granular Progress Formula**:
  - Each word contributes points based on its current streak $S \in [0, 3]$ (where streak 3 = Mastered):
    $$\text{Word Points}_i = \min(S_i, 3)$$
  - Total Maximum Points $= 3 \times N$.
  - Fill Ratio:
    $$\text{Fill Ratio} = \frac{\sum_{i=1}^N \min(S_i, 3)}{3 \times N}$$
  - *Example*: In a 10-word list (30 total points), every correct first-try spelling immediately lifts the gauge by $1/30$ (3.3%), a 2-streak word contributes $2/30$ (6.7%), and a fully mastered word contributes $3/30$ (10.0%).

## 2 Contained Elements & Layout

### 2.1 Gauge Structure
- **2.1.1 Docked Container**: Slim vertical capsule container anchored to the side of the screen (`top: 50%`, `transform: translateY(-50%)`, width: `36px`, height: `260px`, `z-index: 10`).
- **2.1.2 Planter Base Foot**: Carved wooden/soil planter base (`--color-soil-wet`, `--color-soil-border` outline, `box-shadow: 0 4px 0 #2D1A14`) featuring an embedded sprout icon (`🌱`).
- **2.1.3 Glassmorphic Fluid Channel**: Translucent capsule tube (`background: rgba(255, 253, 249, 0.75)`, `backdrop-filter: blur(8px)`, `border: 3px solid var(--color-soil-border)`, `--radius-full`).
- **2.1.4 Fluid Growth Fill**:
  - Vertical liquid bar rising from bottom to top based on `Fill Ratio` (`transition: height 600ms var(--ease-spring)`).
  - Gradient: `--color-sprout-yellow-green` (`#8BC34A`) at base transitioning to `--color-farm-green-main` (`#4CAF50`) at the surface.
  - Glowing Meniscus: A bright 3px glowing cap (`#A5D6A7`) at the liquid surface.
- **2.1.5 Golden Star Topper**:
  - 3D wooden/gold star badge (`--color-coin-gold`) mounted atop the gauge pillar.
  - **Default**: Soft warm amber star.
  - **100% Mastered State**: Star illuminates bright yellow (`--color-star-yellow`), emits a pulsing radial glow (`box-shadow: 0 0 16px var(--color-coin-gold)`), and triggers a star burst particle animation.

## 3 Visual States & Micro-Interactions

### 3.1 States
- **3.1.1 Normal Progression**: Fluid height smoothly springs upward after each successful challenge submission.
- **3.1.2 100% Mastery Celebration**: When `Fill Ratio == 1.0`, triggers celebratory chime and top star sparkle pulse.
- **3.1.3 Non-Intrusive Ambience**: Operates as a purely visual HUD element with no text clutter, matching the cozy tactile farm aesthetic.
