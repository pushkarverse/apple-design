# GEMINI.md

Antigravity & Gemini CLI configuration and execution manual for the Apple Design skill.

---

## Antigravity Execution Directives

When invoked in Antigravity or Gemini pair-programming sessions:

1. **Trigger Keywords:** Automatically activate when the user mentions:
   - Apple design, macOS, iOS, iPadOS, HIG, Human Interface Guidelines.
   - Liquid Glass, glassmorphism, vibrancy, materials hierarchy.
   - Fluid motion, spring physics, interruptible transitions, momentum flick.
   - Continuous curvature, squircle, SF Symbols, Dynamic Type.
   - Safari web ergonomics (`viewport-fit=cover`, `safe-area-inset`).
   - Apple Pay review (App Store Guideline 4.9).
   - "Make this look less generic" or UI/UX design review.

2. **Antigravity Tool Usage:**
   - Use `read_file` to selectively load 8 to 12 reference documents from `references/`.
   - Never read the entire `references/hig/` directory into context. Use `references/lookup.md` to identify exact component pages.
   - Run `run_command` with `node scripts/audit.mjs` when auditing project source code.

3. **Multi-Agent Coordination:**
   - In GSD or multi-agent tracks, assign the "Senior HIG Reviewer" role to verify platform rules, accessibility, and Apple Pay compliance.
   - Assign the "Studio Craft Lead" role to verify typography tracking, G2 squircle geometry, spring parameters, and signature design elements.

4. **Output Discipline:** Follow the standardized 6-part report structure from `SKILL.md` (Summary, Critical, Improvements, Craft Notes, What Works, Platform Notes).

