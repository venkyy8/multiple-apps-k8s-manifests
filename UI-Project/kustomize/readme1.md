
# HTML Sample Project - Kustomize Deployment

This repository demonstrates a simple Kubernetes deployment configuration using [Kustomize](https://kustomize.io/). It separates the base configuration from the production overlay, allowing for clean and maintainable environment-specific customizations.

## 📁 Project Structure

```

htmlsampleproject-kustomize/
├── base/
│   ├── deployment.yaml        # Base Deployment definition
│   ├── service.yaml           # Base Service definition
│   └── kustomization.yaml     # Kustomize config for the base
└── overlays/
    └── production/
        ├── kustomization.yaml     # Kustomize config for production overlay
        ├── deployment-patch.yaml  # Patch for Deployment in production
        └── service-patch.yaml     # Patch for Service in production
    └── Staging/
        ├── inprogress.yaml     

````

---

## 🚀 Getting Started

### Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/)

> ⚠️ Kustomize is also built into `kubectl` (v1.14+), so you can run `kubectl apply -k` without installing Kustomize separately.

---

## 📦 Base Configuration

The `base/` directory contains common Kubernetes resources used across environments. These should be environment-agnostic.

- `deployment.yaml`: Defines a Deployment for the HTML application.
- `service.yaml`: Exposes the Deployment as a Service.
- `kustomization.yaml`: Lists all base resources.

---

## 🌐 Production Overlay

The `overlays/production/` directory customizes the base for the production environment using strategic merge patches.

- `deployment-patch.yaml`: Overrides Deployment fields (e.g., replicas, image tag).
- `service-patch.yaml`: Modifies Service settings for production.
- `kustomization.yaml`: References the base and applies patches.

---

## 🔧 Applying the Configuration

To deploy the Normal configuration:

```sh
kubectl apply -k demo/
````



To deploy the **production** configuration:

```sh
kubectl apply -k overlays/production/
````

To preview the full manifest that will be applied:

```sh
kustomize build overlays/production/
```

Or using `kubectl` (v1.14+):

```sh
kubectl kustomize overlays/production/
```

---

## ✅ Example Use Cases

* Adjusting replica count or container image per environment.
* Overriding service types (e.g., `ClusterIP` in dev, `LoadBalancer` in prod).
* Maintaining reusable Kubernetes manifests with minimal duplication.

---

## 📚 Learn More

* [Kustomize Documentation](https://kubectl.docs.kubernetes.io/pages/app_customization/introduction.html)
* [Strategic Merge Patch](https://kubectl.docs.kubernetes.io/pages/app_customization/patches.html)

---

