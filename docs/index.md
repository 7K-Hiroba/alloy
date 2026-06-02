# Alloy

Grafana Alloy telemetry collector

## Overview

This repository manages the Alloy deployment using the official upstream Helm chart.

## Structure

- `helm/platform/` - Platform resources (observability, secrets, etc.)
- `gitops/` - GitOps manifests for ArgoCD and FluxCD
- `docs/` - Documentation

## Deployment

The application is deployed using the official upstream Helm chart. Platform-specific resources are managed through the `helm/platform/` chart.
