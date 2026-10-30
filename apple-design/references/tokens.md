# Design Tokens & Specifications

> Exact values, formulas, and tokens for Apple design systems: continuous curvature (squircles), Dynamic Type scales, optical tracking, spacing grid, SF Symbols animations, and semantic color palettes.

---

## 1. Continuous Curvature (G2 Squircles)

### The Mathematics: G1 vs G2 Continuity
Standard CSS `border-radius` uses **G1 (Tangent) Continuity**, creating an abrupt jump in curvature between the straight line and circular arc. This produces an optical "pinch" or "bubbly" corner.

Apple hardware and software utilize **G2 (Curvature) Continuity**, also known as a **squircle** or superellipse:

$$\left|\frac{x}{a}\right|^n + \left|\frac{y}{b}\right|^n = 1 \quad (n \approx 4 \text{ to } 5)$$

Curvature changes gradually from 0 at the straight edge to its maximum radius at the corner vertex, reflecting light organically without visible seams.

### Web & Cross-Platform Implementation

#### Native CSS (`corner-shape`)
```css
.apple-card {
  border-radius: 20px;
  corner-shape: squircle; /* Superellipse curve (Chromium / modern baseline) */
}
```

#### Figma / Sketch Export Alignment
In Figma, set **Corner Smoothing** to **60%** (the standard iOS corner curve).

#### SVG Squircle Clip-Path Fallback
```html
<svg width="0" height="0">
  <clipPath id="squircle-24" clipPathUnits="objectBoundingBox">
    <path d="M 0,0.5 C 0,0.1 0.1,0 0.5,0 C 0.9,0 1,0.1 1,0.5 C 1,0.9 0.9,1 0.5,1 C 0.1,1 0,0.9 0,0.5 Z" />
  </clipPath>
</svg>
```

---

## 2. Dynamic Type & Typography Scales

| Style | Size (pt) | Leading / Line-Height | Default Weight | Tracking (Letter-Spacing) |
| :--- | :--- | :--- | :--- | :--- |
| **Large Title** | `34pt` | `41pt` | Regular | `-0.022em` (`-0.75px`) |
| **Title 1** | `28pt` | `34pt` | Regular | `-0.019em` (`-0.53px`) |
| **Title 2** | `22pt` | `28pt` | Regular | `-0.015em` (`-0.33px`) |
| **Title 3** | `20pt` | `25pt` | Regular | `-0.012em` (`-0.24px`) |
| **Headline** | `17pt` | `22pt` | Semi-Bold | `-0.010em` (`-0.17px`) |
| **Body** | `17pt` | `22pt` | Regular | `-0.005em` (`-0.08px`) |
| **Callout** | `16pt` | `21pt` | Regular | `0.000em` (`0.00px`) |
| **Subheadline** | `15pt` | `20pt` | Regular | `+0.005em` (`+0.08px`) |
| **Footnote** | `13pt` | `18pt` | Regular | `+0.010em` (`+0.13px`) |
| **Caption 1** | `12pt` | `16pt` | Regular | `+0.015em` (`+0.18px`) |
| **Caption 2** | `11pt` | `13pt` | Regular | `+0.020em` (`+0.22px`) |

### The 20 pt Optical Breakpoint Rule (SF Pro Text vs. SF Pro Display)
- **SF Pro Text (`< 20 pt`):** Taller x-height, wider proportions, generous counters, and open tracking to maintain legibility on dense Retina screens (6 pt to 19.5 pt).
- **SF Pro Display (`≥ 20 pt`):** Tighter letterforms, narrower proportions, delicate stroke contrast, and negative tracking for display cohesion (20 pt and above).
- **New York (Editorial Serif):** 4 optical cuts: Small ($\le 19\text{pt}$), Medium ($20 - 35\text{pt}$), Large ($36 - 53\text{pt}$), and Extra Large ($\ge 54\text{pt}$).

### Tabular Numbers & Font Features
- **Tabular Figures (`tnum`):** Mandatory for countdown clocks, financial data, and progress tickers so digits do not horizontally jitter:
  ```css
  font-variant-numeric: tabular-nums;
  font-feature-settings: "tnum" 1, "case" 1, "zero" 1;
  ```
- **Case-Sensitive Punctuation (`case`):** Centers colons and dashes with capital numbers (e.g. `12:45:00`).

### Optical Tracking Rule
**Letter-spacing is size-dependent, never fixed.**
- Display and Title text ($\ge 20\text{pt}$): **Negative tracking** (tightens letter pairs as glyphs expand).
- Body text ($16\text{pt} - 17\text{pt}$): **Neutral tracking** ($\approx 0$).
- Small text ($\le 15\text{pt}$): **Positive tracking** (adds breathing room for optical legibility).

---

## 3. Spacing Grid (4pt / 8pt System)

Apple layouts adhere to an $8\text{pt}$ baseline rhythm with $4\text{pt}$ sub-grid half-steps:

