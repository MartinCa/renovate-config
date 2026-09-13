# Agent instructions

Guidance for AI coding agents (and humans) working in this repository.

## Git hooks

Local hooks run through `lefthook`, consuming the shared
[MartinCa/lefthook-configs](https://github.com/MartinCa/lefthook-configs)
fragments pinned at `v2.1.0` in `lefthook.yml` (a thin `remotes:` config):
`lefthook-shared.yml` (secret scan + workflow audit), `langs/json.yml`
(`check-json`), and `commit-msg.yml`.

**AI agents**: do not install the lefthook binary yourself — it is included in
the OpenCode image. If `lefthook` is not on PATH, report this to the user and
ask whether to install it.

This is a non-JS repo: there is no package-manager `prepare` hook, so human
contributors install the standalone lefthook binary and run `lefthook install`
once per clone. The official installer (`curl -fsSL https://get.lefthook.io/install.sh | bash -s 2.1.12`)
or the pinned GitHub release binary (`lefthook_2.1.12_Linux_x86_64` from
evilmartians/lefthook's releases) both work. `v2.1.0` is the current pinned
lefthook-configs ref (see `lefthook.yml`).

- **pre-commit** — `check-json` verifies staged JSON is jq-canonical
  (`jq --indent 2 .`, 2-space indent, trailing newline; fix with
  `jq --indent 2 . file.json > tmp && mv tmp file.json` and re-stage);
  `lefthook-shared.yml` secret-scans the staged diff with `betterleaks`
  (blocks the commit on a leak) and audits staged `.github/workflows/*`
  files with `zizmor` (blocks on a finding).
- **commit-msg** — `commit-msg.yml` enforces Conventional Commits,
  e.g. `feat: ...`, `fix(api): ...`.

zizmor and the JSON syntax check also run in CI (`.github/workflows/ci.yml`).
The secret scan (`betterleaks`) and Conventional-Commits validation are
hook-only: CI does not run `betterleaks` or validate commit messages itself.
Do not bypass the hooks.

Two hook tools must be on `PATH`: `betterleaks` (secret scan, install per its
project README) and `zizmor` (workflow audit, install from zizmor.sh). If a
tool is missing, `LEFTHOOK=0 git commit` skips the hooks entirely — a
pragmatic escape hatch for restricted setups, not a way to dodge the gates.

## Repo conventions

- Preset JSON files must stay jq-canonical (2-space indent, trailing newline):
  `jq --indent 2 . <file> > <file>.tmp && mv <file>.tmp <file>` to fix.
- CI workflows use SHA-pinned actions with version comments and minimal
  permissions — keep it that way for any new workflow.
