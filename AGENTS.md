# AGENTS.md — layer-uv

Standalone candy repo for the `uv` layer — Astral's uv/uvx Python package and
project manager, installed as direct-download binaries. The candy lives in
`charly.yml` at the repo root: the `download:`/extract `plan:` step (with its
explicit `uninstall:` list), the `check:` assertions, and the embedded `skill:`
entity projected into the marketplace corpus as `/charly-coder:uv`.

Canonical files:

- `charly.yml` — the `uv:` candy entity and the `uv-skill:` skill entity.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:uv` — the owning skill. Why uv is a direct download (not pixi),
  the `strip_components` mechanics, and the binary contract. Load before editing
  or troubleshooting the layer.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:` and `download:`, per-distro `distro:` arms,
  package/repo sections, service declarations). Load before editing any entity
  field or plan step.

## Build / validate / test

- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no
  per-repo candy gate** and ships only `.github/workflows/tag-on-merge.yml`.
- The candy's `plan:` `check:` steps are the functional evidence. The three
  checks (`uv-binary`, `uv-version`, `uvx-binary`) fail on an image that installed
  nothing.
- The `download:` step's `uninstall:` list must name both `/usr/local/bin/uv` and
  `/usr/local/bin/uvx` — a bare `to: /usr/local/bin` uninstall would wipe every
  system-local binary.

## Modify this repo

- Edit the `uv:` candy entity AND the `uv-skill:` skill entity in `charly.yml`
  together. The skill is the projected usage source, so a behaviour change not
  mirrored in the skill leaves the corpus stale.
- `${BUILD_ARCH}` expands at build time; keep the URL template arch-correct.
- New behaviour claims belong in the `plan:` as an observable `check:` step, and
  in the skill body.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
