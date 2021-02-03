Replicated KOTS Multi Chart Example
==================

Example project to show how to manage an application that is comprised of multiple charts.


#### Sample Charts

This example uses three charts available from this repo: https://github.com/prometheus-community/helm-charts

The Charts are:

- [alertmanager](https://github.com/prometheus-community/helm-charts/tree/main/charts/alertmanager)
- [prometheus](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus)
- [prometheus-postgres-exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-postgres-exporter)


#### Folder Structure

In this KOTS example, the directory for each of the Helm Charts is at the root of this repostiroy. There is also a `kots` directory which contains the replicated YAML files.

#### CI/CD Process

The CI/CD process would consist of two steps:

1. For each Chart, package it and put the `tar.gz` output in the `kots/manifests` directory.

2. Run the Replicated CLI to create a release that includes the `tar.gz` for each chart and promote to channel.

Assuming that that the current directory is the root directory, a CI/CD process would look similar to this:

``` shell
  helm dependencies update alertmanager
  helm package alertmanager -d kots/manifests/

  helm dependencies update prometheus
  helm package prometheus -d kots/manifests/

  helm dependencies update prometheus-postgres-exporter
  helm package prometheus-postgres-exporter -d kots/manifests/

  replicated release create --yaml-dir=kots/manifests/ --auto -y
```


### Integrating with CI - NOT UPDATED FOR THIS EXAMPLE YET

This repo contains a default[GitHub Actions](https://help.github.com/en/github/automating-your-workflow-with-github-actions/about-github-actions) workflow for ci at [./.github/workflows/main.yml](./.github/workflows/main.yml). You'll need to [configure secrets](https://help.github.com/en/github/automating-your-workflow-with-github-actions/virtual-environments-for-github-actions#creating-and-using-secrets-encrypted-variables) for `REPLICATED_APP` and `REPLICATED_API_TOKEN`. On every push this will:

- Ensure a channel exists for the branch that was pushed to
- Create a release based on the contents of `./manifests`

## Advanced Usage

### Integrating kurl installer yaml

There is a file `kurl-installer.yaml` that can be used to manage [kurl.sh](https://kurl.sh) installer versions for an embedded Kubernetes cluster. This will be automatically released in CI. You can create a release manually with

```
replicated installer create --auto
```

### Tools reference

- [replicated vendor cli](https://github.com/replicatedhq/replicated)

### License

MIT
