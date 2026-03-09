# Newton Protocol Knowledge Base

Context for writing accurate, consistent landing page content about Newton Protocol.

## What Newton Protocol Is

Newton Protocol is a **decentralized policy evaluation and attestation layer** for blockchain transactions. It enables programmable, verifiable compliance at the transaction layer — before a transaction executes, it is evaluated against a policy, and the result is cryptographically attested by a decentralized operator network.

### Core Components

| Component | What It Does |
|-----------|-------------|
| **Policy Engine** | Evaluates transaction intents against Rego policies |
| **Prover AVS** | EigenLayer AVS — decentralized operator network that evaluates policies and produces BLS attestations |
| **Gateway** | JSON-RPC interface between clients and the operator network |
| **SDK** | TypeScript client library (viem extensions) for submitting evaluations |
| **CLI** | Command-line tool for deploying policies |
| **Privacy Layer** | HPKE encryption for evaluating policies on encrypted data |

### How It Works (Simplified)

1. Client submits a transaction intent + policy to the Newton Gateway
2. Gateway broadcasts to EigenLayer operators
3. Operators evaluate the policy (Rego) against the intent
4. Operators sign the result with BLS keys
5. Gateway aggregates signatures into a single attestation
6. Attestation is verified on-chain — transaction proceeds or is blocked

## Target Audience

The landing page targets **institutions, enterprises, fintech companies, financial entities, and web3-native partners** evaluating Newton Protocol. These are decision-makers who:

- Already understand blockchain/web3 fundamentals
- Need compliance, authorization, or risk management infrastructure
- Are evaluating build-vs-buy for policy enforcement
- Care about verifiability, auditability, and decentralization
- Include: fund managers, custodians, DAOs, regulated entities, payment networks, stablecoin issuers, AI agent platforms

The tone is **technical but accessible** — assume familiarity with web3 concepts (wallets, smart contracts, DeFi, EigenLayer) but explain Newton-specific concepts clearly.

## Value Propositions

### For Institutions

- **Compliance without centralization** — policies evaluated by a decentralized network, not a single vendor
- **Verifiable audit trail** — every decision produces an on-chain BLS attestation
- **Programmable rules** — write policies in Rego, deploy via CLI, update without contract redeployment
- **Privacy-preserving** — evaluate policies on encrypted data (HPKE privacy layer)

### For Developers

- **Familiar tooling** — TypeScript SDK, viem extensions, standard JSON-RPC
- **Composable** — works with any EVM chain, any smart contract
- **Decentralized** — no single point of failure, backed by EigenLayer economic security

## Use Cases

### Institutional DeFi
Compliance guardrails for fund managers: sanctions screening, exposure limits, approved protocol lists, multi-party authorization.

### Stablecoins & Payments
Transaction authorization for stablecoin issuers and payment networks: jurisdiction checks, volume limits, merchant verification, real-time compliance.

### AI Agent Security
Guardrails for autonomous AI agents: spending limits, contract allowlists, function restrictions, rate limiting, human-in-the-loop escalation.

### General Policy Enforcement
Any scenario requiring verifiable, decentralized rule enforcement: DAO governance, NFT marketplaces, cross-chain bridges, DePIN networks.

## Terminology

Use these terms consistently:

| Term | Usage |
|------|-------|
| Newton Protocol | Full name on first reference per page |
| Newton | Short name after first reference |
| Policy | A Rego rule set that evaluates transaction intents |
| Intent | A structured description of a desired transaction |
| Attestation | BLS signature proving a policy evaluation result |
| Prover AVS | The EigenLayer AVS that evaluates policies |
| Gateway | The JSON-RPC service between clients and operators |
| Operator | An EigenLayer node running the Newton Prover software |
| Policy evaluation | The act of checking an intent against a policy |

### Terms to Avoid

| Avoid | Use Instead |
|-------|-------------|
| "Blockchain firewall" | "Policy evaluation layer" |
| "Smart contract audit" | "Transaction policy enforcement" |
| "Compliance middleware" | "Decentralized policy evaluation" |
| "Oracle network" | "Prover AVS" or "operator network" |
| "Permission system" | "Policy engine" or "authorization layer" |

## Competitive Positioning

Newton is NOT:
- A smart contract auditing tool
- A centralized compliance vendor
- An oracle network (though it fetches external data for policy evaluation)
- A wallet or custody solution

Newton IS:
- A decentralized policy evaluation and attestation layer
- An EigenLayer AVS with cryptographic proof of evaluation
- A programmable compliance infrastructure
- A privacy-preserving policy engine (via HPKE encryption)

## Supported Chains

- Ethereum mainnet
- Sepolia (testnet)
- Base Sepolia (testnet)
- More chains planned

## Key Differentiators

1. **Decentralized** — no single compliance vendor; operators are economically bonded via EigenLayer
2. **Verifiable** — BLS attestations prove every policy decision on-chain
3. **Programmable** — policies written in Rego, deployed via CLI, updatable without contract changes
4. **Privacy-preserving** — HPKE encryption allows evaluation on encrypted data
5. **Composable** — works with any EVM smart contract, any DeFi protocol
