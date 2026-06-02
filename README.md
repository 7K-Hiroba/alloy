# Alloy

Grafana Alloy telemetry collector

## Structure

```text
├── helm/
│   └── platform/       # Platform dependencies (databases, storage, secrets, observability)
├── compositions/
│   └── crossplane/     # App-specific Crossplane compositions (XRDs, Compositions)
├── gitops/
│   ├── argocd/         # ArgoCD Application manifests
│   └── fluxcd/         # FluxCD Kustomization manifests
├── docs/               # TechDocs content
├── .github/workflows/  # CI/CD (references 7K-Hiroba/workflows-library)
└── catalog-info.yaml   # Backstage catalog entry
```

## Upstream Chart

This application uses the official upstream Helm chart. The chart is deployed directly from its official repository rather than being vendored in this repository.

## Documentation

Full documentation is available at [hiroba.7kgroup.org/apps/alloy](https://hiroba.7kgroup.org/apps/alloy), or locally under `docs/`.

## Contributing

Please read the [Contributing Guide](https://github.com/7K-Hiroba/Hiroba/blob/main/CONTRIBUTING.md) before opening pull requests.

## Part of the Hiroba ecosystem

Scaffolded with [Hiroba](https://github.com/7K-Hiroba/Hiroba) by 7KGroup.
