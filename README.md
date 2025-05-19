# AI Vault Helm Chart.
## Summary
This chart is used to install the AI vault Deployments

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
1. Create helm package package.
   Note, you will need to supply a version number for example 0.1.0, 0.1.1 or 1.0.0
-  From the directory above the repo create 
```
helm package  ai-vault-helm --version 0.2.0
```
Make a note of the outputted package something like ai-vault-helm-0.2.0.tgz

2. Authenticate to the AWS Account that contains the Helm ECR registry.

```
aws ecr get-login-password --region eu-west-2 | docker login --username AWS --password-stdin 475755457693.dkr.ecr.eu-west-2.amazonaws.com
aws ecr get-login-password --region eu-west-2 | helm registry login --username AWS --password-stdin 475755457693.dkr.ecr.eu-west-2.amazonaws.com
```

3. Push the helm package to the helm registry
   ```
   helm push ai-vault-helm-0.2.0.tgz oci://475755457693.dkr.ecr.eu-west-2.amazonaws.com/ai-vault-helm
   ```

5. Verfiy the helm chart has been pushed correctly
```
aws ecr describe-images --registry-id 709825985650 --repository-name ethical-web-ai/ai-vault --region eu-west-2
```
