# Verification & Acceptance Test Specification

This document defines the automated test suites, acceptance criteria, edge-case scenarios, and manual QA validation matrix for **FarmSpell** to guarantee **Multi-Agent Functional Determinism**.

## 1 Day Cycle & Action Point Verification Suite

### 1.1 Initial State & Daily Budget Constraints
- **1.1.1 Test Case: Profile Initialization Default State**:
  - **Given**: A newly created child profile document at `/players/{playerId}`.
  - **When**: The game loads the initial farm state.
  - **Then**:
    - `dayCount` MUST equal `1`.
    - `coins` MUST equal `5`.
    - `currentActions` MUST equal `10` (or configured `maxActionsPerDay`).
    - `plots` array MUST contain exactly `25` elements with indices `0` through `24`, all with `state: "Empty"`.
    - `seedInventory` MUST equal `{"crop_wheat": 3}` (with other crop seed counts at `0`).
    - `harvestedCropInventory` MUST be empty or have `0` for all crops.
    - `unlockedCropIds` MUST equal `["crop_wheat"]`.
- **1.1.2 Test Case: Action Point Decrement on Farm Action**:
  - **Given**: `currentActions == 5`.
  - **When**: The player successfully completes a farm action (Plant, Water, Harvest, or Clear).
  - **Then**:
    - `currentActions` MUST decrement to `4`.
    - The farm plot state MUST update according to the completed action.
- **1.1.3 Test Case: Zero Energy Interaction Lockout**:
  - **Given**: `currentActions == 0`.
  - **When**: The player taps an empty plot, dry crop, ready crop, or withered plot.
  - **Then**:
    - No spelling challenge modal MUST open.
    - An energy-depleted toast/prompt MUST appear directing the player to sleep at the Farmhouse.
    - `currentActions` MUST remain `0`.
- **1.1.4 Test Case: Persistent Build Version Indicator Rendering**:
  - **Given**: The game application running in any view.
  - **When**: Inspecting the viewport overlays.
  - **Then**:
    - A `VersionBadge` element MUST be rendered in the bottom corner of the viewport.
    - The displayed text MUST match the build/commit version string injected at build time (e.g., `v0.1.<count> (<hash>)`).

### 1.2 Day Advancement & Sleep Processing
- **1.2.1 Test Case: Normal Bedtime Advancement**:
  - **Given**: `dayCount == 3`, `currentActions == 0`, and various plots in dry/wet states.
  - **When**: The player taps the Farmhouse to sleep.
  - **Then**:
    - `dayCount` MUST increment to `4`.
    - `currentActions` MUST reset to `maxActionsPerDay` (`10`).
    - For all 25 plots, `isWateredToday` MUST reset to `false`.
- **1.2.2 Test Case: Early Sleep Confirmation**:
  - **Given**: `currentActions == 4` (energy remaining > 0).
  - **When**: The player taps the Farmhouse.
  - **Then**:
    - A confirmation dialog MUST appear stating remaining energy.
    - If confirmed, day advancement executes identically to Item 1.2.1.
    - If cancelled, day state and `currentActions` remain `4`.

## 2 Crop Growth & Withering Verification Suite

### 2.1 Growth Progression Scenarios
- **2.1.1 Test Case: Watered Crop Growth Advancement**:
  - **Given**: A plot with `cropType: "crop_carrot"`, `growthStage: 0`, `totalGrowthDays: 4`, and `isWateredToday: true`.
  - **When**: Day cycle advancement executes (Sleep).
  - **Then**:
    - `growthStage` MUST increment to `1`.
    - `unwateredDaysCount` MUST reset/remain `0`.
    - `state` MUST transition to `"Growing"`.
- **2.1.2 Test Case: Crop Maturity to ReadyToHarvest**:
  - **Given**: A plot with `cropType: "crop_carrot"`, `growthStage: 3`, `totalGrowthDays: 4`, and `isWateredToday: true`.
  - **When**: Day cycle advancement executes (Sleep).
  - **Then**:
    - `growthStage` MUST reach `4` (`growthStage == totalGrowthDays`).
    - `state` MUST transition to `"ReadyToHarvest"`.

