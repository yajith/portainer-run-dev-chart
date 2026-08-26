# portainer-run-dev-chart

A **testing-only** Helm chart repository that automatically tracks the
[`develop`](https://github.com/portainer/portainer-run/tree/develop/chart)
branch of [portainer/portainer-run](https://github.com/portainer/portainer-run).

> ⚠️ These builds are unstable snapshots intended for testing. Do **not** use
> them in production.

## How it works

A GitHub Actions workflow ([`.github/workflows/publish-dev-chart.yml`](.github/workflows/publish-dev-chart.yml)):

1. Clones the `develop` branch of `portainer/portainer-run`.
2. Stamps the chart version as `0.0.0-dev.<short-sha>` so it always sorts
   below every real release.
3. Packages the chart and publishes it two ways:
   - **OCI (GHCR)** — `oci://ghcr.io/yajith/charts/portainer-run`
   - **Classic Helm repo** — `https://yajith.github.io/portainer-run-dev-chart`

The workflow runs on a daily schedule and can also be triggered manually from
the **Actions** tab.

## Usage

### OCI (recommended)

```sh
helm install portainer-run \
  oci://ghcr.io/yajith/charts/portainer-run \
  --version 0.0.0-dev.<sha>
```

List available versions:

```sh
helm show all oci://ghcr.io/yajith/charts/portainer-run
```

### Classic Helm repo

```sh
helm repo add portainer-run-dev https://yajith.github.io/portainer-run-dev-chart
helm repo update
helm install portainer-run portainer-run-dev/portainer-run \
  --devel   # required to install pre-release (0.0.0-dev.*) versions
```
