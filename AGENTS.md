# AGENTS.md — layer-debug-tools

Standalone candy repo for the `debug-tools` layer — the in-container debug
toolkit. The candy lives in `charly.yml` at the repo root: the per-distro
`package:` arms (`fedora` / `arch` / `debian` / `ubuntu`) and the ordered
`plan:` of `check:` assertions over the headline binaries. The repo declares
**no `skill:` entity**; the owning guidance is the family skill
`/charly-versa:debug-tools-layer` (the gap is tracked in
`opencharly/opencharly#291`).

Canonical files:

- `charly.yml` — the `debug-tools:` candy entity (no `skill:` entity).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-versa:debug-tools-layer` — the owning (family) skill. The per-distro
  package-name divergence and the build-scope check probes. Load before editing
  or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, per-distro `distro:` arms, package
  sections, service declarations). Load before editing any entity field or plan
  step.
- **Missing owning skill:** this candy has no `skill:` entity of its own, so no
  page is projected from this repo. The gap is recorded against the named batch
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence: the headline
  binaries at fixed paths and their packages. They must stay valid on every
  distro arm they run on. Scope a distro-specific check in the command itself —
  the check runner does not honour runner-level `exclude-distro` fields.

## Modify this repo

- There is no `skill:` entity here to edit; a package change is mirrored only in
  the `charly.yml` entity and its `plan:`.
- Package changes go in the matching per-distro `package:` arm; keep the arms
  honest (no synthetic "common" list — the tool names genuinely differ).
- New behaviour claims belong in the `plan:` as an observable `check:` step.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
