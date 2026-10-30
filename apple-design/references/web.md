# Safari & Web Platform Ergonomics

> Best practices for engineering web applications, progressive web apps (PWAs), and hybrid webviews that feel native on iOS, iPadOS, and macOS Safari.

---

## 1. Edge-to-Edge Viewport Setup

To allow backgrounds and translucent chrome to extend cleanly underneath the notch, Dynamic Island, and home indicator bar:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```

Without `viewport-fit=cover`, Safari applies artificial white/black letterboxing strips along the screen edges.

---

## 2. Safe Area Inset Management

Once `viewport-fit=cover` is set, interactive elements and typography must respect the four CSS environment variables:

```css
:root {
  --sat: env(safe-area-inset-top);
  --sar: env(safe-area-inset-right);
  --sab: env(safe-area-inset-bottom);
  --sal: env(safe-area-inset-left);
}
```

### Critical Layout Patterns

#### Floating Header / Top Navigation
```css
.site-header {
  position: sticky;
  top: 0;
  /* Use max() so devices without hardware insets retain comfortable padding */
  padding-top: max(16px, env(safe-area-inset-top));
  padding-left: max(16px, env(safe-area-inset-left));
  padding-right: max(16px, env(safe-area-inset-right));
}
```

#### Bottom Navigation / Action Bar
```css
.bottom-bar {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding-bottom: max(16px, env(safe-area-inset-bottom));
  padding-left: max(16px, env(safe-area-inset-left));
  padding-right: max(16px, env(safe-area-inset-right));
}
```

#### Scrollable Container (Clear Home Indicator)
Apply padding to the scrollable container, **not** margins to individual list items, so the bottom-most row scrolls past the home indicator cleanly:
```css
.scroll-view {
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  padding-bottom: calc(16px + env(safe-area-inset-bottom));
}
```

---

## 3. Touch, Pointer, and Typography Ergonomics

### Eliminate Synthetic Delays & Gray Taps
Safari displays a default gray box on touch and introduces a ~300ms click delay if gestures are not configured properly:
```css
/* Disable gray tap highlight */
* {
  -webkit-tap-highlight-color: transparent;
}

/* Optimize touch interactions */
button, a, input, select {
  touch-action: manipulation; /* removes double-tap-to-zoom delay */
}

/* Prevent unwanted callout menus on long press of images/links */
.interactive-element {
  -webkit-touch-callout: none;
}
```

### Font Smoothing & Subpixel Rendering
```css
body {
  font-family: -apple-system, BlinkMacSystemFont, "SF Pro Text", "SF Pro Display", system-ui, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-rendering: optimizeLegibility;
}
```

---

## 4. Web Apple Pay Integration

App Store Review and Apple web checkout guidelines strictly enforce standard Apple Pay rendering. Never draw custom Apple Pay buttons or embed unofficial icon-pack SVGs.

### The Official Web SDK
Include Apple’s official script in your `<head>`:
```html
<script src="https://applepay.cdn-apple.com/jsapi/1.latest/apple-pay-sdk.js"></script>
```

### Using the Custom Element
```html
<apple-pay-button 
  buttonstyle="black" 
  type="buy" 
  locale="en-US">
</apple-pay-button>
```

### Sizing via Official CSS Variables
Style the button **exclusively** through Apple's supported custom properties:
```css
apple-pay-button {
  --apple-pay-button-width: 100%;
  --apple-pay-button-height: 48px;
  --apple-pay-button-border-radius: 12px;
  --apple-pay-button-padding: 0px 16px;
  --apple-pay-button-box-sizing: border-box;
}
```

---

## 5. PWA Meta Tags & Home Screen Icon

For web apps saved to the iOS Home Screen or macOS Dock:

```html
<!-- Enable standalone PWA mode -->
<meta name="apple-mobile-web-app-capable" content="yes">

<!-- Status bar appearance: default | black | black-translucent -->
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

<!-- App title on Home Screen -->
<meta name="apple-mobile-web-app-title" content="MyMacApp">

<!-- High-resolution touch icon (180x180 png, no gloss, system applies continuous curvature) -->
<link rel="apple-touch-icon" href="/icons/apple-touch-icon-180x180.png">
```

---

## 6. Spatial Web & 3D in Safari (visionOS)

- **The HTML `<model>` Element:** Embed native stereoscopic 3D USDZ assets directly inside HTML markup:
  ```html
  <model src="asset.usdz" interactive alt="3D Model"></model>
  ```
- **AR Quick Look:** Allow users to pull 3D objects straight into their physical room:
  ```html
  <a rel="ar" href="model.usdz">
    <img src="preview.png" alt="View in your room">
  </a>
  ```
- **Hover Suppression:** Safari on visionOS automatically suppresses custom `:hover` CSS to prevent rapid eye saccades from flickering states. Style `:active` and smooth transitions instead.
- **CSS 3D Depth:** Supports `transform-style: preserve-3d` and `perspective`, rendered within the window's planar backing store. Specify `-webkit-backface-visibility: hidden` to avoid flattening bugs.

