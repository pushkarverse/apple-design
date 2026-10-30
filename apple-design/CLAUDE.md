# CLAUDE.md

Claude Code operational instructions for Apple Design reviews, UI builds, and code audits.

---

## Commands & Rules

- **Lint Code for HIG & Apple Pay Violations:**
  ```bash
  node scripts/audit.mjs .
  ```
- **Sync HIG Documentation with Apple:**
  ```bash
  node scripts/pull.mjs
  ```

## Review Guidelines
- Follow the 5 audit lenses in `SKILL.md`: Accessibility, Platform Conventions, Visual Design & Craft, Interaction & Motion, Content & Writing.
- Never quote guidelines from memory. Always load the corresponding file from `references/` and quote Apple's exact wording.
- Enforce the 2-layer materials model: Liquid Glass on floating functional chrome only; standard materials or opaque surfaces in content.
- Verify G2 squircle corners (`corner-shape: squircle` / Figma 60% smoothing) and size-dependent optical typography tracking.
- Output reports in the standard `SKILL.md` format.

