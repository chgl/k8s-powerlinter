# kube-powertools

![Last Version](https://img.shields.io/github/v/release/chgl/kube-powertools)
![License](https://img.shields.io/github/license/chgl/kube-powertools)
![Docker Pull](https://img.shields.io/docker/pulls/chgl/kube-powertools)
[![CI](https://github.com/chgl/kube-powertools/actions/workflows/ci.yaml/badge.svg)](https://github.com/chgl/kube-powertools/actions/workflows/ci.yaml)

An always up to date collection of useful tools for your Kubernetes linting and auditing needs.

## Usage

Mount a folder containing your Helm or raw Kubernetes manifests:

```sh
docker run --rm -it -v $PWD:/usr/src/app ghcr.io/chgl/kube-powertools:latest
```

The container image is pushed to three registries:

- `docker.io/chgl/kube-powertools:latest`
- `ghcr.io/chgl/kube-powertools:latest`
- `quay.io/chgl/kube-powertools:latest`

You can use the `vX.Y.Z` image tags corresponding to the [releases](https://github.com/chgl/kube-powertools/releases)
instead of using `latest`.

## Helm Chart Repositories

The kube-powertools image includes a few helpful scripts to simplify working with Helm chart repositories.

### Linting

The image includes a [chart-powerlint.sh](scripts/chart-powerlint.sh) script which can be used to apply several linters to Helm chart repos.

For example, you can mount this repository into the `kube-powertools` container and run the following to lint the sample chart
in the `/samples/charts` dir:

```sh
$ docker run --rm -it -v $PWD:/usr/src/app ghcr.io/chgl/kube-powertools:latest
bash-5.1# CHARTS_DIR=samples/charts chart-powerlint.sh
```

### Generating Chart Documentation

You can auto-generate and format Markdown docs from the chart's values.yaml using [generate-docs.sh](scripts/generate-docs.sh).
This scripts uses either `chart-doc-gen` if the chart dir contains a `doc.yaml`, or `helm-docs` if it doesn't.

### Generating CHANGELOG files

Finally, there's [generate-chart-changelog.sh](scripts/generate-chart-changelog.sh), which can be used to generate a CHANGELOG.md file from
the contantes of a Chart.yaml's [artifacthub.io/changes](https://artifacthub.io/docs/topics/annotations/helm/#supported-annotations) annotation.

You can use this file in conjunction with the [chart-releaser](https://github.com/helm/chart-releaser) tool's `--release-notes-file` option to produce release notes for a GitHub release. See <https://github.com/chgl/charts/blob/master/.github/workflows/release.yaml#L32> and <https://github.com/chgl/charts/blob/master/.github/ct/ct.yaml#L16> for a sample workflow.

## What's included

- [kubectl](https://github.com/kubernetes/kubectl)
- [helm](https://github.com/helm/helm)
- [helm push plugin](https://github.com/chartmuseum/helm-push.git)
- [helm-local-chart-version](https://github.com/mbenabda/helm-local-chart-version)
- [chart-doc-gen](https://github.com/kubepack/chart-doc-gen)
- [kubeval](https://github.com/instrumenta/kubeval)
- [kube-score](https://github.com/zegl/kube-score)
- [chart-testing](https://github.com/helm/chart-testing)
- [polaris](https://github.com/FairwindsOps/polaris)
- [pluto](https://github.com/FairwindsOps/pluto)
- [helm-docs](https://github.com/norwoodj/helm-docs)
- [kube-linter](https://github.com/stackrox/kube-linter)
- [kustomize](https://github.com/kubernetes-sigs/kustomize)
- [conftest](https://github.com/open-policy-agent/conftest)
- [nova](https://github.com/FairwindsOps/nova)
- [kubesec](https://github.com/controlplaneio/kubesec)
- [kubeconform](https://github.com/yannh/kubeconform)
- [kube-no-trouble](https://github.com/doitintl/kube-no-trouble)
- [trivy](https://github.com/aquasecurity/trivy)
- [yq](https://github.com/mikefarah/yq)
- [kubescape](https://github.com/armosec/kubescape)
- [gomplate](https://github.com/hairyhenderson/gomplate)
- [cosign](https://github.com/sigstore/cosign)
- [crane](https://github.com/google/go-containerregistry/tree/main/cmd/crane)
- [checkov](https://github.com/bridgecrewio/checkov)

## Testing locally

```sh
docker build -t kube-powertools:dev .
$ docker run --rm -it -v $PWD:/usr/src/app kube-powertools:dev
bash-5.1# CHARTS_DIR=samples/charts scripts/chart-powerlint.sh
```

## Verify image integrity

All released container images are signed using [cosign](https://github.com/sigstore/cosign) following the [keyless approach](https://github.com/sigstore/cosign/blob/main/KEYLESS.md). To verify the signature:

```sh
COSIGN_EXPERIMENTAL=1 cosign verify ghcr.io/chgl/kube-powertools:latest
```
