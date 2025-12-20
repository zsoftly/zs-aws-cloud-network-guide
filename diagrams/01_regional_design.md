# Regional Network Design

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#00198a', 'primaryTextColor': '#ffffff', 'primaryBorderColor': '#00198a', 'lineColor': '#00198a', 'secondaryColor': '#dae5fa', 'tertiaryColor': '#ffffff'}}}%%
flowchart TB
    INET[Internet]
    IGW[Internet Gateway]
    NAT[NAT Gateway]
    TGW[Transit Gateway]
    OTHER[Other VPCs / Regions]

    subgraph VPC["Production VPC (10.0.8.0/16)"]
        subgraph PUB["Public Subnets"]
            WEB[Web Tier<br>10.0.8-9.0/24]
        end
        subgraph PRIV["Private Subnets"]
            APP[App Tier<br>10.0.10-11.0/24]
            DB[DB Tier<br>10.0.12-13.0/24]
        end
    end

    INET <--> IGW
    IGW <--> WEB
    WEB --> NAT
    NAT --> INET
    APP --> NAT
    DB --> NAT
    APP <--> TGW
    DB <--> TGW
    TGW <--> OTHER

    style INET fill:#ffffff,stroke:#00198a,color:#00198a
    style VPC fill:#ffffff,stroke:#00198a,color:#00198a
    style PUB fill:#dae5fa,stroke:#00198a,color:#00198a
    style PRIV fill:#dae5fa,stroke:#00198a,color:#00198a
    style WEB fill:#00198a,stroke:#00198a,color:#ffffff
    style APP fill:#00198a,stroke:#00198a,color:#ffffff
    style DB fill:#00198a,stroke:#00198a,color:#ffffff
    style IGW fill:#00198a,stroke:#00198a,color:#ffffff
    style NAT fill:#00198a,stroke:#00198a,color:#ffffff
    style TGW fill:#00198a,stroke:#00198a,color:#ffffff
    style OTHER fill:#dae5fa,stroke:#00198a,color:#00198a
```

## Traffic Flow

| Traffic Type                | Path                      |
| --------------------------- | ------------------------- |
| Inbound internet            | Internet → IGW → Web Tier |
| Outbound internet (public)  | Web Tier → IGW → Internet |
| Outbound internet (private) | App/DB → NAT → Internet   |
| Cross-VPC / Cross-region    | App/DB → TGW → Other VPCs |

## Route Tables

**Public Subnet (Web Tier):**

```
0.0.0.0/0       → igw-xxx     (internet via IGW)
10.0.8.0/16     → local
10.0.0.0/12     → tgw-xxx     (other VPCs via TGW)
```

**Private Subnets (App/DB Tier):**

```
0.0.0.0/0       → nat-xxx     (internet via NAT)
10.0.8.0/16     → local
10.0.0.0/12     → tgw-xxx     (other VPCs via TGW)
```

Author: Ditah Kumbong
