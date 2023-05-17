# ✨ So you want to sponsor an audit

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _audit_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted (shared with you after the contest) 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Audit setup

## 🐺 C4: Set up repos
- [ ] Update pot sizes
- [ ] Add the information from the scoping form to the "Scoping Details" section at the bottom of this readme.
- [ ] Delete this checklist.


# Base audit details
- Total Prize Pool: XXX XXX USDC (Notion: Total award pool)
  - HM awards: XXX XXX USDC (Notion: HM (main) pool)
  - QA report awards: XXX XXX USDC (Notion: QA pool)
  - Gas report awards: XXX XXX USDC (Notion: Gas pool)
  - Judge awards: XXX XXX USDC (Notion: Judge Fee)
  - Lookout awards: XXX XXX USDC (Notion: Sum of Pre-sort fee + Pre-sort early bonus)
  - Scout awards: $500 USDC (Notion: Scout fee - but usually $500 USDC)
  - (this line can be removed if there is no mitigation) Mitigation review contest: XXX XXX USDC (*Opportunity goes to top 3 certified wardens based on placement in this contest.*)
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-05-base-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts May 19, 2023 20:00 UTC
- Ends Jun 02, 2023 20:00 UTC

## Automated Findings / Publicly Known Issues

Automated findings output for the contest can be found [here](add link to report) within 24 hours of contest opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

[ ⭐️ SPONSORS ADD INFO HERE ]

# Overview

Base is a secure, low-cost, developer-friendly Ethereum L2 built to bring the next billion users to web3.
It is built on the MIT-licensed OP Stack, in collaboration with Optimism. Coinbase is joining as the second Core Dev team working on the OP Stack to ensure it’s a public good available to everyone.



# Scope
We are basing this contest on [`OP-monorepo`](https://github.com/ethereum-optimism/optimism/commit/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59)  and [`op-geth`](https://github.com/ethereum-optimism/op-geth/commit/3fa9e812447af947c0208838453268a8ea33444b) 
These commit hashes will be considered as a code freeze for the purposes of this contest.

You can see these contracts deployed on testnet here : https://docs.base.org/network-information

## Contracts Overview

### Contracts deployed to L1


| Name                                                                                     | Proxy Type                                                              | Description                                                                                         |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| [`L1CrossDomainMessenger`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/messengers.md)                                    | [`ResolvedDelegateProxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/legacy/ResolvedDelegateProxy.sol) | High-level interface for sending messages to and receiving messages from Optimism                   |
| [`L1StandardBridge`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/bridges.md)                                             | [`L1ChugSplashProxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/legacy/L1ChugSplashProxy.sol)         | Standardized system for transfering ERC20 tokens to/from Optimism                                   |
| [`L2OutputOracle`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/proposals.md#l2-output-oracle-smart-contract)             | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Stores commitments to the state of Optimism which can be used by contracts on L1 to access L2 state |
| [`OptimismPortal`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/deposits.md#deposit-contract)                             | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Low-level message passing interface                                                                 |
| [`OptimismMintableERC20Factory`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#optimismmintableerc20factory) | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Deploys standard `OptimismMintableERC20` tokens that are compatible with either `StandardBridge`    |
| [`ProxyAdmin`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/TODO)                                                         | -                                                                       | Contract that can upgrade L1 contracts                                                              |

### Contracts deployed to L2

| Name                                                                                     | Proxy Type                                 | Description                                                                                      |
| ---------------------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| [`GasPriceOracle`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#ovm_gaspriceoracle)                         | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Stores L2 gas price configuration values                                                         |
| [`L1Block`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#l1block)                                           | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Stores L1 block context information (e.g., latest known L1 block hash)                           |
| [`L2CrossDomainMessenger`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#l2crossdomainmessenger)             | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | High-level interface for sending messages to and receiving messages from L1                      |
| [`L2StandardBridge`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#l2standardbridge)                         | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Standardized system for transferring ERC20 tokens to/from L1                                     |
| [`L2ToL1MessagePasser`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#ovm_l2tol1messagepasser)               | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Low-level message passing interface                                                              |
| [`SequencerFeeVault`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#sequencerfeevault)                       | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Vault for L2 transaction fees                                                                    |
| [`OptimismMintableERC20Factory`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/predeploys.md#optimismmintableerc20factory) | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Deploys standard `OptimismMintableERC20` tokens that are compatible with either `StandardBridge` |
| [`L2ProxyAdmin`](https://github.com/ethereum-optimism/optimism/tree/develop/specs/TODO)                                                       | -                                          | Contract that can upgrade L2 contracts when sent a transaction from L1                           |

## Out of scope

### Legacy and deprecated contracts

| Name                                                            | Location | Proxy Type                                 | Description                                                                           |
| --------------------------------------------------------------- | -------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| [`AddressManager`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/legacy/AddressManager.sol)       | L1       | -                                          | Legacy upgrade mechanism (unused in Bedrock)                                          |
| [`DeployerWhitelist`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/legacy/DeployerWhitelist.sol) | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for managing allowed deployers (unused since EVM Equivalence upgrade) |
| [`L1BlockNumber`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/legacy/L1BlockNumber.sol)         | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for accessing latest known L1 block number, replaced by `L1Block`     |

* Legacy code that doesn't affect bedrock.*

# Additional Context



The key components within the scope of the contest include:

[`L1 Contracts`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/L1)

[`L2 Contracts`](https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock/contracts/L2)

[`op-node`](https://github.com/ethereum-optimism/optimism/tree/develop/op-node)

[`op-geth`](https://github.com/ethereum-optimism/op-geth)

We encourage participants to look for bugs in the following areas:

Client node vulnerabilities
EVM equivalence vulnerabilities
Bridge vulnerabilities
Generic smart contract issues
Migration attacks
Specification errors


# Build & Tests

Please refer to the below documentation for building the repo.

https://stack.optimism.io/docs/build/getting-started/

https://github.com/ethereum-optimism/optimism/tree/develop/packages/contracts-bedrock

