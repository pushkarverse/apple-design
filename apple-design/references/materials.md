# Materials & Liquid Glass

> Comprehensive guide to Apple's material hierarchy, Liquid Glass, standard materials, vibrancy effects, and cross-platform implementation.

---

## 1. The Two-Layer Architecture

Apple interfaces are strictly divided into two distinct visual layers:

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
Liquid Glass exists exclusively to elevate and distinguish the floating functional navigation controls above content. Applying glass to content cards, list rows, or container backgrounds creates visual confusion, high cognitive load, and catastrophic contrast failures.

*Exception:* Transient interactive elements inside the content layer (such as the thumb of a slider or toggle) take on a glass appearance only while actively dragged or manipulated.

---

## 2. Liquid Glass Variants

| Variant | Optical Behavior | Intended Surface |
| :--- | :--- | :--- |
| **Regular** | Blurs and adjusts underlying luminosity. Text-safe, high legibility. | Tab bars, navigation bars, sidebars, alerts, popovers. Any surface containing primary text. |
| **Clear** | Highly translucent, minimal tinting, prioritizes background clarity. | Floating media playback controls, photo/video overlays. |

### Clear Glass Dimming Layer
When floating clear glass over bright, high-contrast imagery or video:
- Apply a **35% dark dimming layer** behind the clear glass control to preserve text and icon contrast.
- If the background media is already dark or handled by standard AV player chrome, omit the extra dimming layer.

---

## 3. Standard Materials (Content Layer)

Inside the content layer, use Apple's four standard material thicknesses to create structural depth:

1. **Ultra-Thin:** Most translucent. Keeps maximum surrounding context visible. Avoid quaternary labels on this material.
2. **Thin:** Light separation for nested group containers.
3. **Regular (Default):** Balanced contrast and background diffusion.
4. **Thick:** Maximum opacity. Best for dense data, small typography, and complex list items.

---

## 4. Vibrancy & Color Rules

- **Liquid Glass has no inherent color:** It dynamically pulls color and luminosity from the scrolling content beneath it.
- **Monochrome Control Labels:** Navigation labels, tab titles, and icons on glass default to monochrome (dark gray/black in light mode, light gray/white in dark mode).
- **Color Budget on Controls:**
  - Color is reserved for **primary actions** (e.g., Done button) or **status indicators** (e.g., unread badge, selected tab).
  - Limit prominent colored glass buttons to **1 or 2 per screen**.
  - To express brand identity, move color into the **content layer** so it naturally reflects through the translucent glass chrome.
- **Four Vibrancy Levels:**
  - `label` (Primary content, highest contrast)
  - `secondaryLabel` (Supporting subtitles, timestamps)
  - `tertiaryLabel` (Placeholder text, subtle dividers)
  - `quaternaryLabel` (Subtle watermarks, disabled hints; avoid on thin/ultra-thin materials)

---

## 5. Scroll-Edge Effects

Do not use hard 1px borders or solid opaque background strips beneath sticky headers or tab bars:
- Allow full-screen content to scroll completely underneath toolbars and tab bars.
- Where scrolling content enters the edge of the floating bar, apply a smooth **scroll-edge blur and fade mask**.
- As content settles at the top or bottom of a scroll view, the material dynamically relaxes its luminosity.

---

## 6. Accessibility & System Overrides

Your materials must gracefully respect system accessibility signals:

1. **Reduce Transparency (`prefers-reduced-transparency: reduce` / `UIAccessibility.isReduceTransparencyEnabled`):**
   - Replace blur and translucency with an opaque, solid background (`#FFFFFF` in light mode, `#1C1C1E` in dark mode).
2. **Increase Contrast (`prefers-contrast: more`):**
   - Apply a defined 1px high-contrast border and increase background fill opacity to $\ge 95\%$.
3. **Reduce Motion (`prefers-reduced-motion: reduce`):**
   - Disable surface morphing, liquid glass refraction shifts, and springy bounces; replace with simple instantaneous opacity cross-fades.

---

## 7. Cross-Platform Code Recipes

### Web (CSS)
```css
/* Functional Floating Chrome */
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

/* Accessibility Fallback */
@media (prefers-reduced-transparency: reduce) {
  .apple-glass-bar {
    background: #ffffff;
    backdrop-filter: none;
    -webkit-backdrop-filter: none;
  }
}
```

### SwiftUI
```swift
// Native Liquid Glass in SwiftUI
.toolbar {
    ToolbarItem(placement: .primaryAction) {
        Button("Done") { commit() }
            .buttonStyle(.glassProminent)
    }
}
.background(.regularMaterial) // Standard content material
```

### React Native
```typescript
import { BlurView } from 'expo-blur';

<BlurView intensity={80} tint="systemMaterial" style={styles.floatingBar}>
  {/* Content */}
</BlurView>
```

### Flutter
```dart
ClipRRect(
  child: BackdropFilter(
    filter: ImageFilter.blur(sigmaX: 20.0, sigmaY: 20.0),
    child: Container(
      color: CupertinoColors.systemBackground.resolveFrom(context).withOpacity(0.7),
      child: child,
    ),
  ),
);
```

