# View & Component Content Specifications

This document defines the functional content, data fields, indicators, and user interaction triggers for every view, modal, and reusable UI component in **FarmSpell**. This specification focuses strictly on functional content requirements, independent of visual styling or layout positioning.

## 1 Global & Navigation Components

### 1.1 Top Status Bar (`StatusBar`)
- **1.1.1 Data Displayed & Indicators**:
  - **1.1.1.1 Active Profile Badge**: Displays the current child player name and selected avatar icon.
  - **1.1.1.2 Coin Balance Indicator**: Displays the current total coin count as a formatted non-negative integer.
  - **1.1.1.3 Daily Action Point Meter**: Displays remaining daily Action Points (`currentActions`) and the maximum daily allowance (`maxActionsPerDay`, default `10`), formatted as `X / Y`.
  - **1.1.1.4 Day Counter**: Displays the current in-game day number (`dayCount`), formatted as `Day N`.
  - **1.1.1.5 Audio Status Indicator**: Displays whether master game audio is currently active or muted.
- **1.1.2 Interactive Controls & Triggers**:
  - **1.1.2.1 Profile & Settings Trigger**: Action opening the Profile Switcher or Parental Gate / Settings view.
  - **1.1.2.2 Audio Mute Toggle**: Action toggling audio output on or off.
  - **1.1.2.3 Shop Launcher**: Action opening the Shop & Inventory modal.
  - **1.1.2.4 Audio Studio Launcher**: Action opening the Audio Recording Studio view.

### 1.2 Toast & Notification Overlay (`ToastNotification`)
- **1.2.1 Data Displayed & Indicators**:
  - **1.2.1.1 Notification Message**: Text string communicating non-blocking gameplay feedback (e.g., "Already watered today!", "Plot is ready to harvest!", "Energy depleted — Go to bed to begin the next day!").
  - **1.2.1.2 Severity Icon**: Visual symbol indicating message category (`Info`, `Success`, `Warning`, `Error`).
- **1.2.2 Interactive Controls & Triggers**:
  - **1.2.2.1 Auto-Dismiss Timer**: Automatic dismissal after 3.0 seconds.
  - **1.2.2.2 Tap-to-Dismiss Trigger**: Immediate dismissal upon user tap.

## 2 Main Farm View Components

### 2.1 Farm Grid Area (`FarmGrid`)
- **2.1.1 Data Displayed & Indicators**:
  - **2.1.1.1 Grid Composition**: Renders a fixed 5x5 collection of 25 individual Farm Plot tiles (`plotIndex` 0 to 24).
- **2.1.2 Interactive Controls & Triggers**:
  - **2.1.2.1 Individual Plot Interaction**: User tap on any tile triggers plot-specific evaluation based on plot state and daily Action Points.

### 2.2 Farm Plot Tile (`FarmPlotTile`)
- **2.2.1 Data Displayed & Indicators**:
  - **2.2.1.1 Plot State Indicator**: Displays the current lifecycle state:
    - `Empty`: Bare soil ready for sowing.
    - `Planted`: Seed sown, awaiting initial growth.
    - `Growing`: Plant actively developing through intermediate growth stages.
    - `ReadyToHarvest`: Crop fully grown and ready to pick.
    - `Withered`: Crop withered due to 2 consecutive unwatered days.
  - **2.2.1.2 Crop Identity Graphic**: When occupied (`Planted`, `Growing`, `ReadyToHarvest`), displays the visual representation of the specific crop type (`Carrot`, `Tomato`, `Corn`, `Strawberry`, `Pumpkin`).
  - **2.2.1.3 Growth Stage Indicator**: For `Growing` crops, displays the current stage relative to total required days (e.g., `Stage 1/2`).
  - **2.2.1.4 Moisture Status Indicator**: Displays whether the plot is `Watered` or `Dry` for the current in-game day.
  - **2.2.1.5 Ready Glow / Harvest Particle**: Special visual highlight when the plot reaches `ReadyToHarvest`.
  - **2.2.1.6 Withered Graphic**: Desaturated/wilted appearance when the plot is `Withered`.
- **2.2.2 Interactive Controls & Triggers**:
  - **2.2.2.1 Empty Plot Tap**: If Action Points > 0, opens the Seed Selection Drawer (`SeedPicker`).
  - **2.2.2.2 Planted or Growing Plot Tap (Dry)**: If Action Points > 0, initiates a Watering Action via the Spelling Challenge Modal.
  - **2.2.2.3 Planted or Growing Plot Tap (Watered)**: Displays an informative toast notification ("Already watered today!"). Consumes 0 Action Points.
  - **2.2.2.4 Ready to Harvest Plot Tap**: If Action Points > 0, initiates a Harvest Action via the Spelling Challenge Modal.
  - **2.2.2.5 Withered Plot Tap**: If Action Points > 0, initiates a Clear Withered Plot Action via the Spelling Challenge Modal.
  - **2.2.2.6 Plot Tap with 0 Action Points**: If Action Points == 0 on any actionable plot, triggers an Energy Depleted prompt pointing to the Farmhouse.

