This file is a merged representation of a subset of the codebase, containing specifically included files, combined into a single document by Repomix. The content has been processed where security check has been disabled.

# File Summary

## Purpose
This file contains a packed representation of the entire repository's contents.
It is designed to be easily consumable by AI systems for analysis, code review,
or other automated processes.

## File Format
The content is organized as follows:
1. This summary section
2. Repository information
3. Directory structure
4. Multiple file entries, each consisting of:
  a. A header with the file path (## File: path/to/file)
  b. The full contents of the file in a code block

## Usage Guidelines
- This file should be treated as read-only. Any changes should be made to the
  original repository files, not this packed version.
- When processing this file, use the file path to distinguish
  between different files in the repository.
- Be aware that this file may contain sensitive information. Handle it with
  the same level of security as you would the original repository.

## Notes
- Some files may have been excluded based on .gitignore rules and Repomix's configuration
- Binary files are not included in this packed representation. Please refer to the Repository Structure section for a complete list of file paths, including binary files
- Only files matching these patterns are included: docs/developers**
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Security check has been disabled - content may contain sensitive information

## Additional Info

# Directory Structure
```
docs/
  developers/
    Concepts/
      eigenlayer-contracts/
        _category_.json
        core-contracts.md
        middleware-contracts.md
      slashing/
        _category_.json
        slashing-concept-developers.md
      _category_.json
      avs-contracts.md
      avs-developer-guide.md
      avs-keys.md
      avs-security-models.md
      rewards.md
      task.md
    HowTo/
      build/
        slashing/
          _category_.json
          design-operator-set.md
          implement-slashing.md
          migrate-to-operatorsets.md
          slashing-veto-committee-design.md
        _category_.json
        avs-permissionlesss.md
        configure-rewards.md
        how-to-build-an-avs.md
        manage-operator-sets.md
        manage-registered-operators.md
        register-avs-metadata.md
        rewards.md
      deploy/
        _category_.json
        deployment-testnet-mainnet.md
      get-started/
        _category_.json
        implement-minimum-onchain-components.md
        quickstart.md
        support.md
      publish/
        _category_.json
        onboard-avs-dashboard.md
      test/
        _category_.json
        test-avs.md
      _category_.json
    Reference/
      _category_.json
      avs-developer-best-practices.md
      eigenlayer-sdks.md
      resources.md
    _category_.json
  

```

# Files

## File: docs/developers/Concepts/eigenlayer-contracts/_category_.json
````json
{
  "position": 1,
  "label": "EigenLayer Contracts"
}
````

## File: docs/developers/Concepts/eigenlayer-contracts/core-contracts.md
````markdown
---
sidebar_position: 1
title: EigenLayer Core Contracts
---

The EigenLayer core contracts are the set of contracts that implement the EigenLayer protocol. The EigenLayer protocol includes
Staking, Operations, and AVS registration and allocation. The contracts for an AVS interact with the EigenLayer contracts. 

The [EigenLayer middleware contracts](middleware-contracts.md) are the higher level interface to the core contracts for new AVS developers. 

The EigenLayer core contracts are documented in the [eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts) repository. The core contracts include: 

| Core contract            | Description                                                                                                                                                                                                                                  | 
|--------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [StrategyManager](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#strategymanager)      | Responsible for handling the accounting for Restakers as they deposit and withdraw ERC20 tokens from their corresponding strategies. The StrategyManager tracks the amount of restaked assets each Restaker has within Eigenlayer.           |
| [DelegationManager](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#delegationmanager)    | Responsible for enabling Restakers to delegate assets to Operators, and withdraw assets. The DelegationManager tracks the amount of assets from each Strategy that have been delegated to each Operator, and tracks accounting for slashing. | 
| [EigenPodManager](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#eigenpodmanager)      | Enables native ETH restaking                                                                                                                                                                                                                 | 
| [AllocationManager](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#allocationmanager)    | Responsible for creating Operator Sets, and Operator registrations to Operator Sets. The Allocation Manager also tracks allocation of stake to a Operator Set, and enables AVSs to slash that stake.                                         
| [RewardsCoordinator](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#allocationmanager)   | Enables AVSs to distribute ERC20 tokens to Operators and Restakers who delegated assets to Operators. The RewardsCoordinator tracks the rewards and enables Operators and Restakers to claim them.
| [PermissionController](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#permissioncontroller) |Enables AVSs and operators to delegate the ability to call certain core contract functions to other addresses.|
| [AVSDirectory](https://github.com/Layr-Labs/eigenlayer-contracts/tree/testnet-holesky/docs#avsdirectory)         | Has been replaced by AllocationManager and will be deprecated in a future release. | 

This documentation matches the functionality available on the Holesky testnet. For mainnet
specific documentation, refer to the `/docs` repository on the `mainnet` branch in the [eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts)
repository.

:::important
AVSDirectory will be deprecated in a future release. We strongly recommend existing AVSs [migrate to using Operator Sets](../../HowTo/build/slashing/migrate-to-operatorsets.md)
on Testnet.
:::
````

## File: docs/developers/Concepts/eigenlayer-contracts/middleware-contracts.md
````markdown
---
sidebar_position: 1
title: EigenLayer Middleware Contracts
---

The EigenLayer middleware contracts are higher level interfaces to the [EigenLayer core contracts](core-contracts.md).
The middleware contracts can be: 
* Deployed as is. The exception is the ServiceManager contract used to register and deregister an AVS with EigenLayer.
* Modified to implement logic specific to the AVS before deploying 
* Not used. In this case, the interfaces present in the middleware contracts must be implemented in the AVS contracts.

We recommend new AVS developers use the middleware contracts as the higher level interface
to the core contracts. 

The middleware contracts are documented in the [eigenlayer-middleware](https://github.com/Layr-Labs/eigenlayer-middleware) repository.
The ServiceManagerBase contract is the reference implementation for the onchain registration and deregistration that each AVS must have.
````

## File: docs/developers/Concepts/slashing/_category_.json
````json
{
  "position": 2,
  "label": "Slashing"
}
````

## File: docs/developers/Concepts/slashing/slashing-concept-developers.md
````markdown
---
sidebar_position: 1
title: Slashing
---

For information on how slashing works, refer to concept content on [Slashing](../../../eigenlayer/concepts/slashing/slashing-concept.md) and
[Operator Sets](../../../eigenlayer/concepts/operator-sets/operator-sets-concept).

For information on how to implement slashing, refer to: 
* [Implement Slashing](../../HowTo/build/slashing/implement-slashing)
* [Design Operator Sets](../../HowTo/build/slashing/design-operator-set.md)
* [Migrate to Operator Sets](../../HowTo/build/slashing/migrate-to-operatorsets.md)
* [Veto Committee Design](../../HowTo/build/slashing/slashing-veto-committee-design.md)
````

## File: docs/developers/Concepts/_category_.json
````json
{
  "position": 1,
  "label": "Concepts"
}
````

## File: docs/developers/Concepts/avs-contracts.md
````markdown
---
sidebar_position: 3
title: AVS Contracts
---

The AVS contracts are the contracts that call the [EigenLayer contacts](eigenlayer-contracts/core-contracts.md). An AVS can split onchain components across
multiple contracts to enable a modular design.

:::note
Before the Slashing release introduced [User Access Management (UAM)](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-003.md), AVS contract calls to EigenLayer were routed through a
single ServiceManager contract. With UAM, a modular approach to AVS contracts is possible. 

The [Hello World](https://github.com/Layr-Labs/hello-world-avs) and [Incredible Squaring](https://github.com/Layr-Labs/incredible-squaring-avs) examples are in the process of being updated to use UAM.
:::
````

## File: docs/developers/Concepts/avs-developer-guide.md
````markdown
---
sidebar_position: 1
title: AVS Overview
---


## What is an Autonomous Verifiable Service (AVS)?

An Autonomous Verifiable Service (AVS) on EigenLayer is a decentralized service built on Ethereum that provides custom verification mechanisms of off-chain operations. Please see the [Intro to EigenLayer](https://docs.eigenlayer.xyz/eigenlayer/overview/) for background context on the broader EigenLayer ecosystem.

An AVS is composed of on-chain contracts for verification and an off-chain network of Operators. Operators execute the service on behalf of the AVS and then post evidence of their execution on-chain to the AVS contracts. Tasks can be initiated via on-chain contracts, off-chain via direct communication with the Operators, or via a task aggregator entity.

The design of the off-chain execution and on-chain verification is entirely flexible based on the needs of the AVS developer. 
- If the Operators perform tasks properly, the AVS can autonomously distribute rewards.
- If the Operators perform tasks maliciously, their delegate stake can be slashed autonomously by the AVS, and the Operator can be removed from the Operator set. 

 Please see the original EigenLayer whitepaper [EigenLayer: The Restaking Collective](/docs/eigenlayer/overview/whitepaper.md) for further background on AVS design.

![AVS Architecture](/img/avs/avs-architecture-v2.png)


## Why Build an AVS?

Launching new Web3 projects requires substantial time and effort to bootstrap capital and operators. Builders should focus on their core product differentiators rather than bootstrapping economic security. Building an Autonomous Verifiable Service (AVS) on EigenLayer offers enhanced security, decentralization, and cost efficiency by utilizing Ethereum’s staking mechanism through restaking. This allows developers to focus more on their product’s core value and innovation without the significant overhead of setting up a new consensus mechanism or validator networks from scratch.

The key benefits of building an AVS on EigenLayer include:
- Security via Restaking: leverage Ethereum’s staking mechanism to secure your service.
- Focus on your project's unique value: spend less time and resources accumulating economic security from scratch.
- Bootstrap your Operator network: quickly access a large network of experienced Operators.
- Decentralization and Trust: build on trust-minimized, decentralized infrastructure.
- Composability: seamlessly integrate with the broader Ethereum ecosystem.


## What Can You Build as an AVS?

The scope of AVS design is broad. It includes **any off-chain service** that can be verified on-chain. This flexibility allows AVS developers to design custom verification mechanisms suited to the unique requirements of their service. The only requirement is that some evidence for the off-chain service’s execution is posted on-chain to enable verification of the service.

Examples of these services include rollup services, co-processors, cryptography services, zk Proof services, and more.

![AVS Categories](/img/avs/avs-categories.png)


## Get in Touch

If you would like to discuss your ideas to build an AVS on EigenLayer, submit your contact information via [this form](https://share.hsforms.com/1BksFoaPjSk2l3pQ5J4EVCAein6l) and we'll be in touch shortly.
````

## File: docs/developers/Concepts/avs-keys.md
````markdown
---
sidebar_position: 4
title: AVS Keys
---

For information on AVS key types, refer to [Keys](../../eigenlayer/concepts/keys-and-signatures).
````

## File: docs/developers/Concepts/avs-security-models.md
````markdown
---
sidebar_position: 3
title: AVS Security Models
---

The security model of an AVS defines who or what is trusted in an AVS, and under what conditions that trust holds. AVSs may 
have different levels of decentralization, slashing risks, and trust assumptions.

Security models available to AVSs in order of decentralization include:
* Proof of Authority. An AVS maintains a whitelist of trusted Operators.
* Permissionless Trusted Operation. An AVS trusts the top N Operators by delegated stake to run the service.
  The Permissionless Operator set can be managed by Operator ejection if SLAs are not met.
* Unique Stake allocation. An AVS requires Operators to have a certain amount of Unique Stake (that is, Slashable Stake) allocated.
  Slashing conditions can be: 
  * Objective. Attributable onchain faults. For example, rollup execution validity. 
  * Subjective. Governance based. For example, token holders in a DAO vote to slash, or vote to veto slashing.
  * Intersubjective Slashing Conditions. Broad-based agreement among all reasonable active observers. For example, data
    withholding.

:::note 
The list of security models is not exhaustive. The EigenLayer protocol provides a slashing function that is maximally flexible.
AVSs have flexibility to design their protocols to slash for any reason. AVSs are encouraged to:
* Create robust legibility and process around how their slashing is designed and individual slashing events. 
* Clearly communicate slashing design and individual slashing events to their Operator and Staker communities. 
:::
````

## File: docs/developers/Concepts/rewards.md
````markdown
---
sidebar_position: 3
title: Rewards Overview
---

Rewards enable AVSs to make rewards to Stakers and Operators. AVSs have the flexibility to set custom logic for rewards 
to individual Operators. Examples of custom logic for rewards include:
* Work completed.
* More equal distribution of Operator support for decentralization or security reasons.

For more information, see:
* [ELIP-001 Operator Directed Rewards](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md#distribution-of-operator-directed-rewards).
* [Rewards for Operators](../../operators/howto/operator-rewards-config.md).
````

## File: docs/developers/Concepts/task.md
````markdown
---
sidebar_position: 4
title: Tasks
---

Tasks are a common design model used for AVS operations. The task design model is not required by the EigenLayer protocol but
is a common mechanism used by AVSs. Use tasks to organize discrete units of work performed by Operators offchain that
are later validated onchain. A Task can be any unit of work written in any language as needed by the AVS.

Tasks can be submitted either:
1) Onchain by the Consumer (end user) to the AVS contracts.
2) Offchain by the Consumer directly to the Operators.
````

## File: docs/developers/HowTo/build/slashing/_category_.json
````json
{
  "position": 2,
  "label": "Slashing"
}
````

## File: docs/developers/HowTo/build/slashing/design-operator-set.md
````markdown
---
sidebar_position: 3
title: Design Operator Sets
---

An [Operator Set](../../../../eigenlayer/concepts/operator-sets/operator-sets-concept.md) is a grouping of different types of work within a single AVS. Each AVS has at least one Operator Set. The 
EigenLayer protocol does not enforce criteria for Operator Sets.

Best practices for Operator Set design are to logically group AVS tasks (and verification) into separate Operator Sets. 
Organize your Operator Sets according to conditions for which you wish to distribute Rewards. Potential conditions include:
* Unique business logic.
* Unique Stake (cryptoeconomic security) amount and types of token required to be allocated from Operators.
* Slashing conditions.
* Ejection criteria.
* Quantity of Operators and criteria for operators allowed.
* Hardware profiles.
* Liveness guarantees.

For more information on Operator Sets, refer to [Operator Sets](../../../../eigenlayer/concepts/operator-sets/operator-sets-concept).
````

## File: docs/developers/HowTo/build/slashing/implement-slashing.md
````markdown
---
sidebar_position: 1
title: Implement Slashing
---

:::important
Before proceeding, review the [Slashing Concept](../../../../eigenlayer/concepts/slashing/slashing-concept.md) content and [Unique Stake Allocation & Deallocation ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
for detailed information on slashing.
:::

The `AllocationManager` provides the interface for the slashing function.

```solidity
    /**
     * @notice Called by an AVS to slash an operator in a given operator set
     */

    function slashOperator(
        address avs,
        SlashingParams calldata params
    ) external;

    /**
     * @notice Struct containing parameters to slashing
     * @param operator the address to slash
     * @param operatorSetId the ID of the operatorSet the operator is being slashed on behalf of
     * @param strategies the set of strategies to slash
     * @param wadsToSlash the parts in 1e18 to slash, this will be proportional to the operator's
     * slashable stake allocation for the operatorSet
     * @param description the description of the slashing provided by the AVS for legibility
     */
    struct SlashingParams {
        address operator;
        uint32 operatorSetId;
        IStrategy[] strategies;
        uint256[] wadsToSlash;
        string description;
    }
```

To implement slashing, AVSs specify:
* Individual Operator
* [Operator Set](../../../../eigenlayer/concepts/operator-sets/operator-sets-concept.md)
* [List of Strategies](../../../../eigenlayer/concepts/operator-sets/strategies-and-magnitudes)
* [List of proportions (as `wads` or “parts per `1e18`”)](../../../../eigenlayer/concepts/operator-sets/strategies-and-magnitudes)
* Description. 

For example, in the `wadsToSlash` parameter: 
* 8% slash is represented as `8e16`, or `80000000000000000`. 
* 25% slash is represented as `2.5e17` or `250000000000000000`. 

The indexes in the two arrays must match across `strategies` and `wadsToSlash`. 

All Strategies supplied must be configured as part of the Operator Set. For all Strategies specified, the Operator’s allocations
to that Operator Set are slashed by the corresponding proportion while maintaining their nominal allocations to all other Operator Sets.
Maintaining nominal allocations is achieved by subtracting the allocated magnitude from both the specified Operator Set, 
and the Operator’s Total Magnitude.

Slashing proportionally reduces funds of all Stakers of the given Strategies that are delegated to the Operator, including funds
in queued deallocations and withdrawals (that haven’t passed `WITHDRAWAL_DELAY`). Operator delegation is decreased directly 
in the `DelegationManager` in each Strategy. Changes are propagated to Staker withdrawals and view functions by referring to their
delegated Operator’s Total Magnitude.

When a slashing occurs, one event is emitted onchain for each slashing. Emitted details identify the Operator
slashed, in what Operator Set, and across which Strategies, with fields for the proportion slashed and meta-data.
```
/// @notice Emitted when an operator is slashed by an operator set for a strategy
/// `wadSlashed` is the proportion of the operator's total delegated stake that was slashed
event OperatorSlashed(
    address operator, OperatorSet operatorSet, IStrategy[] strategies, uint256[] wadSlashed, string description
);
```

## Example

The allocated magnitudes are: 

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 40 |
| `AVS_2_EIGEN` | 2,500 | 25% | 50 |
| `EigenDA_EIGEN` | 2,000 | 20% | 40 |
| `Non-slashable` | 3,500 | 35% | 70 |
| `Total`  | 10,000 | 100% | 200 |

`AVS_1` slashes the Operator for a 50% reduction (`5e17` in `wads`) in the Operator Set `AVS_1_EIGEN`:

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 1,000 | 11% | 20 |
| `AVS_2_EIGEN` | 2,500 | 28% | 50 |
| `EigenDA_EIGEN` | 2,000 | 22% | 40 |
| `Non-slashable` | 3,500 | 39% | 70 |
| `Total` | 9000 | 100% | 180 |

Slashing by one Operator Set does not affect the magnitudes of EIGEN allocated to other Operator Sets. The interactions between
Staker, Operator, AVS, and core contracts are represented in the sequence diagram.

![Sequence Representation of a Slashing](/img/operator-guides/operator-sets-figure-5.png)  
***Figure: Sequence Representation of a Slashing***
````

## File: docs/developers/HowTo/build/slashing/migrate-to-operatorsets.md
````markdown
---
sidebar_position: 2
title: Migrate to Operator Sets
---

**The AVSDirectory method will be deprecated in a future upgrade. All AVSs will need to migrate to [Operator Sets](../../../../eigenlayer/concepts/operator-sets/operator-sets-concept) before the
upcoming deprecation of AVSDirectory.**

Operator Sets are required to [slash](../../../../eigenlayer/concepts/slashing/slashing-concept.md). To migrate to, and start using, Operator Sets: 
1. [Upgrade middleware contracts](#upgrade-middleware-contracts) 
2. [Integrate the AllocationManager](#upgrade-middleware-contracts)
3. [Communicate to Operators](#communicate-to-operators)

Migrating now gives time to switch existing quorums over to Operator Sets. After the migration has occurred,
integrations with slashing can go live on Testnet. M2 registration and Operator Set registration can operate in parallel.

## Upgrade middleware contracts

To migrate to Operator Sets:

1. Upgrade middleware contracts to handle the callback from the AllocationManager. The upgrade provides the RegistryCoordinator
the hooks to handle the callback from the AllocationManager. 
2. From the ServiceManager call, add an account to update the AVSRegistrar:
      * With setAppointee where the target is the AllocationManager.
      * The selector is the setAVSRegistrar selector.
3. Call setAVSRegistrar on the AllocationManager from the appointee account and set the RegistryCoordinator as your AVSRegistrar
so that it becomes the destination for registration and deregistration hooks

See example [RegistryCoordinator implementation with the new hooks](https://github.com/Layr-Labs/eigenlayer-middleware/blob/dev/src/SlashingRegistryCoordinator.sol).

## Integrate the AllocationManager

Integrate the AllocationManager by:

1. Creating Operator Sets through the AllocationManager.
2. Adding (or later removing) specific Strategies to that Operator Set to enable Operators to secure the AVS.
3. Specifying an additional AVSRegistrar contract that applies business logic to gate Operator registration to an Operator Set.

## Communicate to Operators

1. Communicate to Operators how to:
   1. Register for Operator Sets using the new registration pathway. 
   2. Allocate slashable stake for slashable Operator Sets.
2. Migrate to distribution of tasks based on the delegated and slashable stake of Operators registered to the AVS’s Operator Sets.

To ensure community and incentive alignment, AVSs need to conduct offchain outreach to communicate
the purpose and task/security makeup of their Operator Sets with their Operators and Stakers before beginning registration.
Include any potential hardware, software, or stake requirements in the communication. The AVS decides task distribution
within an Operator Set.
````

## File: docs/developers/HowTo/build/slashing/slashing-veto-committee-design.md
````markdown
---
sidebar_position: 4
title: Design Slashing Conditions
---

## Slashing Vetoes

EigenLayer provides a maximally flexible slashing function. AVSs may slash any Operator in any of their Operator Sets for
any reason. Slashing does not have to be objectively attributable (that is, provable on-chain). We encourage AVSs to create
robust legibility and process around individual slashings. Governance, fraud proofs, and decentralization
must be considered in AVS slashing designs. Include delays and veto periods in AVS designs to avoid or cancel slashing
in cases of AVS implementation bugs, improper slashing, or fraud.

**No vetoes are provided by the EigenLayer protocol.**

## Veto Committee Design

One popular AVS design is to utilize a governance mechanism with slashing such that a committee can review a proposed (or queued) 
slashing request. That slashing request can then be either fulfilled or vetoed by a committee of domain experts, governance 
council or multisig address for the AVS. Please see the [vetoable slasher example implementation](https://github.com/Layr-Labs/eigenlayer-middleware/blob/feat/slashing-release-branch/src/slashers/VetoableSlasher.sol) for reference.

Ensure that your slashing process can be resolved within the `DEALLOCATION_DELAY` time window. This is the amount of blocks
between an Operator queuing a deallocation of stake from an Operator Set for a strategy and the deallocation taking effect. 
This will ensure that the slashing event is carried out for the Operator before their stake is deallocated.
````

## File: docs/developers/HowTo/build/_category_.json
````json
{
  "position": 2,
  "label": "Build"
}
````

## File: docs/developers/HowTo/build/avs-permissionlesss.md
````markdown
---
sidebar_position: 7
title: Add ERC-20 Tokens as Restakable Asset
---

# Permissionless Token Strategies

Permissionless Token Support enables any ERC-20 token to be permissionlessly added as a restakable asset, significantly broadening the scope of assets that can contribute to the security of decentralized networks, and unlocking the cryptoeconomic security of ERC-20 tokens on EigenLayer.


With this capability, AVSs can choose to accept any ERC-20 token as a restaked asset to provide cryptoeconomic security for their AVS. This allows AVSs to evaluate the supply and utility of all available tokens to create cross-ecosystem partnerships while ensuring the safety and security of their services. This increases alignment and connectivity across the ecosystem, moving us closer to the ultimate goal of open innovation.



# Adding a New Strategy

To add a new Strategy to the EigenLayer protocol:

* Invoke StrategyFactory.deployNewStrategy().
* Your Strategy is now available to associate with your AVS.

Please see the contract documentation [here](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/StrategyManager.md#strategyfactorydeploynewstrategy) for further detail.
````

## File: docs/developers/HowTo/build/configure-rewards.md
````markdown
---
sidebar_position: 7
title: Configure Rewards
---

Submitting rewards for an AVS is handled by the [RewardsCoorinator core contract](../../Concepts/eigenlayer-contracts/core-contracts.md).

To make a rewards submission for an Operator Set, call the [`createOperatorDirectedOperatorSetRewardsSubmission`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/RewardsCoordinator.md#createoperatordirectedoperatorsetrewardssubmission) function.
````

## File: docs/developers/HowTo/build/how-to-build-an-avs.md
````markdown
---
sidebar_position: 1
title: Build Your Own AVS
---

This section will walk you through the process of designing and building your own AVS from scratch. It is intended to take you from an idea to a working local prototype.

Before proceeding, please review the previous sections on [AVS Overview](../../Concepts/avs-developer-guide.md), [EigenLayer Overview](/docs/eigenlayer/overview/README.md) and the [Quick Start example](../get-started/quickstart.md) to become familiar with the basic concepts.


## Step 1: AVS Design

### Design Your AVS Security (Trust Model)

The first step toward designing your AVS is to determine how its off-chain operations will be validated on-chain. Consider which Operator behaviors should be rewarded and which behaviors are malicious and should be slashed or penalized. Determine which data (or evidence) of their operations can be written on-chain to validate their behavior.

Operators are most often expected to **run the same workload** among all Operators in the AVS's quorum. This ensures that malicious behaviors can be easily validated on-chain. When designing your Operator workload, consider a task that can be easily scaled to run among all Operators in the quorum.

### Task and Signature Aggregation

Operator responses to tasks are often signed using the BLS or ECDSA algorithm. These signatures can be aggregated by any entity at any time, but they are often aggregated by an entity run by the AVS (the "aggregator"). A common AVS design involves combining multiple Operator BLS signatures into a single aggregate signature and written on-chain ([example here](https://github.com/Layr-Labs/eigensdk-go/blob/dev/services/bls_aggregation/blsagg.go) written in Go). The aggregate signature can then be verified to confirm whether any of the individual Operators were included in the aggregate.
      
## Step 2: Idea to Code (Building and Deploying your AVS Locally)

The following section covers the minimum set of smart contract integrations and deployment scripts that an AVS needs to build in order to:
1. Be considered a fully functional AVS for demo and proof of concept purposes.
2. Prepare your AVS to integrate Slashing functionality. Slashing is [live on Holesky Testnet](https://www.blog.eigenlayer.xyz/introducing-slashing/) and Mainnet deployment is proposed for late Q1 2025.


:::info
To begin the process below, fork the example repo here [hello-world-avs](https://github.com/Layr-Labs/hello-world-avs).
:::


### Smart Contract Requirements


**1: Integration with EigenLayer Core (AVS Directory)**  
Implement an instance of ECDSAServiceManagerBase or ServiceManagerBase (BLS).  
Please see the example from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/src/HelloWorldServiceManager.sol) and incredible-squaring-avs [here](https://github.com/Layr-Labs/incredible-squaring-avs/blob/master/contracts/src/IncredibleSquaringServiceManager.sol).

**2: on-chain Verification**  
Implement at least one on-chain provable event. The most common approach is to write an ECDSA or BLS aggregate signature on-chain.
Please see the example from incredible-squaring-avs [here](https://github.com/Layr-Labs/incredible-squaring-avs/blob/8bd0ac663dcc2289cad02af4a7f0002ea07bc1d8/contracts/src/IncredibleSquaringTaskManager.sol#L102) and from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L130).


### Contract Deployment Requirements

Implement deployment scripts to deploy your contracts to your [local Anvil node](https://book.getfoundry.sh/reference/anvil/).

**1: Deploy of EigenLayer Contracts and State**  
Please see the example from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/master/utils/anvil/deploy-eigenlayer-save-anvil-state.sh).

**2: Deploy your AVS contracts**  
Please see the example forge deployment script from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/script/HelloWorldDeployer.s.sol) and bash deployment script [here](https://github.com/Layr-Labs/hello-world-avs/blob/master/utils/anvil/deploy-eigenlayer-save-anvil-state.sh).



### Operator (Off-Chain) Requirements

**1: Operator Registration to AVS**  
Provide a mechanism for the Operator register to the AVS.  

Please see the example from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L41). 

**2: At least one event written to your AVSs on-chain contracts**  
The Operator binary (or off-chain aggregation service code) must write at least one event to the AVSs on-chain contracts to be used for future on-chain verification, rewards, and slashing purposes.  

Please see the example from hello-world-avs [here](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L25).

## Step 3: Deploy and Test Locally

Use [AVS Devnet](../test/test-avs.md) to deploy and test the AVS locally.

## Get in Touch

If you would like to discuss your ideas to build an AVS on EigenLayer, submit your contact information via [this form](https://share.hsforms.com/1BksFoaPjSk2l3pQ5J4EVCAein6l) and we'll be in touch shortly.
````

## File: docs/developers/HowTo/build/manage-operator-sets.md
````markdown
---
sidebar_position: 5
title: Manage Operator Sets
---

To manage [Operator Sets](../../../eigenlayer/concepts/operator-sets/operator-sets-concept.md) for an AVS:
1. [Create Operator Sets](#create-operator-sets)
2. [Modify Strategy composition](#modify-strategy-composition) 

## Create Operator Sets

Creating Operator Sets for an AVS is managed by the [AllocationManager core contract](../../Concepts/eigenlayer-contracts/core-contracts.md). 
[Strategies](../../../eigenlayer/concepts/operator-sets/strategies-and-magnitudes) can be added to Operator Sets when the Operator is created, or Strategies can be added to an existing Operator Set.

To create an Operator Set, call the [`createOperatorSets`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#createoperatorsets) function. 
To add strategies when creating an Operator Set, specify a `params` array containing the strategies.

On creation, an `id` is assigned to the Operator Set. Together the AVS `address` and `id` are a unique identifier for the Operator Set. 

## Modify Strategy Composition

An Operator Set requires at least one [Strategy](../../../eigenlayer/concepts/operator-sets/strategies-and-magnitudes).

To add Strategies to an existing Operator Set, call the [`addStrategiesToOperatorSet`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#addstrategiestooperatorset) function.

To remove Strategies from an Operator Set, call the [`removeStrategiesFromOperatorSet`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#removestrategiesfromoperatorset) function.
````

## File: docs/developers/HowTo/build/manage-registered-operators.md
````markdown
---
sidebar_position: 6
title: Manage Registered Operators
---

## AVSRegistrar

The [AVSRegistrar](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/src/contracts/interfaces/IAVSRegistrar.sol) is called when operators register for and deregister from [Operator Sets](../../../eigenlayer/concepts/operator-sets/operator-sets-concept.md). By default (if the stored address
is 0), the call is made to the ServiceManager contract for the AVS. If the AVS has set a different contract as the AVSRegistrar, the specified contract is called.

### Setting AVSRegistrar

To set a contract as the AVSRegistrar, call the [`setAVSRegistrar`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#setavsregistrar) function. The target contract must also implement 
[`supportsAVS(AVS)`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/src/contracts/interfaces/IAVSRegistrar.sol) returning TRUE or setting the contract as the AVSRegistrar fails.

## Respond to Operator Registrations to Operator Sets

Operators use the [`registerForOperatorSets`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#registerforoperatorsets) function to register for AVS's operator sets. AVSs can reject ineligible 
Operators based on their own custom logic specified in the [AVSRegistrar](#avsregistrar).

For an AVS to reject an Operator attempting to join an Operator Set, the call from [AllocationManager](../../Concepts/eigenlayer-contracts/core-contracts.md) to the 
[`IAVSRegistrar.registerOperator`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/src/contracts/interfaces/IAVSRegistrar.sol) function must revert. 

## Deregister Operators from, or respond to Operator deregistrations, from Operator Sets

Deregistration from an Operator Set can be triggered by either the Operator, or the AVS for the Operator Set, using the
[`deregisterFromOperatorSets`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#deregisterfromoperatorsets) function.

Similar to when an Operator registers for an Operator Set, if the call to [IAVSRegistrar.deregisterOperator](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/src/contracts/interfaces/IAVSRegistrar.sol) reverts, the
deregistration also reverts and does not occur.
````

## File: docs/developers/HowTo/build/register-avs-metadata.md
````markdown
---
sidebar_position: 4
title: Register AVS Metadata
---

Registering metadata for an AVS is managed by the [AllocationManager core contract](../../Concepts/eigenlayer-contracts/core-contracts.md).  

Metadata must be registered:
* Before an AVS can create [Operator Sets](../../../eigenlayer/concepts/operator-sets/operator-sets-concept.md) or register Operators to Operator Sets. 
* To [onboard to the AVS Dashboard](../publish/onboard-avs-dashboard.md).

To register metadata, call the [`updateAVSMetadataURI`](https://github.com/Layr-Labs/eigenlayer-contracts/blob/9a19503e2a4467f0be938f72e80b11768b2e47f9/docs/core/AllocationManager.md#avs-metadata) function. Use the following format to initially register metadata. 
The format is not validated onchain. 

```
{
    "name": "AVS",
    "website": "https.avs.xyz/",
    "description": "Some description about",
    "logo": "http://github.com/logo.png",
    "twitter": "https://twitter.com/avs",
}
```
````

## File: docs/developers/HowTo/build/rewards.md
````markdown
---
sidebar_position: 3
title: Implement Rewards
---
Before implementing rewards, we recommend reading:
* [ELIP-001 Operator Directed Rewards](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md#distribution-of-operator-directed-rewards).
* [Rewards for Operators](../../../operators/howto/operator-rewards-config.md).

AVSs make rewards submissions by calling `createAVSRewardsSubmission` or `createOperatorDirectedAVSRewardsSubmission()` on 
the `RewardsCoordinator` contract. Rewards must come from an AVSs ServiceManager contract. Each rewards submission specifies:  

1. A time range for which the rewards submission is valid. Rewards submissions can be retroactive from the M2 upgrade and last up to 30 days in the future.
2. A list of strategies and multipliers, which enables the AVS to weigh the relative payout to each strategy within a single rewards submission.
3. The ERC20 token in which rewards should be denominated.

Integration notes:
- Reward roots are posted weekly on Mainnet and daily on Testnet.
- Reward roots are on a 7-day activation delay (that is, when it is claimable against) on Mainnet and 2-hour activation delay on Testnet.
- Reward amounts are calculated based on activity across a 24 hour window. Each window's amounts are cumulative and include `day + (day - 1)`. 
Reward roots are posted weekly on Mainnet based on that day's snapshot date which correlates to a 24 hour window. Mainnet and Testnet are functionally 
equivalent in their calculations. The reward roots are only posted weekly for Mainnet.
- Once a rewards submission is made by an AVS, the AVS is unable to retract those rewards. If the AVS does not have any Operators opted into 
the AVS on a day of an active reward, those tokens are not distributed pro-rata to future days, and are refunded to the AVS. There are two cases where this occurs:
    1. An operator is not registered for the entire duration of the submission. The entire operator amount is refunded to the AVS.
    2. If an operator is only registered for m days out of n days duration. The operator is only paid amount/m on each of those m days.
- Operators are only distributed rewards on days that they have opted into the AVS for the full day.
- Due to the rounding in the off-chain process, we recommend not making range submission token amounts with more than 15 significant digits of precision. 
If more than 15 significant digits are provided, the extra precision is truncated.
- Rewards can be made in multiple ERC-20 tokens by calling `createAVSRewardsSubmission` for each ERC-20 token to reward in.  
- There are several requirements for successfully calling `createAVSRewardsSubmission`. We recommended reading further details [here](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/RewardsCoordinator.md#createavsrewardssubmission).

## When rewards are included

An AVSs reward submission is included in the calculation 2 days after it is submitted. For example, if the AVS submits a 
rewards submission on August 2nd, it is included in the August 4th rewards calculation.

## When rewards can be claimed

At most, Restakers and Operators of an AVS will have to wait 16 days to claim a reward (2 day calculation delay + 7 day root
submission cadence + 7 day activation delay). 

At minimum, Restakers and Operators will have to wait 9 days to claim a reward.
````

## File: docs/developers/HowTo/deploy/_category_.json
````json
{
  "position": 3,
  "label": "Deploy"
}
````

## File: docs/developers/HowTo/deploy/deployment-testnet-mainnet.md
````markdown
---
sidebar_position: 6
title: Prepare for and Deploy to Testnet and Mainnet
---


## Preparing and Deploying to Testnet

1. Package the Operator’s long running executable in a way that is easy for Operators to launch  (via binary, docker container, or similar).

2. Author Testnet user and Operator documentation, including:
   - Trust Modeling: clarify any trust assumptions in your architecture to your users. Identify the components that are trusted (centralized) and untrusted (decentralized, trustless).
   - Operator instructions to install, register, deregister.
   - End user (aka “Consumer”) instructions to utilize your AVS service.
   - Communication channels that will be utilized for AVS upgrades.
   - Describe Operator monitoring tooling available, such as GraFana dashboards, log files or similar.

3. Follow the [AVS Developer Security Best Practices](../../Reference/avs-developer-best-practices.md) and [Key Manage Considerations for Developers](../../Reference/avs-developer-best-practices.md#key-management-recommendation-for-developers).

4. Implement the [Node Specification](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/spec/intro) for your Operator executable package.

5.  Follow the [Testnet Dashboard Onboarding instructions](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-dashboard-onboarding).

6. Implement Rewards distributions per the instructions [here](../build/rewards.md).


## Preparing and Deploying to Mainnet

1. Smart Contract Auditing: have your codebase audited with at least 2-3 reputable audit firms.
2. Finalize User and Operator documentation.
3. Follow the [Mainnet Dashboard Onboarding instructions](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-dashboard-onboarding).
````

## File: docs/developers/HowTo/get-started/_category_.json
````json
{
  "position": 1,
  "label": "Get Started"
}
````

## File: docs/developers/HowTo/get-started/implement-minimum-onchain-components.md
````markdown
---
sidebar_position: 2
title: Implement onchain components
---

To build an AVS, the minimum set of functionality to be defined in the [AVS contracts](../../Concepts/avs-contracts.md) is:
* [Registering AVS metadata](../build/register-avs-metadata.md)
* [Managing Operator Sets](../build/manage-operator-sets.md)
    * [Creating and modifying Strategy composition](../build/manage-operator-sets.md#modify-strategy-composition)
* [Managing registered Operators](../build/manage-registered-operators.md)
    * [Responding to Operator registrations](../build/manage-registered-operators.md#respond-to-operator-registrations-to-operator-sets)
    * [Deregistering Operators](../build/manage-registered-operators.md#deregister-operators-from-or-respond-to-operator-deregistrations-from-operator-sets)
* [Distributing Rewards](../build/configure-rewards.md)
````

## File: docs/developers/HowTo/get-started/quickstart.md
````markdown
---
sidebar_position: 1
title: Get started
---

:::note

We are in the process of updating our samples to include Rewards and Slashing capabilities. The Hello World AVS example will be
updated as soon as possible. Use Hello World AVS now to get familiar with EigenLayer. 
For more information on Rewards and Slashing, refer to the [Rewards](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md) and [Slashing](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md) ELIPs,
and [Rewards](../../Concepts/rewards.md) and [Slashing](../../Concepts/slashing/slashing-concept-developers) documentation. 
For questions or support, reach out to us using the Intercom button on the bottom right side of this page or <a href="javascript:void(0)"  id="intercom_trigger_eldocs" >here</a>. 
We will promptly follow up with support!

:::

## Hello World AVS: Local Deployment
The [Hello World AVS](https://github.com/Layr-Labs/hello-world-avs) is a simple implementation designed to demonstrate the core mechanics of how AVSs work within the EigenLayer framework. This example walks you through the process of:
- Spinning up a local chain with EigenLayer contracts and AVS contracts preconfigured.
- Registering an Operator with both EigenLayer and the AVS.
- Consumer client requesting work to be done by the AVS.
- Operator listening picking up this request, performing it, and signing off on it.
- The AVS contract verifying the operator's work.

![Hello World Diagram](/img/avs/hello-world-diagram-v2.png)

## Key Components of Hello World AVS
- AVS Consumer: Requests a "Hello, ___" message to be generated and signed.
- AVS: Takes the request and emits an event for operators to handle.
- Operators: Picks up the request, generates the message, signs it, and submits it back to the AVS.
- Validation: Ensures the operator is registered and has the necessary stake, then accepts the submission.


## Code Walkthrough

The following sections highlight a few crucial components of the Hello World example that implement core AVS functionality. 

### AVS Contract

**[HelloWorldServiceManager.sol](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/src/HelloWorldServiceManager.sol)**

The contract definition declares that it implements `ECDSAServiceManagerBase`, which allows it to inherit the core required functionality of `IServiceManager`. These contracts are included from the [eigenlayer-middleware repo](https://github.com/Layr-Labs/eigenlayer-middleware/tree/dev/docs#eigenlayer-middleware-docs) and are [required components](https://github.com/Layr-Labs/eigenlayer-middleware/tree/dev/docs#system-components) for any AVS.

```sol
contract HelloWorldServiceManager is ECDSAServiceManagerBase, IHelloWorldServiceManager {
    using ECDSAUpgradeable for bytes32;
```

The following functions are responsible for the "business logic" of the AVS. In the case of hello world the business logic includes managing the lifecycle of a "task" (creation and response) with a simple `name` string value.
```sol
function createNewTask(
    string memory name
) external returns (Task memory) {
    // create a new task struct
    Task memory newTask;
    newTask.name = name;
    newTask.taskCreatedBlock = uint32(block.number);

    // store hash of task on-chain, emit event, and increase taskNum
    allTaskHashes[latestTaskNum] = keccak256(abi.encode(newTask));
    emit NewTaskCreated(latestTaskNum, newTask);
    latestTaskNum = latestTaskNum + 1;

    return newTask;
}

function respondToTask(
    Task calldata task,
    uint32 referenceTaskIndex,
    bytes memory signature
) external {
    // check that the task is valid, hasn't been responded to yet, and is being responded in time
    require(
        keccak256(abi.encode(task)) == allTaskHashes[referenceTaskIndex],
        "supplied task does not match the one recorded in the contract"
    );
    require(
        allTaskResponses[msg.sender][referenceTaskIndex].length == 0,
        "Operator has already responded to the task"
    );

    // The message that was signed
    bytes32 messageHash = keccak256(abi.encodePacked("Hello, ", task.name));
    bytes32 ethSignedMessageHash = messageHash.toEthSignedMessageHash();
    bytes4 magicValue = IERC1271Upgradeable.isValidSignature.selector;
    if (!(magicValue == ECDSAStakeRegistry(stakeRegistry).isValidSignature(ethSignedMessageHash,signature))){
        revert();
    }

    // updating the storage with task responses
    allTaskResponses[msg.sender][referenceTaskIndex] = signature;

    // emitting event
    emit TaskResponded(referenceTaskIndex, task, msg.sender);
}
```

Please find a complete list of the requirements to implement an AVS at [Build Your Own AVS: Step 2 Idea to Code](/docs/developers/HowTo/build/how-to-build-an-avs.md#step-2-idea-to-code-building-and-deploying-your-avs-locally).

### Contract Deployment Scripts

**[HelloWorldDeployer.s.sol](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/script/HelloWorldDeployer.s.sol)**

The deployment of the HelloWorld contracts associates the quorums and their asset strategies to the AVS.

```sol
token = new ERC20Mock();
helloWorldStrategy = IStrategy(StrategyFactory(coreDeployment.strategyFactory).deployNewStrategy(token));

quorum.strategies.push(
    StrategyParams({strategy: helloWorldStrategy, multiplier: 10_000})
);
```

### Off-chain Operator Code


**[index.ts](https://github.com/Layr-Labs/hello-world-avs/blob/master/operator/index.ts)**

The following snippets of Operator code manage Operator registration to core EigenLayer protocol, registration to the Hello World AVS, listening and responding to tasks.

```sol
// Register Operator to EigenLayer core contracts and Hello World AVS
const registerOperator = async () => {
    
    // Registers as an Operator in EigenLayer.
    try {
        const tx1 = await delegationManager.registerAsOperator({
            __deprecated_earningsReceiver: await wallet.address,
            delegationApprover: "0x0000000000000000000000000000000000000000",
            stakerOptOutWindowBlocks: 0
        }, "");
        await tx1.wait();
        console.log("Operator registered to Core EigenLayer contracts");
    }
    
    ...
    
    
    const tx2 = await ecdsaRegistryContract.registerOperatorWithSignature(
        operatorSignatureWithSaltAndExpiry,
        wallet.address
    );
    await tx2.wait();
    console.log("Operator registered on AVS successfully");
};

// Listen for new task events on-chain
const monitorNewTasks = async () => {

    helloWorldServiceManager.on("NewTaskCreated", async (taskIndex: number, task: any) => {
        console.log(`New task detected: Hello, ${task.name}`);
        await signAndRespondToTask(taskIndex, task.taskCreatedBlock, task.name);
    });
    console.log("Monitoring for new tasks...");
};



// Generate Hello, Name message string
const signAndRespondToTask = async (taskIndex: number, taskCreatedBlock: number, taskName: string) => {
    const message = `Hello, ${taskName}`;
    const messageHash = ethers.solidityPackedKeccak256(["string"], [message]);
    const messageBytes = ethers.getBytes(messageHash);
    const signature = await wallet.signMessage(messageBytes);

    console.log(`Signing and responding to task ${taskIndex}`);

    const operators = [await wallet.getAddress()];
    const signatures = [signature];
    const signedTask = ethers.AbiCoder.defaultAbiCoder().encode(
        ["address[]", "bytes[]", "uint32"],
        [operators, signatures, ethers.toBigInt(await provider.getBlockNumber()-1)]
    );

    const tx = await helloWorldServiceManager.respondToTask(
        { name: taskName, taskCreatedBlock: taskCreatedBlock },
        taskIndex,
        signedTask
    );
    await tx.wait();
    console.log(`Responded to task.`);
};


```


### Off-chain Task Generator

**[createNewTasks.ts](https://github.com/Layr-Labs/hello-world-avs/blob/master/operator/createNewTasks.ts)**

The following Typescript code generates new tasks at a random interval. This entity that generates tasks for the AVS is also referred to as the "AVS Consumer".

```sol

// Create a New Task (a new name to be signed as "hello, name")
async function createNewTask(taskName: string) {
  try {
    // Send a transaction to the createNewTask function
    const tx = await helloWorldServiceManager.createNewTask(taskName);
    
    // Wait for the transaction to be mined
    const receipt = await tx.wait();
    
    console.log(`Transaction successful with hash: ${receipt.hash}`);
  } catch (error) {
    console.error('Error sending transaction:', error);
  }
}

```



## Local Deployment Test

Please follow the steps under [Local Devnet Deployment](https://github.com/Layr-Labs/hello-world-avs?tab=readme-ov-file#local-devnet-deployment) to deploy an instance of Hello World locally on your machine.
````

## File: docs/developers/HowTo/get-started/support.md
````markdown
---
sidebar_position: 8
title: Get Support
---

If you have any questions or comments throughout the AVS development process, you can get support by reaching out to us using the Intercom button on the bottom right side of this page or <a href="javascript:void(0)"  id="intercom_trigger_eldocs" >clicking here</a>. We will promptly follow up with support!
````

## File: docs/developers/HowTo/publish/_category_.json
````json
{
  "position": 5,
  "label": "Publish"
}
````

## File: docs/developers/HowTo/publish/onboard-avs-dashboard.md
````markdown
---
sidebar_position: 5
title: Onboard to AVS Dashboard
---


This document defines interfaces that AVSs must implement for us to be able to index their data for the V1 [AVS Marketplace](https://app.eigenlayer.xyz/avs).

New AVS Listings: in order for an AVS to have its name, information, and logo indexed, it must invoke `updateAVSMetadataURI()` on the [AVSDirectory contract](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/src/contracts/core/AVSDirectory.sol).
It currently takes about 10 minutes for it to be indexed and the metadata to be updated on the dashboard.

Updating AVS Listings:  if you deploy a new contract for a new version of your AVS, please be sure to remove the previous listing. Invoke the update metadata function with value of null, such as `updateAVSMetadataURI("")` to remove the previous listing. Your listing will then be removed from the application cache within one hour.

## Interface

```javascript
interface IServiceManager {
// Below 3 functions are just proxies to the same-named functions in the AVSDirectory
function registerOperatorToAVS(address operator, Signature memory signature);

function deregisterOperatorFromAVS(address operator);

function updateAVSMetadataURI(string calldata metadataURI);
	
// Below 2 functions are needed for your AVS to appear correctly on the UI
function getOperatorRestakedStrategies(address operator) returns (address[] memory)

function getRestakeableStrategies() returns (address[] memory);
}
```

### registerOperatorToAVS and deregisterOperatorFromAVS
In order to have its list of operators displayed on the UI, an AVS MUST handle operator registration/deregistration by calling `registerOperatorToAVS()` and `deregisterOperatorFromAVS()` on EigenLayer’s AVSDirectory.  Primarily, these functions serve to forward calls to the `AVSDirectory.sol` contract to confirm an operator's registration with the AVS.
```solidity
function registerOperatorToAVS(
        address operator,
        ISignatureUtils.SignatureWithSaltAndExpiry memory operatorSignature
    ) public virtual onlyRegistryCoordinator {
        avsDirectory.registerOperatorToAVS(operator, operatorSignature);
    }

function deregisterOperatorFromAVS(address operator) public virtual onlyRegistryCoordinator {
        avsDirectory.deregisterOperatorFromAVS(operator);
    }
```

### getOperatorRestakedStrategies
This function must be implemented in order to provide the list of strategies that an operator has restaked with the AVS. This allows the AVS to have its total restaked value displayed on the UI.  Given an operator, this function should:
- Retrieve the operator's quorum bitmap from the `RegistryCoordinator.sol` contract.
- Retrieve the addresses of the strategies for each quorum in the quorum bitmap

Note that there is no guarantee is made on whether the operator has shares for a strategy in a quorum or uniqueness of each element in the returned array. The off-chain service should do that validation separately

```solidity
function getOperatorRestakedStrategies(address operator) external view returns (address[] memory) {
        bytes32 operatorId = registryCoordinator.getOperatorId(operator);
        uint192 operatorBitmap = registryCoordinator.getCurrentQuorumBitmap(operatorId);

        if (operatorBitmap == 0 || registryCoordinator.quorumCount() == 0) {
            return new address[](0);
        }

        // Get number of strategies for each quorum in operator bitmap
        bytes memory operatorRestakedQuorums = BitmapUtils.bitmapToBytesArray(operatorBitmap);
        uint256 strategyCount;
        for(uint256 i = 0; i < operatorRestakedQuorums.length; i++) {
            strategyCount += stakeRegistry.strategyParamsLength(uint8(operatorRestakedQuorums[i]));
        }

        // Get strategies for each quorum in operator bitmap
        address[] memory restakedStrategies = new address[](strategyCount);
        uint256 index = 0;
        for(uint256 i = 0; i < operatorRestakedQuorums.length; i++) {
            uint8 quorum = uint8(operatorRestakedQuorums[i]);
            uint256 strategyParamsLength = stakeRegistry.strategyParamsLength(quorum);
            for (uint256 j = 0; j < strategyParamsLength; j++) {
                restakedStrategies[index] = address(stakeRegistry.strategyParamsByIndex(quorum, j).strategy);
                index++;
            }
        }
        return restakedStrategies;        
    }
```
### getRestakeableStrategies
This function must be implemented in order to have all possible restakeable strategies for that AVS displayed on the UI correctly.  These are the strategies that the AVS supports for restaking.  

```solidity
function getRestakeableStrategies() external view returns (address[] memory) {
        uint256 quorumCount = registryCoordinator.quorumCount();

        if (quorumCount == 0) {
            return new address[](0);
        }
        
        uint256 strategyCount;
        for(uint256 i = 0; i < quorumCount; i++) {
            strategyCount += stakeRegistry.strategyParamsLength(uint8(i));
        }

        address[] memory restakedStrategies = new address[](strategyCount);
        uint256 index = 0;
        for(uint256 i = 0; i < _registryCoordinator.quorumCount(); i++) {
            uint256 strategyParamsLength = _stakeRegistry.strategyParamsLength(uint8(i));
            for (uint256 j = 0; j < strategyParamsLength; j++) {
                restakedStrategies[index] = address(_stakeRegistry.strategyParamsByIndex(uint8(i), j).strategy);
                index++;
            }
        }
        return restakedStrategies;
    }

```


Refer to [ServiceManagerBase.sol](https://github.com/Layr-Labs/eigenlayer-middleware/blob/mainnet/src/ServiceManagerBase.sol) for a reference implementation of the interface.

Proxy and Implementation addresses for AVSDirectory contract are available at EigenLayer Contracts -> [Deployments](https://github.com/Layr-Labs/eigenlayer-contracts/?tab=readme-ov-file#deployments).

To look at EigenDA's AVS-specific deployment -> [Deployments](https://github.com/Layr-Labs/eigenlayer-middleware/tree/dev?tab=readme-ov-file#deployments)

## MetadataURI Format

The metadataURI should follow the format outlined in this [example](https://holesky-operator-metadata.s3.amazonaws.com/avs_1.json). The logo MUST be in PNG format. 

```json
{
    "name": "EigenLabs AVS 1",
    "website": "https://www.eigenlayer.xyz/",
    "description": "This is my 1st AVS",
    "logo": "https://raw.githubusercontent.com/layr-labs/eigendata/master/avs/eigenlabs/logo.png",
    "twitter": "https://twitter.com/eigenlayer"
}
```

Note that for proper rendering of your logo on the UI, the logo _must_ be hosted on GitHub and its reference must point to the raw file as the example above shows. If you need a repo for your logo to be hosted publicly, you can make a PR to the `eigendata` repo and have your logo added: https://github.com/Layr-Labs/eigendata.

## Holesky Dashboard onboarding
Once you've gone through the above steps and you've called the `updateAVSMetadataURI` function, your AVS will be reflected on the Holesky dashboard in about 10 minutes.

## Mainnet Dashboard onboarding
To complete the process of onboarding your AVS on to the mainnet marketplace dashboard, please submit this form: [EigenLayer Mainnet Dashboard Onboarding Form](https://forms.gle/8BJSntA3eYUnZZgs8).
````

## File: docs/developers/HowTo/test/_category_.json
````json
{
  "position": 6,
  "label": "Test"
}
````

## File: docs/developers/HowTo/test/test-avs.md
````markdown
---
sidebar_position: 4
title: Test AVS
---
:::note 
AVS Devnet is currently in Public Alpha and is rapidly being upgraded. Features may be added, removed or otherwise improved or modified,
and interfaces will have breaking changes. To report any issues, raise a [GitHub issue](https://github.com/Layr-Labs/avs-devnet/issues).
:::

Use AVS Devnet to test AVSs locally. AVS Devnet includes: 
* A CLI tool for easy configuration, deployment, and management of local devnets.
* Kurtosis integration to provide a standardized way to spin up local Ethereum environments with core EigenLayer contracts.
* Consensus and Execution clients to simulate production-like environments.
* Block Explorer integration for visualizing blockchain activity using a preconfigured [Blockscout explorer](https://github.com/blockscout/blockscout).
* Funded Operators with keys to enable creation of operator accounts with preloaded funds and private keys for testing staking, delegation, and other interactions.
* Tailored configurations for deployment, testing, and debugging.

To install and use, refer to the [avs-devnet README](https://github.com/Layr-Labs/avs-devnet).
````

## File: docs/developers/HowTo/_category_.json
````json
{
  "position": 1,
  "label": "How To"
}
````

## File: docs/developers/Reference/_category_.json
````json
{
  "position": 1,
  "label": "Reference"
}
````

## File: docs/developers/Reference/avs-developer-best-practices.md
````markdown
---
sidebar_position: 6
title: AVS Developer Security Best Practices
---

## AVS Developer Security Best Practices


- Containers should be able to run with least privilege. Least privilege is AVS-dependent. AVS team should outline these 
privileges as part of the operator onboarding docs. If privileges are not specified, operators need to ask the AVS team directly.
- Emit runtime (logs) including security events
- Use Minimal Base Images
    - Use [ko Go containers](https://ko.build/) or similar to build distro-less minimal images. This reduces the attack surface significantly!
- Release updated images with security patches  (for base OS etc ).
- Do not store key material on the container (refer to key management docs).
- Your default user id should start with AVS-NAME-randomness to ensure there are no conflicts with the host.
- Ensure ECDSA keys utilized by AVS are solely for updates, such as modifying IP and port details within a smart contract. These keys should not hold funds. A role-based approach in smart contract design can address this issue effectively.
- AVS team should [sign their images](https://docs.docker.com/engine/security/trust/) for any releases, including upgrades
    - If they publish to Docker, Docker will show the verified badge next to the image.
    - Tag new releases via updated images.
- Establish communication channels (Discord, TG)  with operators. This ensures coordinating upgrades occurs with minimal friction.
- Operators should be in control of upgrades to their AVS software. Avoid software upgrade patterns where an agent checks for updated software and automatically upgrades the software. 
- Release Notes should explain new features including breaking changes / new hardware requirements etc.




# Key Security Considerations for Developers

When working with keys for nodes in an AVS, it is essential to consider the security aspects associated with key access and decryption. Keys should be encrypted either using a password or passphrase, understanding the unique security concerns posed by different access layers is crucial. By proactively addressing these concerns, you can enhance the overall security and integrity of the keys within your system:

- **Prompt for the passphrase and store it in memory:**
    
    In this scenario, the input must remain hidden to prevent the secret phrase from being stored in the terminal session or used buffer. Attackers might search for this secret in the buffer history. The key should not be stored locally or remotely unless encrypted via the AVS's proprietary methods.
    
- **Request the path to a file containing the passphrase:**
    
    Here, buffer vulnerability issues are absent unless the secret is printed or logged. However, an attacker with access to the machine running the AVS could potentially access this file.
    
- **Retrieve the key remotely:**
    
    Encrypting the validator key offers markedly improved protection when the decryption passphrase is stored remotely. Since the passphrase is not located within the validator client's storage, obtaining an unencrypted key from on-disk data becomes impossible. Instead, an attacker would need to execute considerably more advanced attacks, such as extracting the decrypted key from memory or impersonating the validator client process to receive the decryption key.
    
    Nonetheless, despite the increased difficulty, a sophisticated attack could still potentially acquire the validator key. Moreover, the user may inadvertently sign undesirable messages.
    
- **Utilize remote signers:**
    
    Employing remote signers involves delegating the signing process to an external service or device, which can offer additional security layers. The users are responsible for the availability and security of the remote signers, however, it is crucial to establish secure communication channels and verify the trustworthiness of the remote signer to prevent unauthorized access or tampering.

Supporting both local and remote signer methods is a good practice. 

[Web3signer](https://docs.web3signer.consensys.net/) is a remote signer that includes the following features:

- Open-source signing service developed under the Apache 2.0 license, developed by Consensys, and written in Java. 
- Capable of signing on multiple platforms using private keys stored in an external vault, or encrypted on a disk.
- Can sign payloads using secp256k1 and BLS12-381 signing keys (AWS HSM can't at the moment, spring 2023).
- Web3Signer uses REST APIs, and all the major Ethereum Consensus clients support it.

## Key Management Recommendation for Developers

The AVS can implement a feasible and sufficient method of loading the keys. This is asking for a path to a keystore folder. This keystore needs to follow some structure that AVS knows how to read. Currently [eigenlayer-cli](https://github.com/Layr-Labs/eigenlayer-cli) supports creation of encrypted ecdsa and bn254 keys in the [web3 secret storage](https://ethereum.org/en/developers/docs/data-structures-and-encoding/web3-secret-storage/) format. 


:::note

By keys, we refer to any kind of secret, either in plain text or encrypted.

:::

The path to this keystore folder can be provided via an environment variable or argument.
````

## File: docs/developers/Reference/eigenlayer-sdks.md
````markdown
---
sidebar_position: 1
title: EigenLayer SDKs
---

The EigenLayer SDKs wrap common EigenLayer AVS operations and are designed for AVS developers. 
* [EigenLayer Go SDK](https://github.com/Layr-Labs/eigensdk-go)
* [EigenLayer Rust SDK](https://github.com/Layr-Labs/eigensdk-rs)
````

## File: docs/developers/Reference/resources.md
````markdown
---
sidebar_position: 4
title: Developer Resources
---

:::note

We are in the process of updating our samples, SDKs, and the EigenLayer CLI to include Rewards and Slashing capabilities. The samples, SDKs, and CLI will be
updated as soon as possible. Use the samples now to get familiar with EigenLayer.
For more information on Rewards and Slashing, refer to the [Rewards](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md) and [Slashing](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md) ELIPs,
and [Rewards](../Concepts/rewards.md) and [Slashing](../Concepts/slashing/slashing-concept-developers) documentation.
For questions or support, reach out to us using the Intercom button on the bottom right side of this page or <a href="javascript:void(0)"  id="intercom_trigger_eldocs" >here</a>.
We will promptly follow up with support!

:::

### Developer Samples
* [Awesome AVS](https://github.com/Layr-Labs/awesome-avs)
* [Hello World AVS](https://github.com/Layr-Labs/hello-world-avs)
* [Incredible Squaring AVS](https://github.com/Layr-Labs/incredible-squaring-avs)
* [devQuickstart](https://github.com/Layr-Labs/devQuickstart)

### SDKs
These SDKs are wrappers on top of common EigenLayer AVS operations designed to save you time as an AVS builder:
* [EigenLayer Go SDK](https://github.com/Layr-Labs/eigensdk-go)
* [EigenLayer Rust SDK](https://github.com/Layr-Labs/eigensdk-rs)

### EigenLayer Core Repos
* [EigenLayer Contracts](https://github.com/Layr-Labs/eigenlayer-contracts)
* [EigenLayer Middleware](https://github.com/Layr-Labs/eigenlayer-middleware)
* [EigenLayer CLI](https://github.com/Layr-Labs/eigenlayer-cli)
* [EigenDA](https://github.com/Layr-Labs/eigenda)


### Developer Tooling
- [Othentic](https://www.othentic.xyz) - Library of components for AVS builders.
- [Layer](https://www.layer.xyz/) - Containerized Autonomous Verifiable Services (CAVS) via Web Assembly.
- [AltLayer Wizard](https://wizard.altlayer.io/) - AVS-as-a-Service platform.
- [Gadget](https://github.com/webb-tools/gadget) - A framework for building modular AVS and Tangle Blueprints.
````

## File: docs/developers/_category_.json
````json
{
  "position": 4,
  "label": "Developers"
}
````
