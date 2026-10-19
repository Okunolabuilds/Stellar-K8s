# PITCH Storyboards

This document contains the PITCH storyboards for the project. Storyboards provide a narrative, visual-first walkthrough of the problem we solve, our solution, the architecture, and the key user journeys. They are intended to align contributors and stakeholders on the "why" and "how" before diving into implementation details.

> **Note:** Storyboards are lightweight narrative sketches. For authoritative technical details, see the linked documentation throughout this page.

## Table of Contents

- [Problem](#problem)
- [Solution](#solution)
- [Architecture](#architecture)
- [User Journeys](#user-journeys)
- [Related Documentation](#related-documentation)

---

## Problem

**Storyboard: "The Friction Today"**

1. **Scene 1 — A contributor opens the repository.** They are greeted with a large codebase and no clear narrative of what it does or why it exists.
2. **Scene 2 — They hunt for context.** Documentation is scattered; the entry point ([README.md](../README.md)) explains *what* but not *why* or *how* the pieces fit together.
3. **Scene 3 — Onboarding stalls.** New contributors spend significant time reconstructing the project's intent from code and commit history.
4. **Scene 4 — Stakeholders lack a shared picture.** Reviewers and maintainers evaluate changes without a common mental model of the system.

**Pain points:**

- Fragmented understanding of project intent.
- Slow onboarding for new contributors.
- Inconsistent mental models across reviewers and maintainers.

---

## Solution

**Storyboard: "A Shared Narrative"**

1. **Scene 1 — A contributor opens the repository.** The [README.md](../README.md) points them to the PITCH storyboards.
2. **Scene 2 — They read the storyboards.** Problem, solution, architecture, and user journeys are presented in a single, coherent narrative.
3. **Scene 3 — They follow links into detail.** Each storyboard links to the relevant in-depth documentation.
4. **Scene 4 — Everyone shares the same picture.** Contributors, reviewers, and stakeholders align on intent before implementation.

**Outcomes:**

- Faster onboarding through a clear narrative entry point.
- Consistent mental models across the team.
- Documentation that stays discoverable via the site navigation.

---

## Architecture

**Storyboard: "How the Pieces Fit"**

1. **Entry point** — [README.md](../README.md) introduces the project and links to this document.
2. **Narrative layer** — These PITCH storyboards describe intent and journeys.
3. **Reference layer** — Detailed docs (see [Related Documentation](#related-documentation)) describe components and behavior.
4. **Build & CI layer** — Documentation is validated in CI (`make nav-coverage`, `make docs-ci-build`) so links and navigation stay consistent.

```
README.md
   └── docs/pitch-storyboards.md   (this file)
          ├── Problem
          ├── Solution
          ├── Architecture
          └── User Journeys
                 └── links → related docs
```

---

## User Journeys

### Journey 1: New Contributor Onboarding

1. Lands on [README.md](../README.md).
2. Follows the link to the PITCH storyboards.
3. Reads the problem and solution to build context.
4. Follows links into detailed documentation.
5. Makes their first contribution with confidence.

### Journey 2: Reviewer Evaluating a Change

1. Opens a pull request.
2. Consults the storyboards to recall project intent.
3. Verifies the change aligns with the documented architecture and journeys.
4. Approves or requests changes with shared context.

### Journey 3: Stakeholder Seeking an Overview

1. Opens the documentation site.
2. Navigates to the PITCH storyboards via the site nav.
3. Gets a concise narrative overview without reading code.
4. Follows links for deeper detail as needed.

---

## Related Documentation

- [README.md](../README.md) — Project overview and entry point.
- [mkdocs.yml](../mkdocs.yml) — Documentation site configuration and navigation.

---

## Maintaining This Document

- Keep storyboards concise and narrative-first.
- Use relative links to other docs so they resolve on the documentation site.
- When adding a new storyboard, update the [Table of Contents](#table-of-contents) and the site navigation in [mkdocs.yml](../mkdocs.yml).
- Run `make nav-coverage` and `make docs-ci-build` locally to verify navigation and links before opening a pull request.