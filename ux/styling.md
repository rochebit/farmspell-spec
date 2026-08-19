# UX Styling & Design System Specification

This document defines the comprehensive design tokens, styling rules, typography hierarchy, spacing scale, elevation, and animation specifications for **FarmSpell**.

## 1 Design Philosophy & Visual Tokens

### 1.1 Aesthetic & Visual Principles
- **1.1.1 Cozy Illustrated Farm**: Warm, friendly, tactile interface with high-contrast elements tailored for early learners and elementary-age children.
- **1.1.2 Tactile 3D Feel**: Interactive elements utilize subtle bottom borders (3D beveled appearance) that visibly depress upon touch/click to provide satisfying tactile feedback.
- **1.1.3 Glassmorphic Overlays**: Modal dialogs and drawers utilize translucent backdrop blurring (`backdrop-filter: blur(12px)`) over the living farm environment.

### 1.2 Color Palette Tokens

#### 1.2.1 Primary & Environment Tokens
| CSS Token Variable | Hex Value | Intended Usage |
| :--- | :--- | :--- |
| `--color-farm-green-main` | `#4CAF50` | Primary farm lawn, main positive button backgrounds |
| `--color-farm-green-dark` | `#2E7D32` | 3D button bottom shadows, dark accent borders |
| `--color-farm-green-light` | `#81C784` | Plot highlight borders, active selection glow |
| `--color-sprout-yellow-green` | `#8BC34A` | Growing stage badges, progress meters |
| `--color-sky-blue` | `#29B6F6` | Water droplets, watering actions, audio waves |
| `--color-sky-blue-dark` | `#0288D1` | Water button 3D depth shadow |

#### 1.2.2 Soil & Farm Plot Tokens
| CSS Token Variable | Hex Value | Intended Usage |
| :--- | :--- | :--- |
| `--color-soil-dry` | `#8D6E63` | Dry empty plot surface |
| `--color-soil-wet` | `#4E342E` | Watered moist plot surface |
| `--color-soil-border` | `#3E2723` | Farm plot outlines and grid borders |
| `--color-withered-gray` | `#78909C` | Withered crop tint and desaturation layer |

#### 1.2.3 Currency & Reward Tokens
| CSS Token Variable | Hex Value | Intended Usage |
| :--- | :--- | :--- |
| `--color-coin-gold` | `#FFC107` | Coin icons, ready-to-harvest badge backgrounds |
| `--color-coin-gold-dark` | `#FFA000` | Coin container 3D bevel bottom border |
| `--color-star-yellow` | `#FFEB3B` | Success confetti, particle burst stars |

#### 1.2.4 Neutral & Surface Tokens
| CSS Token Variable | Hex Value | Intended Usage |
| :--- | :--- | :--- |
| `--color-surface-card` | `#FFFDF9` | Modal card backgrounds, drawer bodies |
| `--color-surface-overlay` | `rgba(33, 33, 33, 0.60)` | Modal backdrop dimming layer |
| `--color-text-primary` | `#2D231E` | High-contrast body text and letter slots |
| `--color-text-muted` | `#6D5D55` | Secondary captions and helper text |
| `--color-text-inverted` | `#FFFFFF` | Text rendered on dark or vibrant buttons |

#### 1.2.5 Status & Feedback Tokens
| CSS Token Variable | Hex Value | Intended Usage |
| :--- | :--- | :--- |
| `--color-feedback-error` | `#E53935` | Incorrect spelling alert, record button active |
| `--color-feedback-success` | `#43A047` | Correct spelling confirmation, harvest chime |
| `--color-feedback-warning` | `#FB8C00` | Low action point alert, expiration timer |

## 2 Typography & Lettering Scale

