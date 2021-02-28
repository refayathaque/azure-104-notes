### [_Skills measured_](https://docs.microsoft.com/en-us/learn/certifications/exams/az-104)

- _Manage Azure identities & governance (15-20%)_
- _Implement & manage storage (10-15%)_
- _Deploy & manage Azure compute resources (25-30%)_
- _Configure & manage virtual networking (30-35%)_
- _Monitor & back up Azure resources (10-15%)_

---

### Azure concepts

- ✔️ Feb 27
- Core services - Virtual machines, virtual networking (Vnet) & storage
- Virtual machines can be placed on a virtual network - Arranged in "availability sets", placed behind "load balancers" - Abstractions: Azure Batch, Virtual Machine Scale Sets, AKS, Service Fabric
- App services - Web or container apps, fully managed servers, supports major runtimes like Python & Node.js - Benefits in scaling, CI, deployment slots
- Azure storage - Blobs, queues, tables, files - Storage tiers (hot, cool, archive) - Managed or unmanaged - Various levels of replication, local & global
- Data services (SQL server related) - Azure SQL Database, Azure SQL Managed Instance - Non SQL: Cosmos DB, Azure Database for MySQL, MariaDB, Azure Cache for Redis
- Microservices - Service Fabric, Azure Functions, API Management, AKS (a containerized app)
- Networking - Connectivity: VNet, ExpressRoute, VPN Gateway, Azure DNS, Peering, Bastion - Security: Network Security Groups, Azure Private Link, DDoS Protection, Azure Firewall - Delivery: CDN, Azure Front Door, Application Gateway, Load Balancer, Traffic Manager - Monitoring: Network Watcher, Azure Monitor, Vnet Terminal Access Point (TAP)

### Powershell & CLI

- ✔️ Feb 27
- Predictable commands
  - | CLI vnet                 | CLI vm         | PowerShell vm | Powershell vnet           |
    | ------------------------ | -------------- | ------------- | ------------------------- |
    | `az network vnet list`   | `az vm list`   | `Get-AzVM`    | `Get-AzVirtualNetwork`    |
    | `az network vnet create` | `az vm create` | `New-AzVM`    | `New-AzVirtualNetwork`    |
    | `az network vnet delete` | `az vm delete` | `Remove-AzVM` | `Remove-AzVirtualNetwork` |

### Manage resource groups

- ✔️ Feb 27
- ARM (Azure Resource Management) for Resource Groups (RG) - Organization structure for Azure resources, makes it easy to group resources for testing/projects/etc. - All resources within the RG will be deleted if the RG is deleted
- Locks prevent the content of the RG from changing, making it read-only - E.g., a lock won't allow you to even stop a VM in a RG
- For certain resources, like storage accounts, you must declare a resource group at creation
- You can assign policies to RGs, e.g., only allow resources creation in RG to certain regions
- You can move resources to other RGs, other subscriptions or other regions

### Manage subscriptions & governance