### 2.2 Unwatered Pause & 2-Day Withering Matrix
- **2.2.1 Test Case: Single Unwatered Day Growth Pause (No Withering)**:
  - **Given**: A plot with `cropType: "crop_tomato"`, `growthStage: 1`, `unwateredDaysCount: 0`, and `isWateredToday: false` on Day 2.
  - **When**: Day 2 ends and Day 3 begins.
  - **Then**:
    - `growthStage` MUST remain `1` (growth paused).
    - `unwateredDaysCount` MUST increment to `1`.
    - `state` MUST remain `"Growing"`.
    - The crop MUST NOT wither.
- **2.2.2 Test Case: Two Consecutive Unwatered Days Wither Trigger**:
  - **Given**: The unwatered Tomato plot from Item 2.2.1 (`unwateredDaysCount == 1`) remains unwatered on Day 3 (`isWateredToday: false`).
  - **When**: Day 3 ends and Day 4 begins.
  - **Then**:
    - `unwateredDaysCount` MUST reach `2`.
    - `state` MUST transition to `"Withered"`.
    - The crop is lost and produces 0 harvest yield.
- **2.2.3 Test Case: Recovery from 1 Unwatered Day**:
  - **Given**: A plot with `unwateredDaysCount == 1` is watered on Day 3 (`isWateredToday: true`).
  - **When**: Day 3 ends and Day 4 begins.
  - **Then**:
    - `growthStage` MUST increment by 1.
    - `unwateredDaysCount` MUST reset to `0`.
    - `state` MUST remain `"Growing"`.
- **2.2.4 Test Case: Mature Crop Immortality (No Withering Once Ready)**:
  - **Given**: A plot with `state: "ReadyToHarvest"` remains unharvested and unwatered for 5 consecutive days.
  - **When**: Days 2, 3, 4, 5, and 6 advance.
  - **Then**:
    - `state` MUST permanently remain `"ReadyToHarvest"`.
    - Mature crops MUST NEVER wither or lose yield.

### 2.3 Withered Plot Clearing
- **2.3.1 Test Case: Clearing Withered Plot**:
  - **Given**: A plot with `state: "Withered"` and `currentActions == 5`.
  - **When**: The player taps the plot, completes the spelling challenge, and submits correctly.
  - **Then**:
    - `currentActions` MUST decrement to `4`.
    - `state` MUST transition to `"Empty"`.
    - `cropType` MUST reset to `null`.
    - `growthStage` and `unwateredDaysCount` MUST reset to `0`.
    - 0 coins and 0 crops are awarded.

## 3 Spelling Challenge Engine Verification Suite

### 3.1 Input Handling & Case Normalization
- **3.1.1 Test Case: Case-Insensitive Uppercase Normalization**:
  - **Given**: Target word `"CARROT"` in the spelling challenge modal.
  - **When**: The user types lowercase characters `'c'`, `'a'`, `'r'`, `'r'`, `'o'`, `'t'`.
  - **Then**:
    - Letter slots MUST display uppercase `"C"`, `"A"`, `"R"`, `"R"`, `"O"`, `"T"`.
    - Word validation MUST evaluate as identical to `"CARROT"`.
- **3.1.2 Test Case: Non-Alphabetical Input Rejection**:
  - **Given**: An active spelling challenge letter slot awaiting input.
  - **When**: The user presses numerical or special character keys (`'1'`, `'!'`, `'@'`, `' '`).
  - **Then**:
    - The character MUST be ignored.
    - No letter slot MUST be filled and cursor MUST not advance.
- **3.1.3 Test Case: Anti-Assistance Feature Suppression**:
  - **Given**: An active Spelling Challenge modal.
  - **When**: The player types characters on mobile or desktop keyboards.
  - **Then**:
    - The browser/OS MUST NOT display autocomplete suggestions, predictive word candidate bars, autocorrect replacements, or spellcheck error popups.
- **3.1.4 Test Case: Soft Keyboard Visual Viewport Centering**:
  - **Given**: An active Spelling Challenge modal rendered on a mobile or tablet viewport.
  - **When**: The soft OS keyboard opens (reducing `window.visualViewport.height`).
  - **Then**:
    - The modal container bounding box MUST remain strictly contained within the active `visualViewport`.
    - The Action Header, Audio Replay Button, Letter Slots, Attempt Badges, and Action/Backspace Buttons MUST NOT be overlapped or occluded by the soft keyboard.

### 3.2 Attempt Progression & Flash Hint Overlay
- **3.2.1 Test Case: First Failed Attempt Wiggle**:
  - **Given**: Target word `"WATER"`, `attemptCount == 0`.
  - **When**: User submits `"WAXER"`.
  - **Then**:
    - `attemptCount` MUST increment to `1`.
    - Letter slots container MUST execute 600ms error wiggle animation.
    - Letter slots MUST clear or highlight errors for correction.
    - Action point MUST NOT be deducted yet.