### 2.1 Font Families
- **2.1.1 Primary Typeface**: `'Fredoka', 'Quicksand', 'Nunito', system-ui, -apple-system, sans-serif` (rounded, friendly letterforms with clear character distinction for early readers).
- **2.1.2 Monospace / Number Typeface**: `'Fredoka', sans-serif` with tabular numbers enabled (`font-variant-numeric: tabular-nums`).

### 2.2 Type Scale Hierarchy
| Hierarchy Level | Font Size | Line Height | Font Weight | Letter Spacing | Primary Application |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `LetterSlot` | `40px` (`2.5rem`) | `1.0` | `700` (Bold) | `4px` | Spelling challenge input boxes |
| `Heading1` | `32px` (`2.0rem`) | `1.2` | `700` (Bold) | `0px` | Modal titles, day summary banner |
| `Heading2` | `24px` (`1.5rem`) | `1.25` | `600` (SemiBold) | `0px` | Section headers, card titles |
| `ButtonLarge` | `20px` (`1.25rem`) | `1.0` | `700` (Bold) | `0.5px` | Primary interactive buttons |
| `BodyRegular` | `18px` (`1.125rem`)| `1.4` | `500` (Medium) | `0px` | Descriptions, shop item details |
| `CaptionSmall` | `14px` (`0.875rem`)| `1.3` | `600` (SemiBold) | `0.2px` | Badges, sub-labels, timers |

## 3 Spacing, Radii & Elevation

### 3.1 Spacing Scale
- `--space-2xs`: `4px`
- `--space-xs`: `8px`
- `--space-sm`: `12px`
- `--space-md`: `16px`
- `--space-lg`: `24px`
- `--space-xl`: `32px`
- `--space-2xl`: `48px`

### 3.2 Border Radius Scale
- `--radius-sm`: `8px` (Small badges and sub-pills)
- `--radius-md`: `16px` (Buttons, letter slots, shop item cards)
- `--radius-lg`: `24px` (Modals, drawers, farm plot tiles)
- `--radius-full`: `9999px` (Status pills, circular icon buttons)

### 3.3 Touch Target & Sizing Standards
- **3.3.1 Minimum Touch Target**: All interactive controls must satisfy a minimum tap target dimension of `56px × 56px` to ensure effortless interaction for children on touchscreens.
- **3.3.2 Letter Slot Dimensions**: Each spelling slot box must be minimum `56px` wide × `64px` tall.

### 3.4 Elevation & 3D Shadow Tokens
- **3.4.1 Beveled Button Normal**: `box-shadow: 0 5px 0 var(--shadow-color)`
- **3.4.2 Beveled Button Pressed**: `box-shadow: 0 1px 0 var(--shadow-color); transform: translateY(4px);`
- **3.4.3 Modal Overlay Elevation**: `box-shadow: 0 16px 32px rgba(0, 0, 0, 0.25)`

## 4 Animation & Micro-Interaction Tokens

### 4.1 Transition Durations & Easing
- `--duration-instant`: `100ms` (Button press depress)
- `--duration-quick`: `200ms` (Hover states, badge scale)
- `--duration-standard`: `300ms` (Modal open/close, drawer slide)
- `--duration-feedback`: `600ms` (Error wiggle, celebration sequence)
- `--ease-spring`: `cubic-bezier(0.34, 1.56, 0.64, 1)` (Bouncy playful pop)
- `--ease-smooth`: `cubic-bezier(0.25, 0.1, 0.25, 1.0)` (Standard UI transition)

### 4.2 Keyframe Animation Standards
- **4.2.1 Incorrect Attempt Shake**:
  - Horizontal translation oscillation: `-8px` to `+8px` across 5 cycles over `600ms`.
- **4.2.2 Success Pop**:
  - Scale transition: `1.0` -> `1.15` -> `1.0` with `--ease-spring` over `350ms`.
- **4.2.3 Ready-to-Harvest Pulse**:
  - Continuous gentle pulsing glow: `box-shadow: 0 0 12px var(--color-coin-gold)` oscillating over `1500ms` infinite.
