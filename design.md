---
name: Precision Industrial System
colors:
  surface: '#fcf8fa'
  surface-dim: '#dcd9db'
  surface-bright: '#fcf8fa'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f6f3f5'
  surface-container: '#f0edef'
  surface-container-high: '#eae7e9'
  surface-container-highest: '#e4e2e4'
  on-surface: '#1b1b1d'
  on-surface-variant: '#45464d'
  inverse-surface: '#303032'
  inverse-on-surface: '#f3f0f2'
  outline: '#76777d'
  outline-variant: '#c6c6cd'
  surface-tint: '#565e74'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#131b2e'
  on-primary-container: '#7c839b'
  inverse-primary: '#bec6e0'
  secondary: '#505f76'
  on-secondary: '#ffffff'
  secondary-container: '#d0e1fb'
  on-secondary-container: '#54647a'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#271901'
  on-tertiary-container: '#98805d'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#dae2fd'
  primary-fixed-dim: '#bec6e0'
  on-primary-fixed: '#131b2e'
  on-primary-fixed-variant: '#3f465c'
  secondary-fixed: '#d3e4fe'
  secondary-fixed-dim: '#b7c8e1'
  on-secondary-fixed: '#0b1c30'
  on-secondary-fixed-variant: '#38485d'
  tertiary-fixed: '#fcdeb5'
  tertiary-fixed-dim: '#dec29a'
  on-tertiary-fixed: '#271901'
  on-tertiary-fixed-variant: '#574425'
  background: '#fcf8fa'
  on-background: '#1b1b1d'
  surface-variant: '#e4e2e4'
typography:
  display-lg:
    fontFamily: Inter
    fontSize: 36px
    fontWeight: '700'
    lineHeight: '1.2'
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.3'
  headline-sm:
    fontFamily: Inter
    fontSize: 18px
    fontWeight: '600'
    lineHeight: '1.4'
  body-lg:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.5'
  body-md:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.5'
  body-sm:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: '400'
    lineHeight: '1.4'
  data-mono:
    fontFamily: Inter
    fontSize: 14px
    fontWeight: '600'
    lineHeight: '1.4'
    letterSpacing: 0.02em
  label-caps:
    fontFamily: Inter
    fontSize: 11px
    fontWeight: '700'
    lineHeight: '1'
    letterSpacing: 0.05em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  touch-target-min: 44px
  gutter: 1rem
  margin-mobile: 1rem
  margin-desktop: 2rem
  container-max: 1440px
---

## Brand & Style
The design system is engineered for the high-stakes environment of PT Mattel West Plant. It prioritizes **Operational Excellence** and **Cognitive Efficiency**. The brand personality is clinical, reliable, and transparent, ensuring that floor operators and plant managers can make split-second decisions without visual noise.

The design style follows a **Corporate / Modern** aesthetic with a heavy emphasis on **Functional Minimalism**. By utilizing high-density layouts, clear visual hierarchies, and a utilitarian approach to whitespace, the system ensures that complex manufacturing data—from cycle times to machine statuses—remains accessible and actionable. The interface is built to feel like an extension of the factory machinery: precise, robust, and tireless.

## Colors
This design system utilizes a palette rooted in industrial "Andon" logic. The primary colors are deep slates to provide a professional, authoritative foundation, while the functional palette communicates machine state instantaneously.