### 2.3 Farmhouse & Day Transition Control (`FarmhouseControl`)
- **2.3.1 Data Displayed & Indicators**:
  - **2.3.1.1 In-Game Day Badge**: Displays current day number.
  - **2.3.1.2 Bedtime Ready Highlight**: Visual pulse indicator active when daily Action Points reach 0.
  - **2.3.1.3 Sleep Prompt**: Text indicating "Tap to Sleep & Start Next Day".
- **2.3.2 Interactive Controls & Triggers**:
  - **2.3.2.1 End Day / Sleep Trigger**: User tap triggers the end-of-day sequence:
    - If Action Points > 0: Displays a confirmation prompt ("You still have energy remaining! Are you sure you want to sleep?").
    - If Action Points == 0: Immediately executes day advancement, resetting Action Points to `maxActionsPerDay`, processing crop growth and withering, and incrementing `dayCount`.

### 2.4 Seed Selection Drawer (`SeedPicker`)
- **2.4.1 Data Displayed & Indicators**:
  - **2.4.1.1 Target Plot Reference**: Indicator showing which plot index is being planted.
  - **2.4.1.2 Available Seed List**: For each seed type in the player inventory:
    - Seed name and crop icon.
    - Quantity owned in player inventory.
    - Growth time in days.
    - Base selling price per harvested unit.
  - **2.4.1.3 Empty Inventory Notice**: Displayed if the player owns 0 total seeds across all crop types.
- **2.4.2 Interactive Controls & Triggers**:
  - **2.4.2.1 Seed Select Trigger**: User tap on an available seed initiates a Planting Action via the Spelling Challenge Modal.
  - **2.4.2.2 Go to Shop Shortcut**: Action opening the Shop view if the player needs to buy seeds.
  - **2.4.2.3 Close / Dismiss Drawer**: Action closing the drawer without consuming an Action Point.

## 3 Spelling Challenge Interface

### 3.1 Spelling Challenge Modal (`SpellingChallengeModal`)
- **3.1.1 Data Displayed & Indicators**:
  - **3.1.1.1 Target Action Header**: Text describing the action being performed (e.g., "Spell to Plant Carrot", "Spell to Water Plot", "Spell to Harvest Tomato", "Spell to Clear Soil").
  - **3.1.1.2 Word Length Letter Slots**: A row of discrete character boxes matching the exact letter count of the selected target word.
  - **3.1.1.3 Typed Letters**: Uppercase characters currently entered by the player into the letter slots.
  - **3.1.1.4 Active Slot Cursor**: Visual highlight on the current slot awaiting the next typed letter.
  - **3.1.1.5 Remaining Attempts Indicator**: Visual badges representing remaining attempts (3 total attempts permitted).
  - **3.1.1.6 Flash Hint Overlay Card**: Displayed automatically after 2 failed attempts:
    - Full text of the target word.
    - Visual lockout timer (3.0 seconds) during which typing is disabled while audio is replayed.
  - **3.1.1.7 Audio Replay Button State**: Indicates whether audio is currently playing or ready to replay.
- **3.1.2 Interactive Controls & Triggers**:
  - **3.1.2.1 Audio Replay Trigger**: Prominent button that replays the spoken word audio (custom recording if available, otherwise TTS fallback).
  - **3.1.2.2 Character Input Region**: Native keyboard focus receiver handling soft keyboard on touch devices and physical keypresses on desktop:
    - Accepts alphabetical characters (`A-Z`, case-insensitive, displayed in uppercase).
    - Auto-advances focus to the next empty slot upon keypress.
  - **3.1.2.3 Backspace / Delete Trigger**: Removes the last typed character and moves cursor focus back one slot.
  - **3.1.2.4 Submit Word Trigger**: Submits the completed word for validation upon reaching full word length or pressing Enter.
  - **3.1.2.5 Cancel / Exit Trigger**: Closes the modal, consumes 1 Action Point, and leaves the plot unchanged.

## 4 Shop & Inventory Interfaces

### 4.1 Shop Modal (`ShopModal`)
- **4.1.1 Data Displayed & Indicators**:
  - **4.1.1.1 Current Coin Balance**: Real-time player coin total.
  - **4.1.1.2 Navigation Tabs**: "Buy Seeds" storefront tab and "Sell Crops" inventory tab.
- **4.1.2 Interactive Controls & Triggers**:
  - **4.1.2.1 Tab Switcher**: Toggles between Seed Store and Crop Sell views.
  - **4.1.2.2 Close Shop Trigger**: Closes the modal and returns to Main Farm View.

