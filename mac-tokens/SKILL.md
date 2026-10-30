---
name: mac-tokens
description: Apple design tokens, G2 continuous curvature (squircles / superellipses), Dynamic Type typography scales, optical tracking tables, 4pt/8pt spacing rhythm, and SF Symbols 6 animation presets. Use when configuring design systems, Tailwind tokens, CSS variables, or styling typography.
---

# mac-tokens

You are an expert on Apple design tokens, continuous curvature geometry, and typography craft.

---

## 1. Continuous Curvature (G2 Squircles)

### The Mathematics: G1 vs G2 Continuity
Standard CSS `border-radius` uses **G1 (Tangent) Continuity**, creating an abrupt jump in curvature where the straight line meets the circular arc (causing an optical "pinch").

Apple uses **G2 (Curvature) Continuity**, also known as a **squircle** or superellipse:

$$\left|\frac{x}{a}\right|^n + \left|\frac{y}{b}\right|^n = 1 \quad (n \approx 4 \text{ to } 5)$$

Curvature begins at zero along the flat edge and accelerates smoothly into the corner, reflecting light without visible seams.

### Web & Cross-Platform Implementation
- **CSS Native (`corner-shape`):**
  ```css
  .squircle-card {
    border-radius: 20px;
    corner-shape: squircle; /* Modern Chromium baseline */
  }
  ```
- **Figma Alignment:** Set **Corner Smoothing** to **60%** (the standard iOS corner curve).
- **React Native (iOS):** Add `borderCurve: 'continuous'` to View styles.
- **Flutter:** Use `ContinuousRectangleBorder(borderRadius: ...)`.

---

## 2. Dynamic Type & Optical Tracking Tables

| Style | Size (pt) | Leading (Line-Height) | Default Weight | Tracking (Letter-Spacing) |
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

---

## 4. SF Symbols 6 Animation Presets

| Preset | Motion Description | Primary Use Case |
| :--- | :--- | :--- |
| **Wiggle** | Rapid rhythmic oscillation around anchor | Attention needed, error warning, shake to edit |
| **Rotate** | Continuous or stepped 360-degree spin | Syncing, refreshing, loading, compass |
| **Breathe** | Smooth, organic scale & opacity pulse | Active recording, live streaming, calm state |
| **Bounce** | Quick upward jump with spring settle | Action confirmed, item added to cart, favorite |
| **Pulse** | Opacity flash across layers | Heart rate, transmitting signal, ping |
| **Replace** | Morph or cross-fade between two symbols | Play $\leftrightarrow$ Pause, Lock $\leftrightarrow$ Unlock |

---

## Master Knowledge Base Connection

`mac-tokens` connects directly to `apple-design/references/`:
- **Tokens & Formulas:** `apple-design/references/tokens.md`
- **HIG Typography:** `apple-design/references/hig/typography.md`
- **HIG SF Symbols:** `apple-design/references/hig/sf-symbols.md`
- **HIG Layout & Sizing:** `apple-design/references/hig/layout.md`

