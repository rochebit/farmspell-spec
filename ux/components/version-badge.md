# Component Specification: VersionBadge

The `VersionBadge` is a lightweight, persistent UI element anchored in the bottom corner of the viewport (default: bottom-left or bottom-right) that displays the active build and commit version of the running application.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `versionString`**: `string` – The build version identifier injected at build-time (e.g., `"v0.1.48 (b4e79a6)"`).
- **1.1.2 `position`**: `'bottom-left' | 'bottom-right'` – Screen anchor position (default: `'bottom-right'`).

## 2 Contained Elements & Layout

### 2.1 Badge Layout
- **2.1.1 Text Display**: Monospace or clean sans-serif text string (font size: `11px`, weight: `600`).
- **2.1.2 Positioning**: Fixed position pinned to viewport edge with `8px` offset (`bottom: 8px`, `right: 8px` or `left: 8px`), layered behind modal overlays (`z-index: 10`).
- **2.1.3 Non-Intrusive Styling**: Semi-transparent subdued pill background (`rgba(0, 0, 0, 0.2)` or `rgba(255, 255, 255, 0.4)`), desaturated text color, and `pointer-events: none` to prevent interference with gameplay plot touches.

## 3 Visual States

### 3.1 States
- **3.1.1 Default State**: Subtle, low-opacity indicator (`opacity: 0.6`) ensuring zero distraction for children while remaining easily readable for developers and parents.
