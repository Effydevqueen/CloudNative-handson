# Deploying student-tracker with Helm

This guide explains how to deploy the `student-tracker` application using Helm in your Kubernetes cluster.

## Prerequisites

- Kubernetes cluster (e.g., kind, minikube, EKS, etc.)
- [Helm](https://helm.sh/docs/intro/install/) installed
- `kubectl` installed and configured to access your cluster

## 1. Clone or Prepare the Chart

Ensure you are in the root of the repository and the `student-tracker` Helm chart is in `5.0-helm-chart/student-tracker`.

## 2. Create a Namespace (if not already present)
```sh
kubectl create namespace my-app
```

## 3. Review and Edit Values

Edit `my-value.yml` in the `student-tracker` chart directory to set your desired configuration (image, replicas, ingress, etc.).

## 4. Install the Chart

From the `5.0-helm-chart` directory, run:
```sh
helm install student-tracker ./student-tracker -n my-app -f ./student-tracker/my-value.yml
```

- This will deploy the app with the values from `my-value.yml` into the `my-app` namespace.

## 5. Check Deployment Status
```sh
kubectl get pods -n my-app
kubectl get deployments -n my-app
```

## 6. Upgrade the Release (if you make changes)
```sh
helm upgrade student-tracker ./student-tracker -n my-app -f ./student-tracker/my-value.yml
```

## 7. Uninstall the Release
```sh
helm uninstall student-tracker -n my-app
```

## 8. Troubleshooting
- If you see errors about missing templates, ensure `templates/_helpers.tpl` exists and defines all required helpers (e.g., `student-tracker.fullname`).
- If you see `cannot re-use a name that is still in use`, uninstall the release first.
- If pods are not created or are stuck, check events:
  ```sh
  kubectl get events -n my-app --sort-by=.metadata.creationTimestamp
  ```

## 9. Clean Up Old Resources
If you previously deployed resources with `kubectl`, delete them to avoid conflicts:
```sh
kubectl delete deployment my-app -n my-app
```

---

**Note:**
- The chart uses Helm best practices for naming and templating.
- ServiceAccount, labels, and other resources are managed via Helm templates.
- Edit `values.yaml` or `my-value.yml` for custom configuration.

For more details, see the chart's `README.md` or Helm documentation.