| Token | Value | Common Usage |
| :--- | :--- | :--- |
| `space-1` | `4px` | Icon-to-text gap, badge padding |
| `space-2` | `8px` | Related element gap, tight list row padding |
| `space-3` | `12px` | Control inner padding, card compact margins |
| `space-4` | `16px` | Standard screen margin (compact mobile), list item gap |
| `space-5` | `20px` | Card content padding, section separators |
| `space-6` | `24px` | Regular screen margins, group header offset |
| `space-8` | `32px` | Large section spacing, modal header gap |
| `space-12`| `48px` | Major section breaks, hero spacing |

---

## 4. SF Symbols 6 Animation Presets

SF Symbols 6 introduces 6 core animation behaviors for SwiftUI, AppKit, UIKit, and web icon systems:

| Preset | Motion Description | Primary Use Case |
| :--- | :--- | :--- |
| **Wiggle** | Rapid rhythmic oscillation around anchor | Attention needed, error warning, shake to edit |
| **Rotate** | Continuous or stepped 360-degree spin | Syncing, refreshing, loading, compass |
| **Breathe** | Smooth, organic scale & opacity pulse | Active recording, live streaming, calm state |
| **Bounce** | Quick upward jump with spring settle | Action confirmed, item added to cart, favorite |
| **Pulse** | Opacity flash across layers | Heart rate, transmitting signal, ping |
| **Replace** | Morph or cross-fade between two symbols | Play $\leftrightarrow$ Pause, Lock $\leftrightarrow$ Unlock |

---

## 5. Semantic Color Tokens

Colors must be defined by their role, automatically adapting between light and dark appearances:

| Semantic Role | Light Mode Hex | Dark Mode Hex | Usage |
| :--- | :--- | :--- | :--- |
| `systemBackground` | `#FFFFFF` | `#000000` | Primary app canvas background |
| `secondarySystemBackground` | `#F2F2F7` | `#1C1C1E` | Grouped containers, table cells |
| `tertiarySystemBackground` | `#FFFFFF` | `#2C2C2E` | Nested cards, layered panels |
| `label` | `#000000` | `#FFFFFF` | Primary body and heading text |
| `secondaryLabel` | `#3C3C43` (60%) | `#EBEBF5` (60%) | Subtitles, secondary metadata |
| `tertiaryLabel` | `#3C3C43` (30%) | `#EBEBF5` (30%) | Placeholders, inactive icons |
| `quaternaryLabel` | `#3C3C43` (18%) | `#EBEBF5` (16%) | Subtle separators, watermarks |
| `systemBlue` (Accent) | `#007AFF` | `#0A84FF` | Primary action buttons, active links |
| `systemRed` (Destructive) | `#FF3B30` | `#FF453A` | Delete, destructive alerts |
| `systemGreen` | `#34C759` | `#30D158` | Success states, confirmations |
| `systemGray` (Neutral) | `#8E8E93` | `#8E8E93` | Unselected tab icons, subtle borders |

### System Gray Palette (Adaptive Grays 1 through 6)
| Token | Light Hex | Dark Hex | Role |
| :--- | :--- | :--- | :--- |
| `systemGray` | `#8E8E93` | `#8E8E93` | Neutral base gray |
| `systemGray2` | `#AEAEB2` | `#636366` | Secondary borders, disabled controls |
| `systemGray3` | `#C7C7CC` | `#48484A` | Separator lines, slider tracks |
| `systemGray4` | `#D1D1D6` | `#3A3A3C` | Grouped table headers, tertiary fills |
| `systemGray5` | `#E5E5EA` | `#2C2C2E` | Search bar backgrounds, interior cards |
| `systemGray6` | `#F2F2F7` | `#1C1C1E` | Recessed canvas, grouped container base |

### Dark Mode Luminance Elevation (`UIUserInterfaceLevel`)
In Dark Mode, drop shadows are invisible against pure black canvases. Apple uses tone-based luminance stepping:
- **Base Level (Full-screen canvas):** `systemBackground` = `#000000` (pure OLED black).
- **Elevated Level (Modals, Sheets, Popovers):** Steps up to `#1C1C1E` (Dark `systemGray6`).
- **Nested Level:** Steps up to `#2C2C2E` (Dark `systemGray5`).

### Display P3 Wide Color Gamut
- Covers $\approx 25\%$ more chromatic space than sRGB (vivid emeralds, cyans, oranges).
- Specify in CSS: `background-color: color(display-p3 0 0.478 1);` with standard sRGB hex fallbacks.

---

## 6. Sound & Audio Design Principles

- **Audio Causality:** Action-to-sound latency must remain under $< 15-20\text{ms}$. Trigger on `pointerdown`, synchronized with haptic actuation.
- **Avoiding Ear Fatigue:**
  - *Stochastic Micro-Variation:* Micro-randomize pitch ($\pm 15-30\text{ cents}$) and amplitude ($\pm 0.5-1.5\text{ dB}$) on repetitive taps to prevent the machine-gun effect.
  - *Fast Rise & Natural Decay:* Rise times $< 5-10\text{ms}$ with exponential decay ($50-200\text{ms}$).
- **Alert Frequency Resonance:** Avoid boosting the human ear canal resonance zone ($2\text{ kHz} - 5\text{ kHz}$). Anchor alert fundamentals in the warm midrange ($400\text{ Hz} - 1.2\text{ kHz}$).

