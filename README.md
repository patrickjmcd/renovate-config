# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for my repos.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>patrickjmcd/renovate-config"]
}
```

`default.json` builds on `config:recommended` and groups non-major updates so
each repo gets a handful of PRs instead of one per dependency:

| Ecosystem | Non-major updates | Schedule |
|---|---|---|
| npm / pnpm | one "npm non-major dependencies" PR | Monday before 6am (Central) |
| Go modules | one "Go non-major dependencies" PR (`go mod tidy` run) | Monday before 6am |
| Python | one "Python non-major dependencies" PR | Monday before 6am |
| GitHub Actions | one "GitHub Actions" PR | any time |

Major updates still get their own PRs. Repo-specific rules go in that repo's
`renovate.json` after the `extends`; they take precedence over this preset.

The Renovate GitHub App needs access to this repo for `local>` presets to resolve.
