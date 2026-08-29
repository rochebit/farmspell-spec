# UX Specification: User Flows & Interaction Journeys

This document defines the step-by-step functional user flows and interaction journeys for **FarmSpell**. It inherits definitions from [overview.md](overview.md) and [project-overview.md](../project-overview.md).

## 1 Onboarding & Profile Setup Flow

### 1.1 First-Time Parent & Child Onboarding
- **1.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Launch App] --> B{Authenticated?}
    B -- No --> C[Google Sign-In Prompt]
    C --> D[Parent Account Authenticated]
    B -- Yes --> E{Has Child Profiles?}
    D --> E
    E -- Yes --> F[Select Active Child Profile]
    E -- No --> G{Choose Setup Action}
    G -- New Family --> H[Create First Child Profile]
    G -- Join Existing Family --> I[Generate Join Code Screen]
    I --> J[Parent B shares code with existing Parent A]
    J --> K[Parent A authorizes Parent B in Settings]
    K --> L[Child Farm Auto-Discovered via Real-Time Sync]
    H --> M[Initialize Default 5x5 Farm State]
    M --> N[Enter Main Farm View]
    F --> N
    L --> N
```

- **1.1.2 Functional Steps**:
  - **Step 1**: App launches and checks Firebase Authentication state.
  - **Step 2**: If unauthenticated, present Google Sign-In prompt/button.
  - **Step 3**: Upon authentication, query Firestore for profiles matching `authorizedParentUids`.
  - **Step 4**: If no profiles exist, present the **Welcome Onboarding Choice** screen with two options:
    - **Option 1: "Create a New Child Profile"**: Enter child name and select avatar icon; generates a new `/players/{playerId}` document with starter farm.
    - **Option 2: "Connect to an Existing Family"**: Opens the **Join Code Screen** (`JoinCodeModal`), generating a 15-minute 6-digit code (e.g., `JOIN-8492`) without forcing the parent to create a new child profile.
  - **Step 5**: When "Connect to an Existing Family" is chosen, the screen displays the code and listens in real-time. Once the existing parent enters the code on their device, the new parent's app automatically detects the linked profile and transitions directly into the child's farm view.

## 2 Daily Core Gameplay Loop

### 2.1 Farming Action & Day Transition Flow
- **2.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Main Farm View] --> B[Player Taps Farm Plot]
    B --> C{Plot Action Valid & Action Points > 0?}
    C -- No --> D[Show Energy Prompt / Already Watered Toast]
    C -- Yes --> E[Open Spelling Challenge Modal]
    E --> F{Spelling Resolved?}
    F -- Cancelled --> G[Deduct 1 Action Point, Abort Action]
    F -- Solved --> H[Deduct 1 Action Point, Complete Farm Action]
    H --> I{Actions Remaining == 0?}
    I -- Yes --> J[Prompt Player to Sleep at Farmhouse]
    I -- No --> A
    J --> K[Player Taps Farmhouse -> Sleep]
    K --> L[End Day Crop Evaluation & Calendar Increment]
    L --> M[Render Good Morning Summary Screen]
    M --> A
```

- **2.1.2 Functional Steps**:
  - **Step 1**: Player views farm grid showing 25 plot tiles and current action points (`dailyActionsRemaining`).
  - **Step 2**: Player taps a plot tile to initiate an action (Planting, Watering, Harvesting, or Clearing Withered plot).
  - **Step 3**: Spelling Challenge modal opens automatically and plays audio pronunciation.
  - **Step 4**: Child enters target word using native OS keyboard.
  - **Step 5**: Upon correct entry, modal closes with success animation, 1 Action Point is deducted, and the plot action completes.
  - **Step 6**: When Action Points reach 0, tapping the Farmhouse triggers the "Go to Sleep" day transition.
  - **Step 7**: Crops are evaluated for daily growth or withering, `currentDay` increments by 1, Action Points reset to 10, and a "Good Morning" transition screen displays.

## 3 Audio Studio Recording Flow

### 3.1 Custom Voice Recording Journey
- **3.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Child or Parent on Main Farm View] --> B[Tap Microphone Icon on Top Status Bar]
    B --> C[Open Audio Studio - No Parental Gate Required]
    C --> D[Browse Active Word Packs & Custom Lists]
    D --> E[Select Target Word Item]
    E --> F[Tap Record Button]
    F --> G[Speak Voice Recording - Max 5.0s Timer]
    G --> H[Tap Stop or Auto-Stop at 5.0s]
    H --> I[Tap Test Playback]
    I --> J{Satisfied with Voice?}
    J -- Re-record --> F
    J -- Save --> K[Save Audio Blob to Local IndexedDB]
    K --> L[Update Word Status Badge to Custom Recorded]
    L --> M[Child immediately hears their own voice in Farm Challenges!]
```

- **3.1.2 Functional Steps**:
  - **Step 1**: Child or parent taps the prominent **Microphone button** (`🎙️`) on the top HUD Status Bar (open to kids, zero parental gate required).
  - **Step 2**: The Audio Studio opens, presenting active curriculum packs and custom family word lists.
  - **Step 3**: Select any word to view its current audio status (`Custom Recorded` vs `Default TTS`).
  - **Step 4**: Tap `Record` and speak into the microphone (with a visual 5.0s circular countdown gauge).
  - **Step 5**: Tap `Play Preview` to listen back to the voice clip.
  - **Step 6**: Tap `Save` to persist the audio Blob in local `IndexedDB` under composite key `${playerId}_${wordId}`.
  - **Step 7**: The word badge immediately turns to `Custom Voice`, and the child will hear that exact voice recording during subsequent farm spelling challenges!

## 4 Multi-Parent Profile Linking Flow

### 4.1 Join Code Handshake Flow
- **4.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Parent B signs in on Device 2] --> B[Tap Connect to a Child Profile]
    B --> C[Generate 6-Digit Join Code: JOIN-8492]
    C --> D[Display Code & 15m Expiry Timer on Device 2]
    D --> E[Parent B shares code with Parent A]
    E --> F[Parent A opens Child Settings on Device 1]
    F --> G[Parent A enters code: JOIN-8492]
    G --> H[Parent A client fetches /joinCodes/JOIN-8492 & gets Parent B UID]
    H --> I[Parent A client updates /players/childId appending Parent B UID]
    I --> J[Child Profile instantly appears on Parent B Device via real-time query]
```

