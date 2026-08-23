# Game Rules: Spelling Challenges

This document defines the rules governing spelling prompts, input mechanisms, retry handling, and educational assistance features for **FarmSpell**.

## 1 Spelling Trigger & Word Selection

### 1.1 Action Triggers
- **1.1.1**: A **Spelling Challenge** modal opens automatically whenever the player performs any of the following farm actions:
  - Planting a seed on an `Empty` plot.
  - Watering a `Planted` or `Growing` plot.
  - Harvesting a `ReadyToHarvest` plot.
- **1.1.2**: The farm action CANNOT complete until the Spelling Challenge is successfully resolved.

### 1.2 Adaptive Word Selection Engine
- **1.2.1 Active Pool**: The candidate word pool consists of all words from active built-in curriculum packs (`activeWordPackIds`) and active parent custom word lists (`customWordLists.filter(list => list.isActive)`).
- **1.2.2 4-Tier Selection Weighting ($W$)**: Each active word is assigned a review priority weight based on its current `spellingStats`:
  - **🚨 Struggling** (`streak == 0` and `incorrectCount > 0`): **Weight = 40** (highest priority for rapid mistake reinforcement).
  - **✨ New / Unseen** (`correctCount == 0` and `incorrectCount == 0`): **Weight = 30** (steady vocabulary introduction).
  - **🌱 Practicing** (`streak == 1` or `streak == 2`): **Weight = 20** (medium reinforcement toward mastery).
  - **⭐ Mastered** (`streak >= 3`): **Weight = 5** (low-frequency retention review).
- **1.2.3 Recency Buffer Rule**: The last **3 words** presented in the current play session are excluded from selection candidates (unless total active words < 4).
- **1.2.4 Weighted Random Selection**: The engine draws the next word from the non-recent pool proportional to its Selection Weight ($W$).

## 2 Audio Pronunciation & Input Interface

### 2.1 Audio Prompt Execution
- **2.1.1**: When a Spelling Challenge opens, the target word's audio pronunciation plays automatically.
- **2.1.2**: Audio is fetched from the active Profile's custom recordings in IndexedDB. If no custom recording exists for the word, the system automatically uses native browser Text-to-Speech (Web Speech API `window.speechSynthesis`).
- **2.1.3**: An **Audio Replay Button** is present on screen at all times during the challenge. Tapping it re-plays the audio prompt instantly.

### 2.2 Native OS Keyboard Focus Input
- **2.2.1**: The Spelling Challenge uses the device's **Native OS Keyboard** (iPad system keyboard).
- **2.2.2**: Upon opening the challenge, focus is automatically set to a hidden text input field, invoking the native soft keyboard.
- **2.2.3**: As the child types, typed uppercase letter boxes populate on screen corresponding to the target word length.

## 3 Submission, Retries, Streak Tracking & Learning Assistance

### 3.1 Submission & Streak Progression
- **3.1.1**: When the entered letter count matches the target word length, the engine automatically evaluates the submission.
- **3.1.2**: **First-Try Correct Submission (`failedAttemptCount == 0`)**:
  - Plays a cheerful success sound chord and brief star particle animation.
  - Updates `spellingStats[word]`:
    - `correctCount += 1`
    - `streak += 1` (advances streak count)
    - `lastPracticedAt = Date.now()`
  - Closes modal after 800ms, deducts 1 Action Point, and completes the farm action.
- **3.1.3**: **Resolved After Retries (`failedAttemptCount > 0`)**:
  - Plays success sound chord.
  - Updates `spellingStats[word]`:
    - `correctCount += 1`
    - `streak` remains `0` (streak was broken by earlier incorrect attempts).
    - `lastPracticedAt = Date.now()`
  - Closes modal after 800ms, deducts 1 Action Point, and completes the farm action.

### 3.2 Incorrect Submissions & Streak Breaking Rules
- **3.2.1**: **Streak Break on First Mistake**:
  - The instant any incorrect submission occurs (`attempt 1` or subsequent), the word's stats are immediately updated:
    - `incorrectCount += 1`
    - `streak = 0` (**breaks streak immediately**).
- **3.2.2**: **Attempts 1 & 2 (Standard Retries)**:
  - The input field shakes horizontally with a low buzz audio sound.
  - Incorrect letters clear automatically after 600ms, resetting focus for a retry.
  - Increments session `failedAttemptCount` by 1.
- **3.2.3**: **Attempts 3+ (Word Flash & Audio Assistance)**:
  - For any retry where `failedAttemptCount >= 2`, the correct target word text is displayed on screen for **1.5 seconds** and its audio pronunciation is re-played.
  - **Input Lock**: Text input is completely locked and disabled while the word is displayed on screen. The child CANNOT type or enter letters until the 1.5-second word display finishes and disappears.
  - Once the word display disappears, text input unlocks so the child can type the word.
  - Retries remain **unlimited**; the child can keep trying until they successfully type the word.

### 3.3 Challenge Cancellation
- **3.3.1**: If the player voluntarily cancels or exits an active Spelling Challenge modal before completing the word, the modal closes immediately.
- **3.3.2**: Cancelling a challenge costs **1 Action Point** (`dailyActionsRemaining` decrements by 1).
- **3.3.3**: The target farm plot remains in its previous state without changes.
