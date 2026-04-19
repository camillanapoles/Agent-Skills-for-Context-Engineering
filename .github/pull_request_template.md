---
name: about
about: Default pull request template for cnmfs-dev/* branches
---

## Summary

<!-- One sentence: what does this PR change and why? -->

## Branch type

<!-- Check the one that applies -->

- [ ] `cnmfs-dev/skill-<name>` — new or revised skill
- [ ] `cnmfs-dev/docs-<topic>` — documentation only
- [ ] `cnmfs-dev/examples-<name>` — example project
- [ ] `cnmfs-dev/fix-<issue>` — bug or quality fix
- [ ] `cnmfs-dev/chore-<topic>` — manifests, CI, tooling
- [ ] `cnmfs-dev/review-<sha>` — retrospective commit review

## Commit audit matrix entry

<!-- Update docs/commit-audit-matrix.md before merging. Paste the row here: -->

| SHA | Type | Risk | Decision |
|-----|------|------|---------|
| _pending_ | | | |

---

## Quality Gate Checklist

### Gate 1 — Commit Clarity

- [ ] Each commit has a `type(scope): description` message.
- [ ] No commit mixes skill content + docs + manifests.
- [ ] Each commit is independently revertable.

### Gate 2 — Skill Format _(skip if no skill changes)_

- [ ] `SKILL.md` frontmatter: `name` and `description` present and valid.
- [ ] `name` matches directory name (lowercase, hyphens only, ≤64 chars).
- [ ] `SKILL.md` body ≤500 lines.
- [ ] References are one level deep from `SKILL.md`.
- [ ] `## Gotchas` section with ≥3 real failure modes.
- [ ] No vendor-locked examples without abstraction.

### Gate 3 — Marketplace/Plugin _(skip if no manifest changes)_

- [ ] Skill paths in `marketplace.json` match directories on disk.
- [ ] Version numbers consistent between `.claude-plugin/` and `.plugin/`.

### Gate 4 — Multi-Agent Review _(skip if no agent patterns introduced)_

- [ ] New agent decompositions have ≥3 genuinely independent subtasks.
- [ ] Coordination overhead estimated and noted.
- [ ] Gotchas include "over-decomposition" and "agent sprawl" risks.

### Gate 5 — Code Quality _(skip if no executable code)_

- [ ] Python: `ruff check .` passes.
- [ ] TypeScript: `npm run lint && npm run typecheck` passes.
- [ ] Tests pass: `pytest` or `npm test`.

### Gate 6 — Context Engineering Alignment

- [ ] Change adds signal; does not add noise.
- [ ] Artifact references (paths, SHAs, names) preserved verbatim where critical.

---

## Commit `3cec3f5` connection _(only if this PR addresses the REFACTOR decision)_

<!-- If this PR is related to the retrospective review of 3cec3f5, describe how it addresses the multi-agent framing concern. -->

- [ ] `docs/commit-audit-matrix.md` updated with the resolution.
