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

| Updates | PR | Schedule |
|---|---|---|
| npm / pnpm, non-major | one "npm non-major dependencies" PR | Monday before 6am (Central) |
| Go modules, non-major | one "Go non-major dependencies" PR (`go mod tidy` run) | Monday before 6am |
| Python, non-major | one "Python non-major dependencies" PR | Monday before 6am |
| Docker base images, non-major | one "Docker images" PR | any time |
| GitHub Actions, non-major | one "GitHub Actions" PR, **automerged** when CI passes | any time |
| GitHub Actions, major | one "GitHub Actions (major)" PR | any time |
| Node.js / pnpm / `@types/node` | one "Node.js and pnpm" PR (+ one for majors) | any time |

TypeScript majors wait for approval on the Dependency Dashboard: TypeScript 7
changed the compiler API that Next.js and SvelteKit load it through, so they
break builds until those tools catch up.

npm updates wait a day after release (`minimumReleaseAge`), matching pnpm 12's
own minimum release age; otherwise a fresh release fails the PR's first install.

No hourly PR limit: grouping keeps the count low, and the default 2/hour left
most of a newly onboarded repo's updates queued for days.

Major updates of libraries (npm, Go, Python) still get their own PRs, since those can break code. Repo-specific rules go in that repo's
`renovate.json` after the `extends`; they take precedence over this preset.

The Renovate GitHub App needs access to this repo for `local>` presets to resolve.
