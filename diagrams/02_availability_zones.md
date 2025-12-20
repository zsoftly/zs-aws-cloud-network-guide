# Availability Zones Design

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { 'primaryColor': '#00198a', 'primaryTextColor': '#ffffff', 'primaryBorderColor': '#00198a', 'lineColor': '#00198a', 'secondaryColor': '#dae5fa', 'tertiaryColor': '#ffffff'}}}%%
flowchart LR
    subgraph VPC["VPC 10.0.8.0/16"]
        subgraph AZ1["AZ-1"]
            W1[Web<br>10.0.8.0/24]
            A1[App<br>10.0.10.0/24]
            D1[DB<br>10.0.12.0/24]
        end

        RT[Route Table<br>10.0.8.0/16 → local]

        subgraph AZ2["AZ-2"]
            W2[Web<br>10.0.9.0/24]
            A2[App<br>10.0.11.0/24]
            D2[DB<br>10.0.13.0/24]
        end
    end

    W1 <--> RT
    A1 <--> RT
    D1 <--> RT
    RT <--> W2
    RT <--> A2
    RT <--> D2

    style VPC fill:#ffffff,stroke:#00198a,color:#00198a
    style AZ1 fill:#dae5fa,stroke:#00198a,color:#00198a
    style AZ2 fill:#dae5fa,stroke:#00198a,color:#00198a
    style RT fill:#00198a,stroke:#00198a,color:#ffffff
    style W1 fill:#00198a,stroke:#00198a,color:#ffffff
    style W2 fill:#00198a,stroke:#00198a,color:#ffffff
    style A1 fill:#00198a,stroke:#00198a,color:#ffffff
    style A2 fill:#00198a,stroke:#00198a,color:#ffffff
    style D1 fill:#00198a,stroke:#00198a,color:#ffffff
    style D2 fill:#00198a,stroke:#00198a,color:#ffffff
```

## Route Table

All subnets share a route table with the VPC CIDR routed locally:

```
Destination     Target
10.0.8.0/16     local    (all subnets can reach each other)
0.0.0.0/0       igw-xxx  (web tier only)
```

## AZ Strategy

- Bidirectional routing between all subnets via local route
- Cross-AZ traffic stays within VPC (no extra hops)
- Each tier can communicate with its replica in the other AZ

Author: Ditah Kumbong
