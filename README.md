# Build Publish Docker

This composite action will do the below steps:

* Trivy container image
* Docker build
* Auth with Google project & install gcloud
* Auth with Google container registry
* Publish to registry


## Inputs

| Property    | Required |  Description | 
| ----------- | ----------- |-------------|
| file        | yes         | Dockerfile      |
| registry_host   | yes     | Image registry        |
| gke_project | yes | Name of GKE project |
| image_name | yes | Name of container image |
| sha | yes | Image tag |
| google_private_key | yes | Key to connect to Google cloud |
| registry_username | no | Registry username |
| registry_password | no | Registry password |
| docker_build_args | no | Extra Docker build arguments |
## Usage

```
on:
  push: [main]

jobs:
  run-opa-docker:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: build-login-publish-docker image
        uses: ebomart/build-publish-docker@main
        with:
          registry_host: $REGISTRY_HOSTNAME
          gke_project: $GCR_GKE_PROJECT
          image_name: $IMAGE
          google_private_key: ${{ secrets.GOOGLE_PRIVATE_KEY_DEV }}
          sha: ${{ github.sha }}
          file: Dockerfile
```

If you want include Docker build args, use as below:

```
on:
  push: [main]

jobs:
  run-opa-docker:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: build-login-publish-docker image
        uses: ebomart/build-publish-docker@main
        with:
          registry_host: $REGISTRY_HOSTNAME
          gke_project: $GCR_GKE_PROJECT
          image_name: $IMAGE
          google_private_key: ${{ secrets.GOOGLE_PRIVATE_KEY_DEV }}
          sha: ${{ github.sha }}
          file: Dockerfile
          docker_build_args: $DOCKER_BUILD_ARGS
```
