# AZURE-RESOURCE-GROUP

This repository consists of Terraform templates to create Azure Resource Group.

## Usage

- Clone this repo with: `git clone --recurse-submodules https://github.com/cklewar/azure-resource-group`
- Enter repository directory with: `cd azure-resource-group`
- Obtain F5XC API certificate file from Console and save it to `cert` directory
- Pick and choose from below examples and add mandatory input data and copy data into file `main.tf.example`.
- Rename file __main.tf.example__ to __main.tf__ with: `rename main.tf.example main.tf`
- Initialize with: `terraform init`
- Apply with: `terraform apply -auto-approve` or destroy with: `terraform destroy -auto-approve`

## Create Azure Subnet


```hcl
variable "project_prefix" {
  type        = string
  description = "prefix string put in front of string"
  default     = "f5xc"
}

variable "project_suffix" {
  type        = string
  description = "prefix string put at the end of string"
  default     = "02"
}

variable "azure_client_id" {
  type    = string
}

variable "azure_client_secret" {
  type    = string
}

variable "azure_tenant_id" {
  type    = string
}

variable "azure_subscription_id" {
  type    = string
}

variable "azure_region" {
  type    = string
  default = "eastus"
}

provider "azurerm" {
  features {}
  client_id       = var.azure_client_id
  client_secret   = var.azure_client_secret
  tenant_id       = var.azure_tenant_id
  subscription_id = var.azure_subscription_id
  alias           = "eastus"
}

module "azure_resource_group" {
  source                    = "./modules/azure/resource_group"
  azure_region              = var.azure_region
  azure_resource_group_name = format("%s-azure-vm-rg-%s", var.project_prefix, var.project_suffix)
  providers                 = {
    azurerm = azurerm.eastus
  }
}
```