- **3.2.2 Test Case: Second Failed Attempt Flash Hint Trigger**:
  - **Given**: `attemptCount == 1`.
  - **When**: User submits incorrect word `"WATUR"`.
  - **Then**:
    - `attemptCount` MUST increment to `2`.
    - The Flash Hint Learning Card MUST immediately overlay the slots.
    - The full word `"WATER"` MUST be displayed in large green text.
    - Audio pronunciation for `"WATER"` MUST automatically play.
    - Typing input MUST be disabled/locked for exactly 3.0 seconds.
- **3.2.3 Test Case: Third Failed Attempt Challenge Failure**:
  - **Given**: `attemptCount == 2`.
  - **When**: User submits incorrect word a third time.
  - **Then**:
    - `currentActions` MUST decrement by 1.
    - The challenge modal MUST close.
    - The target farm plot MUST remain in its previous state without changes.
- **3.2.4 Test Case: Correct Submission on Any Attempt**:
  - **Given**: Target word `"CORN"`, `attemptCount` is 0, 1, or 2.
  - **When**: User submits `"CORN"`.
  - **Then**:
    - `currentActions` MUST decrement by 1.
    - Success star confetti animation MUST trigger.
    - Farm action MUST complete immediately (e.g., plot planted, watered, harvested).
- **3.2.5 Test Case: Voluntary Challenge Cancellation**:
  - **Given**: An active spelling challenge modal.
  - **When**: User taps the Cancel / Exit button.
  - **Then**:
    - `currentActions` MUST decrement by 1.
    - The challenge modal MUST close.
    - The farm plot MUST remain unchanged.

### 3.3 Custom Word List Management & Selection Verification
- **3.3.1 Test Case: Word List Entry Validation**:
  - **Given**: Parent entering words into `WordListManagerModal`.
  - **When**: Submitting invalid inputs (`"HI"` (< 3 letters), `"SUPERLONGWORD"` (> 10 letters), `"CAT123"` (contains numbers)).
  - **Then**: The system MUST reject the word and highlight the validation error.
- **3.3.2 Test Case: Automatic Uppercase & Deduplication**:
  - **Given**: Parent adds `"apple"`, `"CHAIR"`, and duplicate `"apple"`.
  - **When**: Saving the word list.
  - **Then**: The saved array MUST be normalized to `["APPLE", "CHAIR"]` with duplicates removed.
- **3.3.3 Test Case: Custom Word Pool Drawing in Farm Challenges**:
  - **Given**: A custom word list with `isActive: true` containing `["CHAIR", "TABLE"]`.
  - **When**: The child triggers farm spelling challenges.
  - **Then**: Words from the active custom list MUST appear in the challenge prompt rotation alongside active grade packs.
- **3.3.4 Test Case: Inactive Custom List Exclusion**:
  - **Given**: A custom word list with `isActive: false` containing `["ROBOT"]`.
  - **When**: Farm challenges are generated.
  - **Then**: `"ROBOT"` MUST NEVER be selected for spelling prompts until the list is toggled `isActive: true`.

### 3.4 Adaptive Word Selection & Streak Tracking Verification
- **3.4.1 Test Case: First-Try Correct Streak Progression**:
  - **Given**: Target word `"CARROT"` with initial `spellingStats["CARROT"] = { correctCount: 2, incorrectCount: 0, streak: 2 }`.
  - **When**: Child solves `"CARROT"` on attempt 1 (`failedAttemptCount == 0`).
  - **Then**:
    - `correctCount` MUST increment to `3`.
    - `streak` MUST increment to `3` (reaching Mastered status).
- **3.4.2 Test Case: Streak Reset Upon First Mistake**:
  - **Given**: Target word `"WATER"` with initial `streak == 4`.
  - **When**: Child submits incorrect spelling `"WATUR"` on attempt 1.
  - **Then**:
    - `incorrectCount` MUST increment by 1.
    - `streak` MUST immediately reset to `0`.
- **3.4.3 Test Case: Resolved After Retries Keeps Streak at Zero**:
  - **Given**: Target word `"WATER"` where attempt 1 was incorrect (`streak == 0`).
  - **When**: Child subsequently enters correct spelling on attempt 2 or 3.
  - **Then**:
    - `correctCount` MUST increment by 1.
    - `streak` MUST remain `0`.
