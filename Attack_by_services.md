# Azure Attack Techniques by Service (MITRE ATT\&CK Mapped)

This document provides a technical breakdown of Azure attack techniques categorized by service, with MITRE ATT\&CK mapping, along with function, attack vector, defense strategies, and detection mechanisms.

---

## 🔐 Azure Active Directory (AAD)

**Function**: Identity provider for authentication and authorization.

| Technique                     | MITRE     | 🎯 Attack                                                     | 🛡 Defense                                                                | 🕵️ Detection                                                                             |
| ----------------------------- | --------- | ------------------------------------------------------------- | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------- |
| Password Spray                | T1110.003 | Brute-force against multiple accounts using common passwords. | Enforce Smart Lockout, Conditional Access + MFA, block legacy auth.       | Sign-in logs: `Status.failureReason`, `AuthenticationDetails`, correlate `clientAppUsed`. |
| Consent Grant Abuse           | T1528     | Malicious OAuth apps gain offline\_access/token access.       | Admin consent workflow, restrict user consent settings.                   | AAD audit logs: `ConsentToApp`, unusual `ResourceId`, high `userCount`.                   |
| Token Replay (Pass-the-Token) | T1550.003 | Use stolen refresh/access tokens via API.                     | Refresh token lifetime policy, Conditional Access on token claims.        | Azure AD sign-ins with impossible travel and no credential entry.                         |
| Guest User Enumeration        | T1078.004 | Leverage B2B invite to gain lateral access.                   | Review access reviews, restrict invites, Conditional Access by user type. | AAD logs: external domains in `UserPrincipalName`, `InvitationCreated` events.            |
| Directory Recon via Graph API | T1087.003 | Enumerate users, groups, roles.                               | Scope app permissions tightly, disable unnecessary Graph access.          | Audit logs: `ListUser`, `ListDirectoryRole`, excessive use per IP or app.                 |

---

## 💻 Azure Compute (VMs, App Services, Azure Functions)

**Function**: Hosts workloads – VMs, web apps, background tasks.

| Technique                           | MITRE     | 🎯 Attack                                                | 🛡 Defense                                                               | 🕵️ Detection                                                                                           |
| ----------------------------------- | --------- | -------------------------------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| Custom Script Extension Abuse       | T1059     | RCE by deploying custom scripts via extensions.          | Lock down extension write access with RBAC, alert on extension installs. | Azure Activity logs: `Write` on `Microsoft.Compute/virtualMachines/extensions`, analyze script content. |
| Web Shell Deployment in App Service | T1505.003 | Upload of `.aspx`, `.php` shells via Kudu or FTP.        | Restrict publishing credentials, WAF, deploy from trusted repo.          | App Service logs: frequent file uploads, `w3wp` spawning PowerShell, etc.                               |
| Code Injection (Function/Apps)      | T1055     | Inject malicious code into Function/App Service runtime. | CI/CD validation, restrict write permissions.                            | App logs: suspicious modules (e.g., `Invoke-Expression`, `os.system`).                                  |
| Reverse Shell via App Service       | T1059     | App executes outbound shell to C2 server.                | Egress NSGs, use Defender for App Services.                              | Monitor outbound traffic from App Service IPs, unexpected ports (e.g., 443 to IPs not in CDN).          |
| Startup Task Backdoor               | T1547.001 | Modify init/startup tasks to persist code.               | Restrict startup task config to trusted users, audit changes.            | Activity logs: `Microsoft.Web/sites/config/write`, unexpected `startupCommand`.                         |

---

## 📁 Azure Storage (Blob, Queue, File, Table)

**Function**: Object and file storage platform.

