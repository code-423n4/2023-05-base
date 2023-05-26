![Base Logo](https://raw.githubusercontent.com/base-org/node/main/logo.webp)

# Base audit details

- Total Prize Pool: $100,000 USDC 
  - HM awards: $74,619 USDC 
  - QA report awards: $8,291 USDC 
  - Gas report awards: $0 USDC 
  - Judge awards: $9,950 USDC 
  - Lookout awards: $6,640 USDC 
  - Scout awards: $500 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-05-base/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts May 26, 2023 20:00 UTC
- Ends June 09, 2023 20:00 UTC

**IMPORTANT NOTE:** Prior to receiving payment from this audit you MUST become a [Certified Warden](https://code4rena.com/certified-contributor-application/)  (successfully complete KYC). You do not have to complete this process before competing or submitting bugs. You must have started this process within 48 hours after the audit ends, i.e. **by June 11, 2023 at 20:00 UTC in order to receive payment.**

# Overview

Base is a secure, low-cost, developer-friendly Ethereum L2 built to bring the next billion users on-chain.
It is built on the MIT-licensed OP Stack, in collaboration with Optimism. Coinbase is joining as the second Core Dev team working on the OP Stack to ensure it’s a public good available to everyone.

# Scope

The key components within the scope of the contest include:

- [`L1 Contracts`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L1)

- [`L2 Contracts`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L2)

- [`op-node`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/op-node)

- [`op-geth`](https://github.com/ethereum-optimism/op-geth)

We encourage participants to look for bugs in the following areas:

- Node vulnerabilities
- EVM equivalence vulnerabilities
- Bridge vulnerabilities
- Generic smart contract issues

## External repos

We are basing this contest on [`OP-monorepo`](https://github.com/ethereum-optimism/optimism/commit/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59) and [`op-geth`](https://github.com/ethereum-optimism/op-geth/commit/3fa9e812447af947c0208838453268a8ea33444b).

These commit hashes will be considered as a code freeze for the purposes of this contest.

These repos were added as submodules to the contest's repo. To fetch them, please clone with `git clone --recurse-submodules` or run `git submodule update --init --recursive` if you haven't cloned with submodules.

You can see these contracts deployed on testnet here : <https://docs.base.org/network-information>

## Contracts Overview

### Contracts deployed to L1



| Name                                                                                     | Proxy Type                                                              | Description                                                                                         |
| ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| [`L1CrossDomainMessenger`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/messengers.md)                                    | [`ResolvedDelegateProxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/ResolvedDelegateProxy.sol) | High-level interface for sending messages to and receiving messages from Optimism                   |
| [`L1StandardBridge`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/bridges.md)                                             | [`L1ChugSplashProxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/L1ChugSplashProxy.sol)         | Standardized system for transfering ERC20 tokens to/from Optimism                                   |
| [`L2OutputOracle`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/proposals.md#l2-output-oracle-smart-contract)             | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Stores commitments to the state of Optimism which can be used by contracts on L1 to access L2 state |
| [`OptimismPortal`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/deposits.md#deposit-contract)                             | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Low-level message passing interface                                                                 |
| [`OptimismMintableERC20Factory`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#optimismmintableerc20factory) | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol)                              | Deploys standard `OptimismMintableERC20` tokens that are compatible with either `StandardBridge`    |
| [`SystemConfig`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L1/SystemConfig.sol) | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Store system config on L1 and picked up by L2 as part of chain derivation |
| [`SystemDictator`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/deployment/SystemDictator.sol) | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) |  Helps with deployment of bedrock system. |
| ProxyAdmin                                                         | -                                                                       | Contract that can upgrade L1 contracts                                                              |


### Contracts deployed to L2

| Name                                                                                     | Proxy Type                                 | Description                                                                                      |
| ---------------------------------------------------------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------ |
| [`GasPriceOracle`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#ovm_gaspriceoracle)                         | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Stores L2 gas price configuration values                                                         |
| [`L1Block`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#l1block)                                           | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Stores L1 block context information (e.g., latest known L1 block hash)                           |
| [`L2CrossDomainMessenger`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#l2crossdomainmessenger)             | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | High-level interface for sending messages to and receiving messages from L1                      |
| [`L2StandardBridge`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#l2standardbridge)                         | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Standardized system for transferring ERC20 tokens to/from L1                                     |
| [`L2ToL1MessagePasser`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#ovm_l2tol1messagepasser)               | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Low-level message passing interface                                                              |
| [`SequencerFeeVault`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#sequencerfeevault)                       | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Vault for L2 transaction fees                                                                    |
| [`OptimismMintableERC20Factory`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/specs/predeploys.md#optimismmintableerc20factory) | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Deploys standard `OptimismMintableERC20` tokens that are compatible with either `StandardBridge` |
| ProxyAdmin                                                       | -                                          | Contract that can upgrade L2 contracts when sent a transaction from L1                           |

## Out of scope

### Legacy and deprecated contracts

| Name                                                            | Location | Proxy Type                                 | Description                                                                           |
| --------------------------------------------------------------- | -------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| [`AddressManager`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/AddressManager.sol)       | L1       | -                                          | Legacy upgrade mechanism (unused in Bedrock)                                          |
| [`DeployerWhitelist`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/DeployerWhitelist.sol) | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for managing allowed deployers (unused since EVM Equivalence upgrade) |
| [`L1BlockNumber`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/L1BlockNumber.sol)         | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for accessing latest known L1 block number, replaced by `L1Block`     |

- Legacy code that doesn't affect bedrock.*

# Roles

The following table outlines all the roles and their permissions in the system

| Role | Capability |
| --- | --- |
| L2 ProxyAdmin Owner | Can instantly upgrade all L2 contracts. |
| L1 ProxyAdmin Owner | Can instantly upgrade all L1 contracts. |
| Challenger | Can call `deleteL2Outputs()` in the event of fault. |
| MSD Controller | Controls the Migration SystemDictator contract. |
| System Config Owner | Can modify system config values. |
| Proposer | Can propose new L2 Outputs. |
| Sequencer | Can submit new transaction batches. |
| Guardian | Can pause and unpause the Portal. |


# Previous audits
https://github.com/ethereum-optimism/optimism/tree/develop/technical-documents/security-reviews

https://github.com/sherlock-audit/2023-03-optimism-judging

https://github.com/sherlock-audit/2023-01-optimism-judging

# Assumptions & Roadmap features
    * Sequencer is centralized at the moment
    * Users cannot propose L2 blocks at the moment
    * No fault proofs at the moment
    * Contracts are upgradable
    * Proposer is assumed to always propose correct l2 values
    * Challenger is assumed to challenge only in case of a fault
    * Guardian is assumed to only pause if necessary, not for greifing other users
    * Batcher  is assumed to always propose correct batches

# Known Issues

*Previously known and documented risks will not be will not be accepted as valid findings. Please refer to previous audits, known issues, OP Spec and Assumptions and Roadmap features.

*There is an edge case in which ETH deposited to the OptimismPortal by a contract can be irrecoverably stranded:

When a deposit transaction fails to execute, the sender's account balance is still credited with the mint value. However, if the deposit's L1 sender is a contract, the tx.origin on L2 will be aliased, and this aliased address will receive the minted on L2. In general the contract on L1 will not be able to recover these funds.We have documented this risk and encourage users to take advantage of our CrossDomainMessenger contracts which provide additional safety measures.

*Deposit griefing by filling up the MAX_RESOURCE_LIMIT

This issue is mitigated by PR 5064, which does not completely
resolve the issue but does increase the cost of a sustained griefing attack.
A more complete fix will require architectural changes.

*There are various 'foot guns' in the bridge which may arise from misconfiguring a token. Examples include:

Having both (or neither of) the local and remote tokens be OptimismMintable.
Tokens which dynamically alter the amount of a token held by an account, such as fee-on-transfer and rebasing tokens.
To minimize complexity our bridge design does not try to prevent all forms of developer and user error.

*When running in non-archive mode op-geth has difficulty executing deep reorgs. We are working on a fix.

# Build & Tests

Please refer to the below documentation for building the repo.

<https://stack.optimism.io/docs/build/getting-started/>

<https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock>
