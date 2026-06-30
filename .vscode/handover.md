# Handover: HA Add-on Cleanup Playbook

Reference for cleaning up the corgan2222 Home Assistant add-on repos.
`addon-emqx` is fully done and is the template. Apply the same to
`addon-emqx6` and `addon-grafana`. No secret values are stored in this
file — only secret *names* (which already appear in the public workflows).

## Repos

- `addon-emqx` — DONE (detached from fork, cleaned, released v0.8.4).
- `addon-emqx6` — original repo (NOT a fork). EMQX Enterprise v6.
- `addon-grafana` — fork of hassio-addons/addon-grafana. Grafana Enterprise.
- `hassio-addons_repository` — the store (this repo); also the metadata
  publishing target for all three add-ons.

## How it works (architecture)

Each add-on repo has reusable-workflow callers:

- `ci.yaml` → `hassio-addons/workflows .../app-ci.yaml` (lint + build-test).
- `deploy.yaml` → `.../addon-deploy.yaml` (multi-arch build + push to ghcr,
  then `repository_dispatch: update` to the store).
- `labels.yaml`, `pr-labels.yaml`, `release-drafter.yaml` (kept on purpose).

The store repo (`hassio-addons_repository`) has:

- `repository.json`, `.apps.yml` (channel: stable + `image:` map
  `ghcr.io/corgan2222/<slug>/{arch}`), per-add-on folder
  (config.yaml, DOCS.md, README.md, CHANGELOG.md, icon, logo),
  `lint.yaml`, `repository-updater.yaml`.

**Release flow:** publish a GitHub release → `deploy.yaml` builds the
multi-arch image, pushes to ghcr, and dispatches `update` to the store →
the store's `repository-updater.yaml` bumps the version, copies docs,
generates the README, prepends the CHANGELOG, and announces on Discord.

## THE GOLDEN RULE: edit the SOURCE, not the store

The `repository-updater` regenerates the store content from the source
add-on repo on every release. Direct edits to the store get overwritten.

- `DOCS.md` — copied from `addon-<slug>/<slug>/DOCS.md`.
- `README.md` — GENERATED from `addon-<slug>/<slug>/.README.j2` (a Jinja2
  template). The leftover "Frenck header" (Sponsor/Patreon Frenck, title
  without "Enterprise") lives in this template — fix it there.
- `config.yaml` version/`image:` — filled by the updater from `.apps.yml`.

## The emqx playbook (each step = its own PR + a label)

1. **Detach fork**: GitHub → repo Settings → Danger Zone →
   "Leave fork network". (`emqx6` is already not a fork — skip.)
2. **Remove bot workflows**: delete `.github/workflows/stale.yaml` +
   `lock.yaml`. Dedupe a duplicate `.github/renovate.json` if a root one
   also exists. (Keep labels/pr-labels/release-drafter.)
3. **Disable edge builds**: in `deploy.yaml` remove the `workflow_run`
   trigger, keep only `on: release: published`.
4. **Pin reusable workflows to v3.0.0**: `hassio-addons/workflows/...@main`
   → `@383c10d83acbe341acbb35a4a61bfd14827f00f0` in ci/deploy/labels/
   pr-labels. Put the `# v3.0.0` comment on a SEPARATE line ABOVE `uses:`,
   never inline (see lint gotcha).
5. **Version bump** (add-on specific): bump the bundled app version in the
   Dockerfile `ARG` + the README badge. Verify the upstream release and its
   assets exist first. (emqx: `EMQX_VERSION` e5.10.3 → e5.10.4.)
6. **Security (zizmor) fixes**:
   - pin `release-drafter/release-drafter@v6.1.0` →
     `@b1476f6e6eb133afa41ed8589daba6dc69b4d3f5` (separate comment line).
   - add minimal `permissions:` at the reusable-workflow caller job —
     deploy: `contents: read` + `packages: write`; labels: `issues: write`;
     pr-labels: `{}`. (Values mirror each reusable workflow's own
     `permissions:` at v3.0.0.)
   - `pr-labels.yaml`: switch `pull_request_target` → `pull_request`
     (solo repo, no external fork PRs).
7. **Rebrand the README**: edit `addon-<slug>/<slug>/.README.j2` (NOT the
   store README) — title → "EMQX Enterprise", remove Sponsor/Patreon Frenck,
   use your badges. Propagates on the next release.
8. **Release**: release-drafter auto-drafts on push to main. Publish it:
   `gh release edit <tag> --draft=false --latest`. Publishing fires
   deploy → ghcr → store update → CHANGELOG + Discord.

