# Component Specification: ProfileSwitcherView

The `ProfileSwitcherView` provides an interface for selecting an active child profile, creating a new child profile, or redeeming a share code to link a co-parent's child.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – View visibility state.
- **1.1.2 `profiles`**: `Array<PlayerProfileDocument>` – All child profiles linked to the current parent.
- **1.1.3 `activePlayerId`**: `string` – Currently active profile ID.

### 1.2 Event Handlers
- **1.2.1 `onSelectProfile`**: `(playerId: string) => void` – Sets active profile and switches farm view.
- **1.2.2 `onCreateProfile`**: `(name: string, avatarId: string) => Promise<void>` – Creates new child profile document in Firestore.
- **1.2.3 `onRedeemShareCode`**: `(code: string) => Promise<void>` – Links existing profile using 6-digit code.
- **1.2.4 `onClose`**: `() => void` – Returns to game.

## 2 Contained Elements & Sub-Components

### 2.1 Profile Grid List
- **2.1.1 Child Profile Card**: For each profile in `profiles`:
  - **Avatar Badge**: Visual icon (`56px × 56px`).
  - **Name Label**: Child name (font: `Heading2`).
  - **Stats Pill**: `Day ${dayCount}` | `${coins} Coins`.
  - **Active Indicator**: Green checkmark badge if `playerId == activePlayerId`.
  - **Select Button**: Primary action button to load this child's farm.

### 2.2 Add Profile & Share Code Action Row
- **2.2.1 "Add Child Profile" Card**: Plus button card opening profile creation modal (Name input + avatar icon selector).
- **2.2.2 "Redeem Share Code" Button**: Button opening the 6-digit code entry dialog.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Active Profile Card**: High-contrast outline with `--color-farm-green-main` and active glow.
- **3.1.2 Card Press**: 3D bevel depression on tap.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/profile-switcher-view.html](mocks/profile-switcher-view.html) demonstrates multi-child profile switcher cards, active badges, and add profile trigger.
