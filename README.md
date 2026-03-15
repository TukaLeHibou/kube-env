# kube-env

Guide pédagogique interactif pour déployer un cluster **Kubernetes v1.31 Haute Disponibilité** en production.

## Aperçu

Page web statique (`index.html`) qui documente et visualise une architecture Kubernetes HA complète :

- **7 nœuds** : 3 Control Planes + 4 Workers
- **HAProxy + Keepalived** — VIP `192.168.1.100:6443` avec failover automatique
- **etcd** — Consensus Raft distribué sur 3 nœuds (tolérance 1 panne)
- **Longhorn CSI** — Stockage persistant répliqué entre les workers
- **ArgoCD** — Déploiement GitOps continu

## Contenu de la page

| Section | Description |
|---------|-------------|
| **Topologie** | Visualisation SVG animée des flux de communication inter-composants |
| **Composants** | Détail de chaque composant (API Server, Scheduler, etcd, Kubelet…) |
| **Installation** | Guide étape par étape avec blocs de commandes copiables |
| **Checklist** | Liste de vérification interactive avec suivi de progression |

## Lancement

Aucun build requis. Ouvrir directement dans un navigateur :

```bash
open index.html
# ou
python3 -m http.server 8080
```

## Architecture

```
Internet / kubectl
       │
  ⚖️  HAProxy VIP (192.168.1.100:6443)
       │          Keepalived VRRP
  ┌────┼────────────────┐
  │    │                │
 CP1  CP2              CP3     ← 3 Control Planes
  │    │                │
 etcd-1 ←── Raft ──→ etcd-2/3
  │
  ├── worker-1
  ├── worker-2       ← 4 Workers
  ├── worker-3          Longhorn réplication
  └── worker-4
```
