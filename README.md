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
