#  Apple Design Skill Suite

[![Agent Skills Compatible](https://img.shields.io/badge/Agent%20Skills-Compatible-black?style=for-the-badge&logo=apple)](https://github.com/pushkarverse/apple-design)
[![Supported Platforms](https://img.shields.io/badge/Platforms-iOS%20%7C%20macOS%20%7C%20iPadOS%20%7C%20visionOS-blue?style=for-the-badge)](https://developer.apple.com/design/human-interface-guidelines)
[![License: MIT](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)](LICENSE)

> The definitive Apple UI/UX design, fluid interaction physics, and Human Interface Guidelines (HIG) review skill suite for all AI coding agents — engineered for **Antigravity**, **Claude Code**, **OpenAI-compatible agents**, **Cursor**, **Windsurf**, and **OpenHands**.

---

## ✦ Overview

**Apple Design Skill Suite** transforms AI coding agents into two expert minds at once:
1. **The Senior HIG Auditor:** An uncompromising Apple platform reviewer with instant recall of 124 official guideline documents, catching accessibility defects, App Store rejection risks (Apple Pay Guideline 4.9), Liquid Glass misuse, and viewport layout constraints.
2. **The Studio Design Director:** An interaction lead grounded in Apple's WWDC physics, continuous curvature (G2 squircles), size-dependent optical typography tracking, and natural interruptible spring motion.

Whether you are building native **SwiftUI**, edge-to-edge **Web (React/Next.js)**, **React Native**, **Flutter**, or **Tauri/Electron** desktop apps, this suite ensures your interfaces stop feeling like generic computers and start feeling like fluid extensions of the user.

---

## ⚡︎ Quick Installation

### Option 1: Universal Agent Skills CLI (Claude Code & Antigravity)
Install the entire suite directly into your active agent environment with a single command:
```bash
npx skills add pushkarverse/apple-design
```

### Option 2: Global Antigravity Installation
Clone directly into your global Antigravity skills repository:
```bash
git clone https://github.com/pushkarverse/apple-design.git ~/.gemini/config/skills/apple-design
```

### Option 3: Team Repositories (Git Submodule)
Add the skill suite as a version-controlled submodule in your team codebase:
```bash
git submodule add https://github.com/pushkarverse/apple-design.git .skills/apple-design
```

---

## 🗂 The Multi-Skill Architecture

This repository includes a master core and 6 specialized sub-skills structured like **GSD** and **Ponytail**. Each skill can be invoked individually:

```
apple-design/
├── apple-design/       # Master Knowledge Base (124 HIG pages, WWDC physics & full references)
├── mac-review/         # Specialized UI/UX design review across the 5 Apple lenses
├── mac-audit/          # Deep code-level compliance scanner (Apple Pay & Liquid Glass)
├── mac-motion/         # WWDC fluid spring physics, momentum flick & rubber-banding
├── mac-glass/          # Liquid Glass vs standard materials hierarchy & vibrancy
├── mac-tokens/         # G2 continuous curvature (squircles) & Dynamic Type scales
└── mac-help/           # Quick-reference cheat sheet of Apple HIG rules & formulas
```

---

## 🛠 Available Skills & Commands

| Command | Focus | Example Prompt |
| :--- | :--- | :--- |
| **`@apple-design`** | Master Design Engine | `@apple-design build a native-feeling macOS web app in React & Tailwind` |
| **`@mac-review`** | 5-Lens Design Critique | `@mac-review review this screen mockup and give me a 5-lens audit` |
| **`@mac-audit`** | Codebase Compliance | `@mac-audit scan this checkout page for App Store Guideline 4.9 rejections` |
| **`@mac-motion`** | WWDC Fluid Physics | `@mac-motion build an interruptible bottom sheet with momentum flick in SwiftUI` |
| **`@mac-glass`** | Liquid Glass & Vibrancy | `@mac-glass how do I style a floating tab bar with scroll-edge blur masks?` |
| **`@mac-tokens`** | Tokens & Typography | `@mac-tokens give me the G2 squircle CSS and optical tracking table` |
| **`@mac-help`** | HIG Quick Lookup | `@mac-help what is the minimum touch target for iOS vs macOS?` |

---

## 🔬 Core Engineering Foundations

### 1. Fluid Motion Physics (`references/physics.md`)
* **Zero Latency:** Immediate highlight on `pointerdown`, not `click` or release.
* **Interruptibility:** Any transition can be grabbed and reversed mid-flight; motion starts from live presentation transforms (`getComputedStyle`), never logical targets.
* **Spring Parameters:** Critically damped ($\zeta = 1.0, T_0 = 0.35\text{s}$) by default; momentum bounce ($\zeta \approx 0.8$) only when gesture velocity preceded the motion.
* **Apple Momentum Projection (Exponential Decay):**
  $$\text{projectedDistance} = \left(\frac{v}{1000}\right) \cdot \frac{d}{1 - d} \quad (d \approx 0.998)$$
* **Continuous Rubber-Banding (Soft Boundaries):**
  $$f(x) = \frac{x \cdot L \cdot 0.55}{L + 0.55 \cdot |x|}$$
* **Audio-Haptic Causality:** Multimodal synchronization within $\pm 20\text{ms}$; audio must never lead haptics.

### 2. Materials & Liquid Glass (`references/materials.md`)
* **The 2-Layer Model:**
  * **Functional Layer (Liquid Glass):** Floating chrome (tab bars, toolbars, sidebars, sheets).
  * **Content Layer (Standard Materials):** Ultra-thin, thin, regular, thick.
  * **The Cardinal Rule:** *Never use Liquid Glass in the content layer* (cards, list rows, backgrounds).
* **Regular vs. Clear Variants:** Regular for text legibility; Clear for rich media with optional 35% dark dimming layer.
* **Scroll-Edge Blur Masks:** Floating chrome uses progressive edge masks rather than solid background strips or hard 1px dividers.

### 3. Both iOS (Mobile) & macOS (Desktop) Paradigms (`references/platforms.md`)
* **iOS (Mobile):** $44 \times 44\text{pt}$ touch targets, thumb-zone ergonomics, bottom tab bars (3–5 tabs max), modal sheets with detents ($50\% / 90\%$), Dynamic Island (compact, minimal, expanded), and iOS 18 Controls.
* **macOS (Desktop):** $28 \times 28\text{pt}$ pointer targets, **the Universal Menu Bar rule** (every command reachable from top menu bar), traffic-light window chrome, fluid multi-window resizing, and Settings under App Menu (`Cmd+,`).
* **iPadOS & iPhone Duo:** Responsive size classes (`compact` vs `regular`), tab bar to collapsible sidebar morphing, and fold/hinge reserved regions.
* **visionOS:** $60 \times 60\text{pt}$ gaze hit targets, $20\text{pt}$ ornament overlap, resting gaze line $10^\circ - 15^\circ$ below horizontal, and native HTML `<model>` elements.

### 4. Continuous Curvature (G2 Squircles) & Tokens (`references/tokens.md`)
* **G2 Continuity:** Smooth superellipse curve eliminating circular `border-radius` corner pinches.
* **Figma Alignment:** $60\%$ Corner Smoothing.
* **The 20 pt Optical Breakpoint:** SF Pro Text ($< 20\text{pt}$) with open tracking vs. SF Pro Display ($\ge 20\text{pt}$) with negative tracking.
* **Tabular Numbers:** `font-feature-settings: "tnum" 1, "case" 1, "zero" 1` for jitter-free counters.
* **Display P3 Wide Gamut:** $\approx 25\%$ wider chromatic space than sRGB.
* **Tone-Based Dark Mode Luminance:** `#000000` base canvas stepping up to `#1C1C1E` (systemGray6) for modals.

---

## 🚦 Automated Code Auditor CLI

This suite includes a standalone automated linter that scans codebases for Apple compliance violations:

```bash
# Scan current repository
node apple-design/scripts/audit.mjs .
```

Flags:
- 🚨 **[CRITICAL]** Custom/hand-drawn Apple Pay buttons or logo glyphs in copy (App Store Review Guideline 4.9).
- ⚠️ **[HIGH]** Liquid Glass applied to content cards, lists, or container backgrounds.
- ⚠️ **[HIGH]** Hardcoded screen dimensions (`width: 390px`, `UIScreen.main.bounds`) breaking foldables and dynamic windowing.
- ℹ️ **[MEDIUM]** Fixed bottom footers lacking `env(safe-area-inset-bottom)` padding.

---

## 💻 Cross-Platform Translation Matrix

| Apple Concept | Web / Next.js | React Native | Flutter | SwiftUI / AppKit |
| :--- | :--- | :--- | :--- | :--- |
| **Tab Bar** | `<nav>`, sticky bottom bar | `@react-navigation/bottom-tabs` | `CupertinoTabBar` | `TabView`, `UITabBarController` |
| **Liquid Glass**| `backdrop-filter: blur(24px)` | `expo-blur` (`BlurView`) | `BackdropFilter` | `.glassEffect()`, `UIVisualEffectView` |
| **Squircle** | `corner-shape: squircle` | `borderCurve="continuous"` | `ContinuousRectangleBorder` | `ContinuousRectangle` |
| **Detent Sheet**| `<dialog>`, Vaul sheet | `@gorhom/bottom-sheet` | `showCupertinoModalPopup` | `.sheet(detents: [.medium, .large])` |
| **Safe Areas** | `env(safe-area-inset-*)` | `useSafeAreaInsets()` | `SafeArea` | `.ignoresSafeArea()` |
| **Apple Pay** | `<apple-pay-button>` SDK | `PlatformPayButton` (Stripe) | `ApplePayButton` (`pay`) | `PayWithApplePayButton` |

---

## 🤝 Contributing

Contributions to expand reference guides, add framework translation recipes, or enhance audit rules are warmly welcomed. Please open an issue or submit a pull request!

---

## 📜 Credits & Acknowledgments

Synthesized and unified from foundational open-source engineering and official Apple resources:
- **[Emil Kowalski](https://github.com/emilkowalski)** for the pioneering translation of WWDC fluid interface motion physics to the web platform.
- **[Dick Wu](https://github.com/dickwu)** for the comprehensive 123-page Apple Human Interface Guidelines crawl and cross-platform audit methodology.
- **[Apple Developer](https://developer.apple.com/design/human-interface-guidelines)** for the Human Interface Guidelines, WWDC design sessions, and official Design Resources.

---

## 📄 License

MIT © [Pushkar](https://github.com/pushkarverse)
