# Fork Governance: camillanapoles/Agent-Skills-for-Context-Engineering

**Maintainer**: Camilla Napoles (`cnmfs@cesar.school`)  
**Upstream**: `muratcankoylan/Agent-Skills-for-Context-Engineering`  
**Last updated**: 2026-04-19  
**Version**: 1.0.0

---

## 1. Branch Strategy

### 1.1 Branch Conventions

| Branch | Purpose | Policy |
|--------|---------|--------|
| `cnmfs-dev/main` | Stable mirror of upstream; fork integration base | Never commit own work directly; only upstream syncs |
| `cnmfs-dev/skill-<name>` | One new or revised skill | Short-lived; PR then delete |
| `cnmfs-dev/docs-<topic>` | Documentation changes only | Short-lived; PR then delete |
| `cnmfs-dev/examples-<name>` | Example projects or case studies | Short-lived; PR then delete |
| `cnmfs-dev/fix-<issue>` | Bug or quality fix on existing content | Short-lived; PR then delete |
| `cnmfs-dev/chore-<topic>` | Manifests, CI, tooling, formatting | Short-lived; PR then delete |
| `cnmfs-dev/review-<sha>` | Retrospective review of a specific commit | Time-boxed; close after decision |

### 1.2 Rules

1. **1 branch = 1 objective.** Never mix skill content, docs, examples, and manifests in the same branch.
2. **Short-lived branches only.** Merge or delete within 2 weeks of opening.
3. **No force-push after PR is open.** Use additional commits to address review feedback.
4. **Atomic commits.** Each commit in a branch should be independently revertable without breaking other commits in the branch.
5. **Upstream syncs via `cnmfs-dev/main` only.** Cherry-pick or merge from there into topic branches. Never pull upstream directly into a topic branch.

### 1.3 Upstream Sync Workflow

```
upstream/main
     │
     ▼ (git fetch upstream && git merge upstream/main)
cnmfs-dev/main
     │
     ▼ (cherry-pick or merge specific commits)
cnmfs-dev/<topic>
     │
     ▼ (PR review + checklist)
cnmfs-dev/main (or upstream PR)
```

---

## 2. Commit Standards

### 2.1 Message Format

```
<type>(<scope>): <imperative description>

[optional body — what changes, why, risk level]
[optional: Closes #<issue>]
```

**Types:**

| Type | When to use |
|------|------------|
| `feat` | New skill, new example, new capability |
| `fix` | Corrects a factual error, broken code, or inconsistency |
| `docs` | Pure documentation — README, guides, analysis docs |
| `refactor` | Restructures without changing behavior (e.g., extract reference file) |
| `chore` | Manifests, .gitignore, CI, tooling changes |
| `test` | Adding or updating tests |
| `revert` | Explicitly reverting a previous commit |

**Scopes** (match the area changed):

| Scope | Files affected |
|-------|--------------|
| `skill/<name>` | `skills/<name>/SKILL.md`, `references/`, `scripts/` |
| `example/<name>` | `examples/<name>/` |
| `marketplace` | `.claude-plugin/marketplace.json`, `.plugin/plugin.json` |
| `docs` | `docs/`, root `README.md`, `CONTRIBUTING.md` |
| `template` | `template/SKILL.md` |

**Examples:**

```
feat(skill/latent-briefing): add KV cache memory sharing skill

Implements orchestrator-worker token reduction via Attention Matching
compaction. Adds SKILL.md and references/attention-matching-formulation.md.
Risk: low — additive only.
```

```
fix(skill/multi-agent-patterns): rebalance agent decomposition guidance

Adds explicit anti-pattern for over-decomposition (<3 subtasks).
Addresses review of 3cec3f5: the "agent dissociation" framing was
too promotion-heavy relative to the cost/overhead warnings.
Risk: medium — changes recommended patterns.
```

### 2.2 Atomicity Criteria

A commit is atomic when:
- It changes **one logical thing** (one skill, one doc, one manifest update).
- Reverting it does not break unrelated content.
- Its purpose is clear from the message alone without reading the diff.

---

## 3. Validation Checklist (Quality Gate)

