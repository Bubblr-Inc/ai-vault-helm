# AI Vault Helm Chart.
## Summary
This chart is used to install the AI vault and AI Vault Entity Extraction Deployments.

## Installation
TODO

## Adding a Load Balancer Ingress
TODO

## Testing Rendered Templates

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
helm template ai-vault .
```
