# Google Cloud - GitHub Action Example

An example workflow, using [the GitHub Action for gcloud](https://github.com/actions/gcloud) to deploy [a static website](site/) to an existing Google Kubernetes Engine Cluster.

## Workflow

The [example workflow](.github/main.workflow) will trigger on every push to this repo.

For pushes to the _feature_ branch, the workflow will:
1. Build the Docker image
1. Verify the Google Cloud Platform credentials are correct

For pushes to the _default_ branch (`master`), in addition to the above Actions, the workflow will:
1. Build the Docker image
1. Verify the Google Cloud Platform credentials are correct
1. Tag and Push the image to Google Container Registry
    * The image is available through the following tags: `latest`, the branch name, and first 8 of the commit SHA
    * `gcloud` serves as a [credential helper](https://cloud.google.com/container-registry/docs/pushing-and-pulling) for Docker. This workflow registers `gcloud` as a credential helper and uses the 'docker' command within the `gcloud` action to push the image.
1. Use a Kubernetes Deployment to push an image to the Cluster
    * Note that a GKE deployment requires a unique Tag to update the pods. Using a constant tag `latest` or a branch name `master` may result in successful workflows that don't update the cluster.

### Requirements

1. Google Cloud Platform project
1. GCP Service Account with write access to GCR and GKE for this project
1. GCP Service Account [credentials](https://cloud.google.com/iam/docs/creating-managing-service-account-keys) stored as a JSON key.
    1. The `GCLOUD_AUTH` secret, used by the [gcloud-auth](https://github.com/actions/gcloud/blob/master/auth/README.md) action, requires that this is base64 encoded
1. An existing Kubernetes Engine cluster
    1. [Create a Cluster](https://cloud.google.com/kubernetes-engine/docs/quickstart#create_cluster)

## License

This repository is [licensed under CC0-1.0](LICENSE), which waives all copyright restrictions.