| Technique                         | MITRE     | 🎯 Attack                                                     | 🛡 Defense                                                       | 🕵️ Detection                                                                      |
| --------------------------------- | --------- | ------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Public Blob Access                | T1530     | Access data in publicly open containers.                      | Disable anonymous access globally, enforce private access tier.  | Storage logs: `GetBlob` requests from unauth IPs or anonymous auth.                |
| SAS Token Leakage                 | T1557.002 | Abuse shared token to access files/data.                      | Generate SAS with expiry, IP range, minimum rights.              | Storage logs: SAS key operations from suspicious geos/IPs.                         |
| Credential in Code (Storage Keys) | T1552.005 | Hardcoded connection strings used for unauthorized access.    | Use Azure Key Vault, enforce code scanning with GitHub/Defender. | GitHub Advanced Security alerts, Defender for DevOps alerts.                       |
| SMB Sniffing (Azure Files)        | T1040     | Intercept credentials on SMBv3 over untrusted networks.       | Force encryption, disable SMB1, use Private Endpoint.            | NSG flow logs: unusual SMB ports exposed (445), connections from unknown networks. |
| Data Exfiltration (Blob Upload)   | T1041     | Attacker uploads data to external blob from compromised host. | Egress filtering, Azure Defender for Storage.                    | Audit logs: `PutBlob` to external regions or accounts.                             |

---

## 🌐 Azure Networking (VNets, NSGs, Private Link)

**Function**: Cloud networking for isolation, routing, and segmentation.

| Technique                  | MITRE     | 🎯 Attack                                                        | 🛡 Defense                                                    | 🕵️ Detection                                                                     |
| -------------------------- | --------- | ---------------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| NSG Misconfig (Open Ports) | T1133     | Exposed RDP/SSH enables direct access.                           | Baseline rules: deny all inbound, use JIT + Bastion.          | NSG Flow Logs: traffic on 3389/22 from public IPs.                                |
| Private Endpoint Abuses    | T1041     | Data exfiltration over private link to malicious storage.        | Approve only known resource group/private link IDs.           | Diagnostic logs: unusual outbound traffic via private link.                       |
| Peered VNet Pivoting       | T1021     | Use VNet peering to move laterally.                              | Segregate VNets with NSGs + firewalls, deny all by default.   | Flow logs: traffic between peer VNet IP ranges, scan behavior.                    |
| Custom Route Hijack        | T1562.004 | Manipulate UDR to divert traffic through attacker-controlled VM. | Lock down UDR write permissions, monitor route table changes. | Activity logs: `Microsoft.Network/routeTables/write`                              |
| DNS Tunneling              | T1071.004 | Exfil via encoded data in DNS queries.                           | Use Azure Firewall DNS logging, deny external DNS resolvers.  | DNS logs: high-frequency or long subdomain queries to attacker-controlled domain. |

---

## 🧠 Azure Security Services (Defender, Sentinel, Monitor)

**Function**: Security monitoring, detection, SIEM/SOAR, and compliance.

| Technique                       | MITRE     | 🎯 Attack                                                         | 🛡 Defense                                                       | 🕵️ Detection                                                                      |
| ------------------------------- | --------- | ----------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| Defender Plan Disabled          | T1562.001 | Disable Defender for Storage/VMs to blind detection.              | Use Azure Policy to enforce enablement, alert on config changes. | Activity logs: `Microsoft.Security/pricings/write` with `enabled=false`.           |
| Sentinel Rule Tampering         | T1562.006 | Delete or disable detection rules.                                | RBAC separation (analyst vs admin), change auditing.             | Sentinel workspace logs: `Update` or `Delete` on `Microsoft.OperationalInsights`.  |
| Alert Suppression via Logic App | T1562.006 | Modify alert action rules to suppress notifications.              | Monitor Logic App usage, restrict to security team.              | Activity logs: `Microsoft.Logic/workflows/*`, sudden drop in incident volume.      |
| Diagnostic Settings Deletion    | T1562.008 | Prevent log flow to Sentinel/Log Analytics.                       | Alert on changes to diagnostic settings.                         | Audit logs: `Delete` on `diagnosticSettings`, especially for NSGs, Key Vault, etc. |
| Defender for Key Vault Bypass   | T1562.001 | Abuse Vault without triggering alerts by using service principal. | Key Vault firewall + access policies; Defender alerts review.    | Monitor `SecretList`, `GetSecret` with non-human identities.                       |

---

> **Note**: Add KQL queries, SOAR playbooks, or JSON export as needed based on operational use.
