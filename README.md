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
- ARM (Azure Resource Manager) for Resource Groups (RG) - Organization structure for Azure resources, makes it easy to group resources for testing/projects/etc. - All resources within the RG will be deleted if the RG is deleted
- Locks prevent the content of the RG from changing, making it read-only - E.g., a lock won't allow you to even stop a VM in a RG
- For certain resources, like storage accounts, you must declare a resource group at creation
- You can assign policies to RGs, e.g., only allow resources creation in RG to certain regions
- You can move resources to other RGs, other subscriptions or other regions

### Manage subscriptions & governance

- ✔️ Feb 27
- Account - aka User - A person (user) or a program (app), person would have username, password, MFA, while a program could be an app with a managed identity - Serves as the basis for authentication
- Tenant - Representation of an organization/company - A dedicated instance of Azure AD - _Every Azure Account is part of at least 1 Tenant_ - Usually represented by a public domain name
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
- Monitor - Get full stack visibility, find & fix problems, optimize your performance, & understand customer behavior all in 1 place - Look at metrics & log, & setup alerts & actions - Home for diagnostics
- For VMs to be able to log, you have to install a diagnostics Agent on it & then give it a Storage Account - You can also set custom performance counters for things like CPU, memory, network & disk, & decide on the rate of logging
- Baseline Environment using ARM templates - Go to Deployments in a RG & export the Deployment templates & parameters to have the baselines - This way, if something happens to your resources (or you want to revert to a former state) you can redeploy them - _Sounds like it's Azure's CloudFormation, also akin to Terraform IaC_
- Can also go to Automation script under a specific resource to get templates & parameters for the _current state_ of that resource (not available for _all_ resources & there could also be dependency issues where associated resources ("unexpected") aren't included)
- Create alerts - E.g., set alerts to know when a web app was stopped - Build this out by creating a rule & action group (email/SMS/invoke Azure function/webhook)
- Create metrics - Reports that you generate on the fly to keep track of your resources, their usage, network traffic, etc.
- Log Analytics - Allows you to search through resource logs, you have to connect the resource to the workspace first - Queries are similar to SQL queries, e.g., you can search for log records associated with when a specific VM started up in the last hour

### Create & configure storage accounts

- ✔️ Feb 28
- Azure Storage is for Azure Blobs (objects), Azure Data lake Storage Gen2, Azure Files, Azure Queues (e.g., FIFO/LIFO), & Azure Tables (stores data in columns & rows)
- Costs will vary depending on the region, performance/access tier, replication, account kind (e.g., general purpose V2), etc.
- Can be accessible only in VNets (hence within subnets as well) or be accessible to all networks - If you select VNet you won't be able to access yourself even in the portal, so you'll need to whitelist your IP address
- Blob accounts live within Storage Accounts, & they have containers for objects (containers akin to S3 buckets)
- Block blobs are fine for most use bases - Page blobs for partial updates - Append blobs for adding to a file
- Blob Storage (another kind of Storage Account kind, in addition to General Purpose V1 & V2) - Different from other kinds because _they only store Blobs_ - Can use to host public documents like images/documents/etc.
- Use Access Keys to authenticate your apps when making requests to Azure Storage Accounts (you're provided with 2 keys) - Keys can be regenerated if compromised
- SAS (Shared Access Signature): URI that grants restricted access rights to Azure Storage resources - Can set time limits, which storage resources can be accessed, IP addresses, protocol, & CRUD permissions - Use this when you don't want to provide Access Keys - Essentially a _token_
- Redundant Storage - Locally-redundant (replications (3 copies) in the same data center/region), geo-redundant (replicates to another region (region-pairs), e.g., if you're in Canada Central, it'll replicate to Canada East) & read-access geo-redundant options available for most regions, some regions will have more options available
- You can set a failover, so in the e.g., above, you can set Canada East as the Primary for failover
- If read-access geo-redundant replication chosen you'll have a secondary endpoint to access in case the primary is down (file service won't have a secondary endpoint for some reason)
- RBAC (Role-Baed Access Control) Authentication for Storage - Use Role Assignments to give Apps/Users access to Storage Accounts - There are a lot of specific storage resource based Roles, they are all under "Storage \*"
- Access Tiers - Implication related to how much you're going to get charged for storage & access (2 distinct concepts) - Going for _"Cool" will halve your storage costs but double your access costs (paying twice for read/write) when compared to "Hot"_ - Can set on a file basis too, e.g., you can set 1 file that you know will hard be used in a container to "Cool" while leaving others in the container to "Hot"
- All Access Tiers from most expensive to least: Premium (8x more expensive compared to Hot for storage, but 1/3 of Hot for read/write operations), Hot, Cool, Archive
- Lifecycle Management: Use policies to transition your data to the appropriate access tiers or expire (delete) at the end of the data's lifecycle
- Object Replication: When enabled, blobs are copied asynchronously from a source storage account to a destination account - You can set up replication rules where you declare the source container & the destination container, filters (add prefix match), & what exactly you want to copy over (e.g., only new object/all files/etc.)
- Only the General Purpose V2 & Blob storage account types support the Archive access tier
- Blob Storage account can be used to make all sorts of files publically accessible, like images/documents/static website files - But using a CDN is a more optimized way of serving these static files to users around the world (using caching) - CDN is a global resource & does not live in any region - 4 pricing tiers, with the cheapest 1 being the Microsoft 1, & the most expensive being the Premium Verizon (there is also a Standard Akamai in the middle) - Tiers vary by what benefits they offer
- CDN (Content Delivery Network) - allows you to improve performance by removing the burden of serving static, unchanging files from the main server to a network of servers around the globe; a CDN can reduce traffic to a server by 50% or more, which means you can serve more users or serve the same users faster

### Import & export data to Azure

- ✔️ Feb 28
- Use Import/export jobs to move large amounts of data into/out of Storage Accounts - For exports Azure will actually ship you a hard drive with all your data - For imports you have to send in your hard drive

### Configure Azure files

- ✔️ Feb 28
- Azure Files is part of regular Storage Accounts, unlike Blobs, Files can be accessed as drives from your own computer (over port 445) or can be mounted to VMs to be accessed through their own operating systems - A use case can be giving access to a File Share (the container where Files are stored in Azure Files) several VMs for all developers to store certain files in a centralized location
- Azure File Sync replicates files from your on-premises Windows Server to an Azure file share, enabling you to centralize your file services in Azure while maintaining local access to your data - Install Azure File Sync Agent to your server & set the File Share container for sync to be established - Azure File Sync service is used as a file distribution service
- If you're having issues with Azure File Sync, don't remove & recreate the server endpoint, _removing a server endpoint is not like rebooting a server & is not the solution_ - Removing a server endpoint is a destructive operation, & may result in data loss in the case that tiered files exist outside of the server endpoint namespace

### Implement backup & recovery

- ✔️ Mar 1
- Azure Backup - Can be set up in Backup & Site Recovery (OMS) by creating a Recovery Services Vault (needs specific Subscription, RG & Location/Region, the Location _must be the same as the resources you are backing up_) - You can back up on-premise resources as well - As of now you can back up an Azure VM, File Share or SQL Server in Azure VM - The Backup policy will allow you to choose a schedule & how long to keep backups for
- To backup any resource in Azure, the first thing you need to do is to create a Recovery Services vault (RSV)
- File Recovery from a VM Backup - Select a recovery point & download the executable, which is a script that will mount the disks (will remain mounted for 12 hours) from the selected recovery point as local drives on the machine where it is run
- On-Premises Backup - Install Recovery Services agent, download the vault credentials to register the server to the vault, then schedule backups using Recovery Services agent UI (this method is for just backing up files & folders) - If you want the backup to be Bare Metal Recovery or of a Microsoft SQL Server (there are other options too), you need to install the Microsoft Azure Backup Server
- Backup Reports - Turn on Diagnostics first & choose how to store logs & what to log, you also need to select a storage account - Backup Reports will use Power BI for you to view report dashboards, download reports & create custom reports
- Soft Delete for VM Backups - A soft delete is when you delete something but it doesn't actually get deleted right away, there is a delay, & this allows you to change your mind - This is a security feature in case someone hacks into your account & starts deleting your backups - Enabled by default on new RSV, you get _14 days to delete back up data, can be un-deleted during this 14-day window, but on the 15th day it will auto-delete_
- The primary goal of cloud-based backup services is to _prevent data loss by backing up VMs & other resources that store data_, not achieve high availability in terms of having no downtime if systems go down
- Azure Site Recovery (ASR) / ASR to Site-to-Site Replication - For setting this up in a VM go to Disaster Recovery & you will see the option for you to replicate the VM to another region (Target region, look at established region pairings when deciding, paired regions have a very high-speed connection with each other) - ASR will create new associated resources for the VM like VNet, Storage Accounts, RGs, etc. - You can check the progress of ASR/replication jobs in the RSV under 'Site Recovery jobs' or within the resource itself
- ASR Test Failover - Allows you to test recovery (make sure the replicated resources in the target region is working the way they should) without damaging the source - After verifying that the failover worked correctly you can run 'Cleanup test failover' - But running 'Failover' will spin up replicated resources in the target region & stop copying over things from the compromised/in-operational source region - There are no costs associated with replicated resources just sitting idle in the ASR target region, but there will be costs associated with data transfer between the source & target region to maintain the ASR & keep things in sync & there will be some ASR storage costs

### Azure Virtual Machines

- ✔️ Mar 1
- Create a VM - You'll need to select the Subscription, RG first, then decide on the name, Region (influences pricing differently), Image, Availability options, Inbound port rules (80 & 443 for Web traffic, 22 for SSH, 3389 for RDP), VNet, Subnet, SG, etc. - Spot instance: Getting a VM at a much cheaper price but run the risk of getting evicted at any time (good for low priority tasks)
- Connect to a VM - Using RDP (use administrator credentials you created when configuring the VM), SSH (using private key) or Bastion (if configured)
- VM Availability - Options will vary by Region, & this must be set during VM creation, you can't change Availability options later on - Availability Set: Logical grouping capability for isolating VM resources from each other when they're deployed. Azure makes sure that the VMs you place within an Availability Set run across multiple physical servers, compute racks, storage units, & network switches. If a hardware or software failure happens, only a subset of your VMs are impacted & your overall solution stays operational. (Only works for 2 or more VMs) _99.95%_ availability - Availability Zone: Expand the level of control you have to maintain the availability of the applications & data on your VMs. An Availability Zone is a physically separate zone, within an Azure region. There are three Availability Zones per supported Azure region. Each Availability Zone has a distinct power source, network, & cooling. By architecting your solutions to use replicated VMs in zones, you can _protect your apps & data from the loss of a data center. If 1 zone is compromised, then replicated apps & data are instantly available in another zone. 99.99% availability_
- For Availability Sets: Microsoft automatically assigns Virtual Machines across 2 fault domains (physical servers) & 5 update domains to minimize uptime during planned & unplanned outages by default. _The maximum number of FD is 3 & the maximum number of UD is 20._
- VM Monitoring - Azure Monitoring collects host-level metrics like CPU utilization, disk & network usage for all VMs without any additional software (but you have to Enable guest-level monitoring for more insights & to collect guest-level metrics, logs, etc.) - Need to allocate a storage account for diagnostics data to be sent - Enabling guest-level monitoring takes some time as the storage account may need to be created & the Azure Diagnostics agent installed on the VM
- VM Custom Script Extension - You want a script to run after the VM is deployed, you do this because it's pointless to just have a base image & no software installed on it - Need to install the Custom Script Extension first - It can download Powershell scripts & files from Azure storage & launch a Powershell script on the VM which in turn can download additional software components
- Azure Bastion Service - Must exist on it's own Subnet - More secure way (than RDP/SSH) of remotely connecting to a VM - You need to have open ports or have public IP on the VM itself, you connect to the VM through the Bastion
- Virtual Machine Scale Sets (VMSS) - Akin to AWS ASG - Grow & shrink (scaling) your resource usage depending on your actual needs - Create & manage a group of identical, load balanced VMs. The number of VM instances can automatically increase or decrease in response to demand or a defined schedule - Provides high availability & application resiliency - Allows your application to automatically scale as resource demand changes - You can set a custom scaling policy that'll allow you to set the CPU threshold %s & how many VMs to increase/decrease by, the max/min number of VMs, etc. - What is the maximum number of virtual machines that a virtual machine scale set can support? _1000_

### ARM Templates

- ✔️ Mar 1
- In Azure, automation of VM deployments done using ARM (Azure Resource Manager) templates - If the template had parameters as input then the template & parameter .json files would go hand-in-hand, parameter files hold values for the defined parameters in the template file, the template file simply initializes & declares type on the parameters & the parameter file provides the actual values - But, _in order to use ARM templates in an automation, no other files are required_
  Modify Existing ARM Templates - Most of the modification you'll do will be under "resources" in the template file - Azure will maintain the DSC (Desired State Configuration) whenever you deploy an ARM template, only changing resource configuration that you've changed & leaving everything else the way it is
- You can include a Custom Script Extension in an ARM template & also supply the actual script to then run after the VM is created based on the template configuration

### Manage Azure VM

- ✔️ Mar 1
- You can add Data Disks to a VM, the number of Data Disks you are permitted to add depends on the instance type, with the cheapest instances only allowing 2
- Add another NIC (Network Interface Card) to a VM, allowing it to belong to a different subnet within the same VNet - A new network interface named secondary has been created. The Network interface needs to be added to the Virtual machine. What must be done first in order to ensure that the network interface can be attached to the Virtual Machine? _The VM needs to be stopped first_
- Change VM Size - After you create a virtual machine (VM), you can scale the VM up or down by changing the VM size. In some cases, you must deallocate the VM first (_causing downtime_). This can happen if the new size is not available on the hardware cluster that is currently hosting the VM.
- Redeploy a VM - E.g., in Powershell `Set-AzureRmVM -Redeploy -ResourceGroupName "az102rg" -Name "aznewvm002" - Will wipe all existing configuration & redeploy the VM in the RG from scratch - _If you Redeploy the VM, it will be allocated to a different hardware cluster._ This will ensure that demovm is not affected by the maintenance. If you go to the Redeploy blade of your Virtual Machine, you can see the ability to relocate the VM on a different host.
- You can use the 'Move' command to move resources associated with a VM in a RG to another RG

### Manage VM backups

- ✔️ Mar 2
- VM Backups need to be assigned to a RSV (in order to back up a VM, you have to first create a recovery services vault) , & the RSV must be in the same region - You can choose Azure's built-in backup policies or create your own to have more fine-grained control over the frequency of backups & retention period - You can also keep a weekly/monthly backup, in addition to your daily backups, & you can also decide what the retention period will be for both weekly/monthly
- VM Backup Jobs & Restores - After successful backups your will see "Restore points", & from there you can choose which Restore point you want to restore the VM to - You'll have 2 options, the restore type can be "Create virtual machine" or "Restore disks" (restore VHD files in a network consistent manner, this will restore the disk to the same storage account, & you can create a new VM yourself from there)

### Azure App services

- ✔️ Mar 2
- An abstraction above hardware where you don't have control over the actual hardware, you just upload your code & configuration & Azure will run it - Benefits of using this lie in the additional development tools offered to you like VSCode, Azure DevOps & GitHub Actions integration
- App Service Plan - Web Apps require App Service Plan (think of this as the hosting mechanism), you can have multiple Web Apps running on the same App Service Plan - You need to choose the OS (Linux/Windows), Region (price will vary based on this) & Pricing Tier (determines the location, features, cost & compute resources associated with your app, you can't be as specific as you are when creating VMs, there are Azure-defined plans that you need to choose from, & these plans vary in ACU, memory, storage, auto-scaling, etc.) - ACU (Azure Compute Unit), a performance measurement essentially, defined as "dedicated compute resources used to run applications deployed in the App Service Plan
- Web App - When creating you can chose between Code (for code you have to choose the runtime, e.g., Java, Python, Ruby, Node, etc., & the _runtime you choose must be compatible (some runtimes are cross platform & will work on both Linux & Windows) with the OS you set in your App Service Plan_) or Docker Container, & you have to set an App Service Plan - There is no additional cost incurred with Web Apps because you are paying for the App Service Plan that it's going to run on
- Web App Settings - Can setup CI/CD with Azure DevOps, GitHub, Bitbucket, OneDrive, Dropbox, FTP, local Git & external Git repositories - There is the issue of Cold Start if the instance serving the Web App is idle for a long time, so you can keep it always running & the will prevent your app from being idled out due to inactivity (under Configuration) - Set HTTPS only if you want - You can also place the Web App in a VNet - Set Azure Front Door (load balancing) with WAF (Web Application Firewall) - Set CDN
- Scaling Web Apps - You can use "Scale up" & "Scale out" under Settings - Scaling up means moving to a larger (more expensive) plan that is in the list of Azure-defined Pricing Tier plans, & the transition will be mostly seamless for users - Scaling out is basically autoscaling, you can choose to scale your resource manually to a specific instance count, or via a custom Autoscale policy that scales based on metric(s) thresholds, or scheduled instance count which scales during designated time windows. Enables your resource to performant & cost effective by adding & removing instances based on demand - Azure automatically puts a load balancer in front as well when instances increase - If you decide to do manual scaling, your max instance count will be determined by the Pricing Tier plan (this also applies for Autoscaling, if Autoscaling is provided in the plan, there will be a max instance count)
  - |                   | Basic (Dedicated env for dev/test) | Standard (Dedicated prod env) | Premium (Dedicated prod env, enhanced performance & scale) | Isolated (High-performance, security & isolation) |
    | ----------------- | ---------------------------------- | ----------------------------- | ---------------------------------------------------------- | ------------------------------------------------- |
    | Disk space (GBs)  | 10                                 | 50                            | 250                                                        | 1000                                              |
    | Max instances     | 3                                  | 10                            | 30 (in select regions)                                     | 100                                               |
    | Autoscale         | No                                 | Yes                           | Yes                                                        | Yes                                               |
    | VNet Connectivity | No                                 | Yes                           | Yes                                                        | Yes                                               |
    | Compute Type      | Dedicated                          | Dedicated                     | Dedicated                                                  | Isolated                                          |
  - D1 is a shared env for dev/test, & will only have 1 instance with only 1GB of disk space
- You have defined an autoscale condition with four autoscale rules. The first rule scales out when the CPU utilization reaches 70 percent. The second rule scales back in when the CPU utilization drops below 50 percent. The third rule scales out if memory occupancy exceeds 75 percent. The fourth rule scales back in when memory occupancy falls below 50 percent. When will the system scale-out? _When CPU utilization reaches 70%, **or** memory occupancy exceeds 75%_

### Azure Kubernetes Services (AKS)

- ❌ Mar 2
- Kubernetes (K8s) is an abstraction layer on top of a VM, a Cluster is a grouping in K8s & is essentially a collection of VMs
- When creating your K8s Cluster you have to set the RG, Region (influences pricing), Cluster (collection of VMs, grouped into "Node pools") name, & the Node pool (select the node size, which is basically the VM type, & the node count, how many VMs to have in the pool that's allocated to the Cluster) - _K8s runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine_
- Primary node pool (what is required at Cluster creation) - For production workloads at least 3 nodes (VMs) are recommended for resiliency. For development or test workloads, only 1 node is required. You will _not be able to change the node size (VM type) of this primary node pool after Cluster creation, but you will be able to change the number of nodes & K8s version after creation._ If you would like additional Node pools, you will need to enable the "X" feature on the "Scale" tab which will allow you to add more Node pools after creating the cluster
- Kubectl - What you use from the CLI to interact with K8s clusters - E.g., deploy a Container to AKS Nodes using `kubectl apply -f *.yaml` (prepackaged code that has all configuration in the .yaml file) - `kubectl get nodes` will list all Nodes in your cluster - `kubectl get pods -o wide` will list all your Pods & also which Node they're running on
- To configure `kubectl` to connect to your K8s cluster, use the az aks get-credentials command. This command downloads credentials and configures the K8s CLI to use them.
- Scaling K8s
  - ![kubectl](kubectl.png)
  - Initially the 2 Pods are running their containers in the same Node, but when we told `kubectl` to scale up the `azure-vote-front` Pod by 3 you can see that `kubectl` creating 2 additional instances if this Pod & put them in the 2 other Nodes we have in the Cluster
  - E.g, .yaml can specify how much memory (max/min) is required for a specific container, based on that `kubectl` can figure out how many instances of _that_ container can run on a Node, if say, a VM has a memory of 1GB & the container has a max required memory of 256mB, then `kubectl` when asked to scale, can deploy up to 4 instances of that container in that VM (Node) - And if `kubectl` has to scale further (create more replicas) it will use other Nodes, if there are available, in the Cluster
  - By scaling out you're not incurring additional costs, because your Cluster configuration hasn't changed (unless you decided to change the Primary Node Pool, or add more Node Pools) & you're still managing the same amount of resources, you're just \_making better use" of the Cluster (resources) you've configured - You only pay for the virtual machines instances, storage, and networking resources consumed by your K8s cluster, _not Pods that are running on the VMs_
- Azure Container Instances (ACI) - Much easier & quicker to run containers than AKS because you don't have to configure hardware, use `kubectl`, orchestrate, etc. - By running your workloads in ACI, you can focus on designing & building your applications instead of managing the infrastructure that runs them - You can pull container images from ACR (Azure Container Registry, akin to AWS ECR) or Docker Hub, & you can pick your VM size (CPU cores (1-4) & Memory (0.5 to 16GBs)) - You can also give the container a Fully Qualified Domain Name under "DNS name label" - You can also decide what ports you want open - ACI containers can be burstable (for scaling) from AKS - ACI is good for small apps you don't really need scaling for, e.g., personal pages, prototypes, demos, etc.

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
