### [_Skills measured_](https://docs.microsoft.com/en-us/learn/certifications/exams/az-104)

- _Manage Azure identities & governance (15-20%)_
- _Implement & manage storage (10-15%)_
- _Deploy & manage Azure compute resources (25-30%)_
- _Configure & manage virtual networking (30-35%)_
- _Monitor & back up Azure resources (10-15%)_

---

### Azure concepts

- ✔️ Feb 27
- Core services - Virtual machines, virtual networking (VNet) & storage
- Virtual machines can be placed on a virtual network - Arranged in "availability sets", placed behind "load balancers" - Abstractions: Azure Batch, Virtual Machine Scale Sets, AKS, Service Fabric
- App services - Web or container apps, fully managed servers, supports major runtimes like Python & Node.js - Benefits in scaling, CI, deployment slots
- Azure storage - Blobs, queues, tables, files - Storage tiers (hot, cool, archive) - Managed or unmanaged - Various levels of replication, local & global
- Data services (SQL server related) - Azure SQL Database, Azure SQL Managed Instance - Non SQL: Cosmos DB, Azure Database for MySQL, MariaDB, Azure Cache for Redis
- Microservices - Service Fabric, Azure Functions, API Management, AKS (a containerized app)
- Networking - Connectivity: VNet, ExpressRoute, VPN Gateway, Azure DNS, Peering, Bastion - Security: Network Security Groups, Azure Private Link, DDoS Protection, Azure Firewall - Delivery: CDN, Azure Front Door, Application Gateway, Load Balancer, Traffic Manager - Monitoring: Network Watcher, Azure Monitor, VNet Terminal Access Point (TAP)

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
- Policy - Define to enforce company standards & SLAs - E.g., (built-in policies) enforce tag & its value, allowed locations, allowed VM SKUs (Stock Keeping Unit), not allowed resource types, etc. - Policy scope - Can be set at subscription level or RG
- E.g., Subscriptions help you know which customer/client should be billed for a resource/RG if you're a digital consultancy firm servicing multiple clients
- Management Groups - Helps you manage access, policy, & compliance by grouping multiple Subscriptions together - E.g., you can assign Users to Management Groups if, e.g., you need a User to have access to multiple Subscriptions being covered in that Management Group
- At the lowest level you have a User (Account), & that User is part of a Tenant in AD, that Tenant has Subscriptions, & Subscriptions can get organized into Management Groups

### Monitor resources by using Azure Monitor

