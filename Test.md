# Identity & Access Protocols Explained

## 📌 Core Protocols Overview
| Protocol  | Primary Purpose                     | Key Component         | Modern Use Cases               |
|-----------|-------------------------------------|-----------------------|-------------------------------|
| **OAuth 2.0** | Delegated API access                | Access Tokens         | "Share data" buttons          |
| **SAML**     | Enterprise SSO                      | XML Assertions        | Corporate logins              |
| **OIDC**     | Federated authentication            | ID Tokens (JWT)       | "Sign in with Google"         |

---

## 🔄 Protocol Flows (Mermaid Diagrams)

### 1. OAuth 2.0 Authorization Code Flow
```mermaid
sequenceDiagram
    actor User
    participant App
    participant AuthServer
    participant ResourceServer
    User->>App: "Connect to Google"
    App->>User: Redirect to AuthServer
    User->>AuthServer: Login + Consent
    AuthServer->>User: Returns code
    User->>App: Passes code
    App->>AuthServer: code + secret
    AuthServer->>App: access_token
    App->>ResourceServer: API request
    ResourceServer-->>App: Protected data


