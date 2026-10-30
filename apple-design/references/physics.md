# Fluid Motion & Physics

> Mathematical foundations and implementation rules for fluid, Apple-style interfaces. Translated from Apple's WWDC design talks (*Designing Fluid Interfaces*, *The Details of UI Typography*, *Designing Audio-Haptic Experiences*) for Web, React Native, Flutter, and SwiftUI.

---

## 1. Core Philosophy

> "When we align the interface to the way we think and move, something magical happens — it stops feeling like a computer and starts feeling like a seamless extension of us."

An interface feels alive when:
1. **Motion starts from the current presentation value**, never jumping to a logical target.
2. **Motion inherits the user's velocity**, creating no seam between drag and release.
3. **Motion projects momentum forward**, anticipating where the user is throwing it.
4. **Motion can be grabbed and reversed at any instant** without locking input or hitting a "brick wall."

---

## 2. Spring Physics Parameters

Apple replaced the textbook mass-stiffness-damping physics triplet with two intuitive, designer-friendly parameters:

- **Damping Ratio ($\zeta$):** Controls overshoot and oscillation.
  - $\zeta = 1.0$: **Critically damped.** Settles as quickly as possible with zero bounce or overshoot. Use as the universal default for all UI transitions.
  - $\zeta < 1.0$: **Under-damped.** Overshoots and bounces. Use $\zeta \approx 0.8$ **only** when a physical gesture (flick, throw, fast drag) preceded the motion.
  - $\zeta > 1.0$: **Over-damped.** Sluggish, settles slowly. Avoid in UI.
- **Response ($T_0$):** Speed of the spring in seconds. Roughly corresponds to the natural period of oscillation.
  - $0.3\text{s} - 0.4\text{s}$: Standard interactive UI.
  - Lower values ($0.15\text{s} - 0.25\text{s}$): Snappy toggles, small controls.
  - Higher values ($0.5\text{s} - 0.6\text{s}$): Large panels, sheets, orientation shifts.

### Modern SwiftUI Spring Parameters: `duration` & `bounce`
$$\text{Animation.spring(duration: 0.35, bounce: 0.0)}$$
- `duration`: Perceptual settling time in seconds ($0.35\text{s}$ standard).
- `bounce`: Normalized elasticity in $[-1.0, 1.0]$:
  - `bounce = 0.0`: Critically damped ($\zeta = 1.0$), zero overshoot.
  - `bounce = 0.2`: Under-damped ($\zeta \approx 0.8$), subtle momentum bounce for physical flicks.

### Apple Production Values

| Interaction | Damping Ratio ($\zeta$) | Response ($T_0$) | SwiftUI Parameters | Feel |
| :--- | :--- | :--- | :--- | :--- |
| **Default / Settle** | `1.0` | `0.35s` | `duration: 0.35, bounce: 0.0` | Clean, crisp, no bounce |
| **Reposition / PiP** | `1.0` | `0.40s` | `duration: 0.40, bounce: 0.0` | Controlled, physical |
| **Momentum / Flick** | `0.8` | `0.35s` | `duration: 0.35, bounce: 0.2` | Subtle natural bounce |
| **Drawer / Sheet** | `0.8` | `0.30s` | `duration: 0.30, bounce: 0.2` | Quick, responsive snap |
| **Rotation** | `0.8` | `0.40s` | `duration: 0.40, bounce: 0.2` | Organic, weighted |

---

## 3. Mathematical Formulas

### Momentum Projection (Exponential Decay)

Do not snap to the nearest target from the release coordinate. Use the release velocity to project the resting position, exactly matching inertial scrolling:

$$\text{projectedDistance} = \left(\frac{v}{1000}\right) \cdot \frac{d}{1 - d}$$

- $v$: Release velocity in pixels per second ($\text{px/s}$).
- $d$: Deceleration constant. Default $d \approx 0.998$ for standard iOS inertial feel ($d = 0.99$ for snappier, shorter stops).

```javascript
export function projectMomentum(initialVelocity, decelerationRate = 0.998) {
  return (initialVelocity / 1000) * decelerationRate / (1 - decelerationRate);
}

// Usage:
const projectedY = currentY + projectMomentum(velocityY);
const targetSnapPoint = findNearestSnapPoint(projectedY);
animateSpring(element, { y: targetSnapPoint, velocity: velocityY });
```

### Relative Velocity Handoff

When transitioning from an active drag gesture to a spring animation, normalize the velocity relative to the remaining travel distance:

$$v_{\text{relative}} = \frac{v_{\text{gesture}}}{x_{\text{target}} - x_{\text{current}}}$$

If the spring engine accepts absolute units ($\text{px/s}$, like Framer Motion), pass the raw velocity.