- ✔️ Feb 28
- Monitor - Get full stack visibility, find & fix problems, optimize your performance, & understand customer behavior all in one place - Look at metrics & log, & setup alerts & actions - Home for diagnostics
- For VMs to be able to log, you have to install a diagnostics Agent on it & then give it a Storage Account - You can also set custom performance counters for things like CPU, memory, network & disk, & decide on the rate of logging
- Baseline Environment using ARM templates - Go to Deployments in a RG & export the Deployment templates & parameters to have the baselines - This way, if something happens to your resources (or you want to revert to a former state) you can redeploy them - _Sounds like it's Azure's CloudFormation, also akin to Terraform IaC_ 
- Can also go to Automation script under a specific resource to get templates & parameters for the _current state_ of that resource (not available for _all_ resources & there could also be dependency issues where associated resources ("unexpected") aren't included)
- Create alerts - E.g., set alerts to know when a web app was stopped - Build this out by creating a rule & action group (email/SMS/invoke Azure function/webhook)
- Create metrics - Reports that you generate on the fly to keep track of your resources, their usage, network traffic, etc.
- Log Analytics - Allows you to search through resource logs, you have to connect the resource to the workspace first - Queries are similar to SQL queries, e.g., you can search for log records associated with when a specific VM started up in the last hour

### Create & configure storage accounts

- ✔️ Feb 28
- Azure Storage is for Azure Blobs (objects), Azure Data lake Storage Gen2, Azure Files, Azure Queues (e.g., FIFO/LIFO), & Azure Tables (stores data in columns and rows)
- Costs will vary depending on the region, performance/access tier, replication, account kind (e.g., general purpose V2), etc.
- Can be accessible only in VNets (hence within subnets as well) or be accessible to all networks - If you select VNet you won't be able to access yourself even in the portal, so you'll need to whitelist your IP address
- Blob accounts live within Storage Accounts, & they have containers for objects (containers akin to S3 buckets) 
- Block blobs are fine for most use bases - Page blobs for partial updates - Append blobs for adding to a file
- Blob Storage (another kind of Storage Account kind, in addition to General Purpose V1 and V2) - Different from other kinds because _they only store Blobs_ - Can use to host public documents like images/documents/etc.
- Use Access Keys to authenticate your apps when making requests to Azure Storage Accounts (you're provided with 2 keys) - Keys can be regenerated if compromised
- SAS (Shared Access Signature): URI that grants restricted access rights to Azure Storage resources - Can set time limits, which storage resources can be accessed, IP addresses, protocol, and CRUD permissions - Use this when you don't want to provide Access Keys - Essentially a _token_
- Redundant Storage - Locally-redundant (replications (3 copies) in the same data center/region), geo-redundant (replicates to another region (region-pairs), e.g., if you're in Canada Central, it'll replicate to Canada East) and read-access geo-redundant options available for most regions, some regions will have more options available
- You can set a failover, so in the e.g., above, you can set Canada East as the Primary for failover
- If read-access geo-redundant replication chosen you'll have a secondary endpoint to access in case the primary is down (file service won't have a secondary endpoint for some reason)
- RBAC (Role-Baed Access Control) Authentication for Storage - Use Role Assignments to give Apps/Users access to Storage Accounts - There are a lot of specific storage resource based Roles, they are all under "Storage *"
- Access Tiers - Implication related to how much you're going to get charged for storage and access (2 distinct concepts) - Going for _"Cool" will halve your storage costs but double your access costs (paying twice for read/write) when compared to "Hot"_ - Can set on a file basis too, e.g., you can set one file that you know will hard be used in a container to "Cool" while leaving others in the container to "Hot"
- All Access Tiers from most expensive to least: Premium (8x more expensive compared to Hot for storage, but 1/3 of Hot for read/write operations), Hot, Cool, Archive
- Lifecycle Management: Use policies to transition your data to the appropriate access tiers or expire (delete) at the end of the data's lifecycle
- Object Replication: When enabled, blobs are copied asynchronously from a source storage account to a destination account - You can set up replication rules where you declare the source container and the destination container, filters (add prefix match), and what exactly you want to copy over (e.g., only new object/all files/etc.)
- Only the General Purpose V2 and Blob storage account types support the Archive access tier
- Blob Storage account can be used to make all sorts of files publically accessible, like images/documents/static website files - But using a CDN is a more optimized way of serving these static files to users around the world (using caching) - CDN is a global resource and does not live in any region - 4 pricing tiers, with the cheapest one being the Microsoft one, and the most expensive being the Premium Verizon (there is also a Standard Akamai in the middle) - Tiers vary by what benefits they offer
- CDN (Content Delivery Network) - allows you to improve performance by removing the burden of serving static, unchanging files from the main server to a network of servers around the globe; a CDN can reduce traffic to a server by 50% or more, which means you can serve more users or serve the same users faster

### Import & export data to Azure

- ✔️ Feb 28
- Use Import/export jobs to move large amounts of data into/out of Storage Accounts - For exports Azure will actually ship you a hard drive with all your data - For imports you have to send in your hard drive

### Configure Azure files

- ✔️ Feb 28
- Azure Files is part of regular Storage Accounts, unlike Blobs, Files can be accessed as drives from your own computer (over port 445) or can be mounted to VMs to be accessed through their own operating systems - A use case can be giving access to a File Share (the container where Files are stored in Azure Files) several VMs for all developers to store certain files in a centralized location
- Azure File Sync replicates files from your on-premises Windows Server to an Azure file share, enabling you to centralize your file services in Azure while maintaining local access to your data - Install Azure File Sync Agent to your server and set the File Share container for sync to be established
- If you're having issues with Azure File Sync, don't remove and recreate the server endpoint, _removing a server endpoint is not like rebooting a server and is not the solution_ - Removing a server endpoint is a destructive operation, and may result in data loss in the case that tiered files exist outside of the server endpoint namespace

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
