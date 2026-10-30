---
name: mac-glass
description: Apple Liquid Glass and materials hierarchy. Use when styling translucent chrome, glassmorphism, blur effects, navigation bars, sidebars, sheets, or dark mode appearances across Web, SwiftUI, React Native, and Flutter.
---

# mac-glass

You are an expert on Apple’s material architecture and Liquid Glass. You enforce correct material layering, optical translucency, vibrancy, and scroll-edge effects.

---

## The Two-Layer Architecture

```
┌────────────────────────────────────────────────────────┐
│ 1. FUNCTIONAL LAYER (Liquid Glass)                     │
│    Floating Chrome: Tab Bars · Toolbars · Sidebars     │
│    Floating Surfaces: Sheets · Popovers · Alerts       │
├────────────────────────────────────────────────────────┤
│ 2. CONTENT LAYER (Standard Materials / Solid Opaque)   │
│    App Backgrounds · Cards · List Rows · Media Text   │
│    Depth via: Ultra-Thin · Thin · Regular · Thick      │
└────────────────────────────────────────────────────────┘
```

### The Cardinal Rule
**Never use Liquid Glass in the content layer.**
Liquid Glass exists exclusively to elevate and distinguish floating functional navigation controls above content. Applying glass to content cards, list rows, or container backgrounds degrades contrast and clutters the visual hierarchy.

*Exception:* Transient interactive elements inside the content layer (such as the thumb of an active slider or toggle) take on a glass appearance only while actively dragged.

---

## Liquid Glass Variants

| Variant | Optical Behavior | Intended Surface |
| :--- | :--- | :--- |
| **Regular** | Blurs and adjusts underlying luminosity. Text-safe, high legibility. | Tab bars, navigation bars, sidebars, alerts, popovers. Any surface with text. |
| **Clear** | Highly translucent, minimal tinting, prioritizes background clarity. | Floating media playback controls, photo/video overlays. |

### Clear Glass Dimming Layer
When floating clear glass over bright, high-contrast imagery or video:
- Apply a **35% dark dimming layer** behind the clear glass control to preserve text and icon contrast.
- If the background media is already dark or handled by AV player chrome, omit the extra dimming layer.

---

## Vibrancy & Color Rules

- **No Inherent Color:** Liquid Glass takes on the color of scrolling content behind it.
- **Monochrome Control Labels:** Navigation labels, tab titles, and icons on glass default to monochrome.
- **Color Budget on Controls:** Color is reserved for **primary actions** (e.g., Done button) or **status indicators** (e.g., unread badge, selected tab). Limit prominent colored glass buttons to **1 or 2 per screen**.
- **Four Vibrancy Levels:** `label` (primary text), `secondaryLabel` (subtitles), `tertiaryLabel` (placeholders), `quaternaryLabel` (subtle separators). Avoid quaternary on thin/ultra-thin materials.

---

## Scroll-Edge Effects

- Allow full-screen content to scroll completely underneath toolbars and tab bars.
- Where scrolling content enters the edge of the floating bar, apply a smooth **scroll-edge blur and fade mask** rather than a solid background or hard 1px divider.

---

## Accessibility Overrides

1. **Reduce Transparency (`prefers-reduced-transparency: reduce`):** Replace blur and translucency with an opaque, solid background.
2. **Increase Contrast (`prefers-contrast: more`):** Apply a defined 1px high-contrast border and raise fill opacity to $\ge 95\%$.
3. **Reduce Motion (`prefers-reduced-motion: reduce`):** Replace liquid refraction and morphing animations with simple opacity cross-fades.

---

## Web CSS Recipe

```css
.apple-glass-bar {
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: blur(24px) saturate(140%);
  -webkit-backdrop-filter: blur(24px) saturate(140%);
  border-top: 1px solid rgba(255, 255, 255, 0.4);
}

@media (prefers-color-scheme: dark) {
  .apple-glass-bar {
    background: rgba(30, 30, 30, 0.75);
    border-top: 1px solid rgba(255, 255, 255, 0.12);
  }
}

@media (prefers-reduced-transparency: reduce) {
  .apple-glass-bar {
    background: #ffffff;
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}
```

---

## Master Knowledge Base Connection

`mac-glass` connects directly to `apple-design/references/`:
- **Materials Hierarchy:** `apple-design/references/materials.md`
- **HIG Materials Page:** `apple-design/references/hig/materials.md`
- **Curated Liquid Glass:** `apple-design/references/hig/liquid-glass.md`
- **Color on Glass:** `apple-design/references/hig/color.md`
- **Web & Safari Insets:** `apple-design/references/web.md`

