# Component Specification: ToastNotification

The `ToastNotification` provides lightweight, non-blocking floating alerts and feedback banners across the top or bottom of the screen.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `id`**: `string` – Unique toast instance identifier.
- **1.1.2 `message`**: `string` – Notification text content.
- **1.1.3 `severity`**: `ToastSeverity` – Enum: `'Info' | 'Success' | 'Warning' | 'Error'`.
- **1.1.4 `durationMs`**: `number` – Auto-dismiss duration in milliseconds (default `3000ms`).

### 1.2 Event Handlers
- **1.2.1 `onDismiss`**: `(id: string) => void` – Invoked when the toast auto-dismisses or is tapped.

## 2 Contained Elements & Sub-Components

### 2.1 Toast Card
- **2.1.1 Severity Icon**:
  - `Info`: Water drop or speech bubble icon.
  - `Success`: Star or green checkmark icon.
  - `Warning`: Energy droplet alert icon.
  - `Error`: Red exclamation or cross icon.
- **2.1.2 Message Text**: High-contrast text label (font: `BodyRegular`, weight: `600`).
- **2.1.3 Tap Dismiss Target**: Entire pill satisfies `56px` minimum touch height.

## 3 Visual States & Animations

### 3.1 States & Transitions
- **3.1.1 Slide In / Out**: Slides down from top of viewport (`translateY(-20px) -> translateY(0)`) over `200ms` with `--ease-spring`.
- **3.1.2 Pill Surface**: Pill shape (`--radius-full`) with `--color-surface-card` background, distinct severity color border, and soft drop shadow.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/toast-notification.html](mocks/toast-notification.html) demonstrates toast notification pills across all 4 severity categories (`Info`, `Success`, `Warning`, `Error`).
