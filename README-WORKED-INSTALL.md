# Requirements

An external load balancer with a valid TLS/SSL Certificate
A DNS name pointing to the external load balancer
A PostGres DataBase.
A Kubernetes Cluster

# Installation Worked Example with Terraform

The following example will start with an empty AWS account and will build AI Vault installation.

1. Create a VPC and three sets of subnets public, private and database.
2. Create a Route53 Zone
3. Create an Aurora PostGres Cluster