- **3.4.4 Test Case: 3-Word Recency Buffer Exclusion**:
  - **Given**: Active word pool `["A", "B", "C", "D", "E"]`. Last 3 words presented: `["A", "B", "C"]`.
  - **When**: Engine selects the next word challenge.
  - **Then**: Next candidate MUST only be drawn from `["D", "E"]`.
- **3.4.5 Test Case: 4-Tier Selection Weight Assignment**:
  - **Given**:
    - Word 1 (Struggling: `streak: 0, incorrectCount: 2`) -> Weight MUST equal `40`.
    - Word 2 (New: `correctCount: 0, incorrectCount: 0`) -> Weight MUST equal `30`.
    - Word 3 (Practicing: `streak: 2`) -> Weight MUST equal `20`.
    - Word 4 (Mastered: `streak: 3`) -> Weight MUST equal `5`.
  - **When**: Calculating candidate selection probabilities.
  - **Then**: The candidate weights MUST evaluate precisely to `40`, `30`, `20`, and `5`.

## 4 Economy & Shop Verification Suite

### 4.1 Seed Unlocking Verification
- **4.1.1 Test Case: Successful Seed Unlock Consumes Crops**:
  - **Given**: `unlockedCropIds == ["crop_wheat"]`, `harvestedCropInventory.crop_wheat == 12`.
  - **When**: Player taps "Unlock Seed" on Oat (`crop_oat`, requires `9x crop_wheat`).
  - **Then**:
    - `harvestedCropInventory.crop_wheat` MUST update to `3` (`12 - 9`).
    - `unlockedCropIds` MUST contain `"crop_oat"`.
    - 0 coins deducted, 0 energy cost, 0 spelling challenges.
    - The Oat card in the Shop MUST transition immediately to Unlocked state.
- **4.1.2 Test Case: Insufficient Harvested Crops Lockout**:
  - **Given**: `harvestedCropInventory.crop_wheat == 5` (requires 9).
  - **When**: Player views Oat seeds (`crop_oat`) in the Shop.
  - **Then**:
    - The Unlock button MUST be disabled.
    - Tapping MUST NOT deduct crops and MUST NOT unlock the seed.
- **4.1.3 Test Case: Locked Seed Purchasing Prevention**:
  - **Given**: `unlockedCropIds == ["crop_wheat"]`, `coins == 100`.
  - **When**: Player attempts to purchase Carrot seeds (`crop_carrot`).
  - **Then**:
    - No purchase button MUST be available for locked seeds (only Unlock button is presented).
    - Client MUST NOT allow purchasing locked crop seeds.

### 4.2 Seed Purchasing Verification
- **4.2.1 Test Case: Successful Seed Purchase**:
  - **Given**: `unlockedCropIds` contains `"crop_tomato"`, `coins == 50`, `seedInventory.crop_tomato == 0`, `currentActions == 3`.
  - **When**: Player purchases 1 Tomato seed packet (`crop_tomato`, cost `12` coins).
  - **Then**:
    - `coins` MUST update to `38` (`50 - 12`).
    - `seedInventory.crop_tomato` MUST update to `1`.
    - `currentActions` MUST remain `3` (0 energy cost).
    - 0 spelling challenges MUST be prompted.
- **4.2.2 Test Case: Insufficient Funds Guard**:
  - **Given**: `coins == 5`, `unlockedCropIds` contains `"crop_tomato"`.
  - **When**: Player views Tomato seeds (`crop_tomato`, cost `12` coins).
  - **Then**:
    - Purchase button MUST be disabled with gray styling.
    - Tapping MUST NOT deduct coins and MUST NOT grant seeds.

### 4.3 Crop Selling Verification
- **4.3.1 Test Case: Single Crop Sell**:
  - **Given**: `coins == 100`, `harvestedCropInventory.crop_carrot == 3` (unit value `8` coins).
  - **When**: Player taps "Sell Carrots".
  - **Then**:
    - `coins` MUST update to `124` (`100 + (3 * 8)`).
    - `harvestedCropInventory.crop_carrot` MUST update to `0`.
    - 0 energy cost and 0 spelling challenges.
