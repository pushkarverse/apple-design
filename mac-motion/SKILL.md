---
name: mac-motion
description: Engineering fluid Apple-style motion, WWDC spring physics, gesture tracking, interruptibility, momentum flick projection, and continuous rubber-banding across Web, React Native, Flutter, and SwiftUI. Use when building or refining gestures, sheets, drawers, drag interactions, or page transitions.
---

# mac-motion

You are an expert in Apple's fluid interface motion engineering. You translate the physical principles from Apple's WWDC design talks (*Designing Fluid Interfaces*) into production code.

---

## The Core Principles

1. **Zero Input Latency:** Respond instantly on `pointerdown` (touch down), never on release or click.
2. **1:1 Direct Manipulation:** Content moves locked to the pointer, respecting the initial grab offset. Use `setPointerCapture`.
3. **Interruptibility (Cardinal Rule):** Any running animation must be interruptible and redirectable mid-flight. Always read and animate from the live *presentation* value, never the logical target.
4. **Behavior Over Animation:** Use springs rather than fixed-duration easing curves. Springs naturally handle new input targets mid-motion.

---

## Spring Physics Parameters

Apple uses two intuitive parameters instead of mass/stiffness/damping:

- **Damping Ratio ($\zeta$):** Controls overshoot and oscillation.
  - $\zeta = 1.0$: **Critically damped.** Default for all UI settles. Zero bounce, fast settle.
  - $\zeta \approx 0.8$: **Under-damped.** Use **only** when a physical flick or throw gesture carried momentum.
- **Response ($T_0$):** Settle speed in seconds (typically $0.3\text{s} - 0.4\text{s}$).

| Interaction | Damping Ratio ($\zeta$) | Response ($T_0$) | Feel |
| :--- | :--- | :--- | :--- |
| **Default UI Settle** | `1.0` | `0.35s` | Clean, crisp, no bounce |
| **Reposition / PiP** | `1.0` | `0.40s` | Controlled, physical |
| **Momentum / Flick** | `0.8` | `0.35s` | Natural subtle bounce |
| **Drawer / Sheet** | `0.8` | `0.30s` | Snappy, responsive snap |

---

## Exact Mathematical Formulas

### 1. Momentum Projection (Exponential Decay)
Project the resting position from release velocity:

$$\text{projectedDistance} = \left(\frac{v}{1000}\right) \cdot \frac{d}{1 - d} \quad (d \approx 0.998)$$

```javascript
function projectMomentum(velocity, d = 0.998) {
  return (velocity / 1000) * d / (1 - d);
}

const projectedEndpoint = currentPosition + projectMomentum(releaseVelocity);
const target = nearestSnapPoint(projectedEndpoint);
animateSpring(target, { velocity: releaseVelocity });
```

### 2. Continuous Rubber-Banding (Soft Boundaries)
Progressive resistance past edges:

$$f(x) = \frac{x \cdot L \cdot 0.55}{L + 0.55 \cdot |x|}$$

```javascript
function rubberband(overshoot, dimension, constant = 0.55) {
  return (overshoot * dimension * constant) / (dimension + constant * Math.abs(overshoot));
}
```

### 3. Velocity Handoff
Pass gesture release velocity into the spring's initial velocity without a hard cut or velocity reset.

---

## Code Recipes

### Web (Motion / Framer Motion)
```typescript
import { animate } from 'motion';

// Critically damped default settle
animate(element, { y: 0 }, { type: 'spring', bounce: 0, duration: 0.35 });

// Flick momentum release
animate(element, { y: target }, { type: 'spring', bounce: 0.2, duration: 0.35, velocity: releaseVelocity });
```

### React Native (Reanimated)
```typescript
import { withSpring } from 'react-native-reanimated';

transY.value = withSpring(targetY, {
  damping: 20,
  stiffness: 180,
  mass: 1,
  overshootClamping: !hasFlicked,
  velocity: gestureVelocityY,
});
```

### SwiftUI
```swift
withAnimation(.spring(response: 0.35, dampingRatio: 1.0)) {
    isPresented = true
}
```

---

## Master Knowledge Base Connection

`mac-motion` connects directly to `apple-design/references/`:
- **Physics & Mathematics:** `apple-design/references/physics.md`
- **HIG Motion Foundations:** `apple-design/references/hig/motion.md`
- **Audio-Haptic Harmony:** `apple-design/references/hig/playing-haptics.md`
- **Stack Translations:** `apple-design/references/stacks.md`

