# Base audit details

- Total Prize Pool: $100,000 USDC 
  - HM awards: $74,619 USDC 
  - QA report awards: $8,291 USDC 
  - Gas report awards: $0 USDC 
  - Judge awards: $9,950 USDC 
  - Lookout awards: $6,640 USDC 
  - Scout awards: $500 USDC
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-05-base-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts May 19, 2023 20:00 UTC
- Ends Jun 02, 2023 20:00 UTC

**IMPORTANT NOTE:** Unlike most public Code4rena audits, prior to receiving payment from this audit you MUST become a [Certified Warden](https://code4rena.com/certified-contributor-application/)  (successfully complete KYC). You do not have to complete this process before competing or submitting bugs. You must have started this process within 48 hours after the audit ends, i.e. **by June 4, 2023 at 20:00 UTC in order to receive payment.**

# Overview

Base is a secure, low-cost, developer-friendly Ethereum L2 built to bring the next billion users to web3.
It is built on the MIT-licensed OP Stack, in collaboration with Optimism. Coinbase is joining as the second Core Dev team working on the OP Stack to ensure it’s a public good available to everyone.

# Scope

The key components within the scope of the contest include:

- [`L1 Contracts`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L1)

- [`L2 Contracts`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L2)

- [`op-node`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/op-node)

- [`op-geth`](https://github.com/ethereum-optimism/op-geth)

We encourage participants to look for bugs in the following areas:

- Client node vulnerabilities
- EVM equivalence vulnerabilities
- Bridge vulnerabilities
- Generic smart contract issues
- Migration attacks
- Specification errors

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
| L2ProxyAdmin                                                       | -                                          | Contract that can upgrade L2 contracts when sent a transaction from L1                           |

## Out of scope

### Legacy and deprecated contracts

| Name                                                            | Location | Proxy Type                                 | Description                                                                           |
| --------------------------------------------------------------- | -------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| [`AddressManager`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/AddressManager.sol)       | L1       | -                                          | Legacy upgrade mechanism (unused in Bedrock)                                          |
| [`DeployerWhitelist`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/DeployerWhitelist.sol) | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for managing allowed deployers (unused since EVM Equivalence upgrade) |
| [`L1BlockNumber`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/legacy/L1BlockNumber.sol)         | L2       | [`Proxy`](https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/universal/Proxy.sol) | Legacy contract for accessing latest known L1 block number, replaced by `L1Block`     |

- Legacy code that doesn't affect bedrock.*

# Build & Tests

Please refer to the below documentation for building the repo.

<https://stack.optimism.io/docs/build/getting-started/>

<https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock>
