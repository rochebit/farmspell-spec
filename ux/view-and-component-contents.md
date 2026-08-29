# View & Component Composition Specification

This document specifies how **FarmSpell** assembles modular UI components into complete user-facing views and interactive modal overlays. For detailed prop interfaces, state schemas, event handlers, and styling mappings for individual components, refer to their dedicated specifications in [components/](components/README.md).

## 1 View Composition Matrix

### 1.1 Main Farm Screen (`MainFarmView`)
- **1.1.1 Purpose**: The primary game screen where players manage their 5x5 farm plot grid, monitor energy and day progress, and initiate farming actions.
- **1.1.2 Layout Composition**:
  - **Header Region**: Contains [StatusBar](components/status-bar.md) anchored to the top of the viewport.
  - **Center Playfield**: Contains [FarmGrid](components/farm-grid.md) (rendering 25 [FarmPlotTile](components/farm-plot-tile.md) instances).
  - **Farmhouse Area**: Contains [FarmhouseControl](components/farmhouse-control.md) positioned adjacent to the grid.
  - **Side Gauge Region**: Contains [MasteryMeter](components/mastery-meter.md) docked along the screen edge to display granular spelling growth.
  - **Global Overlay Region**: Host container for [ToastNotification](components/toast-notification.md) alerts, the persistent bottom-corner [VersionBadge](components/version-badge.md), and the mobile [DebugLogOverlay](components/debug-log-overlay.md) (when active).
- **1.1.3 Child Modals & Drawers**:
  - [SeedPicker](components/seed-picker.md) (draws upward upon tapping an empty plot).
  - [SpellingChallengeModal](components/spelling-challenge-modal.md) (invoked on actionable plot interactions).
  - [ShopModal](components/shop-modal.md) (invoked from StatusBar shop button).

### 1.2 Spelling Challenge Learning Overlay (`SpellingChallengeModal`)
- **1.2.1 Purpose**: Full-screen/modal learning layer presented during plot planting, watering, harvesting, and clearing.
- **1.2.2 Component Specification**: Full input handling, audio playback, letter slots, attempt stars, flash hint card, and soft-keyboard visual viewport clearance are defined in [components/spelling-challenge-modal.md](components/spelling-challenge-modal.md).

### 1.3 Farm Market & Economy Modal (`ShopModal`)
- **1.3.1 Purpose**: Marketplace dialog enabling players to unlock new crop seeds, purchase seed packets with coins, and sell harvested crops.
- **1.3.2 Component Specification**: Full tab switcher, locked/unlocked seed card states, prerequisite crop checklists, and bulk selling mechanics are defined in [components/shop-modal.md](components/shop-modal.md).

### 1.4 Audio Recording Studio (`AudioStudioView`)
- **1.4.1 Purpose**: Dedicated view allowing children and parents to record custom word pronunciations for active spelling packs.
- **1.4.2 Component Specification**: Full word list navigator, recording state machine, 5.0-second countdown ring, and IndexedDB audio persistence are defined in [components/audio-studio-view.md](components/audio-studio-view.md).

### 1.5 Parent Management & Profile Switcher (`ProfileSwitcherView`)
- **1.5.1 Purpose**: Multi-profile selection screen, new child creation flow, and family sync code generator.
- **1.5.2 Child Components**:
  - [ParentalGateModal](components/parental-gate-modal.md) (adult math verification gate).
  - [ParentSettingsModal](components/parent-settings-modal.md) (daily action limits and curriculum assignment).
  - [WordListManagerModal](components/word-list-manager-modal.md) (custom word list editor).
  - [ShareCodeModal](components/share-code-modal.md) (15-minute 6-digit Join Code generator).