### 4.2 Seed Storefront Tab (`SeedStorefront`)
- **4.2.1 Data Displayed & Indicators**:
  - **4.2.1.1 Seed Catalog Items**: For each crop type (`Carrot`, `Tomato`, `Corn`, `Strawberry`, `Pumpkin`):
    - Crop name and seed packet icon.
    - Seed purchase cost in coins.
    - Growth duration in days.
    - Harvest selling value in coins.
    - Currently owned seed quantity.
    - Affordability state (enabled if `coins >= cost`, disabled if `coins < cost`).
- **4.2.2 Interactive Controls & Triggers**:
  - **4.2.2.1 Purchase Seed Trigger**: Deducts purchase price from coins and adds 1 seed packet to player inventory (0 Action Point cost, 0 spelling challenges).

### 4.3 Crop Selling Tab (`CropSellingShelf`)
- **4.3.1 Data Displayed & Indicators**:
  - **4.3.1.1 Harvested Crop Items**: For each crop type present in harvested inventory:
    - Crop name and harvest icon.
    - Quantity owned in inventory.
    - Unit selling price in coins.
    - Total value calculation (`quantity * unit price`).
  - **4.3.1.2 Total Inventory Value**: Sum of all sellable crops currently in inventory.
  - **4.3.1.3 Empty Inventory Notice**: Displayed when player holds 0 harvested crops.
- **4.3.2 Interactive Controls & Triggers**:
  - **4.3.2.1 Sell Single Crop Type Trigger**: Instantly sells all units of the selected crop type, awarding coins and clearing that crop count from inventory.
  - **4.3.2.2 Sell All Crops Bulk Trigger**: Instantly sells every harvested crop in inventory in a single action, awarding total coin value.

## 5 Audio Studio Interface

### 5.1 Audio Studio View (`AudioStudioView`)
- **5.1.1 Data Displayed & Indicators**:
  - **5.1.1.1 Word Pack Selector**: List of selectable grade/curriculum word packs.
  - **5.1.1.2 Word Registry List**: Table or list displaying each word in the selected pack:
    - Word text string.
    - Recording Source Badge: Indicates whether word uses `Custom Voice` (locally recorded) or `Default TTS`.
    - Recording Date: Timestamp of latest custom recording if present.
    - Audio Duration: Length of recorded audio clip in seconds.
  - **5.1.1.3 Active Recording Panel**:
    - Currently selected target word.
    - Recording State (`Idle`, `Recording`, `Playing`, `Saving`).
    - 5.0-Second Recording Countdown Gauge: Visual timer indicator displaying elapsed / remaining capture duration.
    - Microphone Permission Warning: Notice displayed if browser microphone access is denied.
- **5.1.2 Interactive Controls & Triggers**:
  - **5.1.2.1 Word Selection Trigger**: Selects a word from the list to preview or record.
  - **5.1.2.2 Record Button**: Starts microphone audio capture (auto-stops at 5.0 seconds).
  - **5.1.2.3 Stop Recording Button**: Manually stops microphone capture prior to 5.0s timeout.
  - **5.1.2.4 Playback Preview Button**: Plays back the newly captured audio clip or existing custom clip.
  - **5.1.2.5 Play TTS Sample Button**: Plays back the browser SpeechSynthesis TTS pronunciation for comparison.
  - **5.1.2.6 Save Recording Trigger**: Encodes and saves audio Blob into IndexedDB (`LocalAudioRecord`), updating the word's status badge.
  - **5.1.2.7 Delete Recording Trigger**: Removes local audio Blob from IndexedDB and reverts word to default TTS.
  - **5.1.2.8 Close Audio Studio**: Returns to Main Farm View.

## 6 Profile & Parent Management Interfaces

### 6.1 Parental Gate Overlay (`ParentalGateModal`)
- **6.1.1 Data Displayed & Indicators**:
  - **6.1.1.1 Adult Verification Challenge**: Randomly generated adult math challenge or written prompt (e.g., "What is 7 × 8?").
  - **6.1.1.2 Error Feedback**: Notice displayed on incorrect challenge answer.
- **6.1.2 Interactive Controls & Triggers**:
  - **6.1.2.1 Number Input Pad / Field**: Interface for entering the answer.
  - **6.1.2.2 Submit Verification Trigger**: Unlocks Parent Settings / Profile Management upon correct answer.
  - **6.1.2.3 Cancel / Dismiss Trigger**: Closes modal and returns to child game view.

### 6.2 Parent Settings Modal (`ParentSettingsModal`)
- **6.2.1 Data Displayed & Indicators**:
  - **6.2.1.1 Active Child Profile Summary**: Child name, avatar, and active word pack.
  - **6.2.1.2 Daily Action Limit Control**: Configured value for `maxActionsPerDay` (range: 5 to 20 actions).
  - **6.2.1.3 Word Difficulty / Pack Selector**: Currently assigned word list (Grade K, Grade 1, Grade 2, Custom Words).
  - **6.2.1.4 Authorized Parents Count**: Number of parent accounts currently linked to this profile.