- ✔️ Feb 27
- Account - aka User - A person (user) or a program (app), person would have username, password, MFA, while a program could be an app with a managed identity - Serves as the basis for authentication
- Tenant - Representation of an organization/company - A dedicated instance of Azure AD - _Every Azure Account is part of at least one Tenant_ - Usually represented by a public domain name
- Subscription - Agreement with Microsoft to use Azure services, & how you're going to pay - All Azure resource usage gets billed to the payment method of the subscription - Free, pay as you go, enterprise agreements
- Tenants need not have subscriptions (meaning they can't create resources) - Tenants can have more than 1 Subscription
- Accounts are basically assigned roles within Tenants so there can be more than 1 Account as an owner (i.e., admin) in a Tenant - Grant other Accounts to be part of the Tenant
- Resource - Entity managed by Azure - E.g., VM, web app, storage account, & these "expected" resources often create "unexpected" resources like, public IP address, network interface card, network security group, etc.
- Accounts can be given read, update, owner rights to resources
- Resource Group (RG) - Way of organizing resources in a subscription - _All resources must belong to only 1 RH_ - Good way to separate out projects & isolate resources by project - Deleting RG deletes all resources inside
- Role assignments - E.g., (from highest access to lowest) - Owner: allows you to grant permissions to other people - Contributor: can CRUD resources they have permissions for - Reader: read only
- Policy - Define to enforce company standards & SLAs - E.g., (built-in policies) enforce tag & its value, allowed locations, allowed VM SKUs, not allowed resource types, etc. - Policy scope - Can be set at subscription level or RG
- E.g., Subscriptions help you know which customer/client should be billed for a resource/RG if you're a digital consultancy firm servicing multiple clients
- Management Groups - Helps you manage access, policy, & compliance by grouping multiple Subscriptions together - E.g., you can assign Users to Management Groups if, e.g., you need a User to have access to multiple Subscriptions being covered in that Management Group
- At the lowest level you have a User (Account), & that User is part of a Tenant in AD, that Tenant has Subscriptions, & Subscriptions can get organized into Management Groups

### Monitor resources by using Azure Monitor

- ✔️ Feb 28
- Monitor - Get full stack visibility, find & fix problems, optimize your performance, & underst& customer behavior all in one place - Look at metrics & log, & setup alerts & actions - Home for diagnostics
- For VMs to be able to log, you have to install a diagnostics Agent on it & then give it a Storage Account - You can also set custom performance counters for things like CPU, memory, network & disk, & decide on the rate of logging
- Baseline Environment using ARM templates - Go to Deployments in a RG & export the Deployment templates & parameters to have the baselines - This way, if something happens to your resources (or you want to revert to a former state) you can redeploy them - _Sounds like it's Azure's CloudFormation, also akin to Terraform IaC_ 
- Can also go to Automation script under a specific resource to get templates & parameters for the _current state_ of that resource (not available for _all_ resources & there could also be dependency issues where associated resources ("unexpected") aren't included)
- Create alerts - E.g., set alerts to know when a web app was stopped - Build this out by creating a rule & action group (email/SMS/invoke Azure function/webhook)
- Create metrics - Reports that you generate on the fly to keep track of your resources, their usage, network traffic, etc.
- Log Analytics - Allows you to search through resource logs, you have to connect the resource to the workspace first - Queries are similar to SQL queries, e.g., you can search for log records associated with when a specific VM started up in the last hour

### Create & configure storage accounts

- ❌ Feb 28
- Azure Storage is for Azure Blobs (objects), Azure Data lake Storage Gen2, Azure Files, Azure Queues (e.g., FIFO/LIFO), & Azure Tables (stores data in columns and rows)
- Costs will vary depending on the region, performance/access tier, replication, account kind (e.g., general purpose V2), etc.
- Can be accessible only in VNets (hence within subnets as well) or be accessible to all networks - If you select VNet you won't be able to access yourself even in the portal, so you'll need to whitelist your IP address
- Blob accounts live within Storage Accounts, & they have containers for objects (containers akin to S3 buckets) 
- Block blobs are fine for most use bases - Page blobs for partial updates - Append blobs for adding to a file
- If read-access geo-redundant replication chosen you'll have a secondary endpoint to access in case the primary is down (file service won't have a secondary for some reason)
- Use Access Keys to authenticate your apps when making requests to Azure Storage Accounts (you're provided with 2 keys) - Keys can be regenerated if compromised
- SAS (Shared Access Signature): URI that grants restricted access rights to Azure Storage resources - Can set time limits, which storage resources can be accessed, IP addresses, protocol, and CRUD permissions - Use this when you don't want to provide Access Keys - Essentially a _token_

### Import & export data to Azure

- ❌ Feb 28

### Configure Azure files

- ❌ Feb 28

### Implement backup & recovery

- ❌ Mar 1

### Azure Virtual Machines

- ❌ Mar 1

### Windows & Linux VMs

- ❌ Mar 1

### Manage Azure VM

- ❌ Mar 1

### Manage VM backups

- ❌ Mar 2

### Azure App services

- ❌ Mar 2

### Azure Kubernetes Services (AKS)

- ❌ Mar 2

### Manage Virtual Networking

- ❌ Mar 2

### Implement & manage virtual networking

- ❌ Mar 3

### Configure name resolution

- ❌ Mar 3

### Secure access to virtual networks

- ❌ Mar 3

### Manage Azure Active Directory

- ❌ Mar 3

### Manage Azure AD objects

- ❌ Mar 3

### Implement multi-factor

- ❌ Mar 4

### Manage role-based access control (RBAC)

- ❌ Mar 4

### Configure load balancing

- ❌ Mar 4

### Monitor & troubleshoot virtual networking

- ❌ Mar 4