- **Primary & Neutral:** Deep Slate (#0f172a) is used for navigation and high-level structure to ground the UI. Slate Gray (#64748b) acts as the neutral state for inactive machines or "Off" statuses.
- **Functional (Andon):** 
    - **Emerald Green:** Indicates "Running" or "Within Spec."
    - **Rose Red:** Indicates "Breakdown" or "Immediate Action Required."
    - **Amber Orange:** Indicates "Downtime" or "Warning."
    - **Sky Blue:** Reserved for "Changeover," "Maintenance," or "Information."
- **Surfaces:** Use a clean White (#ffffff) for cards and data containers against a Light Gray (#f8fafc) page background to create subtle but clear depth.

## Typography
**Inter** is the exclusive typeface for this design system, chosen for its exceptional legibility in data-heavy environments and its neutral, professional tone. 

The type scale is optimized for "At-a-Glance" reading. **Data-mono** (Inter with medium/semibold weights) should be used for machine IDs, part numbers, and timestamps to ensure character distinction. Large display sizes are reserved for critical OEE (Overall Equipment Effectiveness) percentages and target counts. Small labels should use all-caps with increased letter spacing to differentiate headers from live data values in dense tables.

## Layout & Spacing
The layout follows a **Fluid 12-Column Grid** system, compatible with Bootstrap 5 breakpoints. 

### Spacing Philosophy:
1. **Touch-First Density:** While the system is data-dense, all interactive elements (buttons, toggles, row expanders) must adhere to a minimum **44px x 44px** hit area to accommodate gloved or fast-moving operator input.
2. **The "Zone" Model:** Layouts are divided into three zones: 
    - **Header/Global:** Persistent plant-wide stats.
    - **Sidebar:** Navigation and Area selection.
    - **Main View:** High-density dashboards, Kanban boards, or Data Tables.
3. **Reflow:** On mobile/tablet views, machine cards stack vertically, but the primary OEE metric remains fixed at the top of each card for continuous monitoring.

## Elevation & Depth
To maximize performance and visual clarity on industrial touchscreens, this design system avoids heavy shadows. Instead, it utilizes **Low-Contrast Outlines** and **Tonal Layers**.

- **Cards & Containers:** Use a 1px border of Slate-200 (#e2e8f0) to define boundaries. 
- **Z-Axis:** 
    - **Level 0 (Background):** Slate-50 (#f8fafc) - The canvas.
    - **Level 1 (Cards):** White (#ffffff) - The primary workspace.
    - **Level 2 (Modals/Popovers):** White with a soft, 10% opacity neutral shadow to indicate temporary focus.
- **Active State:** Use a 2px colored border (the Andon status color) on the left side of cards to indicate machine state without overwhelming the entire surface with color.

## Shapes
The shape language is **Soft** (roundedness level 1). This provides a subtle modern feel while maintaining the rigid structure expected of industrial software. 

- **Standard Elements:** 0.25rem (4px) corner radius for buttons, input fields, and small UI elements.
- **Containers:** 0.5rem (8px) for cards and main dashboard panels.
- **Status Indicators:** Small status dots or "pills" used in tables should use the maximum radius (pill-shaped) to distinguish them from functional buttons.

## Components

### Machine Status Cards
The centerpiece of the MES. Each card must display:
- **Header:** Machine ID and current state (Andon color-coded).
- **Body:** Current Job ID, Operator Name, and a "Mini-Timeline" showing the last 4 hours of performance.
- **Footer:** Direct action buttons (Pause, Report Issue, Changeover) with minimum 44px height.

### Data Tables
Designed for high-volume part tracking.
- **Compact Rows:** 40px height for view-only, 48px for interactive.
- **Sticky Headers:** Always visible during scroll.
- **Contrast:** Alternating row stripes (Zebra striping) using Slate-50 to aid horizontal eye tracking across many columns.

### Kanban Boards
Used for "Work in Progress" (WIP) tracking.
- **Cards:** Simplified versions of machine cards.
- **Columns:** Fixed width with horizontal scrolling on tablets.
- **Drag-and-Drop:** Visual feedback must use the "Info Blue" border during the drag state.

### Time-Series Timelines
Linear bars representing a 24-hour shift.
- **Colors:** Segments are colored by status (Green = Run, Red = Down, etc.).
- **Interactivity:** Tapping a segment opens a tooltip with the specific downtime reason code.

### Input Fields
- **Industrial Focus:** Large text (Body-lg) inside fields for readability.
- **States:** Error states must use the Rose Red (#ef4444) for both the border and a small descriptive text icon below the field.
