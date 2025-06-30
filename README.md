# Terraform-based-Azure-Infrastructure-Deployment
Terraform-based Azure Infrastructure Deployment 

🧰 Prerequisites
•	✅ Azure CLI installed Download
•	✅ Terraform installed Download
•	✅ Azure Subscription (with Contributor access or above)
•	✅ Code editor (e.g., VS Code)
________________________________________
🪜 Step-by-Step Guide
🧾 Step 1: Login to Azure
bash
CopyEdit
az login
🔐 Log into your Azure account. A browser window will open for authentication.
________________________________________
📁 Step 2: Create a New Directory for Your Terraform Project
bash
CopyEdit
mkdir terraform-azure-infra && cd terraform-azure-infra
📦 This keeps your Terraform files organized.
________________________________________
📄 Step 3: Create Terraform Configuration Files
🔹 main.tf
Defines the resources to provision.
hcl
CopyEdit
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}
🔹 variables.tf
To declare input variables.
hcl
CopyEdit
variable "location" {
  default = "East US"
}
🔹 outputs.tf
To define outputs after deployment.
hcl
CopyEdit
output "resource_group_name" {
  value = azurerm_resource_group.example.name
}
________________________________________
🔍 Step 4: Initialize Terraform
bash
CopyEdit
terraform init
📦 Initializes the working directory and downloads necessary providers.
________________________________________
📝 Step 5: Validate and Format the Configuration
bash
CopyEdit
terraform fmt
terraform validate
🔍 Ensures that your config is syntactically valid and well-formatted.
________________________________________
📋 Step 6: Plan the Deployment
bash
CopyEdit
terraform plan
🔎 Shows what Terraform will do without making changes.
________________________________________
🚀 Step 7: Apply the Configuration
bash
CopyEdit
terraform apply
💥 Provisions the infrastructure. Confirm when prompted.
________________________________________
📦 Step 8: Check Outputs
bash
CopyEdit
terraform output
📄 Displays the values defined in outputs.tf.
________________________________________
🧹 Step 9: Destroy the Infrastructure (When No Longer Needed)
bash
CopyEdit
terraform destroy
⚠️ Tears down the infrastructure. Use carefully.
________________________________________
📘 Example Resources to Add
You can expand your infra by adding:
•	💻 Virtual Machines
•	🛡️ Network Security Groups
•	🌐 Virtual Networks (VNet)
•	📊 Storage Accounts
•	🔄 Load Balancers
________________________________________
📁 Sample Folder Structure
css
CopyEdit
terraform-azure-infra/
├── main.tf
├── variables.tf
├── outputs.tf
└── README.md

👥 Deploy Azure Active Directory Users using Terraform
⚠️ Requires Terraform AzureAD provider, not just azurerm
📄 Step-by-Step
1.	Add the AzureAD Provider
hcl
CopyEdit
terraform {
  required_providers {
    azuread = {
      source  = "hashicorp/azuread"
      version = "~> 2.0"
    }
  }
}

provider "azuread" {
}
2.	Create User
hcl
CopyEdit
resource "azuread_user" "example" {
  user_principal_name = "john.doe@yourdomain.onmicrosoft.com"
  display_name        = "John Doe"
  mail_nickname       = "johndoe"
  password            = "TempPassword@123"  # Use Key Vault or variable
  force_password_change = true
}
________________________________________
🔐 Deploy Azure Key Vault using Terraform
1.	Add to your main.tf
hcl
CopyEdit
resource "azurerm_key_vault" "example" {
  name                        = "exampleKeyVault123"
  location                    = azurerm_resource_group.example.location
  resource_group_name         = azurerm_resource_group.example.name
  tenant_id                   = data.azurerm_client_config.current.tenant_id
  sku_name                    = "standard"

  soft_delete_enabled         = true
  purge_protection_enabled    = true

  access_policy {
    tenant_id = data.azurerm_client_config.current.tenant_id
    object_id = data.azurerm_client_config.current.object_id

    key_permissions = ["Get", "List"]
    secret_permissions = ["Get", "Set", "Delete"]
  }
}

data "azurerm_client_config" "current" {}
2.	Store a Secret
hcl
CopyEdit
resource "azurerm_key_vault_secret" "example" {
  name         = "exampleSecret"
  value        = "SuperSecretValue"
  key_vault_id = azurerm_key_vault.example.id
}
________________________________________
🌍 Deploy Azure Kubernetes Service (AKS) using Terraform
1.	Required Providers & Variables
hcl
CopyEdit
provider "azurerm" {
  features {}
}

resource "azurerm_kubernetes_cluster" "example" {
  name                = "exampleAKSCluster"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  dns_prefix          = "exampleaks"

  default_node_pool {
    name       = "default"
    node_count = 1
    vm_size    = "Standard_DS2_v2"
  }

  identity {
    type = "SystemAssigned"
  }

  tags = {
    Environment = "Dev"
  }
}
________________________________________
☁️ Deploy Hybrid Cloud Network (VNet Peering, VPN Gateway, etc.)
1.	Define VNets
hcl
CopyEdit
resource "azurerm_virtual_network" "on_prem_vnet" {
  name                = "OnPremVNet"
  address_space       = ["10.0.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_virtual_network" "cloud_vnet" {
  name                = "CloudVNet"
  address_space       = ["10.1.0.0/16"]
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}
2.	Add VNet Peering
hcl
CopyEdit
resource "azurerm_virtual_network_peering" "onprem_to_cloud" {
  name                      = "OnPremToCloud"
  resource_group_name       = azurerm_resource_group.example.name
  virtual_network_name      = azurerm_virtual_network.on_prem_vnet.name
  remote_virtual_network_id = azurerm_virtual_network.cloud_vnet.id
  allow_forwarded_traffic   = true
  allow_gateway_transit     = false
}


## Output HLD Diagram 

![image](https://github.com/user-attachments/assets/19a0b79b-2eec-4f86-ba41-f9c6bb664ebf)


