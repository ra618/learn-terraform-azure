 Terraform Configuration for Azure Resource Group Deployment

This repository contains Terraform configuration to deploy an Azure resource group. The resource group serves as the foundation for the infrastructure built in subsequent tutorials.
Prerequisites

Before you begin, ensure you have the following:

    An Azure subscription. If you don't have one, create an Azure account. This tutorial can be completed with an Azure free account or a paid subscription. Note that if you're using a paid subscription, you may incur charges for the resources.
    Terraform 0.14.9 or later installed.
    Azure CLI Tool installed.

Azure CLI Tool Installation

To install the Azure CLI tool, run the following command in your PowerShell prompt as an administrator:

bash

Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi

Authenticate using Azure CLI

Authenticate with Azure using the Azure CLI:

bash

az login

Follow the prompts to enter your Azure login credentials. After successful authentication, your terminal will display your subscription information.
Set up Account Permissions and Environment Variables

    Set your account subscription ID:

bash

az account set --subscription "<SUBSCRIPTION_ID>"

    Create a Service Principal:

bash

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<SUBSCRIPTION_ID>"

Protect the credentials provided in the output. Avoid including them in your code or source control.

    Set environment variables:

bash

$Env:ARM_CLIENT_ID = "<APPID_VALUE>"
$Env:ARM_CLIENT_SECRET = "<PASSWORD_VALUE>"
$Env:ARM_SUBSCRIPTION_ID = "<SUBSCRIPTION_ID>"
$Env:ARM_TENANT_ID = "<TENANT_VALUE>"

Terraform Configuration

    Create a directory named learn-terraform-azure.

bash

New-Item -Path "c:\" -Name "learn-terraform-azure" -ItemType "directory"

    Create a new file named main.tf and paste the following configuration:

hcl

# Configure the Azure provider
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0.2"
    }
  }

  required_version = ">= 1.1.0"
}

provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "rg" {
  name     = "myTFResourceGroup"
  location = "westus2"
}

The location of the resource group is hardcoded in this example. If needed, update the location attribute in main.tf with your Azure region.
Initialize Terraform Configuration

Initialize your Terraform configuration by running:

bash

terraform init

Format and Validate Configuration

Ensure consistent formatting by running:

bash

terraform fmt

To validate the configuration, run:

bash

terraform validate

Apply Terraform Configuration

Apply your Terraform configuration:

bash

terraform apply

Review the execution plan and type yes to proceed.
Inspect State

After applying the configuration, inspect the state:

bash

terraform show

You can also list resources in the state:

bash

terraform state list

For more advanced state management, refer to the terraform state command documentation.
