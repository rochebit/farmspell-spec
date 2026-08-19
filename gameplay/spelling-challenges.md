# Game Rules: Spelling Challenges

This document defines the rules governing spelling prompts, input mechanisms, retry handling, and educational assistance features for **FarmSpell**.

## 1 Spelling Trigger & Word Selection

### 1.1 Action Triggers
- **1.1.1**: A **Spelling Challenge** modal opens automatically whenever the player performs any of the following farm actions:
  - Planting a seed on an `Empty` plot.
  - Watering a `Planted` or `Growing` plot.
  - Harvesting a `ReadyToHarvest` plot.
- **1.1.2**: The farm action CANNOT complete until the Spelling Challenge is successfully resolved.

### 1.2 Word Selection Engine
- **1.2.1**: Words are selected at random from the player's currently selected **Active Word Lists** (see [profiles-and-audio.md](profiles-and-audio.md)).
- **1.2.2**: The selection algorithm enforces a non-repeating queue (a word will not repeat until all other words in the active list have been presented once).

## 2 Audio Pronunciation & Input Interface

### 2.1 Audio Prompt Execution
- **2.1.1**: When a Spelling Challenge opens, the target word's audio pronunciation plays automatically.
- **2.1.2**: Audio is fetched from the active Profile's custom recordings in IndexedDB. If no custom recording exists for the word, the system automatically uses native browser Text-to-Speech (Web Speech API `window.speechSynthesis`).
- **2.1.3**: An **Audio Replay Button** is present on screen at all times during the challenge. Tapping it re-plays the audio prompt instantly.

### 2.2 Native OS Keyboard Focus Input
- **2.2.1**: The Spelling Challenge uses the device's **Native OS Keyboard** (iPad system keyboard).
- **2.2.2**: Upon opening the challenge, focus is automatically set to a hidden text input field, invoking the native soft keyboard.
- **2.2.3**: As the child types, typed uppercase letter boxes populate on screen corresponding to the target word length.

## 3 Submission, Retries & Learning Assistance

### 3.1 Submission & Validation
- **3.1.1**: When the entered letter count matches the target word length, the engine automatically evaluates the submission.
- **3.1.2**: **Correct Submission**:
  - Plays a cheerful success sound chord.
  - Displays a brief star animation.
  - Closes the modal after 800ms.
  - Deducts 1 Action Point and completes the farm action.

### 3.2 Incorrect Submissions & Assistance Rules
- **3.2.1**: **Attempts 1 & 2 (Standard Retries)**:
  - If the typed word does not match the target word, the input field shakes horizontally with a low buzz audio sound.
  - Incorrect letters clear automatically after 600ms, resetting focus for a retry.
  - Increments `failedAttemptCount` by 1.
- **3.2.2**: **Attempts 3+ (Word Flash & Audio Assistance)**:
  - For any retry after 2 failed attempts (`failedAttemptCount >= 2`), the correct target word text is displayed on screen for **1.5 seconds** and its audio pronunciation is re-played.
  - **Input Lock**: Text input is completely locked and disabled while the word is displayed on screen. The child CANNOT type or enter letters until the 1.5-second word display finishes and disappears.
  - Once the word display disappears, text input unlocks so the child can type the word.
  - Retries remain **unlimited**; the child can keep trying until they successfully type the word.

### 3.3 Challenge Cancellation
- **3.3.1**: If the player voluntarily cancels or exits an active Spelling Challenge modal before completing the word, the modal closes immediately.
- **3.3.2**: Cancelling a challenge costs **1 Action Point** (`dailyActionsRemaining` decrements by 1).
- **3.3.3**: The target farm plot remains in its previous state without changes.
