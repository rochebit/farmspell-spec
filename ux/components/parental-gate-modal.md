# Component Specification: ParentalGateModal

The `ParentalGateModal` is a security gate requiring adult verification before entering parent settings, switching profiles, or managing share codes, preventing children from accidentally altering game configurations.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `challengeQuestion`**: `string` – Arithmetic question string (e.g., `"What is 8 × 7?"` or `"Spell the word: PARENT"`).
- **1.1.3 `expectedAnswer`**: `string` – Correct answer string (e.g., `"56"`).

### 1.2 Event Handlers
- **1.2.1 `onVerificationSuccess`**: `() => void` – Triggered when the correct answer is entered.
- **1.2.2 `onVerificationFailure`**: `() => void` – Triggered when an incorrect answer is entered (generates a new challenge).
- **1.2.3 `onDismiss`**: `() => void` – Dismisses the gate and returns to the child game screen.

## 2 Contained Elements & Sub-Components

### 2.1 Challenge Card
- **2.1.1 Title**: "Grown-Ups Only" (font: `Heading1`).
- **2.1.2 Subtitle**: "Please solve this problem to open Settings:" (font: `BodyRegular`).
- **2.1.3 Question Display Box**: High-contrast plaque showing `challengeQuestion` in `28px` bold type.

### 2.2 Keypad & Input Area
- **2.2.1 Answer Input Display**: Box showing typed digits/characters.
- **2.2.2 Number Keypad**: 3x4 grid of buttons (`0-9`, `Clear`, `Submit`) with 56px touch targets.
- **2.2.3 Error Shake**: Red shake animation if the submitted answer does not match `expectedAnswer`.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Default**: Clean centered card on dim backdrop (`--color-surface-overlay`).
- **3.1.2 Error**: Challenge card wiggles and generates a fresh random math question.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/parental-gate-modal.html](mocks/parental-gate-modal.html) demonstrates the math question plaque and number keypad overlay.
