# Apple Intelligence & Generative AI UX

> Principles and guidelines for integrating Generative AI, Writing Tools, and App Intents into Apple-style applications.

---

## 1. Core Principles for AI Integration

Apple’s philosophy on AI emphasizes **human agency, transparency, and utility**:

1. **Provide Clear, Specific Value:** Introduce AI only where it delivers distinct, measurable benefits (saving time, condensing complex data, aiding expression). Never tack on AI features purely as a novelty.
2. **Complement, Don't Displace:** AI should assist and enhance user workflows. The user must always remain the editor-in-chief with final authority over content creation and irreversible actions.
3. **Transparent Capabilities & Limits:** Clearly indicate when content is AI-generated. Avoid hyperbolic claims about AI omnipotence.
4. **Frictionless Error Correction:** Because generative models can produce imperfect or hallucinated results, the UI must make it trivial to edit, regenerate, or completely revert generated outputs with a single click/tap.

---

## 2. Writing Tools Integration

Apple Intelligence provides system-wide Writing Tools (Proofread, Rewrite, Summarize, Create Table/List) natively:

- **Standard Text Views:** If your app uses standard text containers (`UITextView`, `NSTextView`, `WKWebView`, SwiftUI `TextEditor`), Writing Tools works automatically with zero custom code via TextKit 2.
- **Behavior Modes (`UIWritingToolsBehavior`):**
  - `.default`: System determines optimal presentation based on platform/form factor.
  - `.complete`: Full inline experience where rewrites morph directly in place inside the active container.
  - `.limited`: Restricts AI output to a non-destructive floating overlay panel (used when in-place modification would corrupt complex custom canvas layouts).
  - `.none`: Completely disables Writing Tools for password fields, terminal windows, or security-sensitive code.
- **Allowed Formats (`allowedWritingToolsResultOptions`):** Restrict input to `.plainText`, `.richText`, `.list`, or `.table`.
- **Lifecycle Hooks:**
  - `textViewWritingToolsWillBegin(_:)` / `textViewWritingToolsDidEnd(_:)`: Pause autosave, defer cloud sync, or freeze undo coalescing during transformation.
  - `isWritingToolsActive`: State flag indicating when AI mutation is active.
- **Protected Ranges:** Mark specific NSRanges as immutable to protect code snippets, author citations, or timestamps.

---

## 3. Generative Media UX (Genmoji & Image Playground)

- **`NSAdaptiveImageGlyph`:** Unlike standard Unicode emoji, Genmoji and Image Playground assets are custom vector glyphs wrapped in `NSAdaptiveImageGlyph` that scale dynamically with surrounding font size, line-height, and baseline offsets.
- **Embedded Sheets:** Present generation inside lightweight sheets (`presentationDetents([.medium, .large])`) anchored contextually to the insertion point, never taking over the full screen.
- **Progressive Feedback:** Render low-resolution progressive drafts or shimmering placeholders rather than ambiguous blocking loading spinners.
- **Direct Insertion:** Tapping an asset inserts it directly into the active field with an automatic Undo checkpoint.

---

## 4. App Intents, Siri & Privacy Cues

- **Standard System Verbs:** Structure intents around foundational action verbs: *Get, Create, Edit, Delete, Open*.
- **Parameter Summaries:** Intent descriptions must read like natural spoken sentences with dynamic placeholders (e.g., `"Create \(\$target) with \(\$title)"`).
- **System Confirmation Threshold (`IntentConfirmation`):** Compulsory for irreversible, destructive, or financial actions (transferring funds, deleting files, sending bulk external messages).
- **Siri Screen Illumination:** Edge-to-edge organic multi-colored glow wrapping display perimeters signals active system intelligence.
- **Private Cloud Compute (PCC):** Foundation models run on-device (~3B parameters) or on cryptographic stateless servers with verifiable builds and zero data retention. External models (e.g. ChatGPT) require an explicit, un-bypassable system permission alert displaying exact payload contents prior to dispatch.

---

## 5. UI Checklist for AI Features

- [ ] Does the screen explain what AI is doing in simple, jargon-free words?
- [ ] Is there an obvious "Revert" / "Undo" action for every AI generation?
- [ ] Are processing states determinate or accompanied by subtle shimmer effects rather than modal blocking dialogues?
- [ ] Can the user edit generated text inline without having to copy-paste from an AI result card?
- [ ] Does the feature function gracefully (or hide cleanly) if the user turns off Apple Intelligence or lacks network access?

