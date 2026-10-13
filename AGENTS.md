# AGENTS.md

How every repository under this account works:

- Commits follow [Conventional Commits](https://www.conventionalcommits.org): `feat:`, `fix:`, and `!` for a breaking change (`feat!:`). Other types (`ci:`, `docs:`, `refactor:`, …) don't release.
- Pull requests are squash-merged with their title as the commit, so the title is the Conventional Commit and one pull request is one changelog entry. Pull requests into `master` need green CI; the owner can also push to `master` directly.
- Releases are [release-please](https://github.com/googleapis/release-please)'s pull request into `master`, which it keeps up to date with the next version and changelog. Merging it tags the version and publishes the GitHub release. Each repository calls `.github/workflows/release.yml` here; its own `release-please-config.json` and `.release-please-manifest.json` hold the rest.
- Versions are [SemVer](https://semver.org) and tags are `v` + version (`v1.2.3`), on a `master` commit, made only by release-please. They carry no commit hash: the hash is the image's `org.opencontainers.image.revision` label and is in the ops deploy commit. Release-please runs as the account's release app, so CI runs on its pull requests; each repository holds the app's `RELEASE_APP_CLIENT_ID` variable and `RELEASE_APP_PRIVATE_KEY` secret.
- CI builds an app's images on its release and commits their new tags to [ops](https://github.com/sourcelocation/ops) (`apps/<app>`), where Flux deploys them. Rollback: revert that commit.
