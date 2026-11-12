# Windows Server 2025 Hardening & Active Directory Infrastructure Lab

## 📌 Project Overview
This project documents the comprehensive deployment, system hardening, and verification of an Active Directory Domain Services (AD DS) environment (`ETHA.com`). It covers Domain Controller promotion, Read-Only Domain Controllers (RODC), DNS SRV record verification, secure remote management via PowerShell, and disaster recovery planning.

## 🏗️ Environment Architecture
* **Primary Domain Controller:** `STO-DC1` (AD DS, DNS Server, Global Catalog)
* **Secondary Domain Controller:** `STO-SVR1` (AD DS, Global Catalog)
* **Read-Only Domain Controller:** `STO-SVR2` (RODC, Global Catalog)
* **Member Servers & Clients:** `STO-CORE` (Server Core), `STO-CL1` (Windows 11 Enterprise)
* **Domain Name:** `ETHA.com`

---

## ⚙️ Key Tasks & Technical Implementation

### 1. Active Directory & DNS Configuration
* Installed the AD DS role on `STO-DC1` and promoted it to the root Domain Controller for `ETHA.com`.
* Verified core DNS infrastructure records (SOA, NS, A, and SRV records):
  * `_gc` (Port 3268 - Global Catalog)
  * `_kerberos` (Port 88 - Kerberos Authentication)
  * `_ldap` (Port 389 - Lightweight Directory Access Protocol)

### 2. Domain Expansion & Member Integration
* Joined Server Core (`STO-CORE`), member servers (`STO-SVR1`, `STO-SVR2`), and client workstations (`STO-CL1`) to the `ETHA.com` domain.
* Promoted `STO-SVR1` as a secondary Domain Controller for high availability and tested domain failover.

### 3. Read-Only Domain Controller (RODC) & Password Replication Policy (PRP)
* Configured `STO-SVR2` as a Read-Only Domain Controller to secure branch site authentication.
* Established custom **Password Replication Policies (PRP)** to restrict sensitive administrative credentials (such as the Domain `Administrator`) from being replicated or cached on the RODC.
* Managed credential caching settings via the Windows Registry (`CachedLogonsCount`).

### 4. Secure Remote Administration & Maintenance
* Executed secure remote management via PowerShell Remoting (`Enter-PSSession`).
* Gracefully demoted and removed AD DS from `STO-SVR1` using PowerShell cmdlets (`Uninstall-ADDSDomainController`).
* Configured **Remote Server Administration Tools (RSAT)** on Windows 11 client nodes and deployed **Windows Admin Center** on `STO-SVR2` for modern server-to-server management.
