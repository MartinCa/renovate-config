# renovate-config

Base Renovate preset for personal repos.

Frontend projects (see [MartinCa/frontend-kit](https://github.com/MartinCa/frontend-kit))
extend this **alongside** the frontend-specific preset, not instead of it —
add both to the project's `extends` array:

```json
{
  "extends": [
    "github>MartinCa/renovate-config",
    "github>MartinCa/frontend-kit:renovate-frontend"
  ]
}
```

## Lefthook config ref auto-bumping

Repos that consume [MartinCa/lefthook-configs](https://github.com/MartinCa/lefthook-configs)
as a `remotes:` entry in their `lefthook.yml` can extend this preset to have
Renovate bump the pinned `ref:` tag:

```json
{
  "extends": ["github>MartinCa/renovate-config:lefthook"]
}
```

The preset adds a `custom.regex` manager watching `lefthook.yml` (and
`lefthook.yaml`) for `ref: vX.Y.Z` pins pointing at
`MartinCa/lefthook-configs` and bumps them to the latest GitHub release tag.

> Note: preset schemas are validated on the Renovate dashboard itself — this
> repo's CI only checks that all JSON parses, so the regex manager itself is
> not exercised by CI. Test a change by running a Renovate dry run
> (`--dry-run`) against a consumer repo.

## Development

Local commits are guarded by `lefthook`, consuming the shared
[MartinCa/lefthook-configs](https://github.com/MartinCa/lefthook-configs)
fragments pinned at `v2.0.1` in `lefthook.yml` (a thin `remotes:` config).
Staged JSON must be jq-canonical (`jq --indent 2 .`), the staged diff is
secret-scanned with `betterleaks`, staged workflow files are audited with
`zizmor`, and commit messages must follow Conventional Commits.

**AI agents**: do not install the lefthook binary yourself — it is included in
the OpenCode image. If `lefthook` is not on PATH, report this to the user and
ask whether to install it. See `AGENTS.md` for human-contributor install
pointers.
