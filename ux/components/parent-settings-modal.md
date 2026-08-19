# Component Specification: ParentSettingsModal

The `ParentSettingsModal` allows parents to configure gameplay rules, adjust daily energy limits, select curriculum word packs, and manage linked parent accounts.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `profile`**: `PlayerProfileDocument` – Current child profile data.
- **1.1.3 `availableWordPacks`**: `Array<WordPackMetadata>` – List of selectable curriculum packs.

### 1.2 Event Handlers
- **1.2.1 `onUpdateSettings`**: `(updatedSettings: Partial<PlayerSettings>) => Promise<void>` – Saves changes to Firestore.
- **1.2.2 `onOpenShareCode`**: `() => void` – Opens the Share Code generator modal.
- **1.2.3 `onOpenProfileSwitcher`**: `() => void` – Opens the Child Profile switcher.
- **1.2.4 `onClose`**: `() => void` – Closes the modal.

## 2 Contained Elements & Sub-Components

### 2.1 Child Profile Overview
- **2.1.1 Profile Banner**: Displays child name, avatar, and total farm days completed.
- **2.1.2 Profile Switcher Button**: Action button to switch to another child or add a new child profile.

### 2.2 Gameplay Settings Section
- **2.2.1 Daily Action Limit Slider / Stepper**:
  - Setting for `maxActionsPerDay` (range: `5` to `20`, default `10`).
  - Helper note: "Controls how many farming actions your child can take each day before bedtime."
- **2.2.2 Active Spelling Word Pack Selector**:
  - Dropdown selecting grade levels (Kindergarten, 1st Grade, 2nd Grade, Custom List).
  - Preview button to view words in selected pack.

### 2.3 Account & Co-Parent Linking
- **2.3.1 Linked Parent Accounts Count**: Badge indicating `authorizedParentUids.length` linked parents.
- **2.3.2 "Link Another Parent" Button**: Action opening the [ShareCodeModal](share-code-modal.md).

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Default**: Organized card layout inside glassmorphic modal body (`--radius-lg`).
- **3.1.2 Save Pending / Loading**: Button shows spinner while writing updates to Firestore.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/parent-settings-modal.html](mocks/parent-settings-modal.html) demonstrates parent settings with daily AP limit stepper and curriculum selector.
