# Component Specification: VersionBadge

The `VersionBadge` is a persistent UI element anchored in the bottom corner of the viewport (default: bottom-right). It displays the active build and commit version and acts as an interactive manual **Force Update** button to bust cache and reload the latest version on mobile/PWA devices.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `versionString`**: `string` – The build version identifier injected at build-time (e.g., `"v0.1.48 (b4e79a6)"`).
- **1.1.2 `position`**: `'bottom-left' | 'bottom-right'` – Screen anchor position (default: `'bottom-right'`).

### 1.2 Event Handlers
- **1.2.1 `onForceUpdate`**: `() => Promise<void>` – Triggered when the badge is tapped; purges `CacheStorage`, unregisters active Service Workers, and forces a cache-busting reload.

## 2 Contained Elements & Layout

### 2.1 Badge Layout
- **2.1.1 Text Display**: Monospace or clean sans-serif text string (font size: `11px`, weight: `600`).
- **2.1.2 Refresh Icon**: Subtle circular refresh glyph (`↻`) preceding the version text.
- **2.1.3 Positioning**: Fixed position pinned to viewport edge with `8px` offset (`bottom: 8px`, `right: 8px` or `left: 8px`), layered above background but behind modal overlays (`z-index: 10`).
- **2.1.4 Tactile Pill Container**: Rounded pill container (`padding: 4px 8px`, `border-radius: 9999px`) with semi-transparent background (`rgba(0, 0, 0, 0.25)` or `rgba(255, 255, 255, 0.5)`).
- **2.1.5 Interactive Touch Target**: Minimum hit area (`cursor: pointer`, `touch-action: manipulation`, `user-select: none`).

## 3 Visual States & Update Routine

### 3.1 States
- **3.1.1 Default State**: Subtle, low-opacity indicator (`opacity: 0.6`) ensuring zero distraction during standard gameplay.
- **3.1.2 Hover / Focus**: Opacity increases to `1.0` with subtle background glow.
- **3.1.3 Active / Pressed**: Slight scale down (`transform: scale(0.95)`).
- **3.1.4 Updating State**: Refresh icon spins continuously while CacheStorage is purged prior to page reload.
