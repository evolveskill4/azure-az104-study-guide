# Azure Administrator (AZ-104) — Essential Azure CLI Cheatsheet 🛠️☁️

A practical compilation of everyday Azure Command-Line Interface (Azure CLI) commands required for managing identities, networking, compute, storage, and governance under the AZ-104 certification curriculum. Curated by **EvolveSkill**.

---

## 1. Resource Group Management

Create a new resource group in Central US:
> `az group create --name rg-evolveskill-prod --location centralus`

List all resource groups in formatted table:
> `az group list --output table`

Delete resource group and all contained resources asynchronously:
> `az group delete --name rg-evolveskill-prod --no-wait --yes`

---

## 2. Virtual Networking & Subnets

Create a Virtual Network with an initial subnet:
> `az network vnet create --name vnet-evolveskill --resource-group rg-evolveskill-prod --address-prefix 10.0.0.0/16 --subnet-name snet-backend --subnet-prefix 10.0.1.0/24`

Create an additional subnet inside the VNet:
> `az network vnet subnet create --name snet-dmz --vnet-name vnet-evolveskill --resource-group rg-evolveskill-prod --address-prefixes 10.0.2.0/24`

Create a Network Security Group (NSG):
> `az network nsg create --resource-group rg-evolveskill-prod --name nsg-dmz-rules`

Create an inbound security rule allowing HTTPS (port 443):
> `az network nsg rule create --resource-group rg-evolveskill-prod --nsg-name nsg-dmz-rules --name AllowHTTPSInbound --priority 100 --destination-port-ranges 443 --protocol Tcp --access Allow`

---

## 3. Azure Virtual Machines (Compute)

Deploy an Ubuntu Linux VM with automatic SSH keys:
> `az vm create --resource-group rg-evolveskill-prod --name vm-prod-web01 --image Ubuntu2204 --admin-username azureuser --generate-ssh-keys --vnet-name vnet-evolveskill --subnet snet-backend`

Restart an existing virtual machine:
> `az vm restart --resource-group rg-evolveskill-prod --name vm-prod-web01`

Resize a virtual machine SKU:
> `az vm resize --resource-group rg-evolveskill-prod --name vm-prod-web01 --size Standard_B2ms`

Open web port 80 to inbound traffic:
> `az vm open-port --port 80 --resource-group rg-evolveskill-prod --name vm-prod-web01`

---

## 4. Azure Storage Accounts & Blobs

Create a Standard LRS Storage Account:
> `az storage account create --name stavskillprod01 --resource-group rg-evolveskill-prod --location centralus --sku Standard_LRS --kind StorageV2 --allow-blob-public-access false`

Create a private container inside the storage account:
> `az storage container create --name media-backups --account-name stavskillprod01 --auth-mode login`

Upload a local file into the container:
> `az storage blob upload --account-name stavskillprod01 --container-name media-backups --name backup-index.json --file ./backup-index.json --auth-mode login`

---

## 5. Entra ID (Azure AD) & Role-Based Access Control (RBAC)

List Microsoft Entra users:
> `az ad user list --output table`

Assign 'Reader' role to a specific user on a resource group:
> `az role assignment create --assignee "user@example.com" --role "Reader" --resource-group rg-evolveskill-prod`

List all role assignments applied to a resource group:
> `az role assignment list --resource-group rg-evolveskill-prod --output table`

---

## 6. Monitoring & Resource Governance

Query activity logs for failed events in the last 7 days:
> `az monitor activity-log list --max-events 20 --status Failed --output table`

Apply an Azure tag to an entire resource group:
> `az group update --name rg-evolveskill-prod --set tags.Environment=Production tags.Project=EvolveSkill`

---

## 📚 Study Notes & Certification Track
- Full AZ-104 Exam Roadmap & Vouchers: [evolveskill4.com](https://evolveskill4.com)
- Cloud Architecture & Hands-on Labs: [EvolveSkill Substack](https://substack.com/@evolveskill4)

---
*Documented by Rahul Sharma | Technical Content Writer @ EvolveSkill*
