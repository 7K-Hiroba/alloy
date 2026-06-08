---
sidebar_position: 2
---

# Container image

This application uses the **upstream** Grafana Alloy container image (`grafana/alloy`). No custom Dockerfile is maintained in this repository.

## Upstream image

| Field | Value |
| ----- | ----- |
| Image | `grafana/alloy` |
| Registry | `docker.io` |
| Source | <https://github.com/grafana/alloy> |

The image tag is pinned in `helm/base/Chart.yaml` (`appVersion`) and pulled by the upstream Helm chart dependency.

## Image updates

[Renovate](https://docs.renovatebot.com/) watches the upstream image and opens PRs when new versions are released. When merging an image bump:

1. Update `helm/base/Chart.yaml` `appVersion` to match the new Alloy version
2. Update `helm/base/Chart.yaml` `dependencies[0].version` if the upstream Helm chart also changed
3. Run `helm dependency update helm/base`
4. Review upstream changelog for breaking changes

## Runtime expectations

The upstream Helm chart handles:

- Container runs as non-root (UID varies by upstream image)
- Metrics endpoint on port `12345` (path `/metrics`)
- Health endpoint on `/-/ready`
- Config mounted at `/etc/alloy/config.alloy`
