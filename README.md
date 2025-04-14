# AI Vault Helm Chart.
## Summary
This chart is used to install the AI vault and AI Vault Entity Extraction Deployments.

For installation advice please see the public documentation [here](https://github.com/Bubblr-Inc/ai-vault-aws-installation/blob/main/README.md)

```
## Development
### Testing Rendered Templates

To view and test the output of your templates without installing it to your Kubernetes cluster run the following instructions:

1. If you haven't already clone this repo to your workstation or laptop.
   ```
   git clone https://github.com/Bubblr-Inc/ai-vault-helm.git
   ```
2. Change directory into the the helm repo chart directory
   ```
   cd ai-vault-helm
   ```
3. Run the helm debug command
```
helm template ai-vault-helm .
```


### Packaging and Pushing Helm Chart
1. Create helm package package
-  From the directory above the repo create 
```
helm package ai-vault-helm
```
Make a note of the outputted package something like ai-vault-helm-0.1.0.tgz
2. Authenticate to AWS ECR

   ```
aws ecr get-login-password --region eu-west-2 |  helm registry login --username AWS --password-stdin <accountID>.dkr.ecr.eu-west-2.amazonaws.com
   ```

3. Push to AWS ECR HELM
Replace the version with the package created in the Create helm package step

```
helm push ai-vault-helm-<VERSION>.tgz oci://<accountID>.dkr.ecr.eu-west-2.amazonaws.com/
```

4. Verfiy the helm chart has been pushed correctly
```
aws ecr describe-images \
     --repository-name ai-vault-helm \
     --region eu-west-2
```
