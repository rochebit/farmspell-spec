# UI Components Specification Index

This directory contains individual specification documents for every reusable UI component and view in **FarmSpell**. Each component specification defines:
1. Exact functional inputs/props and state.
2. Contained data elements and child components.
3. User interaction handlers and event triggers.
4. Visual states (`Default`, `Hover`, `Pressed`, `Disabled`, `Success`, `Error`).
5. Design tokens and styling mappings inherited from [ux/styling.md](../styling.md).

## 1 Component Inventory

### 1.1 Navigation & Global
- **[status-bar.md](status-bar.md)** – Top status bar displaying child profile, coin total, daily Action Point meter, day count, and navigation shortcuts.
- **[toast-notification.md](toast-notification.md)** – Non-blocking feedback toast notifications with auto-dismiss timers.

### 1.2 Farm & Environment
- **[farm-grid.md](farm-grid.md)** – 5x5 farm grid container managing 25 plot tile instances.
- **[farm-plot-tile.md](farm-plot-tile.md)** – Individual farm plot tile rendering empty soil, growing crops, moisture, harvest glow, and withered states.
- **[farmhouse-control.md](farmhouse-control.md)** – Farmhouse structure and day advancement bedtime trigger.
- **[seed-picker.md](seed-picker.md)** – Seed selection drawer allowing players to choose seeds to sow.

### 1.3 Spelling Challenge
- **[spelling-challenge-modal.md](spelling-challenge-modal.md)** – Modal overlay with audio replay, letter slot boxes, native keyboard listener, attempt counter, and 2-attempt flash hint card.

### 1.4 Economy & Inventory
- **[shop-modal.md](shop-modal.md)** – Modal containing seed purchasing storefront tab and instant crop selling shelf tab.

### 1.5 Audio Studio
- **[audio-studio-view.md](audio-studio-view.md)** – Audio recording studio view with word browser, microphone recorder, 5.0s countdown ring, and TTS fallback controls.

### 1.6 Profile & Parental Controls
- **[word-list-manager-modal.md](word-list-manager-modal.md)** – Custom word list editor, tag-style chip management, and curriculum activation toggles.
- **[parental-gate-modal.md](parental-gate-modal.md)** – Adult math challenge gate preventing accidental access to settings.
- **[parent-settings-modal.md](parent-settings-modal.md)** – Settings modal for daily action limits, word pack curriculum assignment, and co-parent linking.
- **[profile-switcher-view.md](profile-switcher-view.md)** – Multi-child profile switcher and profile creation interface.
- **[share-code-modal.md](share-code-modal.md)** – 6-digit Join Code generator with 15-minute countdown timer.

## 2 Interactive HTML Component Mocks

Interactive visual HTML/CSS mocks demonstrating component rendering, 3D button depression, and animations are located in the `mocks/` folder:
- **[mocks/index.html](mocks/index.html)** – Master interactive component showcase and gallery.
- **[mocks/status-bar.html](mocks/status-bar.html)** – Live visual mock of the HUD StatusBar.
- **[mocks/toast-notification.html](mocks/toast-notification.html)** – Live visual mock of floating toast feedback alerts.
- **[mocks/farm-plot-tile.html](mocks/farm-plot-tile.html)** – Live visual mock showcasing all 6 plot lifecycle states (`Empty`, `Planted`, `Growing`, `Ready`, `Withered`).
- **[mocks/farm-grid.html](mocks/farm-grid.html)** – Live visual mock of the full 5x5 farm grid.
- **[mocks/farmhouse-control.html](mocks/farmhouse-control.html)** – Live visual mock of the Farmhouse in Day and Bedtime modes.
- **[mocks/seed-picker.html](mocks/seed-picker.html)** – Live visual mock of the Seed Selection Drawer.
- **[mocks/spelling-challenge-modal.html](mocks/spelling-challenge-modal.html)** – Live visual mock of the Spelling Challenge learning overlay.
- **[mocks/shop-modal.html](mocks/shop-modal.html)** – Live visual mock of the Shop modal with Buy Seeds and Sell Crops tabs.
- **[mocks/audio-studio-view.html](mocks/audio-studio-view.html)** – Live visual mock of the Voice Recording Studio and 5s timer gauge.
- **[mocks/word-list-manager-modal.html](mocks/word-list-manager-modal.html)** – Live visual mock of the custom word list editor and curriculum manager.
- **[mocks/parental-gate-modal.html](mocks/parental-gate-modal.html)** – Live visual mock of the adult math challenge keypad.
- **[mocks/parent-settings-modal.html](mocks/parent-settings-modal.html)** – Live visual mock of parent settings and AP limit stepper.
- **[mocks/profile-switcher-view.html](mocks/profile-switcher-view.html)** – Live visual mock of multi-child profile switcher cards.
- **[mocks/share-code-modal.html](mocks/share-code-modal.html)** – Live visual mock of the 6-digit Join Code display and countdown timer.
- **[mocks/styles.css](mocks/styles.css)** – Shared CSS design tokens, typography, and tactile button classes.
