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

Left out of the notes: `skip-changelog`, `internal`, `AI: instructions`, `AI: agents`.

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

A change is released as a new tag. A repository picks it up when it moves its `_extends` to that tag, so no repository's notes change by surprise.
