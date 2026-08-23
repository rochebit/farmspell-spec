# Game Rules: Profiles and Audio Studio

This document defines the rules for player profile management, profile persistence, and the Audio Studio recording framework for **FarmSpell**.

## 1 Player Profiles & Persistence

### 1.1 Multi-Profile Rules
- **1.1.1**: The application supports multiple **Player Profiles** on the same device.
- **1.1.2**: Each Profile maintains its own isolated:
  - Game Save State (farm grid, coins, inventory, day counter).
  - Custom Audio Recording Repository (IndexedDB audio Blobs tied to profile ID).
  - Selected Active Word Lists configuration.

### 1.2 Profile Selection & Startup Flow
- **1.2.1**: On initial app installation / launch, the system opens a **"Who is Playing?"** profile selection modal.
- **1.2.2**: Upon selecting or creating a Profile, the system saves `lastActiveProfileId` in `localStorage`.
- **1.2.3**: On subsequent app launches, the system automatically loads `lastActiveProfileId` and enters the farm view immediately without prompting for profile selection.
- **1.2.4**: A **"Switch Profile"** button is accessible in the Settings menu at all times, allowing players to switch profiles or add a new profile.

## 2 Audio Studio & Recording Mechanics

### 2.1 Audio Studio Interface
- **2.1.1**: The **Audio Studio** is directly accessible to children and parents from the main game HUD / Status Bar via the microphone icon button (open access, no parental gate barrier).
- **2.1.2**: The Audio Studio allows children or parents to view all available Word Lists (built-in packs and custom family lists) and individual words within each list.

### 2.2 Micro-Recording Workflow
- **2.2.1**: Tapping a word opens its Audio Recording Drawer featuring:
  - `Record Button` (Red mic circle).
  - `Stop Button`.
  - `Play Test Button`.
  - `Save Button`.
- **2.2.2**: Maximum recording duration is hard-capped at **5.0 seconds**. The system automatically stops recording when 5.0 seconds elapse.
- **2.2.3**: Tapping `Save` stores the recorded audio Blob in IndexedDB under composite key `${profileId}_${wordId}` and updates `hasCustomAudio = true` for that word in the profile.

### 2.3 Browser Text-to-Speech (TTS) Fallback
- **2.3.1**: If `hasCustomAudio == false` for a word, or if an audio Blob fails to load from IndexedDB, the system automatically invokes the browser's native Web Speech API (`window.speechSynthesis`).
- **2.3.2**: The default TTS voice is selected based on browser locale (default `en-US`).
