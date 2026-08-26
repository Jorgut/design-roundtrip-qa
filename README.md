# Design Roundtrip QA

Turn AI-generated UI screenshots, mockups, and prototype pages into target-ready design and implementation guidance.

This skill helps compare and rebuild user-owned or explicitly authorized material across Figma, SwiftUI, Web, React, and other product targets. It extracts a measurable design contract, preserves layout intent, and runs a visual QA loop after each correction.

## Workflow

1. Classify the source and target.
2. Record the frame, grid, typography, spacing, imagery, and component contract.
3. Rebuild with native, editable components for the selected target.
4. Capture the rebuilt state at the matching viewport and scale.
5. Produce a fresh overlay and pixel-difference image.
6. Fix blocking drift and report `pass`, `revise`, or `rework` honestly.

## Install

```bash
npx skills add https://github.com/Jorgut/design-roundtrip-qa -a codex -g
```

The skill is also suitable for Claude Code, OpenCode, and other agents that support `SKILL.md` workflows.

## Important boundary

Public screenshots may be used to study layout logic and test the QA process. Do not clone third-party brands, proprietary interfaces, or copyrighted assets. A pixel-level claim requires a fresh coded or native rebuild plus current overlay and diff evidence; a screenshot placed on the page is not a rebuild.

## Status

The workflow is production-oriented and continues to evolve through real Figma, SwiftUI, and Web validation cases. See `SKILL.md` for the complete operating rules and hard-stop criteria.
