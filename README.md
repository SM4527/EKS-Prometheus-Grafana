<p align="center">

![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white) ![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white) ![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white) ![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white) ![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white) ![Shell Script](https://img.shields.io/badge/shell_script-%23121011.svg?style=for-the-badge&logo=gnu-bash&logoColor=white)

![Stars](https://img.shields.io/github/stars/SM4527/EKS-Prometheus-Grafana?style=for-the-badge) ![Forks](https://img.shields.io/github/forks/SM4527/EKS-Prometheus-Grafana?style=for-the-badge) ![Issues](https://img.shields.io/github/issues/SM4527/EKS-Prometheus-Grafana?style=for-the-badge) ![License](https://img.shields.io/github/license/SM4527/EKS-Prometheus-Grafana?style=for-the-badge)

</p>

# Project Title

EKS-Prometheus-Grafana [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=EKS%20-%20Prometheus%20-%20Grafana&url=https://github.com/SM4527/EKS-Prometheus-Grafana)

## Description

Deploy Prometheus monitoring and Grafana Dashboards on an EKS cluster using Terraform and Helm.

<p align="center">

![image](https://user-images.githubusercontent.com/78129381/153623445-bed0eb98-3c4d-41ae-b8b7-45a062b3e090.png)

</p>

## Getting Started

### Dependencies

* Docker
* AWS user with programmatic access and high privileges 
* Linux terminal
* Deploy an [EKS K8 Cluster](https://github.com/SM4527/EKS-Terraform) with Self managed Worker nodes on AWS using Terraform.
* Deploy a [NGINX Ingress](https://github.com/SM4527/EKS-Nginx-Ingress) on the above EKS cluster (Pod->service->Ingress->ELB+ACM->Route 53->Domain URL)
* Deploy a [Cluster Autoscaler](https://github.com/SM4527/EKS-Cluster-Autoscaler) on the above EKS cluster using Terraform and Helm

### Installing

* Clone the repository
* Set environment variable TF_VAR_AWS_PROFILE
* Review terraform variable values in variables.tf, locals.tf
* Override values in the Helm chart through the "chart_values.yaml" file
* Update the "adminPassword:" attribute with a value, in the grafana section of "chart_values.yaml" file
* Update kubernetes.tf with the AWS S3 bucket name and key name from the output of the [EKS K8 Cluster](https://github.com/SM4527/EKS-Terraform/blob/master/outputs.tf)

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