- **6.2.2 Interactive Controls & Triggers**:
  - **6.2.2.1 Adjust Action Limit**: Slider or stepper modifying `maxActionsPerDay`.
  - **6.2.2.2 Switch Word Pack**: Dropdown/picker changing the active spelling curriculum.
  - **6.2.2.3 Authorize Co-Parent**: Action opening the Authorize Co-Parent modal to enter a co-parent's Join Code.
  - **6.2.2.4 Open Profile Switcher**: Action opening the multi-profile switcher.
  - **6.2.2.5 Save & Close**: Persists settings to Firestore and closes modal.

### 6.3 Profile Switcher & Creator View (`ProfileSwitcherView`)
- **6.3.1 Data Displayed & Indicators**:
  - **6.3.1.1 Child Profiles List**: Cards for each profile linked to the parent account:
    - Child name and avatar icon.
    - Current day number and coin total.
    - Active status badge.
- **6.3.2 Interactive Controls & Triggers**:
  - **6.3.2.1 Select Active Profile Trigger**: Switches the active child profile and loads corresponding farm state.
  - **6.3.2.2 Create New Profile Trigger**: Opens child creation form (Name input field + avatar selection picker).
  - **6.3.2.3 Connect to Child Trigger**: Opens Join Code Modal (`JoinCodeModal`) generating a 6-digit code for this parent to show to an existing parent.

### 6.4 Join Code Modal (`JoinCodeModal`)
- **6.4.1 Data Displayed & Indicators**:
  - **6.4.1.1 6-Digit Join Code Display**: Large, high-contrast display of the generated 6-digit alphanumeric code (e.g., `JOIN-8492`).
  - **6.4.1.2 Expiration Countdown Timer**: Displays remaining validity time (15-minute countdown from generation).
  - **6.4.1.3 Instructions Text**: Guidance: "Show this code to the parent already managing the farm so they can add you in Child Settings."
- **6.4.2 Interactive Controls & Triggers**:
  - **6.4.2.1 Copy Code Trigger**: Copies the 6-digit code to device clipboard.
  - **6.4.2.2 Generate New Code Trigger**: Invalidates prior code and generates a fresh 15-minute code.
  - **6.4.2.3 Close Modal Trigger**: Dismisses modal.

### 6.5 Authorize Co-Parent Modal (`AuthorizeCoParentModal`)
- **6.5.1 Data Displayed & Indicators**:
  - **6.5.1.1 Child Target Name**: Confirms which child profile is receiving the new authorized parent.
  - **6.5.1.2 6-Digit Code Input Box**: Input area for entering the Join Code provided by the new parent.
  - **6.5.1.3 Verification Feedback**: Status indicator confirming parent name upon successful lookup before saving.
- **6.5.2 Interactive Controls & Triggers**:
  - **6.5.2.1 Submit Code Trigger**: Fetches `/joinCodes/{code}`, extracts `parentUid`, and appends UID to `authorizedParentUids`.
  - **6.5.2.2 Cancel Trigger**: Closes modal without modifications.

### 6.6 Word List Manager & Editor Modal (`WordListManagerModal`)
- **6.6.1 Data Displayed & Indicators**:
  - **6.6.1.1 Curriculum & Lists Overview**: Displays two sections:
    - *Built-in Grade Packs*: (Grade K, Grade 1, Grade 2, Sight Words) with word counts and active checkboxes.
    - *Custom Family Lists*: Cards for each parent-created list showing title, word count, active toggle, and creation date.
  - **6.6.1.2 Word List Editor Panel**:
    - List Name Input field (e.g., "Week 3 School Words").
    - Tag-style word chips showing currently entered words with remove (`×`) buttons.
    - Add Word input box with real-time length (3–10 chars) and alphabetical (`A-Z`) validation cues.
  - **6.6.1.3 Active Word Pool Counter**: Live indicator calculating total unique words active in the child's daily farm challenge pool.
- **6.6.2 Interactive Controls & Triggers**:
  - **6.6.2.1 Create New List Trigger**: Opens blank list editor form.
  - **6.6.2.2 Add Word Action**: Validates input, capitalizes, and appends chip to word list.
  - **6.6.2.3 Remove Word Action**: Removes chip from list.
  - **6.6.2.4 Save List Action**: Commits list to `customWordLists` in Firestore.
  - **6.6.2.5 Delete List Action**: Removes custom list from Firestore.
  - **6.6.2.6 Record Audio Shortcut**: Routes directly to `AudioStudioView` with the custom list pre-filtered.
  - **6.6.2.7 Close Modal Trigger**: Dismisses manager and returns to Parent Settings.
