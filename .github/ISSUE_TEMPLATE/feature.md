---
name: Feature / Fix
about: Standard issue format for work items
title: '[SHORT TITLE]'
labels: ''
assignees: ''
---

<!-- Org-wide default issue template. The only copy lives in the organization's
     `.github` repository. Do not add a copy to an individual repo: a repo-level
     template overrides this one. Repo-specific details live in each repo's
     CONTRIBUTING.md, not here (this file is public). -->

## Which flow applies
- **Repos with a `dev` branch:** follow the Release Flow below.
- **Repos with no `dev` branch (libraries released by version tag):** open the PR into `main`. The Release Flow table does not apply; use the tag step in the checklist instead.

---

## Release Flow (repos with a `dev` branch)
Every change moves through the same checkpoints, in this order. Nothing skips a checkpoint.

| Step | Where it lives | How it gets there |
|---|---|---|
| 1. PR Preview | Temporary preview link on the PR (where the repo has previews) | Automatic on every PR. Look at it before merging. |
| 2. Staging (the `dev` branch) | The staging site | Merge the PR into `dev`. Everything deploys automatically. |
| 3. Production (the `main` branch) | The production site | Promote `dev` to `main` with one PR. Everything deploys automatically. Rollback steps are in the repo's CONTRIBUTING.md. |

Rules that go with the flow:
- **Feature branches come off `dev` and PRs target `dev`.** Never open a feature PR against `main`, and never commit or push to `main`. This includes documentation-only changes: they go through `dev` like everything else.
- **Promotion is all or nothing.** The `dev` to `main` PR carries everything on `dev`. Only merge into `dev` what you are ready to ship in the same batch. If one change is not ready, revert that change's own PR on `dev`, or hold the whole promotion. Do not copy selected commits into `main`.
- **Promotion, hotfix and sync PRs use a merge commit.** Never "Squash and merge" or "Rebase and merge" them. Label the promotion PR `skip-changelog`.
- **Promotion to `main` needs a separate, explicit "go ahead"** from an authorized approver. It is not covered by the approval to build the work.
- **Migrations:** production migrations are applied by hand, never by the pipeline. The repo's CONTRIBUTING.md says which databases, in what order, and when.
- **Before promoting:** list the release contents (`git log origin/main..origin/dev --no-merges`), check for migration files, check for an open "Sync main into dev" PR (merge it first), and confirm the merge is clean.
- **Hotfix exception:** an urgent fix that cannot wait for the next promotion may go to `main` by a PR from a `hotfix/<issue>-...` branch off `main`. The steps are in the repo's CONTRIBUTING.md. Everything else waits for the next promotion.

---

## Before You Begin
1. Read the **Implementation Guidance** section below.
2. Build a step-by-step implementation plan and present it for approval.
3. Do not write any code or make any changes until an authorized approver responds with "go ahead" or "proceed."
4. **Always create a new branch for this work. Never commit or push directly to `main`.** The base branch and naming rule are in the repo's CONTRIBUTING.md.
5. Push all commits to the feature branch before ending the session.
6. Open a pull request referencing this issue (`Refs #N`) so the repo owner can review and merge it, unless they have explicitly authorized a direct merge for this issue.
7. After the merge, confirm the deploy succeeded and check the change on the staging site (repos with a `dev` branch). Do not promote to `main` without a separate "go ahead."
8. `Refs #N` does not close an issue. Close it yourself, with a comment, after production is confirmed (or, for tag-released repos, after the release is confirmed).

---

## Execution Packet
- **Goal:** <!-- 1–2 sentences: what does done look like? -->
- **Repo:** <!-- repo name -->
- **Branch:** <!-- name of the branch created for this work, e.g. feature/N-short-description; base branch and naming rule: see the repo's CONTRIBUTING.md -->
- **Files:** <!-- e.g. `worker.js`, `auth.js`, `index.html` -->
- **API / DB:** <!-- endpoints, KV namespaces, D1 tables, or other bindings touched, or "none". Migrations: list the files, or write "No migrations." -->
- **Test Target:** <!-- e.g. PR Preview, then the staging URL, or "none" -->
- **Checklist:**
  - [ ] Create feature branch (base branch and naming: see the repo's CONTRIBUTING.md)
  - [ ] <!-- step 3 -->
  - [ ] <!-- step 4 -->
  - [ ] **Repos with `.github/scripts/check-public-assets.mjs`:** if this work adds, removes, or renames any publicly served file, regenerate the allow-list: `node .github/scripts/check-public-assets.mjs --list > .github/expected-public-files.txt`, then confirm the diff reflects only the intended change before committing it
  - [ ] Verify changes locally and on the PR Preview before requesting review (if the repo has previews)
  - [ ] Open pull request referencing this issue (`Refs #N`), into `dev` where the repo has one; all checks green
  - [ ] PR title is one short line (72 characters or fewer) that reads as the release-note entry, and the PR has one label: `breaking`, `bug`, `enhancement` or `documentation`
  - [ ] **Repos with a `dev` branch:** merge into `dev` using the merge method in the repo's CONTRIBUTING.md; confirm the staging deploy run succeeded and verify on the staging site
  - [ ] **Repos with a `dev` branch:** (after an explicit "go ahead") promote `dev` to `main` with a merge commit and the `skip-changelog` label; confirm the production deploy run succeeded
  - [ ] **Tag-released repos (no `dev` branch):** after merge, push a version tag (`git checkout main && git pull && git tag vX.Y.Z && git push origin vX.Y.Z`), confirm the GitHub Release appeared, then bump the pin in every consumer repo
  - [ ] Close this issue with a comment listing the PR numbers and the deploy run IDs (or the release tag)
- **Verify:** <!-- e.g. specific manual test steps on staging, then production, or a curl/command that should succeed -->
- **Stop condition:** If corrected twice without progress, stop and summarize next steps.

---

## Implementation Guidance
<!-- Direction for whoever builds the plan. This is input, not a plan. -->

---

## Context
<!-- Background, motivation, or user-facing/business impact. Keep brief. -->

## Acceptance Criteria
- <!-- criterion 1 -->
- <!-- criterion 2 -->

## Dependencies
- Depends on: <!-- #N or "none" -->
- Supersedes: <!-- #N or "none" -->

---

## Appendix: Field-Level Spec *(if needed)*
<!-- Detailed field definitions, data shapes, edge cases. Reference only, not required reading for implementation. -->
