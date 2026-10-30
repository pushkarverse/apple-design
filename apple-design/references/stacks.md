# Cross-Platform Stacks & Vocabulary

> Universal translation matrix mapping Apple HIG concepts and native components to Web (React/CSS), React Native, Flutter, and Tauri/Electron.

---

## 1. Vocabulary Translation Table

| Apple Concept | Native (SwiftUI / UIKit) | Web / Next.js / CSS | React Native | Flutter | Tauri / Electron |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Tab Bar** | `TabView`, `UITabBarController` | `<nav role="tablist">`, sticky bottom bar | `@react-navigation/bottom-tabs` | `CupertinoTabBar`, `NavigationBar` | Navigation rail or tab list |
| **Sidebar** | `NavigationSplitView`, `NSOutlineView` | CSS 2-column flexbox sidebar | `@react-navigation/drawer` | `NavigationRail` | Native macOS window sidebar |
| **Toolbar / Nav Bar** | `.toolbar`, `UINavigationBar` | `<header>`, sticky top bar | Native stack header | `CupertinoNavigationBar`, `AppBar` | Window title bar toolbar |
| **Sheet (Detents)** | `.sheet(detents: [.medium, .large])` | `<dialog>`, Vaul bottom sheet | `@gorhom/bottom-sheet` | `showCupertinoModalPopup` | Native sheet modal / dialog |
| **Liquid Glass** | `.glassEffect()`, `UIVisualEffectView` | `backdrop-filter: blur() saturate()` | `expo-blur` (`BlurView`) | `BackdropFilter(filter: blur)` | `window-vibrancy`, `vibrancy` |
| **Continuous Corner** | `ContinuousRectangle`, corner radius | `corner-shape: squircle`, SVG clip | `borderCurve="continuous"` (iOS) | `ContinuousRectangleBorder` | Web CSS squircle |
| **Dynamic Type** | `@ScaledMetric`, Dynamic Type fonts | CSS `rem`, `clamp()`, `font-size` | `PixelRatio`, `allowFontScaling` | `MediaQuery.textScalerOf` | Native font zoom scaling |
| **Haptic Feedback** | `UIImpactFeedbackGenerator`, Core Haptics | `navigator.vibrate()` (Vibration API) | `expo-haptics` | `HapticFeedback.lightImpact()` | Native OS IPC haptic triggers |
| **Safe Areas** | `.ignoresSafeArea()`, `safeAreaInsets` | `env(safe-area-inset-*)` | `useSafeAreaInsets()` | `SafeArea` widget | Title bar chrome insets |
| **Size Classes** | `horizontalSizeClass`, `UserInterfaceSizeClass` | Media queries (`min-width`), container queries | `useWindowDimensions()` | `LayoutBuilder`, `MediaQuery.sizeOf` | Responsive window resize events |
| **Apple Pay** | `PayWithApplePayButton` | `<apple-pay-button>` SDK | `PlatformPayButton` (Stripe) | `ApplePayButton` (`pay` package) | Native Web Apple Pay SDK |

---

## 2. Stack-Specific Guidelines

### Web & React / Next.js
- Use `backdrop-filter: blur(24px) saturate(140%)` for floating headers and nav bars.
- Add `@media (prefers-reduced-transparency: reduce)` to disable blur and supply solid backgrounds.
- Add `@media (prefers-reduced-motion: reduce)` to replace spring transforms with opacity fades.
- Wrap full-width views with `viewport-fit=cover` and apply `env(safe-area-inset-*)` padding.

### React Native
- Always use `borderCurve: 'continuous'` on iOS `View` styles to enable native G2 squircle corners.
- Leverage `react-native-reanimated` with `withSpring` for fluid interruptible interactions.
- Avoid building custom tab bars with absolute positioning that ignore the Home Indicator; wrap navigation in `SafeAreaProvider`.

### Flutter
- Prefer Cupertino widgets (`CupertinoTabScaffold`, `CupertinoPageScaffold`, `CupertinoButton`) when rendering on iOS and macOS.
- Ensure `MediaQuery.textScalerOf(context)` scales fonts and card heights dynamically.
- Honor `MediaQuery.of(context).highContrast` by boosting border weights and dropping translucent fills.

### Tauri & Electron (Desktop)
- Use native window vibrancy (`vibrancy: 'under-window'` in Electron, `window-vibrancy` crate in Tauri) rather than heavy web CSS blur where possible.
- Replicate standard macOS shortcuts (`Cmd+,` for Settings, `Cmd+W` to close window, `Cmd+Q` to quit).
- Ensure all app actions are registered in the top system Menu Bar.

