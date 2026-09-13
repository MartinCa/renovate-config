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
`lefthook.yaml`). It only matches a `ref: vX.Y.Z` pin that sits directly
under a `- git_url: ...MartinCa/lefthook-configs` `remotes:` entry, so a
ref belonging to an unrelated remote in the same file is left alone. The
version must be a clean `vX.Y.Z` release tag — `v1.2.3-extra` style
suffixes do not match.

Renovate's regex manager matches whole file contents. The replacement
rewrites only the matched `git_url:`/`ref:` block (swapping just the
version), so other `remotes:` entries and any comments are preserved
verbatim.

> Note: the matcher is context-based, not schema-based: any `ref: vX.Y.Z`
> line directly following such a `MartinCa/lefthook-configs` `git_url:` line
> is treated as the pin to bump, in any `lefthook.{yml,yaml}` file — Renovate
> does not verify with lefthook that the block is a real `remotes:` entry.
> Keep `ref:` immediately after `git_url:` inside the same block for the
> matcher to see it.

> Note: preset schemas are validated on the Renovate dashboard itself — this
> repo's CI only checks that all JSON parses, so the regex manager itself is
> not exercised by CI. Test a change by running a Renovate dry run
> (`--dry-run`) against a consumer repo.

## Development

Local commits are guarded by `lefthook`, consuming the shared
[MartinCa/lefthook-configs](https://github.com/MartinCa/lefthook-configs)
fragments pinned at `v2.1.0` in `lefthook.yml` (a thin `remotes:` config).
Staged JSON must be jq-canonical (`jq --indent 2 .`), the staged diff is
secret-scanned with `betterleaks`, staged workflow files are audited with
`zizmor`, and commit messages must follow Conventional Commits.

**AI agents**: do not install the lefthook binary yourself — it is included in
the OpenCode image. If `lefthook` is not on PATH, report this to the user and
ask whether to install it. See `AGENTS.md` for human-contributor install
pointers.
