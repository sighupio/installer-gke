<!-- BEGIN_TF_DOCS -->

# GKE Installer - vpc module

<!-- <SD-DOCS> -->

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.3 |
| external | ~> 2.3 |
| google | ~> 3.90 |
| local | ~> 2.4 |
| null | ~> 3.2 |
| random | ~> 3.5 |

## Providers

| Name | Version |
|------|---------|
| external | ~> 2.3 |
| google | ~> 3.90 |
| local | ~> 2.4 |
| null | ~> 3.2 |
| random | ~> 3.5 |

## Inputs

| Name | Description | Default | Required |
|------|-------------|---------|:--------:|
| cluster\_pod\_subnetwork\_cidr | Private subnet CIDR | n/a | yes |
| cluster\_service\_subnetwork\_cidr | Private subnet CIDR | n/a | yes |
| cluster\_subnetwork\_cidr | Private subnet CIDR | n/a | yes |
| name | Name of the resources. Used as cluster name | n/a | yes |
| private\_subnetwork\_cidrs | Private subnet CIDRs | n/a | yes |
| public\_subnetwork\_cidrs | Public subnet CIDRs | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| additional\_cluster\_subnet | List of cidr\_blocks of private subnets |
| cluster\_subnet | Names of the cluster subnet |
| cluster\_subnet\_cidr\_blocks | List of cidr\_blocks of private subnets |
| furyagent | furyagent.yml used by the vpn instance and ready to use to create a vpn profile |
| network\_name | The name of the network |
| private\_subnets | List of names of private subnets |
| private\_subnets\_cidr\_blocks | List of cidr\_blocks of private subnets |
| public\_subnets | List of names of public subnets |
| public\_subnets\_cidr\_blocks | List of cidr\_blocks of public subnets |

## Usage

```hcl
terraform {
  required_version = "~> 1.4"
  required_providers {
    external   = "~> 2.3.1"
    google     = "~> 3.90.1"
    kubernetes = "~> 1.13.4"
    local      = "~> 2.4.0"
    null       = "~> 3.2.1"
  }
}

provider "google" {
  project     = var.gcp_project_id
  region      = "europe-west1"
  zone        = "europe-west1-b"
}

module "vpc" {
  source = "../../modules/vpc"

  name = "fury"

  public_subnetwork_cidrs  = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
  private_subnetwork_cidrs = ["10.0.101.0/24", "10.0.102.0/24", "10.0.103.0/24"]

  cluster_pod_subnetwork_cidr     = "10.2.0.0/16"
  cluster_service_subnetwork_cidr = "10.3.0.0/16"
  cluster_subnetwork_cidr         = "10.1.0.0/16"

}
```

<!-- </SD-DOCS> -->
<!-- END_TF_DOCS -->