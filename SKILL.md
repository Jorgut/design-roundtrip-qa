---
name: design-roundtrip-qa
description: Convert AI-generated UI screenshots, mockups, or prototype pages into target-ready rebuild guidance and visual QA; use when comparing pages made by Stitch, Figma Make, Codex, Claude Code, or other agent-generated UI tools before rebuilding in Figma, SwiftUI, or product code.
metadata:
  short-description: Round-trip AI-generated UI into design and implementation QA.
---

# Design Roundtrip QA

Use this skill when the user wants to take a generated UI page, screenshot, mockup, or prototype and round-trip it back into a cleaner design or implementation workflow.

This is a workflow skill, not a site-cloning skill. It is for user-owned or explicitly authorized design material only.

Public screenshots may be used for analysis and workflow testing, but not for cloning brand assets, copying proprietary UI, or shipping a lookalike product.

When choosing a public test reference, prefer a visually strong sample with clear grid, hierarchy, whitespace, and component rhythm. Avoid weak screenshots that only show utility or data density and do not exercise layout judgment.

## When to use

- The source is a screenshot, mockup, or AI-generated page from Stitch, Figma Make, Codex, Claude Code, or a similar agent.
- The user wants to rebuild the page in Figma, SwiftUI, Web/React, or another implementation target.
- The user needs a comparison pass after regeneration.
- The user wants to identify layout, typography, spacing, alignment, or fidelity issues.
- The user wants to keep design intent stable while moving between design tools and implementation targets.

## What this skill does

1. Classify the input:
   - wireframe
   - low-fidelity concept
   - semi-hi-fi draft
   - high-fidelity page
2. Identify the target:
   - Figma rebuild
   - SwiftUI implementation
   - Web or React implementation
   - design critique only
   - visual regression QA only
3. Extract the design contract:
   - page structure
   - grid and alignment
   - typography hierarchy
   - spacing and rhythm
   - color and visual emphasis
   - component relationships
4. Rebuild guidance:
   - what to keep
   - what to normalize
   - what needs manual correction in Figma or implementation code
   - what should never be inferred freely
5. QA pass:
   - compare the rebuild against the source
   - call out drift, missing pieces, and accidental invention
   - decide whether the result is ready, needs revision, or is only a reference

## Operating modes

### 1. Figma rebuild

Use when the user needs a design artifact, not code.

- Translate the source into frames, layout constraints, components, text styles, color styles, and spacing rules.
- Preserve observed anchors first: margins, baselines, gutters, image ratios, crop behavior, and alignment lines.
- Only infer what the source does not already show.
- If a layout choice is ambiguous, state the ambiguity instead of normalizing it away.

### 2. SwiftUI implementation

Use when the user wants the page to become a real Apple-platform screen.

- Translate the design contract into SwiftUI primitives such as `VStack`, `HStack`, `ZStack`, `Grid`, `ScrollView`, `NavigationStack`, `List`, and reusable `View` components.
- Map visible tokens into SwiftUI-friendly values: spacing constants, corner radius, semantic colors, font roles, image aspect ratios, and safe-area behavior.
- Account for Apple platform behavior: Dynamic Type, dark mode, touch targets, safe areas, accessibility labels, reduced motion, and localization expansion.
- Keep the implementation honest to the source. If a token or font cannot be proven, mark it as an approximation.
- Verify implementation with simulator or screenshot comparison when available. If visual verification is blocked, report the blocked step and provide the exact manual comparison checklist.

### 3. Visual QA only

Use when the rebuild already exists and the work is to check whether it matches the source.

- Compare source and rebuilt output at matching viewport, scale, and color mode.
- Evaluate geometry first, then type, spacing, color, imagery, and interaction states.
- Classify each difference as blocking, acceptable approximation, or intentional improvement.
- Do not call the round-trip complete until blocking drift is fixed or explicitly accepted by the user.

## Core rules

- Do not let the model freely invent layout details when the source is explicit.
- A pixel-level claim requires matching frame size, device scale, safe areas, element bounds, typography, imagery, platform glyphs, and visual effects. Structural similarity is not pixel fidelity.
- Never use Unicode characters, emoji, or an unofficial icon library as a visual substitute for explicit platform icons while claiming pixel fidelity.
- Separate three modes before implementation: platform-native rebuild, Web approximation, or reference-lock calibration. Do not mix their evidence or quality claims.
- If the input is mixed fidelity, label it instead of pretending it is fully resolved.
- Prefer measurable layout language over vague aesthetic language.
- Treat fonts, spacing, alignment, and component bounds as first-class evidence.
- If the user supplies a target screenshot, compare against that target rather than an imagined ideal.
- If the output is still unstable, recommend another reconstruction round instead of calling it done.

## Implementation rules

### Figma

