# Component Specification: AudioStudioView

The `AudioStudioView` is the recording suite where parents or children can record custom voice audio pronunciations for spelling words, saving them locally into IndexedDB.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `activeWordLists`**: `Array<ActiveWordList>` – Combined list of active built-in curriculum packs and activated parent custom word lists.
- **1.1.2 `wordList`**: `Array<WordRecord>` – All words belonging to the child's currently active lists.
- **1.1.3 `localAudioRecords`**: `Record<string, LocalAudioMetadata>` – Metadata of custom recordings in IndexedDB.

### 1.2 Event Handlers
- **1.2.1 `onSaveAudio`**: `(word: string, audioBlob: Blob) => Promise<void>` – Commit custom audio to IndexedDB.
- **1.2.2 `onDeleteAudio`**: `(word: string) => Promise<void>` – Remove recording and revert word to TTS fallback.
- **1.2.3 `onClose`**: `() => void` – Returns to Main Farm View.

## 2 Contained Elements & Sub-Components

### 2.1 Header & Word Navigator
- **2.1.1 Title & Subtitle**: "Voice Recording Studio" – "Record your own voice for spelling words!".
- **2.1.2 Active List Tabs / Selector**: Tabs or dropdown displaying only the **Active Word Lists** enabled by parents (e.g. "Grade 1 Phonics", "Week 4 School Spelling"). Inactive lists are hidden from the child's view.
- **2.1.3 Word Registry Table**: Scrollable list of words in the selected active list:
  - Word string (font: `BodyRegular`, bold).
  - Status Badge: `Custom Voice` (green pill) or `Default TTS` (blue pill).
  - Duration Badge: `${duration.toFixed(1)}s` if recorded.
  - Quick Listen Button: Replays current audio for that word.

### 2.2 Active Word Recording Station
- **2.2.1 Target Word Banner**: Large display of the currently selected word (`32px` bold).
- **2.2.2 5.0-Second Progress Ring**: Circular countdown gauge showing elapsed capture time from `0.0s` to `5.0s`.
- **2.2.3 Audio Visualizer**: Live audio wave visualizer rendering microphone input volume levels during recording.
- **2.2.4 Control Button Deck**:
  - **Record Button**: Large red circular button (`64px × 64px`, `--color-record-red`) with mic glyph.
  - **Stop Button**: Square stop button to halt recording before the 5.0s timeout.
  - **Test Playback Button**: Speaker button to listen to the newly recorded clip.
  - **Save Button**: Green checkmark button to save to local IndexedDB.
  - **Reset to TTS Button**: Trash icon button to delete custom audio and restore TTS fallback.

## 3 Visual States & Feedback

### 3.1 Recording States
- **3.1.1 Idle**: Record button ready, progress ring at 0%.
- **3.1.2 Recording Active**: Record button pulses red, audio wave animates, countdown timer ticks toward 5.0s.
- **3.1.3 Review Ready**: Test Playback and Save buttons light up after capture completes.
- **3.1.4 Microphone Denied Warning**: Warning banner displayed if browser microphone permissions are blocked.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/audio-studio-view.html](mocks/audio-studio-view.html) demonstrates the voice studio with word pack table, 5.0s countdown ring timer gauge, and record/play/save controls.
