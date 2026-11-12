# Windows Server 2025 Hardening & Active Directory Infrastructure Lab

## 📌 Project Overview
This project documents the deployment, hardening, and verification of an Active Directory Domain Services (AD DS) environment (`ETHA.com`). It covers Domain Controller promotion, Read-Only Domain Controllers (RODC), DNS SRV verification, secure remote management, and disaster recovery planning.

## 🏗️ Environment Architecture
* **Primary Domain Controller:** `STO-DC1` (AD DS, DNS Server, Global Catalog)
* **Secondary Domain Controller:** `STO-SVR1` (AD DS, Global Catalog)
* **Read-Only Domain Controller:** `STO-SVR2` (RODC, Global Catalog)
* **Member Servers & Clients:** `STO-CORE` (Server Core), `STO-CL1` (Windows 11 Enterprise)
* **Domain Name:** `ETHA.com`

---

## ⚙️ Key Tasks & Technical Implementation

### 1. Active Directory & DNS Configuration
* Installed AD DS role on `STO-DC1` and promoted it to the root Domain Controller for `ETHA.com`.
* Verified DNS infrastructure records, including SOA, NS, A, and SRV records:
  * `_gc` (Port 3268 - Global Catalog)
  * `_kerberos` (Port 88 - Kerberos Authentication)
  * `_ldap` (Port 389 - LDAP Service)

### 2. Domain Expansion & Member Integration
* Joined Server Core (`STO-CORE`), member servers (`STO-SVR1`, `STO-SVR2`), and client nodes (`STO-CL1`) to `ETHA.com`.
* Promoted `STO-SVR1` as an additional Domain Controller for high availability.

### 3. Read-Only Domain Controller (RODC) & Password Replication
* Configured `STO-SVR2` as an RODC to secure branch site authentication.
* Established **Password Replication Policies (PRP)** to restrict administrative credentials from replicating to the RODC.
* Managed cached logon policies via Windows Registry (`CachedLogonsCount`).

### 4. Remote Administration & Maintenance
* Executed remote management via PowerShell Remoting (`Enter-PSSession`).
* Gracefully demoted and removed AD DS from `STO-SVR1` using PowerShell cmdlets (`Uninstall-ADDSDomainController`).
* Configured **Remote Server Administration Tools (RSAT)** on Windows 11 client nodes and deployed **Windows Admin Center** on `STO-SVR2`.
