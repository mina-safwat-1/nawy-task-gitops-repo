# 📦 Nawy Task GitOps Repository

This repository contains the **Helm chart** used to deploy the `nawy-task` application to Kubernetes (EKS).
It is designed for GitOps workflows, so any changes pushed here can be automatically applied to the cluster using **ArgoCD**.

---

## 📂 Repository Structure

```
│ ── nawy-chart/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── service.yaml
│           └── ingress.yaml
└── README.md
```

## 🚀 How It Works

1. **Deployment:**
   Creates a `Deployment` with the specified number of replicas running your container on port `3000`.
2. **Service:**
   Exposes the deployment inside the cluster on port `80`.
3. **Ingress:**
   Creates an AWS ALB (Application Load Balancer) and routes `www.itiproject.com` traffic to the service.

---

## ⚙️ Configuration

You can customize the deployment by editing `values.yaml`:

| Key                  | Description                    | Default                |
| -------------------- | ------------------------------ | ---------------------- |
| `replicaCount`       | Number of application replicas | `2`                    |
| `image.repository`   | Docker image repository        | `myregistry/nawy-task` |
| `image.tag`          | Docker image tag (version)     | `latest`               |
| `service.port`       | Cluster-facing service port    | `80`                   |
| `service.targetPort` | Container port                 | `3000`                 |
| `ingress.host`       | Domain name for ingress        | `www.itiproject.com`   |
| `ingress.path`       | Path to expose                 | `/`                    |

---

## 🛠️ Deployment

### Deployment

```bash
helm install nawy-task ./charts/nawy-task
```



---


## 👀 GitOps Flow

1. **Developer** builds and pushes a new Docker image.
2. **CI pipeline** updates `image.tag` in `values.yaml`.
3. **GitOps tool** ArgoCD detects the change and syncs manifests to the cluster.
4. **EKS** performs a rolling update, and the new version goes live.

