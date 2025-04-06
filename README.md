# AI Vault Helm Chart.
## Summary
This chart is used to install the AI vault and AI Vault Entity Extraction Deployments.

## Installation

1. Prepare a values file for your installation.  
Create a file names `customValues.yaml` containing the following values. Note, you will modify the environment variables to suit your environment.
_NOTE_ You will likely need an ALB Load Balancer to expoise your Vault Endpoint see the _Adding a Load Balancer via Ingress_ section for details

```
env:
  awsS3BucketHtml: ""
  awsS3BucketHtmlCDN: ""
  htmlPageAwsS3Bucket: ""
  htmlPageCDN: ""
  gpcBaseUrl: ""
  gptDataDbUser: ""
```


### Authenticate to AWS ECR
Authenticate your Helm client to the Amazon ECR registry holding the AI Vault Helm Chart. 

```
aws ecr get-login-password \
     --region eus-west-2 | helm registry login \
     --username AWS \
     --password-stdin 475755457693.dkr.ecr.eu-west-2.amazonaws.com
```

### Install the Helm Chart with your newly created values file.
Install the chart to your kubernetes cluster. 
```
helm install ai-vault-helm oci://475755457693.dkr.ecr.eu-west-2.amazonaws.com/ai-vault-helm --version 0.1.0 --values ./customValues.yaml
```

## Adding a Load Balancer via Ingress
The following example describes setting an ingress for an AWS ALB LoadBalancer.

1. Decide the hostname / url you wish to use to connect to your ai-vault instance. e.g ai-vault.mydomain.com
2. Create or  an ACM TLS Certificate https://docs.aws.amazon.com/res/latest/ug/acm-certificate.html. _Note_ if you are using an existing ACM the skip to the next step.

3. Make an a note of  the ACM certifcates' ARN

4. Make a note of your public subnets ids. 

5. Add an ingress section like the example below, replace the public subnets and certificate ARN with your own values estblished from the previous steps.  The line  `alb.ingress.kubernetes.io/subnets` needs a comma seperated list of subnets representing your environment and the line `alb.ingress.kubernetes.io/certificate-arn` needs the ARN of your ACM certificate. The ACM certificates should be in the same AWS region as your EKS cluster you are deploying the helm chart to.

```
ingress:
  enabled: true
  className: "alb"
  annotations: 
    kubernetes.io/ingress.class: alb
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/healthcheck-path: /v1/client/health
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS": 443}]'
    alb.ingress.kubernetes.io/subnets: subnet-1234567,subnet-89011123,subnet-141516171 #replace with your own public subnets
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:eu-west-1:123456789:certificate/50d7e14a-2345-4241-4567-d8d208f22b67 #replace with your own ACM Certificate ARN
    alb.ingress.kubernetes.io/group.name: ai-vault
    alb.ingress.kubernetes.io/load-balancer-name: ai-vault
  hosts:
    - host: ai-vault.prodsvc.com
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: ai-vault-svc
              port:
                number: 80
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