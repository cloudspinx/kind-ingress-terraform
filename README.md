# kind-ingress-terraform

This repository contains all the necessary code and configurations to set up a local Kubernetes cluster using Kind (Kubernetes in Docker) with Nginx as the Ingress controller, all managed and provisioned through Terraform. The setup provides a scalable, production-like environment for development and testing purposes. By leveraging Terraform's infrastructure as code capabilities, this repository ensures consistent and repeatable cluster deployments.

## Key Features

- **Kind (Kubernetes in Docker)**: Lightweight Kubernetes clusters running inside Docker containers, perfect for local development and testing.
- **Nginx Ingress Controller**: Manages external access to services within the Kubernetes cluster, providing routing, SSL termination, and load balancing.
- **Terraform Automation**: Streamlined infrastructure management using Terraform, enabling easy deployment, scaling, and teardown of Kubernetes clusters.
- **Scalability**: Easily configure and scale clusters to fit various testing and development needs.

## Getting Started

- **Prerequisites**: Ensure Docker, Terraform, Kind, and Kubectl are installed on your system.
- **Deployment**: Follow the step-by-step instructions provided in the documentation to deploy the Kubernetes cluster with Nginx Ingress using Terraform.
- **Customization**: Customize cluster configurations and Nginx Ingress settings to match your development or testing requirements.

This repository is ideal for developers and DevOps engineers looking to create a flexible and manageable local Kubernetes environment with advanced ingress capabilities.
