# GitHub Actions Runner

Custom GitHub Actions runner images for Plyo.

## Docker Images

Available images on Docker Hub: https://hub.docker.com/r/plyo/github-actions-runner/tags

## Build Pipeline

The Docker build workflow runs two parallel jobs:

- `build-arc-runners` — Builds and pushes the main ARC runners image (Dockerfile.new)
- `build-arc-runners-tests` — Builds and pushes the tests image (Dockerfile.tests.new)

Both jobs run in parallel to reduce build time.

## Kubernetes Deployment (plyo-infra repo)

The runners are deployed to Kubernetes clusters using Terraform. The deployment includes:

### Namespaces

- `arc-systems` — Hosts the Actions Runner Controller
- `arc-runners` — Hosts the runner scale sets and secrets

### Secrets

- `github-app` — GitHub App credentials (ID, installation ID, and private key)

### Runner Scale Sets

Four different runner scale sets are deployed:

- `arc-runners` — Standard runners (uses `arc-runners.yaml`)
- `arc-runners-dind` — Runners with Docker-in-Docker support (uses `arc-runners-dind.yaml`)
- `arc-runners-tests` — Test runners (uses `arc-runners-tests.yaml`)
- `arc-runners-tests-dind` — Test runners with Docker-in-Docker (uses `arc-runners-tests-dind.yaml`)

All runners use the `gha-runner-scale-set` Helm chart and are configured via value files in the `values/` directory.
