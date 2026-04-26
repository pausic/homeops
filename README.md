<div align="center">

# homeops

### GitOps-managed Kubernetes homelab

[![k3s](https://img.shields.io/badge/k3s-v1.34.3-blue?logo=kubernetes&logoColor=white)](https://k3s.io/)
[![Flux](https://img.shields.io/badge/flux-v2.7.5-blue?logo=flux&logoColor=white)](https://fluxcd.io/)
[![GitHub last commit](https://img.shields.io/github/last-commit/pausic/homeops?logo=github&color=purple)](https://github.com/pausic/homeops/commits/master)

</div>

---

## :cloud: Overview

This repository defines the state of my single-node [k3s](https://k3s.io/) cluster using [Flux v2](https://fluxcd.io/) and GitOps principles.

## :bricks: Infrastructure

| Component | Details |
|-----------|---------|
| **Node** | Single-node k3s cluster |
| **OS** | Ubuntu |
| **Ingress** | [Traefik](https://traefik.io/) |
| **Storage** | [Longhorn](https://longhorn.io/) |
| **Certificates** | [cert-manager](https://cert-manager.io/) (self-signed CA) |
| **Databases** | [CloudNativePG](https://cloudnative-pg.io/) |
| **Secrets** | [SOPS](https://github.com/getsops/sops) |

## :hourglass_flowing_sand: GitOps Flow

Flux Kustomizations are chained with `wait: true` to enforce deployment order:

```
flux-system (bootstrap)
  └─ infra-sources (HelmRepositories, OCIRepositories)
       └─ infra-controllers (Traefik, Longhorn, CNPG, cert-manager, ...)
            └─ infra-configs (StorageClass, PG Clusters, Middleware, Certificates)
                 └─ apps (Monitoring, Media, Networking, Productivity)
```

## :telescope: Namespaces

- [`flux-system`](./clusters/homelab/flux-system/) 
- [`kube-system`](./infrastructure/controllers/) 
- [`longhorn-system`](./infrastructure/controllers/) 
- [`monitoring`](./apps/monitoring/) 
- [`storage`](./infrastructure/configs/cnpg/) 
- [`media`](./apps/media/) 
- [`networking`](./apps/networking/) 
- [`productivity`](./apps/productivity/) 

## :file_folder: Repository Structure

```
homeops/
├── clusters/homelab/          # Flux bootstrap entry point
├── infrastructure/
│   ├── sources/               # HelmRepository / OCIRepository definitions
│   ├── controllers/           # Infrastructure HelmReleases
│   └── configs/               # Post-controller config (certs, PG clusters, middleware)
└── apps/
    ├── monitoring/            # Observability stack
    ├── media/                 # Media management
    ├── networking/            # Network services
    └── productivity/          # Productivity tools
```
