# GitOps Resources

This directory contains all GitOps manifests for deploying `alloy` via ArgoCD or FluxCD.

Two separate resources are defined for each GitOps tool — one for the **base chart** (Grafana Alloy DaemonSet) and one for the **platform chart** (observability wiring). They are kept separate because they have different lifecycles: the base chart deploys frequently while platform resources change rarely and require manual review before syncing.

## Structure

```text
gitops/
├── argocd/
│   ├── project.yaml              # AppProject scoping source repos and destination namespace
│   ├── root.yaml                 # Root Application that syncs the apps/ directory
│   └── applications/
│       ├── apps/
│       │   ├── namespace.yaml
│       │   ├── configmap.yaml
│       │   ├── project.yaml
│       │   └── platform.yaml     # ArgoCD Application for platform chart
│       └── overlays/
│           ├── staging/
│           │   ├── base-values.yaml
│           │   └── platform-values.yaml
│           └── production/
│               ├── base-values.yaml
│               └── platform-values.yaml
│   └── applicationset.yaml       # ApplicationSet for multi-environment deployment
└── fluxcd/
    ├── git-repository.yaml       # GitRepository source (apply once per cluster)
    ├── environments/
    │   ├── staging.yaml          # Kustomization for staging
    │   └── production.yaml       # Kustomization for production
    ├── apps/
    │   ├── namespace.yaml
    │   ├── configmap.yaml
    │   ├── helmrelease-base.yaml     # HelmRelease for helm/base
    │   ├── helmrelease-platform.yaml # HelmRelease for helm/platform
    │   └── kustomization.yaml
    └── overlays/
        ├── staging/
        │   ├── base-patch.yaml
        │   ├── platform-patch.yaml
        │   └── kustomization.yaml
        └── production/
            ├── base-patch.yaml
            ├── platform-patch.yaml
            └── kustomization.yaml
```

## ArgoCD — standalone deployment

```bash
# Create the AppProject and root Application
kubectl apply -f gitops/argocd/project.yaml
kubectl apply -f gitops/argocd/root.yaml

# Sync the platform chart manually after reviewing the diff
argocd app sync alloy-platform
```

The base Application syncs automatically (prune + selfHeal enabled). The platform Application has no automated sync — trigger it manually via the ArgoCD UI or CLI after reviewing the planned changes.

## FluxCD — standalone deployment

```bash
# Register the Git source (once per cluster)
kubectl apply -f gitops/fluxcd/git-repository.yaml

# Deploy base chart (reconciles automatically every 5m)
kubectl apply -f gitops/fluxcd/apps/helmrelease-base.yaml

# Deploy platform chart (review first, then apply)
kubectl apply -f gitops/fluxcd/apps/helmrelease-platform.yaml

# Manually trigger a platform sync when ready
flux reconcile helmrelease alloy-platform -n flux-system
```

To pause platform reconciliation while investigating an issue, set `suspend: true` in `helmrelease-platform.yaml` and re-apply.
