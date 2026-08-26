# Validation Matrix

Use this matrix for each round-trip case. One row represents one target and one meaningful state set.

| Target | Source scale | Fresh rebuild | Overlay | Diff | Interaction | Verdict |
| --- | --- | --- | --- | --- | --- | --- |
| Figma | recorded | required | required | required | required where applicable | pass / revise / rework |
| SwiftUI / UIKit | recorded | simulator capture | required | required | required | pass / revise / rework |
| Web / React | recorded | browser capture | required | required | required | pass / revise / rework |

## Minimum record

- Source ownership and whether redistribution is allowed
- Logical frame size and original pixel dimensions
- Target platform, OS or browser, and viewport
- Build or launch command
- Screenshot paths sharing one run identifier
- Blocking differences and accepted approximations
- Final verdict with the next correction if the result is not `pass`

## Do not pass

Mark the row `revise` when the newest code has not produced a fresh rebuild, overlay, and diff. Mark it `rework` when the interface is still a screenshot lock, uses substituted platform icons while claiming native parity, or mixes evidence from different viewport scales.
