# Component Specification: JoinCodeModal

The `JoinCodeModal` enables a new parent to generate a temporary 6-digit alphanumeric Join Code (e.g., `"JOIN-8492"`) to show to an existing parent already managing a child's farm.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `parentUid`**: `string` – UID of the authenticated parent creating the code.
- **1.1.3 `joinCode`**: `string | null` – Active 6-digit code string (e.g., `"JOIN-8492"`).
- **1.1.4 `expiresAt`**: `Date | null` – Timestamp when the active code expires (15 minutes).

### 1.2 Event Handlers
- **1.2.1 `onGenerateCode`**: `() => Promise<void>` – Creates a new 15-minute document in Firestore at `/joinCodes/{code}`.
- **1.2.2 `onClose`**: `() => void` – Closes modal.

## 2 Contained Elements & Sub-Components

### 2.1 Code Display Card
- **2.1.1 Title**: "Connect to Child's Farm" (font: `Heading1`).
- **2.1.2 Large Code Plaque**: High-contrast display of the 6-digit code in `40px` bold monospace letters (`--font-letter-slot`).
- **2.1.3 Countdown Timer**: Live timer displaying remaining validity (e.g., `Expires in: 14:32`).
- **2.1.4 Instructions Text**: Clear guidance: "Show this code to the parent already managing the child's farm. They can enter it in Child Settings to add you."

### 2.2 Action Buttons
- **2.2.1 Copy Code Button**: Copies code to clipboard with "Copied!" feedback badge.
- **2.2.2 Refresh / Generate New Code Button**: Generates a new code and resets the 15-minute timer.
- **2.2.3 Done / Close Button**: Dismisses modal.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Active Code**: Bright gold border (`--color-coin-gold`) around the 6-digit plaque.
- **3.1.2 Expired Code State**: Red alert banner ("Code expired") with "Generate New Code" button.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/share-code-modal.html](mocks/share-code-modal.html) demonstrates the 6-digit alphanumeric code plaque, countdown timer badge, and copy trigger.
