# Installation Worked Example with Terraform

The following example will start with an empty AWS account and will build AI Vault installation.

1. Create a VPC and three sets of subnets public, private and database.
2. Create a Route53 Zone.
3. Create an RDS Aurora PostGres Cluster.
4. Create an ACM certificate.
5. Create an EKS Cluster.
6. Install the Ai-Vault Helm chart.


## Install and authenticate the AWS cli 
There are at least two methods, via IAM keys and via SSO. Please follow this guide to authenticate to your AWS account.

https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

## Clone this repo and 
