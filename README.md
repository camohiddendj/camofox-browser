# Camofox Browser Automation Branch

This orphan branch is intentionally separate from upstream source history. It exists only to run repository automation for the fork.

## What Runs

The workflow in `.github/workflows/sync-and-publish.yml` runs once per day and can also be started manually from GitHub Actions.

It performs these steps:

1. Fetch `jo-inc/camofox-browser` `master`.
2. Merge upstream `master` into this fork's `master`.
3. Fetch upstream `v*` release tags.
4. Find the latest upstream release tag.
5. Check GHCR for the expected image tags.
6. If any expected tag is missing, check out the upstream release tag and build with upstream's `Makefile` and `Dockerfile`.

## Published Images

Images are published to:

```text
ghcr.io/camohiddendj/camofox-browser
```

For release `vX.Y.Z`, the workflow publishes:

```text
latest
vX.Y.Z
X.Y.Z
amd64
arm64
vX.Y.Z-amd64
vX.Y.Z-arm64
X.Y.Z-amd64
X.Y.Z-arm64
```

The `latest`, `vX.Y.Z`, and `X.Y.Z` tags are multi-arch manifests for `linux/amd64` and `linux/arm64`. The `amd64` and `arm64` tags point to the latest release for that architecture.

## Required Repository Settings

Set this branch, `automation`, as the default branch so GitHub schedules the workflow from here.

In repository settings:

```text
Settings -> Actions -> General -> Workflow permissions -> Read and write permissions
```

For existing GHCR packages, grant this repository write access:

```text
Package settings -> Manage Actions access -> camohiddendj/camofox-browser -> Write
```

## Operating Notes

This branch should stay small. Do not merge upstream source into it. The upstream source belongs on `master`; this branch only owns automation files.
