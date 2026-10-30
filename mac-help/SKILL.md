---
name: mac-help
description: Quick-reference cheat sheet for all Apple Human Interface Guidelines, minimum hit targets, spring physics values, and cross-platform framework vocabulary. Use when you need a fast lookup of an Apple rule, constant, or formula.
---

# mac-help

Quick-reference cheat sheet for Apple design rules, concrete values, physics constants, and framework vocabulary.

---

## 1. Quick Concrete Values

| Property | Platform | Value | Rule / Context |
| :--- | :--- | :--- | :--- |
| **Touch Target** | Mobile (iOS) | **$44 \times 44\text{pt}$** | $28\times 28\text{pt}$ min if $\ge 10\text{pt}$ hit padding |
| **Pointer Target**| Desktop (macOS)| **$28 \times 28\text{pt}$** | $20\times 20\text{pt}$ absolute minimum |
| **Body Font** | Mobile (iOS) | **$17\text{pt}$** | SF Pro Text, Leading $22\text{pt}$ |
| **Body Font** | Desktop (macOS)| **$13\text{pt}$** | System UI font stack |
| **Contrast (Small Text)** | All | **$4.5:1$** | Text $\le 17\text{pt}$ |
| **Contrast (Large Text)** | All | **$3:1$** | Text $\ge 18\text{pt}$ or bold |
| **Default UI Spring** | All | **$\zeta = 1.0, T_0 = 0.35\text{s}$** | Critically damped, zero bounce |
| **Flick Momentum Spring**| All | **$\zeta = 0.8, T_0 = 0.35\text{s}$** | Subtle organic bounce |
| **Inertial Deceleration** | All | **$d \approx 0.998$** | Exponential decay momentum |
| **Rubber-Band Constant** | All | **$c = 0.55$** | Progressive edge resistance |
| **Figma Corner Smoothing**| Design | **$60\%$** | Apple continuous squircle |
| **Clear Glass Dimming** | All | **$35\%$ dark** | Behind clear glass over bright media |

---

## 2. Core Architectural Rules

1. **2-Layer Rule:** Liquid Glass belongs strictly on floating functional chrome (tab bars, toolbars, sidebars, sheets). **Never** in content cards or lists.
2. **Apple Pay (Guideline 4.9):** Never draw custom Apple Pay buttons or use glyphs in text (`Pay`). Use native SDK buttons or unaltered artwork marks.
3. **Menu Bar Rule (macOS):** Every single action and toolbar button must be reachable from the top system Menu Bar.
4. **Thumb-Zone Rule (iOS):** Primary navigation and primary buttons live in the bottom half of the screen.
5. **Interruptibility:** Every animation must be interruptible. Read and start from live presentation transforms, not target values.
6. **Zero Latency:** Highlight on `pointerdown`, not `click` or release.
7. **Safari Edge-to-Edge:** Use `<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">` with `env(safe-area-inset-*)`.

---

## 3. Framework Vocabulary Cheat Sheet

| Apple HIG | Web (React/CSS) | React Native | Flutter | Tauri / Electron |
| :--- | :--- | :--- | :--- | :--- |
| **Tab Bar** | `<nav>`, bottom bar | `@react-navigation/bottom-tabs` | `CupertinoTabBar` | Navigation rail / tabs |
| **Sidebar** | CSS flex sidebar | `@react-navigation/drawer` | `NavigationRail` | Native window sidebar |
| **Sheet (Detents)**| `<dialog>`, Vaul sheet | `@gorhom/bottom-sheet` | `showCupertinoModalPopup` | Native sheet modal |
| **Liquid Glass** | `backdrop-filter: blur()` | `BlurView` (`expo-blur`) | `BackdropFilter` | Window vibrancy |
| **Squircle** | `corner-shape: squircle` | `borderCurve: 'continuous'` | `ContinuousRectangleBorder` | Web CSS squircle |
| **Dynamic Type** | CSS `rem`, `clamp()` | `allowFontScaling` | `MediaQuery.textScalerOf` | System zoom / font scale |

---

## Master Knowledge Base Connection

`mac-help` connects directly to `apple-design/references/`:
- **Full HIG Directory:** `apple-design/references/hig/` (124 pages)
- **HIG Index:** `apple-design/references/lookup.md`
- **Platform Paradigms:** `apple-design/references/platforms.md`
- **Cross-Platform Stacks:** `apple-design/references/stacks.md`

