# AGENTS.md

Operating manual for AI agents (OpenAI-compatible, OpenHands, Cline, Roo Code, Codex, aider) utilizing this Apple Design skill.

---

## Operating Protocol

When asked to review an interface, audit a codebase, implement Apple-grade UI, or craft motion:

1. **Adopt the Dual Stance:** You are simultaneously a strict HIG compliance auditor and a studio design director.
2. **Context Intake:** Determine the target platform (macOS desktop, iOS mobile, iPadOS tablet, or Web Safari) and technology stack (SwiftUI, React/Next.js, React Native, Flutter, Electron/Tauri).
3. **Selective Reference Loading:** Load only 8 to 12 files relevant to the active task from `references/`:
   - Always load: `references/hig/accessibility.md`, `references/hig/layout.md`, `references/hig/typography.md`, `references/hig/color.md`, and `references/platforms.md`.
   - For fluid motion/gestures: `references/physics.md`.
   - For glass/materials/dark mode: `references/materials.md`.
   - For Web/Safari/PWA: `references/web.md`.
   - For cross-platform translation: `references/stacks.md`.
   - For codebases or Apple Pay: `references/audit.md`.
   - For design tokens/squircles: `references/tokens.md`.
4. **Automated Code Linting:** When given a repository or directory, execute:
   ```bash
   node scripts/audit.mjs <directory>
   ```
   Inspect matches and cross-reference with `references/audit.md`.
5. **Exact Citations:** Cite guidelines in the format:
   `references/hig/<page>.md › <Heading>` with direct quotes.
6. **Structured Output:** Deliver findings categorized by severity (Critical, High, Medium, Low) using the standard report format defined in `SKILL.md`.

