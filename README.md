# Introspect Data Helm Chart Repository

## Prerequisites

*   [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) installed
*   [helm](https://docs.helm.sh/using_helm/#installing-helm) installed (requires `kubectl`)
*   Your local kubeconfig set up to talk to your K8s cluster
*   Our private helm repo needs to be installed (hosted on in this repo!)
    *   We're following [this](https://hackernoon.com/using-a-private-github-repo-as-helm-chart-repo-https-access-95629b2af27c) process... mostly
    *   Add the helm chart to your local repos list:

```bash
helm repo add id-public  "https://raw.githubusercontent.com/introspectdata/helm-public/master/"
```
## Helpful Reading

*   [Using Helm](https://github.com/kubernetes/helm/blob/master/docs/using_helm.md)

## CI Workflow

This project has a bit of an 'odd' CI workflow through CircleCI:

*   Only commits to master trigger builds
*   Within that build, each individual chart needs to be specified in the build configuration
   *   If you're adding a new chart--make sure you update the build!
*   This specific build profile has it's SSH deploy key set to read/write - so once it lints, packages and re-indexes the helm repo, it pushes to github again
    *   **VERY IMPORTANT** - the comment of the commit from CircleCI has a [`[skip ci]`](https://circleci.com/docs/2.0/skip-build/) directive that makes it so that it doesn't loop infinitely.

## Useful Commands

### Rebuilding the Helm Repo Index

```bash
helm repo index . --merge index.yaml
```

### Install Helm!

```bash
wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz -O /tmp/helm.tar.gz
sudo tar xvf /tmp/helm.tar.gz -C /usr/local/bin/ --strip 1
rm -rf /tmp/helm.tar.gz
```