Run this checklist before opening a PR and before merging. Every "No" is a blocker.

### Gate 1 — Commit Clarity

- [ ] Each commit has a clear `type(scope): description` message.
- [ ] The commit touches only files related to its stated scope.
- [ ] No commit mixes skill content + docs + manifests.

### Gate 2 — Skill Format (for skill branches only)

- [ ] `SKILL.md` has valid YAML frontmatter with `name` and `description`.
- [ ] `name` matches the parent directory name (lowercase, hyphens only, ≤64 chars).
- [ ] `description` is ≤1024 chars and includes "Use when" trigger conditions.
- [ ] `SKILL.md` body is ≤500 lines (move excess to `references/`).
- [ ] References are one level deep from `SKILL.md` (no nested references).
- [ ] Skill includes a `## Gotchas` section with ≥3 real failure modes.
- [ ] No vendor-locked examples without abstraction.
- [ ] No time-sensitive dates in the skill body.

### Gate 3 — Marketplace/Plugin Consistency (for manifest changes)

- [ ] Skill listed in `.claude-plugin/marketplace.json` matches its directory name.
- [ ] Version in `.claude-plugin/marketplace.json` and `.plugin/plugin.json` are in sync.
- [ ] No orphaned skill paths (every path in the manifest exists on disk).

### Gate 4 — Multi-Agent Pattern Review (for skill/multi-agent-* or agent-heavy examples)

- [ ] New agent decompositions have ≥3 genuinely independent subtasks.
- [ ] Coordination overhead is estimated and documented.
- [ ] Gotchas section includes "over-decomposition" and "agent sprawl" risks.
- [ ] The example does not promote dissociating agents without clear isolation benefit.

### Gate 5 — Code Quality (for branches with executable scripts or tests)

- [ ] Python scripts pass `ruff check .` with no errors.
- [ ] TypeScript passes `npm run lint` and `npm run typecheck`.
- [ ] Tests pass: `pytest` (Python) or `npm test` (TypeScript).
- [ ] No new dependencies added without security check (`gh-advisory-database`).

### Gate 6 — Context Engineering Alignment

- [ ] Change adds signal; does not add noise to the context window.
- [ ] If compressing or summarizing, artifact references (paths, SHAs, names) are preserved verbatim.
- [ ] No "lost-in-middle" risk (critical content is not buried in the middle of a long document).

---

## 4. Upstream Sync Policy

### 4.1 When to sync

Sync from upstream when:
- A new skill is published upstream.
- A bug fix or quality improvement lands upstream.
- The upstream version number changes.

### 4.2 How to sync

```bash
# 1. Add upstream remote (one-time setup)
git remote add upstream https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering.git

# 2. Fetch upstream changes
git fetch upstream main

# 3. Update cnmfs-dev/main
git checkout cnmfs-dev/main
git merge upstream/main --no-ff -m "chore(docs): sync upstream main @ <upstream-sha>"

# 4. Review what changed
git log cnmfs-dev/main..HEAD --oneline

# 5. Cherry-pick or merge into relevant topic branches as needed
```

### 4.3 After sync

- Review each file changed in the upstream merge.
- Apply the validation checklist to upstream-introduced changes.
- Document the sync in `docs/commit-audit-matrix.md`.

---

## 5. Definition of Done

A topic branch is ready to merge when:

1. All 6 gates in Section 3 pass with no blockers.
2. At least one self-review round has occurred (re-read the diff cold the next day).
3. Commit messages follow Section 2.1 format.
4. `docs/commit-audit-matrix.md` is updated with the decision (keep/refactor/revert).
5. If the branch revises agent patterns: rationale is documented in the commit body.

---

## 6. References

- [Commit Audit Matrix](./commit-audit-matrix.md) — classification and decisions for all commits
- [Agent Skills Specification](./agentskills.md) — official format reference
- [Skills Improvement Analysis](./skills-improvement-analysis.md) — gap analysis vs. Anthropic best practices
- [multi-agent-patterns skill](../skills/multi-agent-patterns/SKILL.md) — canonical guidance on when agents should be separated
