
# 🕵️ Azure Tenant Discovery and Recon – Structured Overview

## 1. 🎯 Objective

Perform external enumeration to discover Azure tenant information for a target organization using **only a domain name or email address**.

> **Example Target**: `retailcorp` or `retailcorp.com`

---

## 2. 📤 Information That Can Be Extracted

| Info Extracted                                 | Method/Tool                             |
|------------------------------------------------|------------------------------------------|
| Tenant in use, federation status               | login.microsoftonline.com GET request    |
| Tenant name, ID, domains                       | OpenID config & AADInternals             |
| Authentication type                            | AADInternals or realm query              |
| Azure services in use                          | MicroBurst subdomain enumeration         |
| Email ID validation                            | Microsoft CredentialType API             |
| Directory name, branding                       | AADInternals                              |

---

## 3. 🌐 Passive HTTP Requests

### 🔍 Check Federation and Tenant Name

```bash
GET https://login.microsoftonline.com/getuserrealm.srf?login=USERNAME@DOMAIN&xml=1
```

- Input: `root@retailcorp.com`
- Output: Federation or managed status, domain name, and brand.

### 🔐 Get Tenant ID

```bash
GET https://login.microsoftonline.com/[DOMAIN]/.well-known/openid-configuration
```

- Input: `retailcorp.com`
- Output: Extract `"issuer"` field to get Tenant ID.

### ✅ Validate Email ID

```bash
POST https://login.microsoftonline.com/common/GetCredentialType/
```

- Input JSON:
```json
{
  "Username": "targetuser@retailcorp.com"
}
```

- Output: Exists, federated, or invalid user.

---

## 4. 🛠️ AADInternals Tool

- Repository: [AADInternals](https://github.com/Gerenios/AADInternals)
- PowerShell Import:

```powershell
Import-Module "C:\AZAD\Tools\AADInternals\AADInternals.psd1" -Verbose
```

### Key Commands

| Task                                      | Command Example                                                              |
|-------------------------------------------|------------------------------------------------------------------------------|
| Get login & branding info                 | `Get-AADIntLoginInformation -UserName root@retailcorp.onmicrosoft.com`      |
| Get tenant ID                             | `Get-AADIntTenantID -Domain retailcorp.onmicrosoft.com`                     |
| Get domain list                           | `Get-AADIntTenantDomains -Domain retailcorp.onmicrosoft.com`               |
| Full outsider recon                       | `Invoke-AADIntReconAsOutsider -DomainName retailcorp.onmicrosoft.com`       |

---

## 5. 🧰 Azure Services Enumeration with MicroBurst

- Repository: [MicroBurst](https://github.com/NetSPI/MicroBurst)
- PowerShell Import:

```powershell
Import-Module "C:\AZAD\Tools\MicroBurst\MicroBurst.psm1" -Verbose
```

### Subdomain Enumeration

```powershell
Invoke-EnumerateAzureSubDomains -Base retailcorp -Verbose
```

- Looks for common Azure service subdomains:
  - `retailcorp.sharepoint.com`
  - `retailcorp.blob.core.windows.net`
  - `retailcorp.onmicrosoft.com`, etc.

---

## 6. ✅ Summary Table

| Task                        | Tool/API                                 | Output                                 |
|-----------------------------|-------------------------------------------|----------------------------------------|
| Identify Federation         | login.microsoftonline.com                | Federation vs. Managed authentication |
| Extract Tenant ID           | OpenID configuration endpoint             | GUID tenant ID                         |
| Validate User               | GetCredentialType API                     | Valid/Invalid User                     |
| All Tenant Recon            | AADInternals                              | Domains, branding, auth type, etc.     |
| Azure Services Enumeration  | MicroBurst                                | Subdomains for Azure services          |

