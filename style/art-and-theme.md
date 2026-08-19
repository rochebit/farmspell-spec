# Visual Style & Art Direction Specification

This document defines the visual design system, color palette tokens, typography rules, and visual style guidelines for **FarmSpell**.

## 1 Design Philosophy & Visual Aesthetic

### 1.1 Cozy Illustrated Farm Aesthetic
- **1.1.1**: The visual direction for **FarmSpell** follows a **Cozy Illustrated Farm** aesthetic—warm, inviting, and relaxing.
- **1.1.2**: **Shape Language**: Soft rounded corners (`border-radius: 16px` to `24px`), chunky touchable buttons, and soft drop shadows to create a tactile, welcoming interface for children.
- **1.1.3**: **Visual Overlays**: Glassmorphism translucent backdrops (`backdrop-filter: blur(10px)`) for modals and drawers over the farm view.

## 2 Color Palette Tokens

### 2.1 Theme Color Palette

| Token Name | Hex Code | Primary Usage |
| :--- | :--- | :--- |
| `--color-grass-green` | `#4CAF50` | Primary farm background, vibrant grass tiles |
| `--color-sprout-green` | `#8BC34A` | Growing plant stages, positive progress indicators |
| `--color-soil-brown` | `#795548` | Empty plot fill, soil textures |
| `--color-rich-dirt` | `#4E342E` | Card borders, text labels, high-contrast outlines |
| `--color-wheat-gold` | `#FFC107` | Ready to harvest glow, star reward accents |
| `--color-coin-gold` | `#FFD54F` | Coin balance badges, currency rewards |
| `--color-sky-blue` | `--2196F3` | Daily watering actions, audio studio accents |
| `--color-record-red` | `#E53935` | Microphone recording indicator, active record button |

## 3 Typography for Early Readers

### 3.1 Font Stack & Sizing
- **3.1.1 Font Family**: Primary font stack uses rounded sans-serif typefaces (**Fredoka**, **Quicksand**, or **Nunito** with system rounded fallbacks).
- **3.1.2 Legibility Standards**:
  - Spelling Slot Letters: `40px` bold uppercase (`2.5rem`) with `4px` letter box spacing.
  - Page Headers & Modal Titles: `32px` bold (`2.0rem`).
  - Button Labels & Stats: `20px` medium (`1.25rem`).
  - Toast Messages & Hints: `16px` regular (`1.0rem`).

## 4 Micro-Animations & Visual Feedback

### 4.1 Interaction Animations
- **4.1.1 Crop Growth Scale**: Smooth CSS scaling transition (`transform: scale(0.85) -> scale(1.0)` over 300ms) when a crop advances growth stage.
- **4.1.2 Plot Tap Feedback**: Gentle scale compression (`transform: scale(0.95)`) on tap down.
- **4.1.3 Incorrect Spelling Shake**: Horizontal keyframe wiggle (`translateX(-6px) -> translateX(6px)`) over 600ms on incorrect word submission.
- **4.1.4 Success Confetti Burst**: Particle canvas effect emitting colorful stars upon completing a spelling challenge or harvesting.
