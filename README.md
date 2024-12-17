# Minimal GKE Cluster Terraform Configuration

This Terraform configuration creates a minimal Google Kubernetes Engine (GKE) cluster with necessary networking and security components.

## Infrastructure Overview

The configuration creates:
- A VPC network and subnet with secondary IP ranges for pods and services
- A service account for GKE nodes with minimal required permissions
- A GKE cluster with:
  - 2 nodes (e2-standard-2 machine type - 2 vCPU, 8GB memory each)
  - Private networking
  - Basic monitoring and logging enabled

## Prerequisites

1. Install Google Cloud SDK
```bash
# For Debian/Ubuntu
sudo apt-get install google-cloud-sdk

# For MacOS with Homebrew
brew install google-cloud-sdk
```

2. Install Terraform
```bash
# For Debian/Ubuntu
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform

# For MacOS with Homebrew
brew install terraform
```

## Setup Instructions

1. Authenticate with Google Cloud:
```bash
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
```

2. Enable required APIs:
```bash
gcloud services enable container.googleapis.com
gcloud services enable compute.googleapis.com
```

3. Configure Terraform:
```bash
cd environments/dev
cp terraform.tfvars.template terraform.tfvars
# Edit terraform.tfvars with your project details
```

4. Initialize and apply Terraform:
```bash
terraform init
terraform plan
terraform apply
```

## Connecting to the Cluster

After the cluster is created, configure kubectl:
```bash
gcloud container clusters get-credentials minimal-gke-cluster \
    --zone us-central1-a \
    --project YOUR_PROJECT_ID
```

Verify the connection:
```bash
kubectl cluster-info
kubectl get nodes
```

## Cleanup

To destroy all resources:
```bash
terraform destroy
```
