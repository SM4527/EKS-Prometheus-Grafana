# Project Title

EKS-Prometheus-Grafana

## Description

Deploy Prometheus monitoring and Grafana Dashboards on an EKS cluster using Terraform and Helm.

## Getting Started

### Dependencies

* docker & docker-compose
* aws user with programmatic access and high privileges 
* linux terminal
* Deploy an [EKS K8 Cluster](https://github.com/SM4527/EKS-Terraform) with Self managed Worker nodes on AWS using Terraform.
* Deploy a [NGINX Ingress](https://github.com/SM4527/EKS-Nginx-Ingress) on the above EKS cluster (Pod->service->Ingress->ELB+ACM-> Route 53->Domain URL).
* Deploy a [Cluster Autoscaler](https://github.com/SM4527/EKS-Cluster-Autoscaler) on the above EKS cluster using Terraform and Helm.

### Installing

* clone the repository
* set environment variable TF_VAR_AWS_PROFILE
* review terraform variable values in variables.tf, locals.tf
* override values in the Helm chart through the "chart_values.yaml" file
* update kubernetes.tf with the AWS S3 bucket name and key name from the output of the [EKS K8 Cluster](https://github.com/SM4527/EKS-Terraform/blob/master/outputs.tf)

### Executing program

* Configure AWS user with AWS CLI.

```
docker-compose run --rm aws configure --profile $TF_VAR_AWS_PROFILE

docker-compose run --rm aws sts get-caller-identity
```

* Specify appropriate Terraform workspace.

```
docker-compose run --rm terraform workspace show

docker-compose run --rm terraform workspace select default
```

* Run Terraform apply to create the EKS cluster, k8 worker nodes and related AWS resources.

```
./run-docker-compose.sh terraform init

./run-docker-compose.sh terraform validate

./run-docker-compose.sh terraform plan

./run-docker-compose.sh terraform apply
```

* Verify prom and grafana related deployments, pods and services are running and the Ingress is setup correctly.

```
./run-docker-compose.sh kubectl get all -A | grep -i monitoring

./run-docker-compose.sh kubectl get ingress -n monitoring
```

* View the Grafana application URL using the Domain Https URL, prefixed by "grafana." and when prompted, enter grafana admin username and password.

## Help

* Grafana ingress is not connected to the AWS Network load balancer. 

```
Issue: The external address field on the ingress remains empty. ideally, it is expected to list the NLB Domain address.

Fix:  Added the below assignment under the Ingress section of grafana service in the helm chart_values.yaml
ingressClassName: nginx

```

Reference: https://kubernetes.io/blog/2020/04/02/improvements-to-the-ingress-api-in-kubernetes-1.18/#specifying-the-class-of-an-ingress

## Authors

[Sivanandam Manickavasagam](https://www.linkedin.com/in/sivanandammanickavasagam)

## Version History

* 0.1
    * Initial Release

## License

This project is licensed under the MIT License - see the LICENSE file for details

## Repo rosters

### Stargazers

[![Stargazers repo roster for @SM4527/EKS-Prometheus-Grafana](https://reporoster.com/stars/dark/SM4527/EKS-Prometheus-Grafana)](https://github.com/SM4527/EKS-Prometheus-Grafana/stargazers)
