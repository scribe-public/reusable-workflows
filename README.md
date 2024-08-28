# reusable-workflows
Reusable workflows to support easy integration of Scribe tools in Github workflows

## SSDF
### Background
The SSDF is a simple reusable workflow that generates evidence and performs policy evaluation of a subset of the SSDF requirements - namely the SSDF PS category, using the Scribe tools.

The workflow is designed for compliance evaluation for a Docker image built by a Gihub workflow. 

### Usage

1. At the end of your Docker build workflow, add the following step:

```yaml
on:
  workflow_dispatch:

jobs:

  # Typically here will come the jobs that build a docker image and push it to DockerHub

  evaluate_ssdf:  
    name: Evaluate SSDF
    uses: scribe-public/reusable-workflows/.github/workflows/ssdf.yaml@main
    with:
        scribe_product_name: "Scribot"
        scribe_product_version: "v0.0.1-discovery-demo"
        target: "scribesecurity/heyman:latest"
    secrets: 
        SCRIBE_TOKEN: ${{ secrets.SCRIBE_TOKEN }}
        GH_TOKEN: ${{ secrets.GH_TOKEN }}
        DOCKERHUB_USERNAME: ${{ secrets.DOCKERHUB_USERNAME }}
        DOCKERHUB_PASSWORD: ${{ secrets.DOCKERHUB_PASSWORD }}

```
Note: 
* The `scribe_product_name` and `scribe_product_version` are a user defined name and version of the product on ScribeHub. You do not need to create the product on ScribeHub, the workflow will create it for you.

2. Add the following secrets to your repository:
* SCRIBE_TOKEN ([Generate it on Scribehub](https://app.scribesecurity.com/settings/tokens))
* GH_TOKEN (A Github token with read access to organization and repository data)
* DOCKERHUB_USERNAME
* DOCKERHUB_PASSWORD (The content can be the password or a token)

3. Run the workflow.

### Expected Results
* On the [ScribeHub products page](https://app.scribesecurity.com/producer-products) you will see a new/updated product with the name and version you provided. Scribe will automatically perform a vulnerability analysis of the new Docker image.
* On the [ScribeHub policy page](https://app.scribesecurity.com/policy/evaluation) you will see a new policy evaluation for the product you created. Use the filters to choose your product and view the results.


## Github Discovery and Evaluation Demo
### Background
The Github Discovery and Evaluation Demo is a simple reusable workflow that generates evidence and performs policy evaluation of some policies using the Scribe tools.

### Usage
A reference usage is given [here](.github/workflows/github-demo-usage.yaml)

1. Add the use of the workflow in a build workflow file, or in a new workflow file that will be triggered by the end of your build workflow.

2. The reference workflow:

```yaml
name: Usage of Github Demo

on:
  workflow_dispatch:

jobs:

# Typically here will come the jobs that build a docker image and push it to DockerHub

  discover_evaluate_github:  
    name: Discover and Evaluate Github
    uses: scribe-public/reusable-workflows/.github/workflows/github-demo.yaml@main
    with:
        scribe_product_name: "Test-Product"
        scribe_product_version: "Some-Version"
    secrets: 
        SCRIBE_TOKEN: ${{ secrets.SCRIBE_TOKEN }}
        GH_TOKEN: ${{ secrets.GH_TOKEN }}
```

3. Add the following secrets to your repository:
* SCRIBE_TOKEN
* GH_TOKEN (A Github token with read access to organization and repository data)

4. Define the product name and version. The product name and version are used to group the evidence and policy evaluation results on ScribeHub.

5. Run the workflow.

## K8S Discovery and Evaluationa Demo
### Background
The K8S Discovery and Evaluation Demo is a simple reusable workflow that generates evidence and performs policy evaluation of some policies using the Scribe tools.

The workflow fires a minikube cluster, deploys the images given, and then performs the discovery and evaluation.

### Usage
A reference usage is given [here](.github/workflows/k8s-demo-usage.yaml)

1. Add the use of the workflow in a build workflow file, or in a new workflow file that will be triggered by the end of your build workflow.

2. The reference workflow:

```yaml
name: Usage of K8s Demo

on:
  push:
  workflow_dispatch:

jobs:

# Typically here will come the jobs that build a docker image and push it to DockerHub

  discover_evaluate_k8s:  
    name: Discover and Evaluate K8s
    uses: scribe-public/reusable-workflows/.github/workflows/k8s-demo.yaml@main
    with:
        scribe_product_name: "Test-Product"
        scribe_product_version: "Some-Version"
        targets: "nginx:latest"
    secrets: 
        SCRIBE_TOKEN: ${{ secrets.SCRIBE_TOKEN }}
```

3. Add the SCRIBE_TOKEN secret to your repository.

4. Define the product name and version. The product name and version are used to group the evidence and policy evaluation results on ScribeHub.

5. Define the targets. The targets are the images that will be deployed in the minikube cluster. 
The targets field is a comma separated list of images. The images must be available in a public registry.