- **4.1.2 Functional Steps**:
  - **Step 1**: Parent B signs into their Google account on Device 2 and taps `Connect to a Child`.
  - **Step 2**: Device 2 generates a temporary document in Firestore at `/joinCodes/JOIN-8492` containing Parent B's UID and a 15-minute expiration timestamp.
  - **Step 3**: Parent B provides the 6-digit code `JOIN-8492` to Parent A (who is currently managing the child's farm).
  - **Step 4**: Parent A opens `Parent Settings` -> `Co-Parent Access` on Device 1 and enters the 6-digit code.
  - **Step 5**: Parent A's client executes a direct `get()` on `/joinCodes/JOIN-8492`, extracts Parent B's `parentUid`, and updates `/players/{playerId}` by appending Parent B to `authorizedParentUids`.
  - **Step 6**: Because Parent A is already authorized on the child profile, the write succeeds client-side immediately.
  - **Step 7**: Parent B's device real-time listener (`where("authorizedParentUids", "array-contains", parentB.uid)`) fires instantly, and the child's farm appears on Device 2.

## 5 Custom Word List Management Flow

### 5.1 Creating & Activating Custom Word Lists
- **5.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Parent unlocks Parental Gate] --> B[Open Word Lists & Curriculum in Settings]
    B --> C[Tap 'Create New Word List']
    C --> D[Enter List Name e.g. 'Friday Spelling Test']
    D --> E[Add Words e.g. 'CHAIR', 'TABLE', 'PLANT']
    E --> F[Client validates 3-10 letter words & auto-capitalizes]
    F --> G[Save List to Firestore /players/playerId/customWordLists]
    G --> H{Record Voice Audio Now?}
    H -- Yes --> I[Open Audio Studio filtered to new custom list]
    I --> J[Parent records custom voice for each word]
    H -- Later / Default --> K[Words use default browser TTS in gameplay]
    G --> L[Toggle List Active Status ON]
    L --> M[New words immediately enter daily farm spelling rotation]
```

- **5.1.2 Functional Steps**:
  - **Step 1**: Parent solves the adult verification math challenge to access `Parent Settings`.
  - **Step 2**: Tap `Manage Spelling Words & Lists` to open the `WordListManagerModal`.
  - **Step 3**: Tap `Create New List` to open the list editor.
  - **Step 4**: Enter a descriptive list name (e.g., "Week 4 School Spelling") and add target words via tag input or newline separation.
  - **Step 5**: Client validates that each word contains only alphabetical characters (`A-Z`) with length between 3 and 10 letters, automatically converting to uppercase and stripping duplicate entries.
  - **Step 6**: Tap `Save Word List`. The list is committed to the child profile's `customWordLists` array in Firestore, immediately syncing across all authorized parents' devices.
  - **Step 7**: A prompt asks: "Would you like to record your voice for these words now?". Selecting "Yes" seamlessly routes the parent to `AudioStudioView` with the newly created list pre-selected.
  - **Step 8**: The custom list's `isActive` toggle ensures the new words are immediately included in the farm's spelling challenge rotation.

## 6 Seed Unlock & Marketplace Purchasing Flow

### 6.1 Unlocking & Purchasing New Seeds
- **6.1.1 Flow Diagram**:

```mermaid
flowchart TD
    A[Player Opens Shop Modal] --> B[View Seed Storefront Tab]
    B --> C{Crop in unlockedCropIds?}
    C -- Yes --> D{Coins >= Seed Cost?}
    D -- Yes --> E[Tap 'Buy 1' -> Deduct Coins & Add Seed to Inventory]
    D -- No --> F[Buy Button Disabled - Insufficient Coins]
    C -- No --> G[Render Locked Card with Prerequisite Crop Checklist]
    G --> H{Holds all required crops in Inventory?}
    H -- No --> I[Unlock Button Disabled - Missing Harvested Crops]
    H -- Yes --> J[Tap 'Unlock Seed']
    J --> K[Deduct Prerequisite Crops from Inventory]
    K --> L[Append Crop ID to unlockedCropIds in Firestore]
    L --> M[Sparkle Fanfare Animation -> Card Converts to Unlocked]
    M --> D
```

- **6.1.2 Functional Steps**:
  - **Step 1**: Player opens the Shop modal from the top HUD.
  - **Step 2**: The Seed Storefront displays all 20 catalog crops organized by tier.
  - **Step 3**: For crops not yet unlocked, the card displays required prerequisite crops and the player's held quantities.
  - **Step 4**: When the player holds sufficient crops, the "Unlock Seed" button activates.
  - **Step 5**: Tapping "Unlock Seed" immediately deducts the required crop quantities from inventory and appends the crop ID to `unlockedCropIds`.
  - **Step 6**: A celebration sparkle animation plays, and the seed card converts into an active purchasable item.
  - **Step 7**: The player can now purchase seed packets using Coins at any time.
