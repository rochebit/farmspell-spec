# Component Specification: SpellingChallengeModal

The `SpellingChallengeModal` is the core learning overlay presented whenever a farm action (planting, watering, harvesting, clearing) requires spelling a target word.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `targetWord`**: `string` – Uppercase string representing the target word (e.g., `"CARROT"`, `"WATER"`).
- **1.1.3 `actionType`**: `FarmActionType` – Enum: `'Plant' | 'Water' | 'Harvest' | 'Clear'`.
- **1.1.4 `customAudioUrl`**: `string | null` – Local IndexedDB audio Blob URL if custom recording exists.
- **1.1.5 `attemptCount`**: `number` – Current failure count for this challenge (`0`, `1`, or `2`).
- **1.1.6 `maxAttempts`**: `number` – Fixed constant `3`.

### 1.2 Event Handlers
- **1.2.1 `onSolveSuccess`**: `() => void` – Triggered upon entering the exact target word.
- **1.2.2 `onAttemptFail`**: `(failedAttemptCount: number) => void` – Triggered upon an incorrect submission.
- **1.2.3 `onCancelChallenge`**: `() => void` – Triggered when user exits the challenge (deducts 1 AP, cancels action).

## 2 Contained Elements & Sub-Components

### 2.1 Action Banner & Audio Button
- **2.1.1 Action Title**: "Spell to ${actionType}!" (font: `Heading1`, bold).
- **2.1.2 Audio Speaker Button**: Large circular button (`64px × 64px`, `--color-sky-blue`) with audio wave icon. Tapping plays `customAudioUrl` or invokes SpeechSynthesis TTS.

### 2.2 Word Slot Display
- **2.2.1 Slot Container**: Horizontal row of discrete character boxes matching `targetWord.length`.
- **2.2.2 Individual Letter Box**:
  - Minimum dimensions `56px × 64px` with `--radius-md` corners and `--color-rich-dirt` border.
  - Renders typed uppercase letter in `40px` bold font (`--font-letter-slot`).
  - Active Cursor: Pulsing bottom underline indicator on the current active slot.

### 2.3 Attempt Badges & Assistance Overlays
- **2.3.1 Attempt Hearts / Stars**: Row of 3 star icons indicating remaining attempts.
- **2.3.2 Flash Hint Learning Card**: Displayed automatically when `attemptCount == 2`:
  - Card overlay revealing the full `targetWord` in large green text.
  - Automatically triggers audio playback.
  - 3.0-second countdown lock preventing typing until audio finishes.

### 2.4 Keyboard Focus Receiver, Anti-Assistance & Viewport Centering
- **2.4.1 Native Focus Receiver**: Hidden input capturing soft touch keyboard and physical keyboard events with all operating system and browser auto-complete, auto-correct, predictive text, and spellcheck features disabled.
- **2.4.2 Visual Viewport Centering (Zero Keyboard Overlap)**:
  - The modal container is bound to the active **Visual Viewport** (`window.visualViewport` / CSS `dvh` / `interactive-widget=resizes-content`).
  - When the soft OS keyboard deploys, the modal automatically recalculates its vertical bounds and centers itself within the visible portion of the screen above the keyboard.
  - The Action Title, Audio Button, Word Letter Slots, Attempt Stars, and Controls must maintain 100% visibility with minimum 16px safety padding, ensuring no overlap or occlusion by the soft keyboard.
- **2.4.3 On-Screen Backspace Button**: Tactile button (`56px × 56px`) with backspace glyph to delete the last character.
- **2.4.4 Cancel Action Button**: Text button ("Cancel Action — Uses 1 Energy") to dismiss modal.

## 3 Visual States & Animations

### 3.1 States
- **3.1.1 Incorrect Attempt**: Entire slot container executes horizontal wiggle animation (`translateX(-8px) -> translateX(8px)` over `600ms`) with red border flash (`--color-feedback-error`).
- **3.1.2 Success Resolution**: All letter slots turn vibrant green (`--color-feedback-success`) with star particle confetti burst before modal closes.
- **3.1.3 Audio Playing State**: Pulsing concentric ripples around the audio speaker button.
- **3.1.4 Visual Viewport Soft Keyboard Adaptation**: Smooth CSS transition on container position/padding when the virtual keyboard opens or closes, keeping the challenge perfectly centered in the visible area.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/spelling-challenge-modal.html](mocks/spelling-challenge-modal.html) demonstrates the learning modal overlay with uppercase letter slots, active cursor indicator, audio speaker button, attempt stars, tactile control buttons, and viewport centering logic.
