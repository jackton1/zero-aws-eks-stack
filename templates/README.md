

# Overview
Your infrastructure should be up and running, your terraform repository is the source of truth for your infrastructure, here is [a list of components and resources][zero-resource-list] that comes with the EKS-stack

# How to 
## Managing your Terraform
#### Why terraform
The repository follows infrastructure as code as a core principle, it allows repeatable and reproducible infrastructures and makes DevOps much more manageable; to learn more about it we suggest reading the [Terraform's workflow guide][tf-workflow].

#### Intended workflows
To make changes to the infrastructure you would modify the terraform code changing the components you wish to modify, then plan the changes with `terraform plan` to make sure you are making the desired changes; then apply the changes with `terraform apply` on your staging environment. Once you reach the satisfactory state, you should do the same on production environment and check-in the changes of your infrastructure code, as this repo should be the source of truth of your deployed infrastructure. 
Our infrastructure is divided into a few areas.
1. Initial setup
  - [remote state][tf-remote-state]
  - [secrets][tf-secrets]
2. Infrastructure 
  - [production][tf-production-env]
  - [staging][tf-staging-env]
3. Kubernetes utilities
  - [production][tf-production-utilities]
  - [staging][tf-staging-utilities]


#### Style guide, resources, and Configuring your infrastructure as code
For more information about the terraform in this repo, please see [Link][zero-tf-guide]


## Using the Kubernetes Cluster

Before using the cluster the first time you'll need to set up your local `kubectl` context:
```shell
make update-k8s-conf
```

Then you should be able to run commands normally:
```shell
kubectl get pods -A
```


### Apply Configuration
To init and apply the terraform configs, simply run the `make` and specify the
environment. The default environment is `staging`
```shell
$ make ENVIRONMENT=<environment>
```

### Set up an application
Configure your k8s context

```shell
$ aws eks update-kubeconfig --name <cluster-name> --region us-east-1
```

#### Extra features built into my kubernetes cluster
Outlines and best practices utilities that comes with your EKS cluster.
Please see [Link][zero-k8s-guide]
- Dashboards
- Logging
- Monitoring
- Ingress / TLS certificates (auto provisioning)
- AWS IAM integration with Kubernetes RBAC
...

# Resources
###  Infrastructure
This [architecture-diagram][architecture-diagram] displays the original setup you get from the terraform templates

Commonly used links in AWS console
|Resources  |Links| 
|---        |---|
|Route 53   |https://console.aws.amazon.com/route53/home | 
|IAM        |https://console.aws.amazon.com/iam/home#/users|
|ECR        |https://console.aws.amazon.com/ecr/repositories|
|RDS        |https://console.aws.amazon.com/rds|
### Suggested readings
- [Terraform workflow][tf-workflow]
- [Why do I want code as infrastructure][why-infra-as-code]
- 


<!-- Links -->
[tf-workflow]: https://www.terraform.io/guides/core-workflow.html
[why-infra-as-code]: https://www.oreilly.com/library/view/terraform-up-and/9781491977071/ch01.html
<!-- code -->
[tf-remote-state]: ./terraform/bootstrap/remote-state
[tf-secrets]: ./terraform/bootstrap/secrets
[tf-production-env]: ./terraform/environments/production
[tf-staging-env]: ./terraform/environments/staging

[tf-production-utilities]: ./kubernetes/terraform/environments/production
[tf-staging-utilities]: ./kubernetes/terraform/environments/staging

[zero-tf-guide]: ./terraform/README.md
[zero-k8s-guide]: ./kubernetes/terraform/modules/kubernetes/README.md
[zero-architecture-diagram]: https://github.com/commitdev/zero-aws-eks-stack/blob/master/docs/architecture-overview.svg
[zero-resource-list]: https://github.com/commitdev/zero-aws-eks-stack/blob/master/docs/resources.md
