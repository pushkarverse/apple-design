# Platform Paradigms

> Architectural and design conventions specific to macOS, iOS, iPadOS, iPhone Duo, and visionOS.

---

## 1. macOS (Desktop Paradigm)

macOS is a high-density, multi-window environment designed for precision pointer and keyboard productivity.

- **Universal Menu Bar Coverage:** Every single user action, toolbar button, and command **must** exist in the system Menu Bar. Never create an action that can only be triggered via a button on screen.
- **Traffic Light Window Chrome:** Window controls (close, minimize, zoom/full-screen) live in the top-left corner. Respect standard macOS title bar metrics and title bar accessories.
- **Fluid Resizing & Multi-Windowing:** Windows must resize continuously down to compact widths and up to ultra-wide displays without breaking layouts.
- **Sidebar Hierarchy:**
  - Limit sidebars to at most **two levels** of hierarchy.
  - Sidebars must be collapsible/hideable.
  - Critical actions must never be trapped exclusively at the bottom of a sidebar.
- **Settings Architecture:** General and account settings belong under the **App Menu (`App Name > Settings...` / `Cmd+,`)** in a dedicated settings window with a fixed-height toolbar.
- **Pointer Ergonomics:**
  - Interactive elements must have clear hover feedback and pointer cursors (`pointing-hand` or native hover highlights).
  - Minimum hit target on desktop: **$28 \times 28\text{pt}$ default**, $20 \times 20\text{pt}$ absolute minimum.

---

## 2. iOS (Mobile Paradigm)

iOS is a direct-manipulation, touch-first environment optimized for rapid one-handed reach and glanceable status.

- **Thumb-Zone Ergonomics:** Primary navigation and key interactive targets live in the lower half of the screen. Top corners are reserved for non-destructive, secondary navigation (Back, Cancel).
- **Target Sizes:**
  - Standard touch target: **$44 \times 44\text{pt}$ minimum**.
  - Secondary tight target: $28 \times 28\text{pt}$ (must have $\ge 10\text{pt}$ transparent hit padding).
- **Navigation Structure:**
  - Top-level navigation belongs in a persistent **Tab Bar** (3 to 5 tabs max). Tabs navigate between peer sections; tabs do **not** trigger modal actions.
  - Avoid "More" overflow tabs where possible; consolidate hierarchy instead.
- **Modal Sheets & Detents:**
  - Modals should open as **sheets with detents** (medium $\approx 50\%$ height, large $\approx 90\%$ height) featuring a top grabber.
  - Always support swipe-to-dismiss unless discarding unsaved data.
- **Live Activities & Dynamic Island:**
  - Dynamic Island presentations:
    - *Compact:* Fits beside the sensor housing.
    - *Minimal:* Circular 28pt indicator when multiple activities share the island.
    - *Expanded:* Long-press reveals interactive controls and rich metrics.
  - Lock Screen & StandBy: Provide high-contrast glanceable information that degrades gracefully when background materials are removed.
- **System Controls (iOS 18+):**
  - App toggles and quick actions should expose widgets to Control Center, Lock Screen action buttons, and the hardware Action Button.

---

## 3. iPadOS (Tablet Paradigm)

iPadOS bridges touch direct-manipulation with desktop productivity:

- **Adaptive Navigation:** In compact widths, use a bottom Tab Bar. When width expands to regular, the Tab Bar smoothly morphs into a collapsible **Sidebar**.
- **Multitasking Compatibility:**
  - Layouts must flawlessly adapt between Split View ($1/3$, $1/2$, $2/3$ ratios), Slide Over, and floating Stage Manager windows.
  - Never assume full-screen tablet dimensions; query `horizontalSizeClass` and window bounds.
- **Pointer & Trackpad Support:**
  - Pointers morph into round dots and snap magnetically to buttons, creating a subtle lift and scale effect.
- **Apple Pencil & Scribble:**
  - All standard text inputs must support automatic handwriting recognition (Scribble) and Pencil double-tap/squeeze shortcuts.

---

## 4. iPhone Duo (Foldable Paradigm)

For foldable dual-screen devices and dynamic multi-posture hardware:

- **Size Classes Over Device Constants:** Never branch on `userInterfaceIdiom == .phone` or hardcoded screen widths (`390px`, `402px`). Use `horizontalSizeClass` and container queries.
- **Posture Awareness:**
  - *Outer Display:* Acts as a compact phone.
  - *Inner Display:* Acts as a regular tablet/canvas.
  - *Half-Fold (Laptop/Tent Mode):* Rearranges two panes into primary content on top and interactive controls below.
- **Reserved Regions:** Ensure floating buttons, dialogs, and text columns do not sit directly on the physical hinge or fold line.
- **Vertical Toolbars:** Standard system bars relocate to the outer sides on unfolded screens; custom bars must support vertical positioning.

---

## 5. visionOS (Spatial Computing Influence)

Key spatial principles that enhance spatial, web, and native applications:

- **Window & Container Geometry:**
  - **Default Canvas:** **$1280 \times 720\text{pt}$** landscape canvas placed at **$\sim 2.0\text{ meters}$** in the primary line of sight.
  - Windows render in vector points (`pt`) that dynamically scale to maintain constant visual angular size.
  - Windows subtly billboard (yaw toward the user) during repositioning.
- **Floating Ornaments:**
  - Contextual toolbars or controls floating on a slightly elevated z-plane.
  - **Edge Overlap:** Bottom ornaments visually anchor by **overlapping the bottom edge of the parent window by $20\text{pt}$**.
  - **Scroll Independence:** Ornaments remain fixed while window content scrolls underneath.
  - Width must never exceed parent window width.
- **Eye & Gesture Hit Targets:**
  - **Minimum Hit Area:** **$60 \times 60\text{pt}$** interactive bounding box for gaze-and-pinch input.
  - **Standard Visual Button:** $44 \times 44\text{pt}$ visual element with at least $8\text{pt}$ transparent padding on all sides.
  - **Center-to-Center Spacing:** $\ge 60\text{pt}$ between centers of adjacent controls.
  - **Item Separation:** $\ge 4\text{pt}$ gap between list rows to prevent hover state flicker during eye saccades.
- **Ergonomic Viewing Comfort:**
  - Natural resting eye-line is **$10^\circ - 15^\circ$ below horizontal**.
  - Horizontal comfort cone is **$\pm 30^\circ$ ($60^\circ$ total cone)** without turning the head.
  - **Never Head-Lock UI:** Head-locked elements cause vestibular disorientation; world-anchor all windows.
- **Concentric Nested Corner Radius Formula:**
  $$R_{\text{inner}} = R_{\text{outer}} - \text{Padding}$$
- **Spatial Audio & Acoustic Meshing:**
  - *Point Source:* Distance decay attenuation and binaural panning based on 3D coordinates.
  - *Ambient Audio:* Multi-channel background textures without distance drop-off.
  - Real-world LiDAR mesh reverberation matches physical room acoustic characteristics.

