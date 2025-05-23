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

2. Update the Repo Index file (index.yaml)
   
```
helm repo index .
```

3. Push the repo to the GitHub