## Gotchas

- **Every PR goes red on `Verify`**: `pr-labels` requires a label from
  {breaking-change, bugfix, documentation, enhancement, refactor,
  performance, new-feature, maintenance, ci, dependencies, translations},
  but `labels.yaml` is disabled (GitHub auto-disables cron workflows after
  60 days of inactivity), so the labels never get created. Fix:
  `gh label create <label> -R <repo>` then
  `gh pr edit <n> -R <repo> --add-label <label>`.
  If the PR was opened BEFORE the label existed and `Verify` already failed,
  do NOT `gh run rerun` (it replays the old, label-less event) — instead
  remove + re-add the label to fire a fresh `labeled` event.
- **yamllint vs Prettier comment conflict**: SHA-pin comments must be on a
  separate line above `uses:`. yamllint wants ≥2 spaces before an inline
  comment; Prettier wants exactly 1 → an inline `@sha # vX` can never pass
  both (app-ci runs both linters).
- **Windows CRLF**: local prettier/yamllint warn about line endings; git
  stores LF so CI is clean. Verify locally with
  `prettier --end-of-line lf` and CR-stripped yamllint.
- **repository-updater double-run**: a stable release dispatches both
  "stable" and "beta" into the single store repo → two updater runs →
  duplicate CHANGELOG + double Discord. FIXED in `repository-updater.yaml`
  (concurrency + idempotent changelog + announce gating + printf fix).
  The store is shared, so this is fixed once for all add-ons.
- **HA shows the old version / duplicated changelog after a release** =
  HA add-on store CACHE. The store + ghcr image are correct; HA just needs
  a refresh: Settings → Add-ons → Add-on Store → ⋮ → "Check for updates"
  (or restart Supervisor, or `ha addons reload`).
- **Store lint** runs only yamllint + JSON lint (no markdownlint).
- **.yamllint ignores `*/config.yaml`** in the store, so add-on config files
  are not actually linted — a known coverage gap.

## Key facts / pins

- `hassio-addons/workflows` v3.0.0 =
  `383c10d83acbe341acbb35a4a61bfd14827f00f0`
- `release-drafter/release-drafter` v6.1.0 =
  `b1476f6e6eb133afa41ed8589daba6dc69b4d3f5`
- repository-updater / repository-lint pinned v2.0.6 =
  `3846ae0fd09acec8ac1ac308ceacd052b9d01bec`
- Secrets used (NAMES only — set in repo settings, never store values):
  `DISPATCH_TOKEN`, `UPDATER_TOKEN`, `DISCORD_WEBHOOK`.

## Channels (edge / stable / beta)

`addon-deploy` picks the channel from the trigger:
push/workflow_run → edge (version = git SHA); full release → stable
(version = tag); pre-release → beta (version = tag). All three targets
point at the one store repo (`channel: stable`), so only stable actually
reaches users; edge/beta dispatches resolve to stable. A real beta channel
would need a separate `channel: beta` store repo.

## Per-add-on TODO

- **emqx** — DONE: detached, cleaned, pinned v3.0.0, EMQX e5.10.4, security
  fixes, v0.8.4 released. README rebrand of `.README.j2` in progress.
- **emqx6** — NOT a fork (skip detach). Apply steps 2–8. Extra real failure:
  `YAMLLint` red (the auto-generated, line-wrapped `description` in
  `config.yaml` — fix the wrapped line). Bundled: EMQX Enterprise v6.
- **grafana** — fork (detach). Apply steps 2–8. Extra real failures:
  `Build amd64` + `Hadolint` red — fix the Dockerfile. Bundled: Grafana
  Enterprise.
- **store (this repo)** — `repository-updater` hardened (done). Optionally
  remove stale/lock here too.

## emqx PRs (reference)

- addon-emqx #1 remove bot workflows + dedupe renovate (merged)
- addon-emqx #2 deploy only on release / drop edge (merged)
- addon-emqx #3 pin reusable workflows to v3.0.0 (merged)
- addon-emqx #4 EMQX Enterprise e5.10.4 (merged)
- addon-emqx #5 tighten workflow security / zizmor (merged)
- hassio-addons_repository #2 dedupe changelog + harden updater (merged)

## Conventions (preferences)

- Commits AND PR titles/descriptions in ENGLISH (subject + body).
- Never self-attribute as co-author; post as the user.
- Never push to main/master directly — always via PRs.
