
# UI-Project Kubernetes Manifests with Kustomize

This repository contains Kubernetes manifests for the **UI-Project**, organized using [Kustomize](https://kustomize.io/) for easy customization and environment overlays.

---

## Directory Structure

```

UI-Project/
├── kustomize/
│   ├── base/                  # Base manifests (deployments, services, etc.)
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   └── kustomization.yaml
│   └── overlays/
│       └── production/        # Production overlay to customize base manifests
│           ├── deployment-patch.yaml
│           ├── service-patch.yaml
│           └── kustomization.yaml

````

---

## How to Apply the Manifests

### Apply Base Resources

The base directory contains core manifests for your application.

```bash
kubectl apply -k UI-Project/kustomize/base
````

---

### Apply Production Overlay

* The production overlay customizes the base manifests, for example by adding a name prefix and patching specific resources.


* When creating patch files in overlays, include only the fields you want to change or overwrite in the base manifests. Do NOT copy the entire deployment or service file — patches should be minimal partial manifests that specify just the updates.

```bash
kubectl apply -k UI-Project/kustomize/overlays/production
```

This command will:

* Apply the base manifests from `../../base`
* Add the prefix `prod-` to resource names
* Apply patches defined in `deployment-patch.yaml` and `service-patch.yaml`

---

## Patches

Patch files contain partial resource manifests used to modify or override specific fields in base resources.

Example patch file (`deployment-patch.yaml`):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: htmlsampleproject-deployment  # Must match the resource in base
spec:
  replicas: 3                         # Example patch to update replicas
```

Patches are referenced in the overlay `kustomization.yaml` with explicit target information:

```yaml
patches:
  - path: deployment-patch.yaml
    target:
      kind: Deployment
      name: htmlsampleproject-deployment
  - path: service-patch.yaml
    target:
      kind: Service
      name: htmlsampleproject-service
```

---

## Notes

* Always run `kubectl apply -k <directory>` from the overlay directory to apply all base resources plus overlays and patches.
* The `namePrefix` in overlays helps distinguish environments (e.g., `prod-` for production).
* Patches should only contain the changes you want to apply, not full manifests.
* You can safely re-run `kubectl apply -k` after making changes to update your cluster incrementally.

---

## Troubleshooting

* If you see errors related to deprecated fields like `bases:` or `patchesStrategicMerge:`, update your `kustomization.yaml` to use `resources:` and `patches:` with explicit targets.
* Ensure patch file `metadata.name` matches the resource name in the base manifests.
* If applying a LoadBalancer service on a self-hosted cluster (e.g., on EC2), ensure you have a cloud provider or MetalLB set up to handle external IP assignment.

---

## References

* [Kustomize Official Documentation](https://kustomize.io/)
* [Kubectl Apply -k Docs](https://kubernetes.io/docs/reference/kubectl/kustomize/)

---
