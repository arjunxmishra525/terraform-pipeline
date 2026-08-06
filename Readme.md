# ☁️ Azure Enterprise Infrastructure as Code (IaC)

This repository serves as the single source of truth for provisioning and managing Azure infrastructure using Terraform. It is built with a modular approach to ensure scalability, security, and DR-readiness across multiple environments.

---

## 📑 Table of Contents
1. [Architecture Overview](#-architecture-overview)
2. [Directory Structure](#-directory-structure)
3. [Modules Dictionary](#-modules-dictionary)
4. [Environment Strategy](#-environment-strategy)
5. [Deployment Guide](#-deployment-guide)
6. [State Management](#-state-management)

---

## 🏗️ Architecture Overview

The infrastructure deployed via this code follows a Hub-and-Spoke topology principle. Key components include:
*   **Edge Security & Load Balancing:** Application Gateway and Public IPs.
*   **Core Networking:** Virtual Networks, Subnets, and NAT Gateways.
*   **Compute:** Virtual Machine scale sets and Bastion for secure remote access.
*   **Governance & Secrets:** Resource Groups and Azure Key Vault.

---

## 📂 Directory Structure

The repository maintains a strict boundary between reusable modules and environment-specific variable injections.

```text
├── .github/workflows/         # CI/CD pipelines for validation and deployment
├── environment/
│   ├── dev/                   # Sandboxed environment for testing
│   └── prod/                  # Mission-critical production environment
└── Module/                    # Internal Terraform registry (reusable blocks)

🧩 Modules DictionaryAll infrastructure components are modularized to enforce DRY (Don't Repeat Yourself) principles.Module NameDescriptionKey Resources Deployedazurerm_resource_groupBase logical containerazurerm_resource_groupazurerm_virtual_networkCore network boundaryazurerm_virtual_networkazurerm_subnetNetwork segmentationazurerm_subnetazurerm_NSGNetwork-level access controlazurerm_network_security_group, rulesazurerm_NICVM network interfacesazurerm_network_interfaceazurerm_NIC+NSG_AssocitionBinds NSGs to NICsazurerm_network_interface_security_group_associationazurerm_application_gatewayL7 load balancingazurerm_application_gateway, WAF policiesazurerm_bastionSecure RDP/SSH accessazurerm_bastion_host, azurerm_public_ipazurerm_nat_gatewayOutbound internet accessazurerm_nat_gateway, azurerm_public_ipazurerm_Virtual_MachineCompute instancesazurerm_linux_virtual_machine / windowsazurerm_key_vaultSecret & certificate storeazurerm_key_vault, access policies🌍 Environment StrategyDEV (/environment/dev): Utilizes standard SKUs, smaller VM sizes, and relaxed network rules for rapid development and testing.PROD (/environment/prod): Employs premium SKUs, zone-redundancy, strict Azure RBAC enforcement, and locked-down Network Security Groups.🚀 Deployment GuidePrerequisitesTerraform CLI installed locally.Valid Azure credentials with Contributor or Owner access at the subscription level.ExecutionNavigate to your target environment and execute the standard Terraform workflow:Bash# 1. Navigate to the environment
cd environment/dev  # or environment/prod

# 2. Initialize the working directory (downloads providers)
terraform init

# 3. Format and validate the code
terraform fmt
terraform validate

# 4. Preview the infrastructure changes
terraform plan -out=tfplan

# 5. Apply the changes
terraform apply tfplan

🗄️ State Managemen

tState files (terraform.tfstate) are strictly managed remotely. Do not commit state files to version control.Backend: Azure Blob StorageState Locking: Enabled via Azure Storage Account leasing to prevent concurrent state corruption during CI/CD runs.
👨‍💻 Author
Arjun Mishra
Senior DevOps Engineer
