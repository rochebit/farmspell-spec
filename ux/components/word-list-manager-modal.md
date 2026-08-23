# Component Specification: WordListManagerModal

The `WordListManagerModal` allows parents to create, edit, delete, and activate custom spelling word lists (such as weekly school homework), as well as toggle built-in curriculum packs.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `isOpen`**: `boolean` – Modal visibility state.
- **1.1.2 `builtInPacks`**: `Array<WordPackMetadata>` – List of built-in curriculum packs (Grade K, Grade 1, Grade 2, Sight Words).
- **1.1.3 `activeWordPackIds`**: `Array<string>` – IDs of currently active built-in packs.
- **1.1.4 `customWordLists`**: `Array<CustomWordList>` – Parent-created custom lists from `PlayerProfileDocument`.

### 1.2 Event Handlers
- **1.2.1 `onToggleBuiltInPack`**: `(packId: string) => Promise<void>` – Toggles a built-in pack's active status in Firestore.
- **1.2.2 `onToggleCustomList`**: `(listId: string) => Promise<void>` – Toggles a custom list's active status in Firestore.
- **1.2.3 `onSaveCustomList`**: `(list: CustomWordList) => Promise<void>` – Saves/updates a custom list in `customWordLists`.
- **1.2.4 `onDeleteCustomList`**: `(listId: string) => Promise<void>` – Deletes a custom list.
- **1.2.5 `onOpenAudioStudio`**: `(listId: string) => void` – Routes to the Audio Studio filtered to this list.
- **1.2.6 `onClose`**: `() => void` – Closes modal.

## 2 Contained Elements & Sub-Components

### 2.1 Modal Header
- **2.1.1 Title**: "Spelling Lists & Curriculum" (font: `Heading1`).
- **2.1.2 Active Word Count Badge**: Pill displaying `Total Active Words: ${totalCount}`.
- **2.1.3 Close Button**: Top-right dismiss button.

### 2.2 Built-in Curriculum Section
- **2.2.1 Grade Pack Cards**: Checkbox cards for each built-in grade level with word count.

### 2.3 Custom Family Lists Section
- **2.3.1 Custom List Cards**: For each custom list:
  - List Name & Word Count (e.g., "Week 3 School Words • 8 Words").
  - Active Toggle Switch.
  - "Edit Words" button.
  - "🎙️ Record Voices" button (shortcut to Audio Studio).
  - Delete button.
- **2.3.2 "Create New Word List" Button**: Primary action opening the List Editor dialog.

### 2.4 List Editor Form / Drawer
- **2.4.1 List Name Input**: Text input (max 50 chars).
- **2.4.2 Word Tag Chips**: List of chips showing entered words with remove (`×`) buttons.
- **2.4.3 Word Input Box**: High-contrast input field with uppercase auto-conversion and 3–10 letter validation cue.
- **2.4.4 Action Row**: "Save List" button and "Cancel" button.

## 3 Visual States & Styling Mappings

### 3.1 States
- **3.1.1 Active List Card**: Highlighted green outline (`--color-farm-green-main`) when toggled ON.
- **3.1.2 Validation Error**: Red border on word input if string is < 3 letters, > 10 letters, contains numbers, or is a duplicate.
- **3.1.3 Modal Container**: Glassmorphic modal with `--radius-lg` and `--color-surface-card` body.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/word-list-manager-modal.html](mocks/word-list-manager-modal.html) demonstrates custom list creation, word chips, and audio recording shortcuts.
