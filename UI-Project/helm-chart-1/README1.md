
# helm-chart-1

A Helm chart for deploying **htmlsampleproject** with environment-specific configurations.

---

## Directory Structure

```

helm-chart-1/
├── Chart.yaml
├── values.yaml                  # Default values (common to all environments)
├── values/                      # Folder containing environment-specific values files
│   ├── dev.yaml
│   ├── prod.yaml
│   └── staging.yaml
├── templates/
│   ├── deployment.yaml
│   └── service.yaml

````

---

## Overview

This Helm chart deploys the `htmlsampleproject` application with the ability to customize configurations for different environments such as development, staging, and production.

- `values.yaml`: Contains default configurations common to all environments.
- `values/dev.yaml`: Overrides and additions for the development environment.
- `values/staging.yaml`: Overrides and additions for the staging environment.
- `values/prod.yaml`: Overrides and additions for the production environment.
- `templates/`: Contains Kubernetes manifests templated for Helm.

---

## Usage

### Installing the Chart

You can install the chart with default values:

```bash
helm install my-release ./helm-chart-1-chart -f values.yaml
````

### Installing with Environment-Specific Values

To install the chart with environment-specific overrides, specify both the default and environment file(s):

```bash
# For development
helm install my-release ./helm-chart-1 -f values.yaml -f values/dev.yaml

# For staging
helm install my-release ./helm-chart-1 -f values.yaml -f values/staging.yaml

# For production
helm install my-release ./helm-chart-1 -f values.yaml -f values/prod.yaml
```

---

## Updating the Chart

To upgrade the release with updated configuration:

```bash
helm upgrade my-release ./htmlsampleproject-chart -f values.yaml -f values/prod.yaml
```

---

## Customization

Modify the values files to customize the deployment according to your environment needs, such as:

* Number of replicas
* Image repository and tags
* Resource limits and requests
* Service type and ports
* Environment variables for your app

---

## Contributing

Feel free to open issues or submit pull requests to improve the chart.

---

