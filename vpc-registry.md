# VPC CIDR Registry

A central registry for VPC CIDR allocations to prevent overlaps when connecting
to Transit Gateway.

## How to Use

1. Check the allocation table below for available CIDR blocks
2. Create a PR adding your VPC(s) to the appropriate region table
3. Get approval from the platform team
4. Once merged, the CIDR block is reserved for your team

## CIDR Allocation Scheme

```
Region Blocks (/12):
├── Canada:     10.0.0.0/12   (10.0.0.0 - 10.15.255.255)   [Primary]
├── USA:        10.16.0.0/12  (10.16.0.0 - 10.31.255.255)
├── Frankfurt:  10.32.0.0/12  (10.32.0.0 - 10.47.255.255)
└── Singapore:  10.48.0.0/12  (10.48.0.0 - 10.63.255.255)

VPC Size: /16 (65,536 IPs per VPC)
```

---

## Canada Region (10.0.0.0/12) - Primary

| CIDR Block   | Environment | Product/Team  | Owner         | Status    |
| ------------ | ----------- | ------------- | ------------- | --------- |
| 10.0.0.0/16  | dev         | platform-core | platform-team | allocated |
| 10.0.1.0/16  | qa          | platform-core | platform-team | allocated |
| 10.0.2.0/16  | stg         | platform-core | platform-team | allocated |
| 10.0.3.0/16  | prd         | platform-core | platform-team | allocated |
| 10.0.4.0/16  | dr          | platform-core | platform-team | allocated |
| 10.0.5.0/16  | -           | -             | -             | available |
| 10.0.6.0/16  | -           | -             | -             | available |
| 10.0.7.0/16  | -           | -             | -             | available |
| 10.0.8.0/16  | -           | -             | -             | available |
| 10.0.9.0/16  | -           | -             | -             | available |
| 10.0.10.0/16 | -           | -             | -             | available |
| 10.0.11.0/16 | -           | -             | -             | available |
| 10.0.12.0/16 | -           | -             | -             | available |
| 10.0.13.0/16 | -           | -             | -             | available |
| 10.0.14.0/16 | -           | -             | -             | available |
| 10.0.15.0/16 | -           | -             | -             | available |

## USA Region (10.16.0.0/12)

| CIDR Block   | Environment | Product/Team | Owner | Status    |
| ------------ | ----------- | ------------ | ----- | --------- |
| 10.16.0.0/16 | -           | -            | -     | available |
| 10.16.1.0/16 | -           | -            | -     | available |
| 10.16.2.0/16 | -           | -            | -     | available |
| 10.16.3.0/16 | -           | -            | -     | available |
| 10.16.4.0/16 | -           | -            | -     | available |
| 10.16.5.0/16 | -           | -            | -     | available |
| 10.16.6.0/16 | -           | -            | -     | available |
| 10.16.7.0/16 | -           | -            | -     | available |

## Frankfurt Region (10.32.0.0/12)

| CIDR Block   | Environment | Product/Team | Owner | Status    |
| ------------ | ----------- | ------------ | ----- | --------- |
| 10.32.0.0/16 | -           | -            | -     | available |
| 10.32.1.0/16 | -           | -            | -     | available |
| 10.32.2.0/16 | -           | -            | -     | available |
| 10.32.3.0/16 | -           | -            | -     | available |
| 10.32.4.0/16 | -           | -            | -     | available |
| 10.32.5.0/16 | -           | -            | -     | available |
| 10.32.6.0/16 | -           | -            | -     | available |
| 10.32.7.0/16 | -           | -            | -     | available |

## Singapore Region (10.48.0.0/12)

| CIDR Block   | Environment | Product/Team | Owner | Status    |
| ------------ | ----------- | ------------ | ----- | --------- |
| 10.48.0.0/16 | -           | -            | -     | available |
| 10.48.1.0/16 | -           | -            | -     | available |
| 10.48.2.0/16 | -           | -            | -     | available |
| 10.48.3.0/16 | -           | -            | -     | available |
| 10.48.4.0/16 | -           | -            | -     | available |
| 10.48.5.0/16 | -           | -            | -     | available |
| 10.48.6.0/16 | -           | -            | -     | available |
| 10.48.7.0/16 | -           | -            | -     | available |

---

## PR Template

When requesting VPC allocations, use this format in your PR description:

```
## VPC Allocation Request

**Product/Team:** [your-product-name]
**Owner:** [team-slack-channel or email]
**Region:** [Canada | USA | Frankfurt | Singapore]

### Requested VPCs:

| Environment | Requested CIDR |
|-------------|----------------|
| dev | 10.x.x.0/16 |
| qa | 10.x.x.0/16 |
| stg | 10.x.x.0/16 |
| prd | 10.x.x.0/16 |
| dr | 10.x.x.0/16 |

### Justification:
[Brief description of the product and why these VPCs are needed]
```

---

## Example: Requesting 5 VPCs for a New Product

Team "payments" wants 5 VPCs in Singapore for their new product:

**Before PR:**

```
| 10.0.5.0/16 | - | - | - | available |
| 10.0.6.0/16 | - | - | - | available |
| 10.0.7.0/16 | - | - | - | available |
| 10.0.8.0/16 | - | - | - | available |
| 10.0.9.0/16 | - | - | - | available |
```

**After PR:**

```
| 10.0.5.0/16 | dev | payments | @payments-team | allocated |
| 10.0.6.0/16 | qa | payments | @payments-team | allocated |
| 10.0.7.0/16 | stg | payments | @payments-team | allocated |
| 10.0.8.0/16 | prd | payments | @payments-team | allocated |
| 10.0.9.0/16 | dr | payments | @payments-team | allocated |
```

---

## Rules

1. **No overlapping CIDRs** - Transit Gateway requires unique ranges
2. **Use /16 blocks** - Provides 65,536 IPs per VPC, enough for growth
3. **Sequential allocation** - Pick the next available block to keep things tidy
4. **One PR per product** - Request all environments together
5. **Update status** - Mark as `allocated` once approved

## Questions?

Contact the platform team via #platform-support.
