# Enterprise Hybrid Infrastructure Lab: On-Premises to Microsoft Azure

## Overview
This project demonstrates the design, deployment, and security hardening of an enterprise hybrid infrastructure, extending an on-premises Windows Server environment to Microsoft Azure. 

The architecture implements hybrid identity synchronization, secure cross-premises connectivity, centralized cloud governance, and business continuity capabilities aligned with Zero Trust principles.


## Architecture Diagram
![Hybrid Architecture](docs/architecture.png)


## Technical Specifications & Components

### 1. On-Premises Environment (Virtual Lab)
* **Operating System:** Windows Server 2022 Standard
* **Roles & Features:** Active Directory Domain Services (AD DS), DNS, IIS Web Server
* **Local Subnet:** `192.168.10.0/24`
* **Domain Name:** `corp.local`

### 2. Microsoft Azure Architecture
* **Virtual Network (VNet):** `10.100.0.0/16`
  * GatewaySubnet: `10.100.255.0/27` (Virtual Network Gateway)
  * Workload Subnet: `10.100.1.0/24` (Protected by NSGs)
* **Hybrid Connectivity:** IPsec/IKE Site-to-Site VPN connecting local perimeter to Azure VNet.
* **Identity & Access Management:** Hybrid identity powered by **Microsoft Entra Connect** (Password Hash Synchronization).
* **Cloud Governance & Management:** Hybrid server onboarding via **Azure Arc Connected Machine Agent**.
* **Business Continuity & Resiliency:** Automated backup of critical server volumes to an **Azure Recovery Services Vault** using the MARS Agent.

## Implementation Phases

### Phase 1: On-Premises Core Infrastructure
* Provisioned local domain controller and configured Active Directory directory schema.
* Configured enterprise network segmentation, local DNS forwarders, and IIS services.

### Phase 2: Hybrid Network Interconnection
* Deployed Azure Virtual Network Gateway and configured Local Network Gateway mapping on-premises endpoints.
* Established IPsec Site-to-Site tunnel and validated end-to-end bidirectional routing between `192.168.10.0/24` and `10.100.1.0/24`.

### Phase 3: Hybrid Identity Synchronization
* Installed and configured **Microsoft Entra Connect** on the primary domain controller.
* Implemented Password Hash Synchronization (PHS) and scoped synchronization to dedicated Organizational Units (OUs).

### Phase 4: Unified Governance via Azure Arc
* Deployed Azure Connected Machine Agent using a service principal.
* Applied compliance monitoring policies via **Azure Policy** to audit OS baseline security configurations.

### Phase 5: Disaster Recovery & Backup Integration
* Provisioned an Azure Recovery Services Vault with geo-redundant storage.
* Configured scheduled daily backups of critical system files and SQL data volumes.


## Verification & Key Artifacts

### 1. Secure Cross-Premises VPN Status
*Verified active S2S tunnel status:*
*(Place screenshot here: screenshots/01-vpn-connected.png)*

### 2. Directory Synchronization Verification
*Showing synced on-premises users in Microsoft Entra ID admin center:*
*(Place screenshot here: screenshots/02-entra-synced-users.png)*

### 3. Azure Arc Management
*Local Windows Server visible and managed as an Azure resource:*
*(Place screenshot here: screenshots/03-azure-arc-connected.png)*

### 4. Recovery Services Vault Completed Job
*Successful backup execution log in Azure:*
*(Place screenshot here: screenshots/04-backup-success.png)*


## Author
**Xavi Cladera**  
*Systems & Hybrid Infrastructure Administrator*  
* [LinkedIn Profile](https://www.linkedin.com/in/xavicladera)
