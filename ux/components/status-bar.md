# Component Specification: StatusBar

The `StatusBar` is a persistent top-level navigation and HUD bar rendered across the main game view. It communicates current player status, currency, energy, and day progress while providing navigation triggers.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `playerName`**: `string` – Name of the active child player.
- **1.1.2 `avatarId`**: `string` – Identifier for the active child avatar icon.
- **1.1.3 `coins`**: `number` – Current player coin balance (non-negative integer).
- **1.1.4 `currentActions`**: `number` – Remaining daily action points (integer `0` to `maxActionsPerDay`).
- **1.1.5 `maxActionsPerDay`**: `number` – Daily action point cap (default `10`).
- **1.1.6 `dayCount`**: `number` – Current in-game day number (1-indexed integer).
- **1.1.7 `isAudioMuted`**: `boolean` – Master audio mute state.

### 1.2 Event Handlers
- **1.2.1 `onOpenProfileSettings`**: `() => void` – Triggered when the player profile pill is tapped.
- **1.2.2 `onToggleAudio`**: `() => void` – Triggered when the audio mute toggle is tapped.
- **1.2.3 `onOpenShop`**: `() => void` – Triggered when the Shop launcher button is tapped.
- **1.2.4 `onOpenAudioStudio`**: `() => void` – Triggered when the Audio Studio launcher button is tapped.

## 2 Contained Elements & Sub-Components

### 2.1 Profile Pill
- **2.1.1 Avatar Graphic**: Circular badge rendering `avatarId` icon (dimensions: `44px × 44px`).
- **2.1.2 Player Name Label**: Text displaying `playerName` (font: `BodyRegular`, weight: `700`).

### 2.2 Coin Balance Pill
- **2.2.1 Coin Icon**: Gold coin graphic (`--color-coin-gold`).
- **2.2.2 Coin Count Label**: Formatted number string using tabular numbers (`font-variant-numeric: tabular-nums`).

### 2.3 Action Energy Gauge
- **2.3.1 Energy Icon**: Water droplet / lightning energy symbol (`--color-sky-blue`).
- **2.3.2 Action Text**: Fraction string formatted as `${currentActions} / ${maxActionsPerDay}`.
- **2.3.3 Low Energy State**: Pulsing warning badge (`--color-feedback-warning`) when `currentActions == 0`.

### 2.4 Day Counter Pill
- **2.4.1 Calendar Icon**: Sun / calendar graphic.
- **2.4.2 Day Text**: String formatted as `Day ${dayCount}`.

### 2.5 Navigation Action Buttons
- **2.5.1 Audio Studio Button**: Microphone icon with 3D tactile press feel.
- **2.5.2 Shop Button**: Barn / store basket icon with 3D tactile press feel.
- **2.5.3 Audio Mute Toggle Button**: Speaker icon reflecting `isAudioMuted` state.

## 3 Visual States & Styling Mappings

### 3.1 Component States
- **3.1.1 Default**: Semi-translucent glassmorphic header bar (`backdrop-filter: blur(8px)`, background: `rgba(255, 253, 249, 0.85)`).
- **3.1.2 Button Hover / Focus**: Scale `1.05` transition over `150ms`.
- **3.1.3 Button Active / Pressed**: 3D bevel depression (`translateY(3px)`).
- **3.1.4 Zero Action Alert**: Subtly pulses Action Energy Gauge with `--color-feedback-warning` outline.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/status-bar.html](mocks/status-bar.html) demonstrates the glassmorphic HUD bar, avatar pill, tabular coin counter, AP droplet gauge, and 3D tactile icon buttons.
