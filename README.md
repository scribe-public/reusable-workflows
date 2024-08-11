# reusable-workflows
Reusable workflows to support easy integration of Scribe tools in Github workflows

## SSDF
### Background
The SSDF is a simple reusable workflow that generates evidence and performs policy evaluation of a subset of the SSDF requirements - namely the SSDF PS category, using the Scribe tools.

The workflow is designed for compliance evaluation for a Docker image built by a Gihub workflow. 

### Usage

1. At the end of your Docker build workflow, add the following step:

```yaml
    evaluate_ssdf:  
        name: Evaluate SSDF
        uses: scribe-public/reusable-workflows/.github/workflows/ssdf.yaml@main
        with:
            scribe_product_name: < Product name on ScribeHub>
            scribe_product_version: <Product version on ScribeHub>
            sbom_of_image: < name of the built image, for example scribesecurity/my-mage:latest>
        secrets: inherit
```
Note: 
* The `scribe_product_name` and `scribe_product_version` are a user defined name and version of the product on ScribeHub. You do not need to create the product on ScribeHub, the workflow will create it for you.

2. Add the following secrets to your repository:
* SCRIBE_TOKEN ((Generate it on Scribehub)[https://app.scribesecurity.com/settings/tokens])
* GH_TOKEN (A Github token with read access to organization and repository data)
* DOCKERHUB_USERNAME
* DOCKERHUB_PASSWORD (The content can be the password or a token)

3. Run the workflow.

### Expected Results
* On the (ScribeHub products page)[https://app.scribesecurity.com/producer-products] you will see a new/updated product with the name and version you provided. Scribe will automatically perform a vulnerability analysis of the new Docker image.
* On the (ScribeHub policy page)[https://app.scribesecurity.com/policy/evaluation] you will see a new policy evaluation for the product you created. Use the filters to choose your product and view the results.



