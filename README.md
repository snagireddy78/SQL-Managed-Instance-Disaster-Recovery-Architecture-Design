# SQL-Managed Instance / Disaster Recovery Architecture Details and Step by Step Implementation 
SQL Managed Instance Installation process - Step by Step to Disaster Recovery - Ready for Massive roll out 
Powered by: Hugo Marques - Azure Architect

This document provide the Best Practice guidence for the SQL-Managed Instance implementation considering a Disaster Recovery Architecture with a full Failover Group configured.

ALl process can be implemented by Azure Resource Manager configuration, Powershell, ARM Templates or Infrastructure as a Code using Terraform. This Step by Step guide covers the first scenario which is using Azure Resouce Manager.

## Introduction
**SQL Database Managed Instance** is a deployment option in Azure SQL Database that is highly compatible with SQL Server, providing out-of-the-box support for most SQL Server features and accompanying tools and services. 
Managed Instance also provides native virtual network (VNET) support for an isolated, highly-secure environment to run your applications. Now you can combine the rich SQL Server programming surface area in the cloud with the operational and financial benefits of an intelligent, fully-managed database service, making Managed Instance the best PaaS destination for your SQL Server workloads.


## E2E Architecture
In order to ilustrate what you will have at the end of this deployment, please find here the **end to end Architecture.**
* Included on this deployment - please consider as **Best Practice** based on numbers of big customers deployment:
* 2 Regions (MUST be Pair Regions at this moment - this will garantee the Disaster Recovery desing. Across the region pairs Azure serializes platform updates (planned maintenance), so that only one paired region is updated at a time. In the event of an outage affecting multiple regions, at least one region in each pair will be prioritized for recovery. - For the example below, I chose **EAST-US2 and CENTRAL-US.** 
Check here  [link to pair regions!](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions#what-are-paired-regions) the option available that makes sense for your deployment.

E2E SQL-Managed Instance Architecture:

<img src="Images/sqlmiarchitecture.png">

## E2E Architecture (visio)
The Visio version can be found here [link to visio!](https://github.com/hcmarque/SQL-Managed-Instance-Disaster-Recovery-Architecture-Design/blob/master/Images/SQLMI-DR.vsdx)

## Requirements to have your Disaster Recovery Implementation working:
For the First Step, let’s create your Infrastructure, which include the following: 
* **Resource Groups (RG)** - We will create a dedicate RG for Network and another for SQL-MI; **Attention:** Both SQL Instances needs to be created on the same Resource Group, even located in different Regions;
* **Networks in 2 Azure Regions**: (EAST-US2 and CENTRAL-US in this design;
* **VPN Gateways between the Regions;** **Attention:** The virtual networks used by the the managed instances need to be connected through a VPN Gateway or Express Route. When two virtual networks connect through an on-premises network, ensure there is no firewall rule blocking ports 5022, and 11000-11999. Global VNet Peering is **not supported**.
* **Connections betweem the VPN Gateways;**
* Please find here more details about the Network Requirements for SQL Managed Instances:  [link to pair regions!](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auto-failover-group#enabling-geo-replication-between-managed-instances-and-their-vnets)


## Step 1: Main Activity: Create two Resources Groups - SQL-MI and Network
* **1.1** Create the Resource Group that will be used for both SQL Managed Instances (Primary and Secondary) –  Needs to be under the same Resource Group, otherwise your Failover Group wont work on later step). Click on `Create a Resource Group`

<img src="Images/picture01.png">

* **1.2** Select the desired `Subscription`, `Resource group` name and `Region`. **Remember** In case of Disaster Recovery design, please consider the Pair regions considerations mentioned E2E Architecture considerations. In this Architecture Desing Example, we will consider EAST-US2 and CENTRAL-US for the Instances.
Now CLick on `Review + Create`.

<img src="Images/picture02.png">

* **1.3** As soon as you have the Validation passed green light, click on `Create`

<img src="Images/picture03.png">





