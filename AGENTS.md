# AGENTS.md

How every repository under this account works:

- Commits follow [Conventional Commits](https://www.conventionalcommits.org): `feat:`, `fix:`, and `!` for a breaking change (`feat!:`). Other types (`ci:`, `docs:`, `refactor:`, …) don't release.
- Releases are [release-please](https://github.com/googleapis/release-please)'s pull request into `master`, which it keeps up to date with the next version and changelog. Merging it tags the version and publishes the GitHub release. Each repository calls `.github/workflows/release.yml` here; its own `release-please-config.json` and `.release-please-manifest.json` hold the rest.
- CI builds an app's images on its release and commits their new tags to [ops](https://github.com/sourcelocation/ops) (`apps/<app>`), where Flux deploys them. Rollback: revert that commit.
