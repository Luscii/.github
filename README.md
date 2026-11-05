# .github

Organisation-wide defaults for Luscii repositories.

## Release notes

[`.github/release-drafter.yml`](.github/release-drafter.yml) is the Release Drafter (v7) base that Luscii repositories extend. It groups each merged PR once, under the first category that matches its labels, and strips the conventional-commit prefix from each entry. Everything no other category takes lands under 🔧 Internal Changes, so the notes have no `What's Changed` heading.

| Category | Labels |
|---|---|
| 🚨 Breaking Changes (major) | `version: major` |
| ✨ New Features (minor) | `version: minor`, `feature` |
| 🐛 Bug Fixes | `bug`, `fix` |
| 📦 Examples | `examples` |
| ⬆️ Dependency Updates | `dependencies` |
| 🏗️ Infrastructure | `infrastructure` |
| 📚 Documentation | `documentation` |
| 🔧 Internal Changes | everything else |

Left out of the notes, and of version resolution: `skip-changelog` and `internal`, labels a person sets on purpose.

### Using it

In the repository, `.github/release-drafter.yml`:

```yaml
_extends: Luscii/.github:release-drafter.yml@v1
```

A repository that deploys a pre-release and promotes it after the deploy adds:

```yaml
prerelease: true
latest: false
```

A repository whose tags start with `v` keeps them with `tag-template: "v$RESOLVED_VERSION"`.

The workflow must run `release-drafter/release-drafter@v7` or later; v5 and v6 cannot read this file.

### Changing it

This repository releases like the Luscii `gha-` repositories:

1. Every merge to `main` updates a draft release (`Draft Release`, from [`.github/release-drafter-self.yml`](.github/release-drafter-self.yml), which extends the base and adds the `v` tag).
2. Publishing the draft tags `vX.Y.Z` and moves `vX`, `vX.Y` and `latest` to it (`Release` → `Promote Release`).

A repository that pins `@v1` picks up every `v1.*` release; a breaking change to the base (a `feat!:` PR, or the `version: major` label) becomes `v2`, which each repository adopts by moving its `_extends`.

PRs are labelled by [`.github/labeler.yml`](.github/labeler.yml), the same rules any repository extending the base can copy.