### Continuous Rubber-Banding (Soft Boundaries)

At physical boundaries, never stop abruptly. Progressive resistance signals responsiveness while showing the user that a boundary has been reached:

$$f(x) = \frac{x \cdot L \cdot c}{L + c \cdot |x|}$$

- $x$: Distance pulled past the boundary.
- $L$: Dimension of the dragging container (width or height).
- $c$: Resistance constant ($c \approx 0.55$).

```javascript
export function rubberband(overshoot, dimension, constant = 0.55) {
  return (overshoot * dimension * constant) / (dimension + constant * Math.abs(overshoot));
}
```

---

## 4. Interruptibility Rules

Interruptibility is the single most critical principle separating standard animations from fluid interfaces:

1. **Never lock user input during transitions:** Pointers, swipes, and taps must register mid-flight.
2. **Always start from the presentation value:** When interrupting an active motion, read the live on-screen transform via `window.getComputedStyle(el).transform` or framework equivalents. Never animate from the previous logical target.
3. **Blend velocities (Additive Animation):** When reversing or redirecting a moving view, add the new velocity to the remaining velocity. Never hard-cut or reset velocity to zero (the "brick wall" effect).
4. **Decompose 2D motion:** Animate X and Y axes with independent springs. A coupled 2D vector spring desynchronizes when dragging diagonally with unequal velocities.

---

## 5. Implementation Across Stacks

### Web (Motion / Framer Motion & CSS)

```typescript
import { animate } from 'motion';

// Critically damped default UI motion
animate(element, { y: 0 }, { 
  type: 'spring', 
  bounce: 0, 
  duration: 0.35 
});

// Gesture release with inherited velocity
animate(element, { y: snapTarget }, { 
  type: 'spring', 
  bounce: hasFlicked ? 0.2 : 0, 
  duration: 0.35,
  velocity: releaseVelocity 
});
```

### React Native (Reanimated)

```typescript
import Animated, { withSpring, useAnimatedGestureHandler } from 'react-native-reanimated';

// Apple critically damped spring
const config = {
  damping: 20,
  stiffness: 180,
  mass: 1,
  overshootClamping: true, // true = critically damped
};

// With velocity handoff
transY.value = withSpring(targetY, {
  ...config,
  velocity: gestureVelocityY,
});
```

### SwiftUI

```swift
// Apple native spring
withAnimation(.spring(response: 0.35, dampingRatio: 1.0)) {
    isPresented = true
}

// Bouncy momentum release
withAnimation(.spring(response: 0.35, dampingRatio: 0.8)) {
    offset = targetOffset
}
```

### Flutter

```dart
final SpringDescription kAppleDefaultSpring = SpringDescription.withDampingRatio(
  mass: 1.0,
  stiffness: 180.0,
  ratio: 1.0, // critically damped
);

final SpringSimulation simulation = SpringSimulation(
  kAppleDefaultSpring,
  currentPosition,
  targetPosition,
  releaseVelocity,
);
```

---

## 6. Gesture Interaction Discipline

- **Highlight on `pointerdown`:** Never wait for `click` or release. Latency destroys the physical illusion.
- **Hysteresis Threshold (~10px):** For scrollable content, require a 10px directional movement before committing to a pan/drag gesture to prevent blocking taps.
- **`setPointerCapture`:** Capture pointer ID immediately on drag start so gestures continue cleanly when fingers leave element boundaries.
- **Spatial Origin Awareness:** Set `transform-origin` to the trigger element (button, icon) so menus and popovers visibly emerge from their physical origin.
- **Symmetric Trajectories:** If an element enters from the right, it must dismiss to the right. Asymmetric exit paths disorient spatial memory.

---

## 7. Audio-Haptic Latency & Synchronization

*(From WWDC "Designing Audio-Haptic Experiences" & Core Haptics guidelines)*

- **Multimodal Window of Simultaneity:** The human brain perceives audio and haptics as a single physical event only when they arrive within **$\pm 20\text{ms} - 30\text{ms}$** of each other.
- **Audio Must Never Lead Haptics:** Audio leading haptics by as little as $> 20\text{ms}$ feels jarring and breaks physical causality. Haptics leading audio by $\sim 30\text{ms}$ is natural (mimics real-world sound propagation).
- **Latency Windows:**
  - $< 30\text{ms}$: Instant direct manipulation (illusion of physical machinery).
  - $30 - 50\text{ms}$: Responsive for standard taps.
  - $> 100\text{ms}$: Detached, sluggish.
  - $> 180\text{ms}$: Broken causality (triggers after finger release, causing accidental double-taps).
- **Engine Prewarming:** Call `engine.start()` ahead of time (e.g. on pointer down or view appearance) to eliminate actuator cold-start delays.

