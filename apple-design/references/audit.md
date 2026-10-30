# Code Audit & Pattern Rules

> Concrete search patterns, regex rules, and AST anti-patterns for automated and manual source code audits.

---

## 1. Apple Pay Compliance (Critical — App Store Guideline 4.9)

Any violation here will cause an immediate App Store review rejection or merchant compliance penalty.

| Pattern to Search | Why It Fails | Required Fix |
| :--- | :--- | :--- |
| `\uF8FF\s*Pay`, `&#xF8FF;.*Pay`, Apple logo SVG next to "Pay" | Violates "Never use the Apple logo to represent the name Apple in text" (`apple-pay.md`) | Use official `<apple-pay-button>` or unaltered artwork mark |
| `fa-apple-pay`, `fa-cc-apple-pay`, `siApplepay`, `ionicons/apple-pay` | Third-party icon redraws are strictly forbidden | Use official Apple artwork download |
| CSS applied to Apple Pay mark: `border-radius`, `box-shadow`, `transform`, `filter` | "No alterations other than height" permitted on the mark | Remove custom CSS; set height only |
| Custom `<button>` or `TouchableOpacity` with text "Apple Pay" or "Buy with Apple Pay" | Custom buttons are rejected under Guideline 4.9 | Use native `PayWithApplePayButton`, `<apple-pay-button>`, or payment SDK native button |
| Text-only "Apple Pay" in a checkout list where Visa/Mastercard show logos | Inconsistent payment marks | Use official graphical Apple Pay mark |
| Button hidden or greyed out because a variant/size was not selected | "Don't hide or disable Apple Pay button" | Show button; prompt for options on tap |

---

## 2. Liquid Glass & Material Misuse (High)

| Pattern to Search | Why It Fails | Required Fix |
| :--- | :--- | :--- |
| `glassEffect` / `UIGlassEffect` on list rows, cards, or background containers | Liquid Glass is strictly forbidden in the content layer (`materials.md`) | Use standard materials (`.background(.regularMaterial)`) or solid surfaces |
| More than 3 custom glass views nested without container grouping | Degrades legibility; excessive GPU shader load | Consolidate chrome into a single floating bar; use `GlassEffectContainer` |
| `.clear` glass applied over busy text or lists | Clear variant is reserved for rich media (photos/videos) | Use `.regular` glass variant |
| Solid opaque color painted behind toolbars (`toolbarBackground(.black)`) | Blocks dynamic material and eliminates scroll-edge effect | Remove solid underlay; let content scroll underneath |
| No check for `prefers-reduced-transparency` or `isReduceTransparencyEnabled` | Fails accessibility compliance | Add opaque fallback when transparency reduction is active |

---

## 3. Fixed-Width & Layout Bugs (High)

| Pattern to Search | Why It Fails | Required Fix |
| :--- | :--- | :--- |
| Hardcoded widths: `width: 390px`, `width: 393px`, `.frame(width: 402)` | Breaks on dynamic sizing, split screen, and foldable devices (iPhone Duo) | Use responsive container queries, flexbox, or size classes |
| `UIScreen.main.bounds` | Deprecated; returns screen size instead of current window frame | Use window frame or `view.bounds` |
| `userInterfaceIdiom == .phone` used to toggle between single and split views | Dual-screen foldables and multitasking require size-class branching | Branch on `horizontalSizeClass == .compact` vs `.regular` |
| Absolute-positioned footers missing bottom safe-area insets | Content collides with Home Indicator bar | Add `padding-bottom: max(16px, env(safe-area-inset-bottom))` |

---

## 4. Touch Target & Accessibility Violations (Critical / High)

| Pattern to Search | Why It Fails | Required Fix |
| :--- | :--- | :--- |
| Interactive buttons smaller than $44 \times 44\text{pt}$ (mobile) or $28 \times 28\text{pt}$ (desktop) without hit padding | Fails Apple accessibility guidelines (`accessibility.md`) | Expand hit bounds to at least $44 \times 44\text{pt}$ |
| Icon-only buttons lacking `accessibilityLabel` or `aria-label` | Screen readers cannot voice the action | Add descriptive accessibility label |
| Hardcoded text colors with contrast ratio $< 4.5:1$ against surface | Unreadable under poor lighting | Use semantic colors (`label`, `secondaryLabel`) |
| Animations lacking `prefers-reduced-motion` fallbacks | Vestibular discomfort for motion-sensitive users | Replace springs with opacity cross-fade |