- Keep a rebuild checklist that a designer can execute manually when direct Figma automation is not available.
- Separate design-token uncertainty from visible evidence. For example, mark an unknown font as an approximation instead of silently inventing a brand typeface.

### Web or React

- Translate the design contract into layout primitives such as CSS grid, flexbox, tokens, reusable components, and responsive breakpoints.
- If the source uses proprietary platform components that cannot be reproduced faithfully on the Web, label the result as a Web approximation. Do not call it an official UI kit implementation.
- For authorized calibration, cropped source evidence may be used only in separate overlays, diffs, annotations, and asset-preparation work. Never render a reference-lock screenshot layer inside the delivered page or claim it as a rebuild.
- Use screenshot comparison after implementation when possible, and call out differences by geometry, typography, color, content, and interaction state.

### Visual evidence

- Capture or receive the source screenshot.
- Capture the rebuilt output at matching viewport, scale, color mode, and device class.
- Normalize source and rebuild to the same exact pixel dimensions before comparison; browser chrome and device framing must be excluded or measured separately.
- Record both logical points and source pixel scale. If the supplied screenshot was resized, compare once at the logical target and once at the original crop dimensions or device scale so resampling blur is not misclassified as layout drift.
- Produce both a 50% alpha overlay and an absolute pixel-difference image. Inspect double edges, duplicated text, crop drift, icon mismatch, and blur/material mismatch.
- After every correction, recapture the rebuild and regenerate the overlay and diff. Old comparison images cannot validate new code.
- Compare structure first, then spacing, typography, color, imagery, and interaction states.
- If the UI spans multiple pages, states, or breakpoints, check the complete set rather than sampling.
- Use clear screenshot names when producing artifacts:
  - `source-<viewport>.png`
  - `rebuild-<viewport>.png`
  - `diff-<viewport>.png`
  - `annotated-<viewport>.png`

## Release evidence contract

Do not treat a successful build or a single screenshot as a completed round trip. For every target that is reported as verified, retain a small evidence set with the same run identifier:

| Evidence | Required content |
| --- | --- |
| Source | The measured source frame, with ownership and scale recorded |
| Rebuild | A fresh coded or native render at the matching viewport and state |
| Overlay | A 50% source/rebuild composite inspected for double edges and duplicated content |
| Diff | An absolute pixel-difference image, with the comparison dimensions recorded |
| Interaction | A log or screenshot proving the important state transitions still work |
| Verdict | Blocking differences, accepted approximations, and the next action |

Regenerate the complete set after every correction. If the target is SwiftUI or UIKit, distinguish an Apple-native implementation from a fixed-canvas calibration fixture. If the target is Web or React, label browser and platform substitutions as approximations. Never carry evidence from an older build into a newer verdict.

## Public showcase gate

Before publishing a case or its screenshots:

1. Confirm that source material is user-owned, licensed, or used only for non-distributable analysis.
2. Replace third-party brands, app artwork, copy, and cropped platform assets with original showcase material.
3. Confirm the delivered interface is made from editable/native/code components, not a reference screenshot layer.
4. Include the source boundary, target, viewport, verification command, and known limitations in the case README.

If any gate is unresolved, publish the workflow documentation only and mark the case `local QA` or `revise`.

### Pixel-fidelity hard stop

Do not return `pass` when any of these remains:

- source and rebuild dimensions differ
- any primary edge produces a visible double line in the overlay
- platform icons are substituted with approximate glyphs
- text, icon, or image content is rendered twice
- a reference image is used as the visual output without being disclosed as reference-lock calibration
- any full-page, header, navigation, card, or component screenshot is rendered as the delivered interface instead of being rebuilt as code or native components
- the latest code has not produced a fresh rebuild screenshot, overlay, and diff

## Output shape

When used well, this skill should produce:

- a short fidelity classification
- a list of structural observations
- a target-specific rebuild checklist
- a QA checklist for the regenerated page
- a list of blocking vs acceptable differences
- a final verdict: pass / revise / rework

For formal test runs, use this compact report shape:

```markdown
# Design Roundtrip QA Test: <case name>

Source:
- Material: <screenshot, mockup, prototype, app screen>
- Ownership: <user-owned, authorized, or public-reference analysis only>
- Target: <Figma, SwiftUI, Web/React, visual QA only>

Fidelity:
- Classification: <wireframe, low-fi, semi-hi-fi, high-fi>
- Known uncertainty: <fonts, data, motion, assets, states>

Design contract:
- Frame:
- Layout:
- Typography:
- Color:
- Components:

Target rebuild:
- Keep:
- Normalize:
- Do not infer:
- Implementation mapping:

Visual QA:
- Blocking differences:
- Acceptable approximations:
- Intentional improvements:

Verdict:
- <pass, revise, rework>
```

## Out of scope

- cloning third-party websites
- stealing assets or brand systems
- replacing a real design review with pure generation
- broad UX audits that are not about the round-trip itself
