# EDR Workloads

Welcome to the EDR SRE Gitops Repository.

This repository contains GitOps configuration which follows the ArgoCD App of Apps pattern. 

## Pull Requests

Changes to the master branch require a pull request and review by codeowners. Whenever possible, changes contained in a pull request should affect only a single service in a single environment. Pull request titles must adhere to formatting requirements. 

### Titles
Pull request titles must match formatting requirements, or they will fail checks and cannot be merged. Presently there are 2 allowed formats. Which to use depends on the scope of changes.

1. Most pull requests will include changes that affect only a single service in a single enironment. In this case, the argo diff will be generated and added to the pull request as a comment. These PRs should use the following title format:

    ```[<env>] <svc>: <description>```,
    e.g. ```"[inteks] localintelligence: Update 1.0.xx to 1.0.yy"```

2. For anything else, the general keyword should be used:

    ```general: <description>```, 
    e.g. ```"general: Update readme"```

### Adding a New Application
When adding a new application, the diff check will fail because the application doesn't exist. This is expected. The PR can still be approved and merged. It is the responsibility of the submitter/reviewer to verify that this is indeed a new application, and not a typo. 

In order to use the update_chart workflow for the new application, the app name will need to be added to the service option list (on.workflow_dispatch.inputs.service.options) in the [update_chart.yaml](.github/workflows/update_chart.yaml) workflow.


## Usage

Each Environment has a folder named after the environment. The folder contains an ArgoCD application.

#### Configure an Application

Once ArgoCD is installed, create a new `application.yaml` file with the following configuration.

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: edr-workloads
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/trellix-edr/gitops-eks.git
    targetRevision: HEAD
    path: ENV_NAME/chart
    helm:
      release: edr-workloads
      values: |
        apiManagementDBless:
          enable: true
  destination:
    server: "https://kubernetes.default.svc"
    namespace: argocd
```

#### Deploy the edr-workloads

Apply the yaml to bootstrap the cluster.

```
kubectl apply -n argocd -f application.yaml
```

## Repo Structure

The structure of this repository follows the ArgoCD [App of Apps](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/#app-of-apps) pattern. The configuration in this repository is organized into two directories: `chart` and `edr-workloads`.

```
├── chart
└── edr-workloads
```

### Chart

The `chart` directory contains a Helm chart which represents the root Application for the ArgoCD App of Apps pattern. This Helm chart in turn deploys additional ArgoCD Application resources which represent additional add-on Helm charts.

```
chart
├── templates
│   └── api-management-dbless.yaml
│   └── ...
├── Chart.yaml
├── values.yaml
```

### edr-workloads

The `edr-workloads` directory contains a dedicated Helm chart that deploys each individual add-on.

```
edr-workloads
├── api-management-dbless.yaml
│   └── Chart.yaml
│   └── values.yaml
│   └── ...
```

