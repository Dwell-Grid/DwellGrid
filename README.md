
# DwellGrid

### Connected Properties. Programmable Finance. Stronger Communities.

DwellGrid is a Reactive-powered property management and community finance platform that combines residential property management, programmable rental payments, cross-chain financial coordination, and tenant-led crowdfunding.

The platform uses smart contracts to enforce financial rules and Reactive Network to coordinate time-sensitive financial operations across supported blockchain networks.

> **Project status:** Under development  
> **Buildathon:** Reactive Omni Buildathon 2026  
> **Track:** Every Second Counts

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Our Solution](#our-solution)
- [Core Features](#core-features)
- [How Reactive Network Powers DwellGrid](#how-reactive-network-powers-dwellgrid)
- [System Architecture](#system-architecture)
- [Decentralization Model](#decentralization-model)
- [Supported Networks](#supported-networks)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Development Roadmap](#development-roadmap)
- [Security Considerations](#security-considerations)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

Traditional property management platforms primarily focus on property listings, tenant records, rental agreements, and payment tracking.

DwellGrid extends these capabilities by introducing programmable financial operations and community-driven financial coordination.

The platform is designed around three interconnected areas:

1. **Property Management:** Tools for landlords, property managers, and tenants to manage residential properties and rental activities.
2. **Programmable Financial Operations:** Smart-contract-enforced rental payments, payment distribution, and time-sensitive reward programmes.
3. **Community Crowdfunding:** A system that allows tenants to propose community projects and collectively fund shared improvements.

DwellGrid uses a Web2 application for user interaction and supporting services, while smart contracts and Reactive Network handle authoritative financial execution and cross-chain coordination.

## The Problem

Residential property management involves more than collecting rent.

Landlords and property managers need reliable ways to manage properties, tenants, rental information, and maintenance. Tenants need transparent payment processes and opportunities to participate in decisions affecting their residential communities.

However, conventional property management applications generally rely on centralized infrastructure for financial records and payment-related decisions.

There are also limitations when communities want to coordinate financial contributions across different blockchain networks.

DwellGrid addresses these challenges by introducing programmable financial infrastructure into property management.

## Our Solution

DwellGrid combines property management with decentralized financial automation.

Rather than treating blockchain as merely another payment option, DwellGrid uses smart contracts to enforce financial conditions and Reactive Network to coordinate operations triggered by events across supported networks.

The platform aims to give landlords and tenants greater financial transparency while allowing residential communities to organize and fund shared projects.

---

## Core Features

### 1. Property Management

A unified property management environment for landlords, property managers, and tenants.

Planned capabilities include:

- Property registration and management.
- Residential unit management.
- Tenant and landlord records.
- Rental information and agreement metadata.
- Vacancy management and property listings.
- Maintenance requests.
- Rental payment history.
- Property and tenant dashboards.

Property metadata and private tenant information are handled by the application infrastructure rather than unnecessarily exposing personal information on public blockchains.

### 2. Programmable Rental Payments

DwellGrid introduces configurable financial operations enforced through smart contracts.

Tenants can interact with supported payment contracts and authorize payment arrangements according to their chosen conditions.

Planned capabilities include:

- Scheduled rental payments.
- Conditional payment execution.
- Payment limits and authorization controls.
- Split payments.
- Transparent payment histories.

A payment contract cannot independently withdraw arbitrary funds from a tenant's wallet. Payments must be authorized through an appropriate token approval, permit, or smart-wallet execution mechanism.

Landlords and property managers can also configure payment distribution rules.

For example, a rental payment of 1,000 USDC could be distributed as follows:

| Recipient | Allocation |
|---|---:|
| Landlord | 90% |
| Property manager | 5% |
| Maintenance reserve | 5% |

The smart contracts enforce the configured allocation rules.

### 3. Early Rent Rewards — Reactive-Powered Workflow

Early Rent Rewards is DwellGrid's primary time-sensitive financial coordination feature for the Reactive Omni Buildathon.

The feature allows landlords or property managers to create a limited-time reward campaign for tenants who pay their rent within a specified period.

**Reward allocation model: First-come, first-served.**

The first qualifying tenants processed by the Reactive coordinator receive fixed reward allocations until all available reward slots are exhausted or the campaign expires.

#### Example

A landlord creates a campaign with:

- Reward per qualifying tenant: 10 USDC.
- Available reward slots: 10.
- Total reward budget: 100 USDC.
- Campaign duration: A predefined time window.
- Participating networks: Base Sepolia and Unichain Sepolia.

Tenants can make qualifying rent payments on either supported network.

The payment contracts emit events when eligible payments are received.

Reactive observes these events, evaluates campaign conditions, coordinates the shared reward allocation, and requests callbacks to the appropriate destination-chain contracts.

The destination contracts record the reward entitlements and enforce the corresponding claim conditions.

**Cross-chain allocation rule:** The reward order is determined by the order in which qualifying events are processed by the Reactive coordinator. This does not claim that independent blockchains provide a globally synchronized chronological ordering of payments.

Each chain maintains its own reward vault. Reactive coordinates reward allocation but does not bridge tokens between networks.

### 4. Tenant-Led Community Crowdfunding

DwellGrid allows any tenant to propose a community project and invite other tenants to contribute.

Examples include:

- Shared solar installations.
- Backup power infrastructure.
- Communal repairs.
- Shared residential amenities.
- Emergency community assistance.

#### How it works

1. A tenant creates a community campaign.
2. The campaign defines its funding target, deadline, and financial conditions.
3. Other tenants discover the campaign and contribute.
4. Tenants can contribute from supported blockchain networks.
5. Reactive observes contribution events and coordinates cross-chain campaign progress.
6. Smart contracts enforce campaign completion, release, and refund conditions.

#### Multi-chain contribution model

DwellGrid uses a local-vault approach.

Contributions remain on the blockchain where they were deposited. They are not automatically bridged or consolidated into a single chain.

For example:

- A tenant contributes 2,000 USDC on Base.
- Another contributes 3,000 USDC on Unichain.
- Both contributions belong to the same logical campaign.

Reactive coordinates the cross-chain campaign state while each chain's vault independently maintains custody and accounting of its contributions.

Campaign release and refund rules must be enforced by the relevant vault contracts.

---

## How Reactive Network Powers DwellGrid

Reactive Network is a core component of DwellGrid's cross-chain coordination architecture.

DwellGrid uses Reactive contracts to subscribe to events emitted by approved contracts on participating networks.

When a subscribed event occurs, the Reactive contract evaluates the event and can request a callback to a destination-chain contract.

### Early Rent Rewards execution flow

1. A landlord creates and funds an Early Rent Rewards campaign.
2. A tenant makes a qualifying rental payment on Base Sepolia or Unichain Sepolia.
3. The payment contract emits a `RentPaid` event.
4. The Reactive contract deployed on Reactive Lasna receives the subscribed event.
5. The Reactive coordinator validates the source and evaluates the campaign conditions.
6. If the payment qualifies and reward slots remain, the coordinator allocates a reward slot.
7. Reactive requests a callback to the relevant destination-chain contract.
8. The callback receiver authenticates the request and records the tenant's reward entitlement.
9. The tenant can claim the reward according to the destination vault's rules.

When the campaign expires or all reward slots are allocated, the coordinator can request callbacks to close the campaign on participating chains.

### Why Reactive?

Base and Unichain maintain independent blockchain states.

A contract deployed on one network cannot directly read another network's contract storage.

Reactive provides the event-driven coordination layer needed to observe events across these networks and trigger destination-chain execution.

**Reactive is not a bridge.** It coordinates event processing and callbacks. It does not automatically transfer assets between participating networks.

### Callback reliability

Because callbacks execute as separate transactions, DwellGrid must account for:

- Callback authentication.
- Callback gas funding.
- Duplicate callback prevention.
- Failed callback recovery.
- State reconciliation.
- Campaign expiration and closure.
- Reward-vault liquidity.

The destination contract must not assume that a callback has succeeded merely because a request was submitted.

---

## System Architecture

DwellGrid consists of three main architectural layers.

### 1. Application Layer

The Web2 application provides the user-facing experience.

Responsibilities include:

- Tenant and landlord dashboards.
- Property management interfaces.
- Campaign discovery.
- Wallet connection.
- Transaction submission.
- Payment and campaign history.
- Notifications.
- Blockchain event indexing.

### 2. Smart Contract Layer

Smart contracts are responsible for authoritative financial state and execution.

Expected contract components include:

| Contract | Responsibility |
|---|---|
| RentPayment | Receives and accounts for authorized rental payments |
| PaymentDistributor | Enforces payment distribution rules |
| RewardVault | Holds and pays reward funds on its local chain |
| RewardCampaign | Maintains campaign parameters and reward eligibility |
| CommunityCampaign | Manages crowdfunding campaigns and contributions |
| Governance | Enforces approved protocol and community decisions |
| CallbackReceiver | Authenticates and processes Reactive callbacks |

These are proposed logical components. They may be combined or separated during implementation based on the final contract design.

### 3. Reactive Coordination Layer

The Reactive contract deployed on Reactive Lasna is responsible for:

- Subscribing to approved payment and contribution events.
- Validating event sources.
- Coordinating cross-chain campaign state.
- Allocating limited Early Rent Rewards slots.
- Evaluating campaign conditions.
- Requesting callbacks to destination-chain contracts.

### Conceptual architecture

```text
                  DWELLGRID APPLICATION
              Tenant Dashboard | Landlord Dashboard
                            |
                    Wallet Interaction
                            |
             +--------------+--------------+
             |                             |
        BASE SEPOLIA                 UNICHAIN SEPOLIA
             |                             |
       Smart Contracts                Smart Contracts
       - Rent Payments                - Rent Payments
       - Reward Vault                 - Reward Vault
       - Campaign Vault               - Campaign Vault
       - Callback Receiver            - Callback Receiver
             |                             |
             +-------------+---------------+
                           |
                     Events observed
                           |
                    REACTIVE LASNA
                           |
                DwellGrid Reactive Contract
                           |
                 Cross-chain evaluation
                           |
                  Callback requests
                           |
             +-------------+---------------+
             |                             |
        BASE CALLBACK                UNICHAIN CALLBACK
             |                             |
       Update local state          Update local state
       Enforce local rules          Enforce local rules
```

---

## Decentralization Model

DwellGrid follows an on-chain financial authority model.

The application backend is not the authority over users' funds or financial entitlements.

### On-chain responsibilities

- Rental payment execution and accounting.
- Reward campaign parameters and allocations.
- Reward entitlement enforcement.
- Crowdfunding contribution records.
- Campaign funding conditions.
- Fund release and refund rules.
- Community voting.
- Protocol governance permissions.

### Off-chain responsibilities

- User profiles.
- Property descriptions and images.
- Private rental information.
- Search and discovery.
- Notifications.
- Transaction indexing.
- Application analytics.

The backend reads blockchain events and contract state to present information to users.

If the backend becomes unavailable, deployed contracts must continue enforcing their rules. Users should retain a way to interact with the contracts through another compatible interface.

Private personal information should not be published on public blockchains.

### On-chain governance

Governance is intended to provide transparent control over important protocol and community decisions.

Potential governance responsibilities include:

- Approving supported payment tokens.
- Managing protocol configuration.
- Approving community crowdfunding expenditures.
- Enforcing community voting outcomes.
- Controlling authorized protocol upgrades.

Governance permissions must be explicitly defined and limited. No unrestricted administrator permission should allow arbitrary withdrawal of tenant funds.

---

## Supported Networks

The initial buildathon implementation targets:

| Network | Role |
|---|---|
| Base Sepolia | Origin and destination contracts |
| Unichain Sepolia | Origin and destination contracts |
| Reactive Lasna | Reactive coordination |

The initial implementation focuses on the networks relevant to the Every Second Counts track.

Additional networks may be considered in future versions after their Reactive support and settlement requirements have been verified.

---

## Technology Stack

The following is the intended technology direction. Specific framework and library choices should be finalized by the implementation team.

| Component | Technology |
|---|---|
| Smart contracts | Solidity |
| Contract development | Foundry or Hardhat |
| Reactive coordination | Reactive Network Omni |
| Reactive test network | Lasna |
| EVM networks | Base Sepolia and Unichain Sepolia |
| Frontend | Web application |
| Backend | API and indexing services |
| Database | PostgreSQL or another suitable database |
| Wallet integration | EVM-compatible wallet integration |
| Token standard | ERC-20-compatible test assets |

The application stack may evolve as the implementation progresses.

---

## Project Structure

A proposed monorepo structure:

```text
dwellgrid/
│
├── contracts/
│   ├── src/
│   │   ├── RentPayment.sol
│   │   ├── PaymentDistributor.sol
│   │   ├── RewardVault.sol
│   │   ├── RewardCampaign.sol
│   │   ├── CommunityCampaign.sol
│   │   ├── Governance.sol
│   │   └── CallbackReceiver.sol
│   │
│   ├── test/
│   └── script/
│
├── reactive/
│   ├── src/
│   │   └── DwellGridReactive.sol
│   ├── test/
│   └── deployment/
│
├── frontend/
│
├── backend/
│
├── docs/
│   ├── ARCHITECTURE.md
│   ├── CONTRACTS.md
│   ├── REACTIVE_WORKFLOW.md
│   ├── SECURITY.md
│   └── API.md
│
├── .env.example
├── .gitignore
└── README.md
```

This is a proposed layout, not a claim that every directory or component has already been implemented.

---

## Development Roadmap

### Phase 1 — Architecture and Contract Design

- Define contract responsibilities.
- Define financial authorization rules.
- Design payment and reward events.
- Define campaign identifiers and cross-chain state.
- Establish callback authentication requirements.

### Phase 2 — Smart Contract Development

- Implement rent payment contracts.
- Implement reward campaign and vault contracts.
- Implement crowdfunding contracts.
- Implement payment distribution.
- Implement campaign access and governance rules.
- Write unit and integration tests.

### Phase 3 — Reactive Integration

- Deploy the Reactive coordinator on Lasna.
- Subscribe to approved Base Sepolia payment events.
- Subscribe to approved Unichain Sepolia payment events.
- Implement event validation and reward allocation.
- Implement callback requests.
- Deploy destination callback receivers.
- Test duplicate and failed callback handling.

### Phase 4 — End-to-End Testing

- Test qualifying rent payments on both networks.
- Verify first-come, first-served reward allocation.
- Test reward exhaustion and campaign expiration.
- Verify callback execution on destination chains.
- Measure origin-event-to-callback latency.
- Test cross-chain crowdfunding accounting.

### Phase 5 — Application Development

- Build tenant dashboards.
- Build landlord dashboards.
- Integrate wallet connections.
- Implement property management interfaces.
- Integrate campaign discovery and contribution interfaces.
- Add blockchain indexing and notifications.

### Phase 6 — Demonstration and Submission

- Deploy contracts to the required test networks.
- Document contract addresses.
- Capture transaction hashes for the complete Reactive workflow.
- Prepare a demonstration video under three minutes.
- Publish technical documentation.
- Verify that the public repository contains the required implementation and deployment information.

---

## Security Considerations

Security is a core requirement because DwellGrid handles financial assets.

The implementation must address:

### Access control

- Only authorized parties can perform restricted operations.
- Callback receivers authenticate the configured Reactive callback proxy.
- Governance permissions must be explicit and limited.

### Campaign integrity

- Campaign parameters must be validated.
- Campaign identifiers must be consistently mapped across networks.
- Only approved source contracts and chains can contribute to authoritative cross-chain state.

### Replay and duplicate protection

- Callback operations must be idempotent.
- The same payment event must not allocate multiple rewards.
- Event identifiers should include sufficient source-chain identity, such as chain ID, transaction hash and log index.

### Financial safety

- Reward vaults must have sufficient funds.
- Payment and distribution calculations must handle token decimals correctly.
- Campaigns must enforce deadlines and contribution limits.
- Refund and release rules must not depend on an unrestricted backend administrator.

### Reactive reliability

- Callback failures must be recoverable.
- Callback gas funding must be monitored.
- Local vault state must remain authoritative for local funds.
- Cross-chain progress must distinguish requested callbacks from successfully executed callbacks.

### Privacy

- Sensitive tenant and landlord information must remain off-chain.
- Personal information should never be embedded in public event data unnecessarily.

---

## Contributing

DwellGrid is being developed as an open-source project.

Contributions are welcome across:

- Smart contract development.
- Reactive Network integration.
- Backend development.
- Frontend development.
- Testing and security.
- Technical documentation.

### Contribution workflow

1. Fork or clone the repository.
2. Create a feature branch.
3. Implement the assigned feature.
4. Add or update relevant tests.
5. Submit a pull request describing the changes.

Contributors should coordinate contract interfaces and event definitions before implementing dependent components.

---

## License

The project's open-source license should be specified before public release.

---

## Acknowledgements

Built for the **Reactive Omni Buildathon 2026**.

Powered by **Reactive Network**.

**DwellGrid — Connected Properties. Programmable Finance. Stronger Communities.**
