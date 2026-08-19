# Component Specification: FarmGrid

The `FarmGrid` is the central game board layout component rendering all 25 farm plots in a fixed 5x5 grid arrangement.

## 1 Component Inputs & State

### 1.1 Props Interface
- **1.1.1 `plots`**: `Array<PlotStateData>` – Array of exactly 25 plot state objects corresponding to indices `0` through `24`.
- **1.1.2 `hasActionsRemaining`**: `boolean` – Flag indicating if player has remaining Action Points today.

### 1.2 Event Handlers
- **1.2.1 `onSelectPlot`**: `(plotIndex: number) => void` – Callback invoked when any plot tile is selected.

## 2 Contained Elements & Layout Structure

### 2.1 Grid Container
- **2.1.1 Structure**: CSS Grid container with 5 equal columns and 5 equal rows (`grid-template-columns: repeat(5, 1fr)`).
- **2.1.2 Aspect Ratio**: Square tile aspect ratio (`aspect-ratio: 1 / 1`), preserving a responsive square geometry across tablet and mobile screens.
- **2.1.3 Gap & Spacing**: `--space-sm` (`12px`) grid gap.
- **2.1.4 Ground Surface**: Framed within a rounded outer field border (`--radius-lg`, background: `--color-farm-green-main`).

### 2.2 Plot Children
- **2.2.1 Plot Instances**: Maps each item in `plots` to an individual [FarmPlotTile](farm-plot-tile.md) component with key `plotIndex`.

## 3 Responsiveness & Constraints

### 3.1 Sizing Rules
- **3.1.1 Tablet / Desktop**: Max grid container width `640px` centered in viewport.
- **3.1.2 Mobile Touch**: Adapts to `calc(100vw - 32px)`, ensuring each plot maintains at least `56px × 56px` touchable surface area.

## 4 Visual HTML Mockup

- **Live HTML Mock**: [mocks/farm-grid.html](mocks/farm-grid.html) demonstrates the 5x5 responsive grid layout with mixed crop types and growth stages.