- **4.3.2 Test Case: Bulk "Sell All" Action**:
  - **Given**: `coins == 0`, `harvestedCropInventory` containing 2 Carrots (`8` ea), 2 Tomatoes (`24` ea), and 1 Corn (`18` ea).
  - **When**: Player taps "Sell All Crops".
  - **Then**:
    - Total calculation: `(2 * 8) + (2 * 24) + (1 * 18) = 16 + 48 + 18 = 82`.
    - `coins` MUST update to `82`.
    - All crop counts in `harvestedCropInventory` MUST reset to `0`.

## 5 Audio Studio & Storage Bounds Verification Suite

### 5.1 Recording & IndexedDB Bounds
- **5.1.1 Test Case: 5.0-Second Maximum Recording Auto-Stop**:
  - **Given**: Microphone stream active in Audio Studio.
  - **When**: User begins recording and leaves it running.
  - **Then**:
    - The recording MUST automatically stop at exactly `5.0` seconds.
    - The captured audio Blob MUST NOT exceed `5.0` seconds duration.
- **5.1.2 Test Case: IndexedDB Record Persistence**:
  - **Given**: A captured audio Blob for word `"PUMPKIN"`.
  - **When**: User taps "Save".
  - **Then**:
    - A record MUST be written to IndexedDB `audio_store` with key `word: "PUMPKIN"`.
    - `mimeType` MUST be `"audio/webm"` or `"audio/mp4"`.
    - The word's status badge in the UI MUST immediately transition to `"Custom Voice"`.
- **5.1.3 Test Case: Delete Recording Reversion to TTS**:
  - **Given**: Custom recording exists for `"PUMPKIN"`.
  - **When**: User taps "Reset to TTS / Delete".
  - **Then**:
    - The IndexedDB record MUST be deleted.
    - Word status badge MUST revert to `"Default TTS"`.
    - Subsequent audio playback for `"PUMPKIN"` MUST route to `window.speechSynthesis`.

## 6 Multi-Parent Sync & Security Rules Verification Suite

### 6.1 Security Rules Access Policies
- **6.1.1 Test Case: Authorized Parent Profile Read/Write**:
  - **Given**: Authenticated parent user with `auth.uid == "parent_uid_123"`.
  - **When**: Reading or writing `/players/{playerId}` where `"parent_uid_123" in resource.data.authorizedParentUids`.
  - **Then**: Request MUST succeed.
- **6.1.2 Test Case: Unauthorized Parent Profile Block**:
  - **Given**: Authenticated parent user with `auth.uid == "stranger_uid_999"`.
  - **When**: Attempting to read or write `/players/{playerId}` where `"stranger_uid_999"` is NOT in `authorizedParentUids`.
  - **Then**: Firestore Security Rules MUST reject the request with `PERMISSION_DENIED`.

### 6.2 Join Code Handshake & Security Verification
- **6.2.1 Test Case: Collection Enumeration / List Denial**:
  - **Given**: Authenticated user with `auth.uid == "user_any"`.
  - **When**: Attempting to query, scan, or list documents in `/joinCodes` (`getDocs(collection(db, "joinCodes"))`).
  - **Then**: Firestore Security Rules MUST reject the query with `PERMISSION_DENIED`.
- **6.2.2 Test Case: Exact Document Get Allowed**:
  - **Given**: Active join code document at `/joinCodes/JOIN-8492` containing `parentUid: "parent_b"`.
  - **When**: Authenticated parent user executes `getDoc(doc(db, "joinCodes", "JOIN-8492"))`.
  - **Then**: Request MUST succeed and return the document data.
- **6.2.3 Test Case: 15-Minute Join Code Expiration**:
  - **Given**: A join code generated at timestamp `T0` with `expiresAt: T0 + 15 minutes`.
  - **When**: Parent A attempts to process the code at `T0 + 16 minutes`.
  - **Then**: Client-side check MUST reject the code as expired and NOT add `parentUid` to `authorizedParentUids`.
- **6.2.4 Test Case: Successful Join Code Authorization Handshake**:
  - **Given**: Unexpired join code `"JOIN-8492"` for Parent B (`auth.uid == "parent_2"`), and Parent A (`auth.uid == "parent_1"`) authorized on `player_abc`.
  - **When**: Parent A enters `"JOIN-8492"` and submits.
  - **Then**:
    - Parent A client fetches `/joinCodes/JOIN-8492`, extracting `parentUid: "parent_2"`.
    - Parent A client updates `/players/player_abc` appending `"parent_2"` to `authorizedParentUids`.
    - Update succeeds because `"parent_1"` is already in `resource.data.authorizedParentUids`.
    - Real-time snapshot on Parent B device receives `player_abc` in query results.
