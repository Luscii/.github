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
_extends: Luscii/.github:release-drafter.yml@1
```

A repository that deploys a pre-release and promotes it after the deploy adds:

```yaml
prerelease: true
latest: false
```

A Go module adds `tag-template: "v$RESOLVED_VERSION"`; see [Version tags](#version-tags).

The workflow must run `release-drafter/release-drafter@v7` or later; v5 and v6 cannot read this file.

### Changing it

1. Every merge to `main` updates a draft release (`Draft Release`, from the base itself).
2. Publishing the draft tags `X.Y.Z` and moves `X`, `X.Y` and `latest` to its commit (`Release` → `Promote Release`). A published pre-release moves nothing.

A repository that pins `@1` picks up every `1.*` release. A breaking change to the base (a `feat!:` PR, or the `version: major` label) becomes `2`, which each repository adopts by moving its `_extends`.

PRs are labelled by [`.github/labeler.yml`](.github/labeler.yml), the same rules any repository extending the base can copy.

## Version tags

Luscii and OMRON repositories tag releases `X.Y.Z`, without a `v`: `1.4.2`, not `v1.4.2`. The release *name* keeps the `v` (`v1.4.2`); only the tag is bare. Repositories that others pin also move `X`, `X.Y` and `latest` to each full release, so a consumer pins `@1` (or `?ref=1` for a Terraform module).

**The one exception is a Go module.** Go only resolves module versions from `vX.Y.Z` tags (`go install …@v1.4.2`), so a Go module sets `tag-template: "v$RESOLVED_VERSION"`, and its moving tags are `vX` and `vX.Y`.

GitHub Actions are no exception: an action is pinned `@1`, like everything else.

The `v1.0.0`, `v1` and `v1.0` tags in this repository come from before this rule. They are frozen: nothing moves them any more, and they go once no repository pins `@v1`.
