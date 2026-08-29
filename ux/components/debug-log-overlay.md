# Component Specification: DebugLogOverlay

The `DebugLogOverlay` is a floating, collapsible diagnostics window visible exclusively when debug mode is enabled (via `?debug=true` or `?dev=true` URL parameters). It provides on-device log inspection, error tracking, and copy-to-clipboard capabilities for mobile and tablet debugging.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Whether the full debug panel is expanded or minimized to a floating pill.
- **1.1.2 `logs`**: `LogEntry[]` – Array of captured log objects:
  ```typescript
  interface LogEntry {
    id: string;
    timestamp: number;
    level: 'log' | 'info' | 'warn' | 'error';
    message: string;
    stack?: string;
  }
  ```
- **1.1.3 `activeFilter`**: `'all' | 'error' | 'warn' | 'log'` – Currently selected severity filter tab.

### 1.2 Event Handlers
- **1.2.1 `onToggleExpand`**: `() => void` – Toggles between minimized floating pill and expanded diagnostics drawer.
- **1.2.2 `onClearLogs`**: `() => void` – Clears current log buffer.
- **1.2.3 `onCopyLogs`**: `() => Promise<void>` – Formats all logs into a text payload and copies them to the device clipboard.
- **1.2.4 `onSetFilter`**: `(filter: 'all' | 'error' | 'warn' | 'log') => void` – Changes the active severity filter.

## 2 Contained Elements & Layout

### 2.1 Minimized Floating Trigger Pill
- **2.1.1 Floating Button**: Compact pill anchored in the bottom-left corner (`bottom: 8px, left: 8px`, `z-index: 9999`).
- **2.1.2 Error Counter Badge**: Displays bug icon + total log count and unread error count (e.g., `🐞 Logs (12) | 🔴 2`).
- **2.1.3 Visual Pulse on New Error**: Red flash animation whenever a new `error` level log is captured.

### 2.2 Expanded Diagnostics Drawer
- **2.2.1 Header Bar**:
  - **Title**: "Mobile Debug Console" with active log count.
  - **Filter Segment Tabs**: `All (N)` | `Errors (N)` | `Warns (N)` | `Logs (N)`.
  - **Action Toolbar**:
    - **Copy Button**: "📋 Copy All" button with temporary "Copied!" feedback state.
    - **Clear Button**: "🗑️ Clear" button.
    - **Minimize Button**: "✕" button to collapse back to floating pill.
- **2.2.2 Log Stream Container**:
  - Scrollable viewport (`max-height: 45vh`, `overflow-y: auto`, `-webkit-overflow-scrolling: touch`).
  - Monospace font (`font-size: 11px`, `line-height: 1.4`).
- **2.2.3 Log Row Item**:
  - **Timestamp Badge**: Formatted as `HH:MM:SS.mmm`.
  - **Severity Color Coding**:
    - `error`: High-contrast red background tint (`rgba(239, 68, 68, 0.15)`), red text (`#EF4444`).
    - `warn`: Amber tint (`rgba(245, 158, 11, 0.15)`), amber text (`#F59E0B`).
    - `info` / `log`: Clean dark theme text (`#E2E8F0`) with subtle separator border.
  - **Expandable Stack Trace**: Collapsible chevron revealing error call stack when present.

## 3 Visual States & Viewport Constraints

### 3.1 States
- **3.1.1 Visibility**: Rendered in DOM strictly when debug mode is active (`?debug=true` or `?dev=true` or `sessionStorage.farmspell_debug_mode === "true"`).
- **3.1.2 Touch Friendly**: All buttons maintain minimum `44px × 44px` touch targets for mobile/iPad ease of use.
- **3.1.3 Backdrop Glassmorphism**: Dark semi-translucent background (`rgba(15, 23, 42, 0.92)`, `backdrop-filter: blur(12px)`) ensuring clear legibility over bright gameplay scenes.
