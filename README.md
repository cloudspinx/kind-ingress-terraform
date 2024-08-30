# Set up Kind Kubernetes Cluster with Nginx Ingress using Terraform

This repository contains all the necessary code and configurations to set up a local Kubernetes cluster using Kind (Kubernetes in Docker) with Nginx as the Ingress controller, all managed and provisioned through Terraform. The setup provides a scalable, production-like environment for development and testing purposes. By leveraging Terraform's infrastructure as code capabilities, this repository ensures consistent and repeatable cluster deployments.

## Key Features

- **Kind (Kubernetes in Docker)**: Lightweight Kubernetes clusters running inside Docker containers, perfect for local development and testing.
- **Nginx Ingress Controller**: Manages external access to services within the Kubernetes cluster, providing routing, SSL termination, and load balancing.
- **Terraform Automation**: Streamlined infrastructure management using Terraform, enabling easy deployment, scaling, and teardown of Kubernetes clusters.
- **Scalability**: Easily configure and scale clusters to fit various testing and development needs.

## Getting Started

- **Prerequisites**: Ensure [Docker](https://docs.docker.com/engine/install/), [Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli), [Kind](https://kind.sigs.k8s.io/), and [Kubectl](https://kubernetes.io/docs/tasks/tools/) are installed on your system.
- **Deployment**: Follow the step-by-step instructions provided in the documentation to deploy the Kubernetes cluster with Nginx Ingress using Terraform.
- **Customization**: Customize cluster configurations and Nginx Ingress settings to match your development or testing requirements.

### 1. Clone the repository

```bash
git clone https://github.com/cloudspinx/kind-ingress-terraform.git
```
Change into the `kind-ingress-terraform` directory:
```bash
cd kind-ingress-terraform
```
### 2. Update configurations for deployment

Update helm chart version number for Nginx Ingress.  Get latest from [github releases page](https://github.com/kubernetes/ingress-nginx/releases). 
```bash
$ vim variables.tf
chart_version = "4.11.2"
```
Open the `main.tf` file and update the following:

```hcl
cluster_name    = "cluster1" # Name of the cluster to create
cluster_version = "1.31.0" # kubernetes version number used to create cluster, see https://github.com/kubernetes-sigs/kind/releases for latest
```
Optionally update the port on your local system that Nginx Ingress will use. In this guide we defaulted to port `8080`, you can use any other port provided no service is using it in your local system.

```hcl
extra_port_mappings {
        container_port = 80
        host_port      = 8080 # You can change to any other port ( make sure the port is not in use )
        listen_address = "0.0.0.0"
```
To test that ingress is working, we have provided `demo-ingress.yml` and `demo-ingress-app.yml` manifests that can be customized do deploy your app and use Ingress.

You can edit `demo-ingress.yml` to change host name:

```bash
host: webapp.example.com # Use any other suitable dns name
```
For SSL related configurations; see https://kubernetes.github.io/ingress-nginx/user-guide/tls/

### 3. Create resources:
Deploy kind cluster:
```bash
terraform init
terraform plan
terraform apply
```
It will output cluster endpoint and Nginx Ingress app version:
```
cluster_endpoint = "https://127.0.0.1:38041"
nginx_ingress_app_version = "1.11.2"
```

Test it works:
```bash
kubectl get nodes
kubectl get ingressclass
```
Create test app and ingress:
```
kubectl apply -f demo-ingress-app.yml
kubectl apply -f demo-ingress.yml
```
Confirm services are deployed:
```
kubectl get pods
kubectl get svc
kubectl get ingress
```
The easiest way of using webapp.example.com as configured is to map the domain to local machine IP address - 127.0.0.1 or actual IPv4 address.
```bash
$ sudo vim /etc/hosts
127.0.0.1  webapp.example.com
```
Test service access through Ingress using curl:
```bash
$ curl http://webapp.example.com:8080/banana
banana

$ curl http://webapp.example.com:8080/apple
apple
```

## Destroying the environment

Destroy the environment by running:
```bash
terraform destroy
```

Reference blog post is available at https://cloudspinx.com/set-up-kind-kubernetes-cluster-with-nginx-ingress-using-terraform/
## NOTE
This repository is ideal for developers and DevOps engineers looking to create a flexible and manageable local Kubernetes environment with advanced ingress capabilities.
