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
- Only files matching these patterns are included: docs/eigenlayer** and docs/developers**
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Security check has been disabled - content may contain sensitive information

## Additional Info

# Directory Structure
```
docs/
  
  eigenlayer/
    archived/
      _category_.json
      arhived-slashing.md
    concepts/
      operator-sets/
        _category_.json
        allocation-deallocation.md
        operator-sets-concept.md
        strategies-and-magnitudes.md
      slashing/
        _category_.json
        safety-delays-concept.md
        slashable-stake-risks.md
        slashing-concept.md
        unique-stake.md
      _category_.json
      keys-and-signatures.md
    deployed-contracts/
      _category_.json
      deployed-contracts.md
    economy/
      _category_.json
      economy-calculation-and-formulas.md
      sidecar.md
    information-and-transparency/
      _category_.json
      disclosures.md
      faq.md
    legal/
      _category_.json
      disclaimers.md
      privacy-policy.md
      terms-of-service.md
    overview/
      _category_.json
      key-terms.md
      README.md
      support.md
      whitepaper.md
    reference/
      _category_.json
      allocation-manager-interface.md
      safety-delays-reference.md
    resources/
      _category_.json
      apis-and-dashboards.md
      infinite-hackathon.md
      learning-resources.md
    rewards-claiming/
      claim-rewards/
        _category_.json
        via-cli.mdx
        via-ui.md
      _category_.json
      rewards-claiming-faq.md
      rewards-claiming-overview.md
      rewards-snapshot-data.md
    security/
      _category_.json
      audits.md
      bug-bounty.md
      guardrails.md
      multisig-governance.md
      withdrawal-delay.md
    _category_.json
    releases.md
    roadmap.md

```

# Files


````markdown
---
sidebar_position: 1
title: (Archived) Slashing
---

For the latest documentation on slashing, refer to [Slashing](../concepts/slashing/slashing-concept.md).

:::note 
Slashing is currently available on the Holesky testnet.
:::

The following is not a complete description of the Slashing and Operator Sets upgrade and is qualified in its entirety by reference to the [ELIP-002: Slashing via Unique Stake & Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md).

The Slashing & Operator Set update adds new protocol interfaces and primitives for Operator Sets, Unique Stake Allocations, and Slashing to provide:

* A new, iterative rewards mechanism for AVSs to reward Operators based on tasks tied to Operator Sets and slashable Unique Stake.
* A mechanism for Operators to allocate and deallocate Unique Stake to and from Operator Sets.
* A slasher function for AVSs to slash an Operator’s Unique Stake allocated to a single Operator Set.

An Operator Set is a logical and segmented set of Operators created by the AVS. These groups of Operators may be split up for whatever reason an AVS can think of. AVSs may assign arbitrary “tasks” to Operator Sets that can represent anything Operators may be asked to do.

Unique Stake is an accounting tool defined on the level of Operator Sets that ensures AVSs and Operators maintain key safety properties when handling staked security and slashing on EigenLayer. Unique Stake is allocated to different Operator Sets on an opt-in basis by Operators. Only Unique Stake is slashable by AVSs, and it represents proportions of the Operator’s delegated stake from Stakers.

When Stakers deposit assets on EigenLayer, they are stored in accounting contracts known as Strategies. Strategies are different expressions of security on EigenLayer that may represent different tokens, for example. In order to make delegations slashable, Operators must allocate individual proportions of their delegated stake as Unique Stake to Operator Sets. Allocations are exclusively slashable by the AVS that created that Operator Set.

## Slashable Stake Risks

Once the Slashing and Operator Sets upgrade is live on mainnet, AVSs may create Operator Sets (which may include slashable Unique Stake) and Operators may allocate their delegated stake to any created Operator Sets. If a Staker is already delegated to an Operator, its stake can become slashable as soon as the Operator opts-in to an Operator Set and allocates Unique Stake. While the allocation of delegated stake to an Operator Set may be subject to the Allocation Config Delay and Allocation Delay, Stakers should consider the increased risk posed by allocation of their delegated stake as slashable Unique Stake to an AVS. To better understand the safety delays for Stakers, please see the Allocation Config Delay and Allocation Delay described in further detail in the [Allocating and Deallocating to Operator Sets section of ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md).

Stakers should review and confirm their risk tolerances for existing and future delegations to Operators and that Operator’s slashable allocations. Stakers will be able to monitor available on-chain data on app.eigenlayer.xyz, such as the creation of Operator Sets, Operator allocations to Operator Sets, the applicable Allocation Delay set by an Operator and the history of slashing activity on app.eigenlayer.xyz.

## Operator Sets

Operator Sets bring the guarantees of Unique Stake to the protocol and commitment-based slashing to AVSs. AVSs may use them to differentiate Operators based on unique business logic, hardware profiles, liveness guarantees, or composition of stake.

To ensure community and incentive alignment, it is generally expected that AVSs will conduct off-chain outreach to communicate the purpose and task/security makeup of their Operator Sets with their Operators and Stakers prior to beginning registration. This likely would include any potential hardware, software, or stake requirements. It is up to the AVS to decide task distribution within an Operator Set.

In order for Operators to join Operator Sets, there is a new registration process handled via the AllocationManager. Both AVSs and Operators will have control over who, how, and when Operators can register. After the instantiation of an Operator Set and its optional registrar contract by an AVS, Operators can begin the registration process. The AVS may use the registrar contract to apply additional business logic to joining Operators to Operator Sets. These may be conditions like amounts of allocated stake that ensure an Operator has adequate Unique Stake applied as security before being registered and assigned tasks.

##  Registration

The registration flow follows the pattern illustrated below and is completed in one or two transactions depending on the requirements set by the AVS:

![](/img/operator-guides/operator-registration-allocation.png)

##  Unique Stake Allocation & Deallocation

Note: Please review the entire [Unique Stake Allocation & Deallocation ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md) in detail. The following is not a complete description of the Slashing and Operator Sets upgrade and is qualified in its entirety by reference to the [Unique Stake Allocation & Deallocation ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#unique-stake-allocation--deallocation).

Unique Stake is a mechanism to guarantee slashable stake can only be allocated to one Operator Set at a time. Unique Stake benefits both Operators and AVSs with certain properties when reasoning about EigenLayer slashing:

* **Greater control over slashing risk:** With Unique Stake, the risk of slashing is isolated to the individual AVS and Operator Set, and Operators can control how much of their stake any AVS can slash. AVSs are not exposed to risk from any other AVS or their slashings.
* **Guaranteed slashable stake:** AVSs have the tools they need to understand the amount of Unique Stake that can be slashed at a given time across their Operator Sets.
* **Permissionless onboarding of AVSs:** Since slashing is localized to individual AVSs, there is no need for a common veto committee which means launching an AVS on EigenLayer remains permissionless.

When Stakers deposit assets on EigenLayer, they are stored in accounting contracts known as Strategies. Strategies are different expressions of security on EigenLayer. Today, they represent different types of restaked assets (e.g., tokens) delegated to Operators that AVSs can leverage for securing their services and upholding cryptoeconomic guarantees. In order to make delegations slashable, Operators must allocate individual proportions of them as Unique Stake to Operator Sets. Allocations are exclusively slashable by the AVS that created that Operator Set.

In figure 1, Operator 1 has a delegation of 100 staked ETH for the ETH Strategy. Operator 1 then allocates proportions of that ETH as Unique Stake in Operator Sets across several AVSs.



![Operator Allocations to Operator Sets](/img/operator-guides/operator-sets-figure-3.png)  
***Figure 1: Operator Allocations to Operator Sets***

The 85 allocated ETH is slashable exclusively by the AVS originating each Operator Set. In this case, AVS 2, 3, and 4 can slash their associated Operator Sets 2, 3, and 4, respectively.

Let’s consider another example with three Operators. Figure 2 illustrates two Operator Sets instantiated by AVS 1. AVS 1 has created two Operator Sets for different tasks. For example, this AVS may use Operator Set 1 for assigning generation of ZK proofs to Operators, an expensive computation, and Operator Set 2 for verification of those proofs, a cheaper computation.

![Example of an AVS's Unique Stake](/img/operator-guides/operator-sets-figure-4.png)  
***Figure 2: Example of an AVS’s Unique Stake***

Operator 1 is registered to Operator Set 1 but has not yet allocated any Unique Stake. Operator 2 has allocated 10% of its ETH delegation to Operator Set 1 (amounting to a nominal allocation of 10 ETH). This is exclusively slashable by AVS 1 in Operator Set 1. Operator 2 has also allocated 5% (5 ETH) to Operator Set 2, which is exclusively slashable by AVS 1 in Operator Set 2.

Along with Operator 3’s 20% allocation (20 ETH), Operator Set 1 has a total Unique Stake of 30 ETH available to slash, with the certainty it cannot be slashed elsewhere. Operator Set 2 has allocations totalling 15 ETH of Unique Stake. AVS 1 may distribute more valuable tasks against which to slash and reward to Operator Set 1 in order to take advantage of the greater amount of Unique Stake.

### Allocating and Deallocating to Operator Sets

Unique Stake is allocated to Operator Sets in the protocol via a function provided in the `AllocationManger`. In the `AllocationManager`, these allocations are tracked using an accounting tool known as magnitudes.

For each Strategy, an Operator starts with a protocol-defined Total Magnitude of 1x10^18 (`INITIAL_TOTAL_MAGNITUDE`). This Total Magnitude can never increase; to account for slashing events originated by an AVS, the protocol *monotonically decreases* the Strategy’s total magnitude for the slashed Operator. Operators can allocate magnitudes to Operator Sets using the `modifyAllocations` function. The proportion of an Operator’s delegation assigned as Unique Stake to an Operator Set is equal to the magnitude allocated to that Operator Set divided by the Operator’s Total Magnitude. For a given strategy, the sum of all magnitude allocations can never be greater than the Total Magnitude (the sum of the proportions cannot exceed 100%), ensuring the property of Unique Stake that no two Operator Sets can slash the same stake.

Below is an example of an Operator Magnitude allocation for the EIGEN Strategy. This will be expanded upon in the next section.

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 3,000 | 30% | 30 |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 2,500 | 25% | 25 |
| `Total` | 10,000 | 100% | 100 |

In this example, the Operator submitted one transaction to allocate to three Operator Sets simultaneously for the Eigen strategy using the `modifyAllocations` function. It allocated various magnitudes across AVSs in the Operator’s EIGEN Strategy and uses a total magnitude of 10,000 as opposed to 1x1018 for legibility.

Allocations and deallocations are subject to some safety delays in the protocol. The delays vary from protocol-configured constants to custom constraints that Operators can add for additional safety. They are instantiated in the `AllocationManager` alongside the other constants:

| Parameter | Description | Value | Setter & Configuration |
| :---- | :---- | :---- | :---- |
| `ALLOCATION_CONFIG_DELAY` | The amount of blocks between an Operator queuing an `ALLOCATION_DELAY` change and the change taking effect. | 126000 blocks (~17.5 days) | Core Protocol: Set via governance |
| `ALLOCATION_DELAY` | The amount of blocks it takes for an Operator’s allocation to be live in an Operator Set for a given Strategy. It must be set by the Operator prior to any allocations and applies globally to all Operator Sets and Strategies.  The protocol provides no constraints on this value. It can be any unsigned integer value and can be changed by the Operator.  | Unsigned integer value representing a number of blocks  | Operator: Set via `AllocationManager` Must be set in order to allocate |
| `DEALLOCATION_DELAY` | The amount of blocks between an Operator queuing a deallocation of stake from an Operator Set for a strategy and the deallocation taking effect. This delay also applies to an Operator *deregistering* from an Operator Set, either by their own action or that of the AVS. | 100800 blocks (~14 days) | Core Protocol: Set via governance |
| `INITIAL_TOTAL_MAGNITUDE` | The initial value of the monotonically decreasing total magnitude for every Operator for every strategy. This is set high enough to start out with a large level of precision in magnitude allocations and slashings. | 1e18 | Core Protocol: Constant, unlikely to change |
| `WITHDRAWAL_DELAY` | The amount of blocks between a Staker queueing a withdrawal and the withdrawal becoming non-slashable and completable. | 100800 blocks (~14 days) | Core Protocol: Set via governance |

Before allocating for their first Operator Set, an Operator is required to set an `ALLOCATION_DELAY` in the `AllocationManager`. If an Operator is registering with EigenLayer for the first time, they will be required to provide an `ALLOCATION_DELAY` during registration. It takes the amount of time specified in the `ALLOCATION_CONFIG_DELAY` for the Operator's `ALLOCATION_DELAY` to be set initially or updated. This delay is to ensure Stakers have time to adjust to changes in their delegated Operator’s stake allocations. Stakers can withdraw their funds if an allocation is viewed as undesirable, subject to the `WITHDRAWAL_DELAY`.

The `AllocationManager` interface handles all allocation and deallocation signals:

```solidity
interface IAllocationManager {

   /**
    * @notice struct used to modify the allocation of slashable magnitude to an operator set
    * @param operatorSet the operator set to modify the allocation for
    * @param strategies the strategies to modify allocations for
    * @param newMagnitudes the new magnitude to allocate for each strategy to this operator set
    */
   struct AllocateParams {
       OperatorSet operatorSet;
       IStrategy[] strategies;
       uint64[] newMagnitudes;
   }

   /**
     * @notice Called by the delegation manager OR an operator to set an operator's allocation delay.
     * This is set when the operator first registers, and is the number of blocks between an operator
     * allocating magnitude to an operator set, and the magnitude becoming slashable.
     * @param operator The operator to set the delay on behalf of.
     * @param delay the allocation delay in blocks
     */
    function setAllocationDelay(
	address operator, 
	uint32 delay
    ) external;

    /**
     * @notice Modifies the proportions of slashable stake allocated to an operator set  from a list of strategies
     * Note that deallocations remain slashable for DEALLOCATION_DELAY blocks therefore when they are cleared they may
     * free up less allocatable magnitude than initially deallocated.
     * @param operator the operator to modify allocations for
     * @param params array of magnitude adjustments for one or more operator sets
     * @dev Updates encumberedMagnitude for the updated strategies
     * @dev msg.sender is used as operator
     */
    function modifyAllocations(
	address operator, 
	AllocateParams[] calldata params
    ) external;

    /**
     * @notice struct used to modify the allocation of slashable magnitude to an operator set
     * @param operatorSet the operator set to modify the allocation for
     * @param strategies the strategies to modify allocations for
     * @param newMagnitudes the new magnitude to allocate for each strategy to this operator set
     */
    struct AllocateParams {
        OperatorSet operatorSet;
        IStrategy[] strategies;
        uint64[] newMagnitudes;
    }

    /**
     * @notice This function takes a list of strategies and for each strategy, removes from the deallocationQueue
     * all clearable deallocations up to max `numToClear` number of deallocations, updating the encumberedMagnitude
     * of the operator as needed.
     *
     * @param operator address to clear deallocations for
     * @param strategies a list of strategies to clear deallocations for
     * @param numToClear a list of number of pending deallocations to clear for each strategy
     *
     * @dev can be called permissionlessly by anyone
     */
    function clearDeallocationQueue(
        address operator,
        IStrategy[] calldata strategies,
        uint16[] calldata numToClear
    ) external;
}
```

Magnitude allocations can only be made to valid Operator Sets and only from non-slashable magnitude. Allocations are queued in the `AllocationManager` and become active automatically after the `ALLOCATION_DELAY`. Magnitude is not allocated until the Operator’s `ALLOCATION_DELAY` has passed, i.e. the allocation is not pending. Additional magnitude allocations cannot be made from existing queued allocations, magnitude already allocated to an Operator Set, or pending deallocations.

***An AVS may slash an Operator up to the total allocated amount of Unique Stake per Strategy under the following conditions:***

* ***The Operator is registered to the Operator Set the AVS wishes to slash.***
* ***The Operator Set is configured to include the allocated strategy.***

***Deallocations are the primary means of making Unique Stake non-slashable.*** ***Operators should handle allocations to registered Operator Sets as if they can be slashed at any time.*** For example, AVSs may add or remove Strategies to Operator Sets at will, which may instantly make any allocated strategy slashable. Deregistration from an Operator Set is another such case. An Operator is slashable by that Operator Set for the duration of the `DEALLOCATION_DELAY` after a deregistration, but the allocations to that Operator Set _will still exist._ If the Operator re-registers after the delays have elapsed, those Operator Set allocations immediately become slashable again.

Deallocations act similarly to allocations and are queued in the `AllocationManager` and take effect automatically after the `DEALLOCATION_DELAY`. This is a globally set constant across all Operators and Operator Sets. This delay allows AVSs to update their view of Unique Stake to reflect the Operator’s reduced allocation and guarantees appropriate delays for tasks to remain slashable. Queued deallocations *cannot* be canceled. After the delay, this stake is considered non-slashable.

Some notes and caveats impacting UX:

* If an allocation to an Operator Set is made non-slashable by no longer meeting the criteria above, a deallocation does not go through the 14 day `DEALLOCATION_DELAY` and instead takes effect immediately.
* A given (Operator, Strategy) pair can only have one pending allocation *OR* deallocation transaction per Operator Set at a given time.
* A single transaction can modify multiple allocations.
* An Operator Set deregistration ***does not*** also queue a deallocation. They have to be queued separately, as a deregistration may be used to signal other states, like a period of Operator inactivity. Previously allocated magnitude that has not been deallocated becomes instantly slashable upon re-registration.

### Magnitude Allocation Flow

An illustrative example of these magnitudes is useful in showing the allocation flow. Suppose, after initial delays, the Operator’s queued allocations are applied for delegated tokens in the EIGEN strategy according to the following magnitudes:

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 3,000 | 30% | 30 |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 2,500 | 25% | 25 |
| `Total`  | 10,000 | 100% | 100 |

The above represents non-slashable and slashable stake, by Operator Set (in this case one per AVS, but this may be multiple sets). Now presume a deallocation is queued by the Operator signaling a reduction in the allocation to Operator Set AVS_1_EIGEN. The number of delegated tokens does not change, but their proportions do.

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` (*pending deallocation*) | 3,000 -> *2,000* | 30% -> *20%* | 30 -> *20* |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 2,500 | 25% | 25 |
| `Total`  | 10,000 | 100% | 100 |

The 10 EIGEN in reduced magnitude is still considered slashable until the deallocation is complete. The below is the result following the deallocation and its associated delays. Note the non-slashable stake increase.

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 20 |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 3,500 | 35% | 35 |
| `Total`  | 10,000 | 100% | 100 |

Now, a deposit occurs for an additional 100 EIGEN by a Staker who has delegated to the Operator. Instantly, that deposit is applied, following the proportions laid out in the allocation magnitudes.

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 40 |
| `AVS_2_EIGEN` | 2,500 | 25% | 50 |
| `EigenDA_EIGEN` | 2,000 | 20% | 40 |
| `Non-slashable` | 3,500 | 35% | 70 |
| `Total`  | 10,000 | 100% | 200 |

Each Operator Set’s slashable stake and the overall non-slashable stake increase commensurately. This example is expanded in [this forum post](https://forum.eigenlayer.xyz/t/the-mechanics-of-allocating-and-slashing-unique-stake/13870#p-143651-allocation-3) with more details. We will reference this example again later in the context of slashing.

### Deposits, Delegations, & Withdrawals

Magnitude allocations make a proportion of an Operator’s delegated stake slashable by an AVS. As a result, new delegations and deposits are immediately slashable by the same proportion. There is no "activation delay". There is no change in the deposit and delegation interface.

Withdrawals and undelegation, like deallocations and deregistrations, are slashable for the `WITHDRAWAL_DELAY` after they are queued and automatically become unslashable after the delay has passed. The escrow process remains unchanged: withdrawals must be queued and completed in separate transactions. When the withdrawal is completed, slashings are applied to the stake received.



## Slashing of Unique Stake

With Unique Stake allocated to Operator Sets, AVSs can begin assigning slashable tasks with economic commitments from their Operators. It is key to AVS designs to consider what is a slashable offense and to effectively communicate these conditions with Operators and Stakers.

**The protocol provides a slashing function that is maximally flexible; an AVSs may slash any Operator within any of their Operator Sets for any reason.**  Slashing does not have to be objectively attributable (i.e., provable on-chain), but AVSs are encouraged to create robust legibility and process around individual slashings. It is expected that governance, fraud proofs, decentralization, and more shall be considered in AVS slashing designs. Other delays and veto periods may be included in AVS designs to avoid or cancel slashing in cases of AVS implementation bugs, improper slashing, or fraud, but **no vetoes** are provided by the EigenLayer protocol.

The `AllocationManager` provides the interface for the slashing function:

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

To slash, AVSs specify the individual Operator that will be slashed, the Operator Set, the list of Strategies that will be slashed, the list of proportions to slash (as `wads` or “parts per `1e18`”), and a description for legibility. For example, an 8% slash would be represented as `8e16`, or `80000000000000000` as expected in the `wadsToSlash` parameter. A 25% slash, or `2.5e17`, the contract will expect `250000000000000000` as `wadsToSlash`. The indexes in the two arrays should match across `strategies` and `wadsToSlash`.

All Strategies supplied must be configured as part of the Operator Set. For all Strategies specified, the Operator’s allocations to that Operator Set will be slashed by the corresponding proportion while maintaining their nominal allocations to all other Operator Sets. Under the hood this is accomplished by subtracting allocated magnitude from both the specified Operator Set, and the Operator’s Total Magnitude. This is illustrated in the example below.

Slashing proportionally reduces funds of all Stakers of the given Strategies that are delegated to the Operator, including funds in queued deallocations and withdrawals (that haven’t passed `WITHDRAWAL_DELAY`). Operator delegation is decreased directly in the `DelegationManager` in each Strategy. Changes are propagated to Staker withdrawals and view functions by referring to their delegated Operator’s Total Magnitude.

When a slashing occurs, an event is emitted onchain, one for each slashing. Details are emitted identifying the Operator slashed, in what Operator Set, and across which Strategies, with fields for the proportion slashed and meta-data:
```
/// @notice Emitted when an operator is slashed by an operator set for a strategy
/// `wadSlashed` is the proportion of the operator's total delegated stake that was slashed
event OperatorSlashed(
    address operator, OperatorSet operatorSet, IStrategy[] strategies, uint256[] wadSlashed, string description
);
```

Returning to our example from above, let’s assume that `AVS_1_Eigen` Operator Set slashes the Operator in question by 50%. Recall the final allocated magnitudes were the following:

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 40 |
| `AVS_2_EIGEN` | 2,500 | 25% | 50 |
| `EigenDA_EIGEN` | 2,000 | 20% | 40 |
| `Non-slashable` | 3,500 | 35% | 70 |
| `Total`  | 10,000 | 100% | 200 |

Now, `AVS_1` slashes the Operator for a 50% reduction (`5e17` in `wads`) in the Operator Set `AVS_1_EIGEN`:

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 1,000 | 11% | 20 |
| `AVS_2_EIGEN` | 2,500 | 28% | 50 |
| `EigenDA_EIGEN` | 2,000 | 22% | 40 |
| `Non-slashable` | 3,500 | 39% | 70 |
| `Total` | 9000 | 100% | 180 |

Note, slashing by one Operator Set does not affect the magnitudes of EIGEN allocated to other Operator Sets. The interactions between Staker, Operator, AVS, and core contracts are represented visually in the sequence diagram below:

![Sequence Representation of a Slashing](/img/operator-guides/operator-sets-figure-5.png)  
***Figure 5: Sequence Representation of a Slashing***

### Burning of Slashed Funds

In this release, when funds are slashed by an AVS, the EigenLayer core contracts will make slashed funds permanently inaccessible (“burned”). ERC-20s have this done by sending them to the dead `0x00...00e16e4` address. This is done to ensure proper accounting with various LRT protocols. Natively Restaked ETH will be locked in EigenPod contracts, permanently inaccessible. The Ethereum Pectra upgrade is anticipated to unblock development of an EigenLayer upgrade which would burn Natively Restaked ETH by sending it to a dead address, instead of permanently locking it within EigenPod contracts as planned in this release.
````

## File: docs/eigenlayer/concepts/operator-sets/_category_.json
````json
{
  "position": 2,
  "label": "Operator Sets"
}
````

## File: docs/eigenlayer/concepts/operator-sets/allocation-deallocation.md
````markdown
---
sidebar_position: 4
title: Allocation and Deallocation
---

## Allocations

Allocations are made by magnitude and can only be made:
* To valid [Operator Sets](operator-sets-concept).
* From non-slashable [magnitude](strategies-and-magnitudes).

Allocations are not made until the Operator [`ALLOCATION_DELAY`](../../reference/safety-delays-reference.md) has passed (that is, the allocation is not pending). Allocations
cannot be made from an of:
* Existing queued allocations
* Magnitude already allocated to an Operator Set
* Pending deallocations.

## Deallocations

Deallocations are similar to allocations and are not made until the Operator [`DEALLOCATION_DELAY`](../../reference/safety-delays-reference.md) has passed (that is, the 
deallocation is not pending). After the delay, the stake is non-slashable. The delay:
* Enables AVSs to update their view of [Unique Stake](../slashing/unique-stake.md) to reflect the Operator’s reduced allocation.
* Guarantees appropriate delays for tasks to remain slashable.

Queued deallocations cannot be canceled. Deallocations happen immediately (that is, the `DELLOCATION_DELAY` does not apply) 
if the Operator is not registered to the AVS, or the strategy being deallocated is not part of the Operator Set.

If an Operator deregisters, the Operator remains slashable for the `DEALLOCATION_DELAY` period following the deregistration. 
After the deregistration, the allocations to that Operator Set still exist, and if the Operator re-registers, those Operator 
Set allocations immediately become slashable again. That is, a deregistration does not queue a deallocation.

Each Operator/ Strategy pair can have only one pending allocation or deallocation transaction per Operator Set at a time. 
A single transaction can modify multiple allocations.
````

## File: docs/eigenlayer/concepts/operator-sets/operator-sets-concept.md
````markdown
---
sidebar_position: 1
title: Operator Sets Overview
---

:::note
Operator Sets are currently available on the Holesky testnet. [ELIP-002 Slashing via Unique Stake & Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
introduced Operator Sets.

Currently, Operators register to an AVS to earn rewards in the AVSDirectory. We recommend existing AVSs [migrate to Operator Sets on testnet](../../../developers/HowTo/build/slashing/migrate-to-operatorsets.md). 
:::

Operator Sets determine which Operators secure an AVS and earn rewards. Each AVS defines one or more Operator Sets that
Operators may opt into. The opted in Operators are the set of Operators that are responsible for securing that service.
By opting into the Operator Set for an AVS, Operators gain access to the AVS rewards, and the AVS slashing risks.

AVSs group Operators into Operator Sets based on unique business logic, hardware profiles, liveness guarantees, or composition 
of stake. Operators use Operator Sets to allocate and deallocate [Unique Stake](../slashing/unique-stake.md). AVSs use Operator Sets to assign tasks to Operator 
Sets to perform the service provided by the AVS.

Operators are responsible for ensuring that they fully understand the slashing conditions and slashing risks of AVSs before 
opting into an Operator Set and allocating  stake to the Operator Set, as once allocated, those funds may be slashable 
according to any conditions set by that AVS. 

## For AVS Developers

For information on designing Operator Sets, refer to [Design Operator Sets](../../../developers/HowTo/slashing/design-operator-set.md).

## For Operators

For information on allocating to Operator Sets, refer to [Allocate and Register to Operator Set](../../../operators/howto/operator-sets.md).
````

## File: docs/eigenlayer/concepts/operator-sets/strategies-and-magnitudes.md
````markdown
---
sidebar_position: 4
title: Strategies and Magnitudes
---

:::note
Operator Sets are currently available on the Holesky testnet. [ELIP-002 Slashing via Unique Stake & Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
introduced Operator Sets.
:::

Magnitudes are the accounting tool used to track Operator allocations to [Operator Sets](operator-sets-concept). Magnitudes represent proportions 
of an Operator’s delegations for a specific Strategy.

Strategies are the accounting tool used to track Stakers deposited assets. Strategies are expressions of security on EigenLayer. 
For example, a strategy may represent a specific token.

For each Strategy:
* An Operator starts with a protocol-defined Total Magnitude of 1x10^18 (`INITIAL_TOTAL_MAGNITUDE`).
* The sum of all of an Operator’s Magnitudes cannot exceed the `INITIAL_TOTAL_MAGNITUDE`.
* The protocol consistently decreases the Strategy’s total magnitude for the slashed Operator to account for slashing events originated by an AVS.

The proportion of an Operator’s delegation assigned as Unique Stake to an Operator Set is equal to the magnitude allocated 
to that Operator Set divided by the Operator’s Total Magnitude. The sum of all magnitude allocations never being greater 
than the Total Magnitude ensures the property of Unique Stake. That is, ensures that no two Operator Sets can slash the same stake.

## Example

The table displays an example of an Operator Magnitude allocation for the EIGEN Strategy. The table represents slashable 
and non-slashable stake by Operator Set.

For legibility, the example uses a total magnitude of 10,000 instead of 1x1018.

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 3,000 | 30% | 30 |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 2,500 | 25% | 25 |
| `Total` | 10,000 | 100% | 100 |

The Operator deallocates 10 EIGEN to AVS_1_EIGEN. The following is the result and the non-slashable stake increases. 

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 20 |
| `AVS_2_EIGEN` | 2,500 | 25% | 25 |
| `EigenDA_EIGEN` | 2,000 | 20% | 20 |
| `Non-slashable` | 3,500 | 35% | 35 |
| `Total`  | 10,000 | 100% | 100 |

A Staker who has delegated to the Operator deposits 100 EIGEN. The following is the results and Magnitudes and proportions 
stay the same and the EIGEN for each Operator Set increases. 

|  | Magnitude | Proportion | EIGEN |
| :---- | :---- | :---- | :---- |
| `AVS_1_EIGEN` | 2,000 | 20% | 40 |
| `AVS_2_EIGEN` | 2,500 | 25% | 50 |
| `EigenDA_EIGEN` | 2,000 | 20% | 40 |
| `Non-slashable` | 3,500 | 35% | 70 |
| `Total`  | 10,000 | 100% | 200 |
````

## File: docs/eigenlayer/concepts/slashing/_category_.json
````json
{
  "position": 1,
  "label": "Slashing"
}
````

## File: docs/eigenlayer/concepts/slashing/safety-delays-concept.md
````markdown
---
sidebar_position: 4
title: Safety Delays
---

:::important
When the Slashing and Operator Set upgrade is live on mainnet, stake can become slashable for a Staker that has previously 
delegated stake to an Operator. Stakers are responsible for ensuring that they fully understand and confirm their risk tolerances 
for existing and future delegations to Operators and the Operator’s slashable allocations. Additionally, stakers are responsible 
for continuing to monitor the allocations of their chosen Operators as they update allocations across various Operator Sets.
:::

Safety delays are applied when allocating or deallocating to prevent rapid stake movements. Safety delays:
* Ensure stability. Delays ensure gradual transitions when stake is being allocated or dellocated enabling AVSs to adjust to changes in Operator security.
* Reduce risks from slashing. Delays ensure that staked assets remain at risk for a period after deallocation preventing the withdrawal of stake immediately before a slashing event to avoid slashing penalties.
* Preventing stake cycling to collect rewards. Delays ensure commitment periods to securing an AVS.

For more information on provided safety delays, refer to the [Safety Delays reference](../../reference/safety-delays-reference).
````

## File: docs/eigenlayer/concepts/slashing/slashable-stake-risks.md
````markdown
---
sidebar_position: 4
title: Slashable Stake Risks
---

:::important
When the Slashing and Operator Set upgrade is live on mainnet, stake can become slashable for a Staker that has previously 
delegated stake to an Operator. Stakers are responsible for ensuring that they fully understand and confirm their risk tolerances 
for existing and future delegations to Operators and the Operator’s slashable allocations. Additionally, stakers are responsible 
for continuing to monitor the allocations of their chosen Operators as they update allocations across various Operator Sets.
:::

When the Slashing and Operator Sets upgrade is live on mainnet, AVSs can create [Operator Sets](../operator-sets/operator-sets-concept) that may include slashable 
[Unique Stake](unique-stake.md), and Operators can allocate their delegated stake to Operator Sets. If a Staker has previously delegated stake 
to an Operator, the delegated stake becomes slashable when the Operator opts into an Operator Set and allocates Unique Stake.

Stakers are responsible for understanding the increased risk posed by allocation of their delegated stake as slashable 
Unique Stake to an AVS. While the allocation of delegated stake to an Operator Set may be subject to the [Allocation Config 
Delay and Allocation Delay](../../reference/safety-delays-reference.md), it is important to understand the increased risk.

For more information on the safety delays for Stakers, refer to the :
* [Safety Delays reference](../../reference/safety-delays-reference.md)
* [Allocating and Deallocating to Operator Sets section of ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#unique-stake-allocation--deallocation).
````

## File: docs/eigenlayer/concepts/slashing/slashing-concept.md
````markdown
---
sidebar_position: 1
title: Slashing Overview
---

:::note
Slashing is currently available on the Holesky testnet. The Slashing release implements [ELIP-002: Slashing via Unique Stake & Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md).
:::

Slashing is a type of penalty determined by an AVS as a deterrent for broken commitments by Operators. Broken commitments
may include improperly or inaccurately completing tasks assigned in [Operator Sets](../operator-sets/operator-sets-concept) by an AVS. 
Slashing results in a burning/loss of funds. AVSs can only slash an Operator’s [Unique Stake](unique-stake.md) allocated to a single Operator Set.

An AVS may slash an Operator up to the total allocated amount of Unique Stake per [Strategy](../operator-sets/strategies-and-magnitudes) under the following conditions:
* The Operator is registered to the Operator Set the AVS wishes to slash.
* The Operator Set is configured to include the allocated strategy.
* All applicable safety and time delays have passed.

:::important
The EigenLayer protocol provides a slashing function that is maximally flexible. That is, AVSs may slash any Operator that
has delegated stake to that AVS within any of their Operator Sets. AVSs have flexibility to design their protocols to slash
for any reason. Slashing does not have to be objectively attributable (that is, provable onchain), but AVSs are encouraged to
create robust legibility and process around how their slashing is designed and individual slashing events. Operators are responsible
for ensuring that they fully understand the slashing conditions and slashing risks of AVSs before delegating stake to them, as once
delegated, those funds may be slashable according to the conditions set by that AVS.
:::

## Slashing sequence

The interactions between Staker, Operator, AVS, and core contracts during a slashing are represented in the sequence diagram.

![Sequence Representation of a Slashing](/img/operator-guides/operator-sets-figure-5.png)

## Burning slashed funds

When funds are slashed by an AVS, the EigenLayer core contracts make slashed funds permanently inaccessible (burned).
ERC-20s have this done by sending them to the dead 0x00...00e16e4 address. The dead address is used to ensure proper
accounting with various LRT protocols.

Natively Restaked ETH will be locked in EigenPod contracts, permanently inaccessible. The Ethereum Pectra upgrade is anticipated
to unblock development of an EigenLayer upgrade which would burn Natively Restaked ETH by sending it to a dead address, instead
of permanently locking it within EigenPod contracts as planned in this release.

## For AVS Developers 

For information on:
* AVS security models and slashing, refer to [AVS Security Models](../../../developers/Concepts/avs-security-models.md). 
* Design considerations for slashing, refer to [Design Operator Sets](../../../developers/HowTo/slashing/design-operator-set.md) and [Design Slashing Conditions](../../../developers/HowTo/slashing/slashing-veto-committee-design.md).
* Implementing slashing, refer to [Implement Slashing](../../../developers/HowTo/slashing/implement-slashing.md).

## For Operators

For information on allocating to Operator Sets, refer to [Allocate and Register to Operator Set](../../../operators/howto/operator-sets.md).
````

## File: docs/eigenlayer/concepts/slashing/unique-stake.md
````markdown
---
sidebar_position: 3
title: Unique Stake
---

:::note
Operator Sets and Slashing are currently available on the Holesky testnet. [ELIP-002 Slashing via Unique Stake & Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
introduced Operator Sets and Slashing.
:::

Unique Stake ensures AVSs and Operators maintain key safety properties when handling staked security and slashing on EigenLayer. 
Unique Stake is allocated to different [Operator Sets](../operator-sets/operator-sets-concept) on an opt-in basis by Operators. Only Unique Stake is slashable by AVSs, 
and the Unique Stake represents proportions of the Operator’s delegated stake from Stakers. Unique Stake allocations are 
exclusive to one Operator Set and solely slashable by the AVS that created that Operator Set.

Benefits of Unique Stake to Operators and AVSs include:
* Greater control over slashing risk. The risk of slashing is isolated to the individual AVS and Operator Set, and Operators 
control how much of their stake any AVS can slash. AVSs are not exposed to risk from other AVSs or their slashings.
* Guaranteed slashable stake. AVSs can understand the amount of Unique Stake that can be slashed at a given time across their Operator Sets.
* Permissionless onboarding of AVSs. There is no need for a common veto committee because slashing is localized to individual AVSs. 
No need for a common veto committee means launching an AVS on EigenLayer is permissionless.

## Example 1

Operator 1 has a delegation of 100 staked ETH. Operator 1 allocates proportions of that ETH as Unique Stake in Operator Sets 
across several AVSs. The 85 allocated ETH is slashable exclusively by the AVS for each Operator Set. That is, AVS 2, 3, and 4 
can slash their associated Operator Sets 3, 4, 5, and 6 respectively.

<img src="/img/operator-guides/operator-sets-figure-3.png" width="75%" style={{ margin: '50px'}}>
</img>

## Example 2

AVS 1 has two Operator Sets for different tasks. AVS 1 uses Operator Set 1 for assigning generation of ZK proofs to Operators, 
an expensive computation, and Operator Set 2 for verification of those proofs, a cheaper computation.

Operator 1 is registered to Operator Set 1 but has not allocated any Unique Stake. Operator 2 has allocated 10% of its ETH
delegation to Operator Set 1 (10 ETH). The 10% allocation by Operator 2  is exclusively slashable by AVS 1 in Operator Set 1. 
Operator 2 has also allocated 5% (5 ETH) to Operator Set 2, which is exclusively slashable by AVS 1 in Operator Set 2.

Including the 20% allocation from Operate 3 (20 ETH), Operator Set 1 has a total Unique Stake of 30 ETH available to slash. 
The Unique Stake of 30 ETH cannot be slashed elsewhere. Operator Set 2 has allocations totalling 15 ETH of Unique Stake. 
The Unique Stake of 15 ETH cannot be slashed elsewhere. AVS 1 may distribute more valuable tasks against which to reward and 
slash to Operator Set 1 to take advantage of the greater amount of Unique Stake.

<img src="/img/operator-guides/operator-sets-figure-4.png" width="75%" style={{ margin: '50px'}}>
</img>
````

## File: docs/eigenlayer/concepts/_category_.json
````json
{
  "position": 4,
  "label": "Concepts"
}
````

## File: docs/eigenlayer/concepts/keys-and-signatures.md
````markdown
---
sidebar_position: 3
title: Keys and Signatures
--- 

In the EigenLayer ecosystem, signatures play a crucial role in ensuring the integrity and authenticity of operations. 
Signatures cryptographically confirm that a specific address has signed a given message (for example, a string value)
with its private key. 

:::warning
Poor key management can lead to compromized operators, network disruptions, or financial losses. Key Management Best 
Practices are outlined for [Institutional Operators](../../operators/howto/managekeys/institutional-operators.md) and
[Solo Stakers](../../operators/howto/managekeys/solo-operators.md).
:::

## Operator Keys

An Operator has two types of keys:
* A single Operator key used to authenticate to the EigenLayer core contracts.
* Multiple AVS keys used to sign messages for AVSs.

:::warning
As security best practice, Operators should:
* Not reuse their Operator key as an AVS signing key.
* Not reuse their Ethereum key for EigenLayer operations if they are also Ethereum stakers.
* Use a different key for every AVS.
:::

The Operator key must be an ECDSA key and is used for actions including registering to EigenLayer, changing Operator parameters,
and force undelagating a staker. 

Always interact with with the EigenLayer core contracts using the [eigenlayer-cli](https://github.com/Layr-Labs/eigenlayer-cli) or other operator-built tools. 

Do not load a Operator key into any AVS software. If authorizing any action programmatically triggered on the AVS contracts 
use an AVS key, not the Operator key.

For information on key management best practices, refer to [Key Management Best Practices for Node Operators](../../operators/howto/managekeys/institutional-operators.md).

## AVS Signing Keys

AVS keys are used by AVS software run by Operators to sign messages for AVSs. The required AVS key type is specified by the AVS, and is most
commonly BN254. 

## BLS and ECDSA Signature Types

The primary signatures types used in EigenLayer are BLS12-381 (Boneh-Lynn-Shacham), BN254 (Barreto-Naehrig), and ECDSA (Elliptic Curve Digital Signature Algorithm).

| Feature                   | BLS12-381                                                              | BN254                                                                 | ECDSA                                                                 |
|:--------------------------|:-----------------------------------------------------------------------|:----------------------------------------------------------------------|:----------------------------------------------------------------------|
| **Signature Size**        | 48 bytes (BLS12-381 curve)                                             | 32 bytes (BN254 curve)                                                | ~64 bytes (secp256k1)                                                 |
| **Key Size**              | 32 bytes                                                               | 32 bytes                                                              | 32 bytes                                                              |
| **Signature Aggregation** | Supports native aggregation.  Single operation for multiple signatures | Supports native aggregation. Single operation for multiple signatures | Not natively aggregatable. Each signature must be verified separately |
| **Gas Cost in Ethereum**  | Higher for single signatures, lower for aggregated                     | Lower than BLS12-381                                                  | Lower initially but increases with more signatures                    |

Until the Pectra upgrade, BN254 remains the cheaper option. After the upgrade, the cost of verifying the more secure BLS12-381
signature will decrease, making migration to this cheaper and more secure signature type viable for developers.

The native aggregation offered by BLS, combining multiple operator signatures into one, reduces onchain storage needs, 
verification time, and gas costs. BLS signatures require a slightly more complex implementation that includes an aggregator entity.
Given the reduction in storage, verification time, and gas costs, we recommend the of BLS signatures for production systems.

**Note:** As of [eigenlayer-middleware v0.2.1](https://github.com/Layr-Labs/eigenlayer-middleware/releases/tag/v0.2.1-mainnet-rewards), the [ECDSAServiceManagerBase contract](https://github.com/Layr-Labs/eigenlayer-middleware/blob/v0.2.1-mainnet-rewards/src/unaudited/ECDSAServiceManagerBase.sol) was not yet fully audited. Please check the most recent release as this is expected to change.
````

## File: docs/eigenlayer/deployed-contracts/_category_.json
````json
{
  "position": 9,
  "label": "Contract Addresses and Docs"
}
````

## File: docs/eigenlayer/deployed-contracts/deployed-contracts.md
````markdown
---
sidebar_position: 1
title: Contract Addresses and Docs
---

## EigenLayer Core Restaking Contracts

The EigenLayer core contracts are located in this repo: [`Layr-Labs/eigenlayer-contracts`](https://github.com/Layr-Labs/eigenlayer-contracts). They enable restaking of liquid staking tokens (LSTs) and beacon chain ETH to secure new services, called AVSs (Autonomous Verifiable Services).

### Deployment Addresses

An up-to-date reference of our current mainnet and testnet contract deployments can be found in the core repository README: [`eigenlayer-contracts/README.md#deployments`](https://github.com/Layr-Labs/eigenlayer-contracts?tab=readme-ov-file#deployments).

### Technical Documentation

Our most up-to-date contract-level documentation can be found in the core repository's docs folder: [`eigenlayer-contracts/docs`](https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/docs).
````

## File: docs/eigenlayer/economy/_category_.json
````json
{
  "position": 8,
  "label": "Data and Metrics"
}
````

## File: docs/eigenlayer/economy/economy-calculation-and-formulas.md
````markdown
---
sidebar_position: 1
title: Economy Calculation and Formulas
---

## Overview

EigenLayer strives to do its best and provide legibility and transparency to users.

Therefore, we built a website to show the critical metrics of the network, metrics that we deem
important for users to understand the protocol and its performance. Please see the Eigen Economy site at **[economy.eigenlayer.xyz](https://economy.eigenlayer.xyz/)**.


## Data Quality and Reconciliation

As a foundation to showcase EigenLayer's economy, we provide the best data quality possible by indexing data from Ethereum directly and reconciling each data point with other independent sources to guarantee the most accurate and up-to-date information.


## Data Freshness

Please refer to each metric below for their data freshness.


## Economy Metrics


### ETH TVL / EIGEN TVL / Total TVL in USD

Definition: Dollar value of total assets staked/restaked in EigenLayer, including all ETH (LSTs and native ETH), EIGEN tokens, and all other permissionless assets restaked.

Formula:

1. For all strategies' TVL in EigenLayer, except the beacon strategy (aka, native-ETH strategy) and EIGEN strategy:

- Index strategies in EigenLayer from all `StrategyAddedToDepositWhitelist` events minus `StrategyRemovedFromDepositWhitelist` events from the `StrategyManager` contract, which will include all strategies except the beacon strategy.
- For each strategy in EigenLayer, get their underlying tokens.
- Convert underlying tokens to token amounts via token decimals, `token amount = underlying token / power(10, token decimals)`.
- Multiply token amounts by the corresponding token's pricing from Coingecko, and sum them up.
    - Note that some tokens may lack pricing data on Coingecko; these will be excluded from the TVL in USD calculation.


2. For the beacon strategy:

- Index all `PodDeployed` events from the `EigenPodManager` contract.
- For each EigenPod, query the beacon chain to check which validators have pointed their withdrawal credentials to the pod.
    - Withdrawal credentials will be of the format: `0x010000000000000000000000 + <eigen_pod_address>`
    - Note: Multiple validator withdrawal credentials can point to a single EigenPod.
- For each EigenPod, get all its validators' ETH balance
- Sum up all validators balance, multiply by ETH pricing from Coingecko

- Note:
    - This approach is also [adopted by defillama](https://github.com/DefiLlama/DefiLlama-Adapters/blob/1e921c7ab6684500cfd73b6890713f495ba28f2a/projects/eigenlayer/index.js#L13)     
    - We will consider in the future to switch to use [EigenPod Upgrade](https://www.blog.eigenlayer.xyz/introducing-the-eigenpod-upgrade/) native data, to remove dependency on beacon chain data and be more close to rest strategies


3. For EIGEN strategy:

Follow the same steps in 1, with exception that EIGEN strategy is backed by bEIGEN (Backing EIGEN) token instead of
EIGEN token.
Coingecko only provides EIGEN token pricing, so we need to use EIGEN token pricing multiply by bEIGEN token amounts
to calculate TVL in USD for EIGEN strategy.


4. Sum up above 3 values to get the total TVL in USD for EigenLayer, or use them separately for ETH TVL and EIGEN TVL


Data Sources: Ethereum events, ERC20 contracts, Beacon Chain data, Coingecko
Data Fresh Frequency: Every 1 hour



### # of Restakers¹

Definition: Number of addresses staked/restaked in EigenLayer

Formula:

- Index `OperatorSharesIncreased` and `OperatorSharesDecreased` events from `DelegationManager` contract.
- Calculate delegation balance for each staker.
- Count # of unique stakers who has non-zero balance on at least 1 strategy.

Data Sources: Ethereum events
Data Fresh Frequency: Every 1 hour


### # of EIGEN Holders

Definition: Number of unique addresses that hold EIGEN tokens.

Formula:

- Index all `Transfer` events from EIGEN token contract.
- Calculate EIGEN token balance for each wallet address.
- Count # of unique addresses that have non-zero EIGEN token balance.

Data Sources: Ethereum events
Data Fresh Frequency: Every 1 hour



### % of ETH Restaked

Definition: Percentage of total ETH that is restaked, out of ETH circulating supply.

Formula:

- Index `OperatorSharesIncreased` and `OperatorSharesDecreased` events from `DelegationManager` contract.
- Calculate total delegated ETH amount for all ETH strategies, by convert shares to underlying tokens by strategy's ratio of shares to underlying token, then convert underlying tokens amount to tokens amount with token decimals `token amount = underlying token / 1e18`.
- Divide total delegated ETH amount by ETH circulating supply from Coingecko.

Data Sources: Ethereum events, Coingecko
Data Fresh Frequency: Every 1 hour



### % of circulating EIGEN staked

Definition: Percentage of circulating supply of EIGEN that is staked. This excludes locked tokens that are staked.

Formula:

 Index `OperatorSharesIncreased` and `OperatorSharesDecreased` events from `DelegationManager` contract.
- Calculate total delegated EIGEN amount for EIGEN strategy, by converting shares amount to underlying tokens amount as 1:1, then convert underlying tokens amount to tokens amount with token decimals `token amount = underlying token / 1e18`.
- Subtract the amount of locked tokens that are staked from the total delegated EIGEN amount.
- Divide the adjusted EIGEN amount by EIGEN circulating supply from Coingecko.

Data Sources: Ethereum events, Coingecko
Data Fresh Frequency: Every 1 hour



### Total Rewards Earned

Definition: Dollar value of total rewards earned in EigenLayer.

Formula:

- Index all `AVSRewardsSubmissionCreated` and `RewardsSubmissionForAllEarnersCreated` events from the `RewardsCoordinator` contract.
- For each rewards submission, get the token amount by converting `amount` with the reward token decimals.
- Multiply the token amount by the corresponding token's pricing from Coingecko, and sum them up.

Data Sources: Ethereum events, ERC20 contracts, Coingecko
Data Fresh Frequency: Every 1 hour



### Total AVS FDV

Definition: US Dollar value of all AVS Token FDVs

Note: EIGEN is not counted in the AVS FDV calculation.

Formula: 

- Retrieve tokens from all Mainnet AVSs that have an associated token.
- For each token, obtain its FDV (Fully Diluted Valuation) from Coingecko.
- Sum up the FDVs of all tokens to get the total AVS FDV.

Data Sources: Coingecko
Data Fresh Frequency: Every 1 hour



### Restakers Funnel

Definition: The funnel of restakers in EigenLayer, which includes the number of restakers who restaked (delegated) more than \$1M, \$50M, and \$100M cumulatively.

Formula:

- Index `OperatorSharesIncreased` and `OperatorSharesDecreased` events from the `DelegationManager` contract.
- For each restaker, get their delegated shares amount to date, convert shares to underlying tokens by strategy's ratio of shares to underlying token, then convert to tokens amount via token decimals, then convert to USD amount by multiplying with the corresponding token pricing from Coingecko.
- Sum up all USD value of delegated tokens for each restaker, count them by \$1M, \$50M, and \$100M thresholds.
- Cumulate thresholds, meaning the number of restakers who delegated more than \$1M includes that of who delegated more than \$50M and \$100M, the number of restakers who delegated more than \$50M includes that of who delegated more than \$100M.

Data Sources: Ethereum events, ERC20 contracts, Coingecko.
Data Fresh Frequency: Every 1 hour.



### Operators Funnel

Definition: The funnel of operators in EigenLayer, which includes the number of operators who:
 1. Registers on EigenLayer.
 2. Are active (registers to at least one AVS on EigenLayer with delegated shares larger than 0 in ETH or EIGEN strategies) on EigenLayer.
 3. Have earned rewards.


Formula:

- `Registered Operators`: Index `OperatorMetadataURIUpdated` event from the `AVSDirectory` contract, count the number of unique operator addresses registered
- `Active Operators`:
    - Index `OperatorAVSRegistrationStatus` event from the `AVSDirectory` contract, count the number of unique operator addresses who are registered to at least 1 AVS.
    - Index `OperatorSharesIncreased` and `OperatorSharesDecreased` events from `DelegationManager` contract, count the number of operators who have shares larger than 0 in any of ETH and EIGEN strategies, and also registered to at least 1 AVS, as "number of active operators".
- `Operators that have earned rewards`: Count number of operators above who have earned rewards, by querying rewards data published (see `rewards` section for details).

Data Sources: Ethereum events, EigenLayer rewards data
Data Fresh Frequency: Every 1 hour.



### AVSs Funnel


Definition: The funnel of AVSs in EigenLayer, which includes AVSs who:
1. Are in development on EigenLayer testnet and mainnet.
2. Are active by having at least 1 active operator registered to it on EigenLayer mainnet.
3. Have distributed rewards to operators and stakers on EigenLayer mainnet.

Note this is the only metric that contains data from testnet, all other metrics are for mainnet only.

Formula:
- `AVSs in Development`: Use data across mainnet, testnet and private channels.
- `Active AVSs`: Count number of AVSs who have at least 1 "active operator" registered to it on EigenLayer mainnet.
- `AVSs that have distributed rewards`: Index `avs_reward_submission_created` event from the `RewardCoordinator` contract, count number of AVSs who have also have distributed rewards to operators and stakers, and also in above "active AVSs" list.

Data Sources: Ethereum events from testnet and mainnet, private data.
Data Fresh Frequency: Every 1 hour.


 ¹ _The number of restakers reflects the various ways LRT holders create EigenPods. As a result, many users of LRT platforms may appear as one or a few wallets in the data. This metric aims to provide insight into the LRT-holders' participation._
````

## File: docs/eigenlayer/economy/sidecar.md
````markdown
---
sidebar_position: 2
title: Sidecar
---

## Overview

The EigenLayer Sidecar is an open source, permissionless, verified indexer enabling anyone (AVS, operator, etc) to access EigenLayer's protocol in real-time.

Sidecar provides the following benefits to users:
- Access to EigenLayer protocol data through easy-to-use APIs.
- Running your own Sidecar allows you to validate rewards roots posted on chain by being able to re-create them.
- Direct database access gives power-users the ability to explore protocol data directly and natively.

## How to Use Sidecar

Please see the [README.md documentation here](https://github.com/Layr-Labs/sidecar?tab=readme-ov-file#eigenlayer-sidecar).
````

## File: docs/eigenlayer/information-and-transparency/_category_.json
````json
{
  "position": 7,
  "label": "Information and Transparency"
}
````

## File: docs/eigenlayer/information-and-transparency/disclosures.md
````markdown
---
sidebar_position: 2
title: Disclosures Related to Employee and Investor Staking
---

# Disclosures Related to Employee and Investor Staking

***Last Revised on September 30, 2024***

### EMPLOYEE AND INVESTOR LOCKUP ON EIGEN

EIGEN provided by Eigen Labs to its employees and [Investors](https://www.eigenlabs.org/#investors) is subject to the following lockup schedule: 4% of each recipient’s EIGEN will unlock each month starting September 2025 and an additional 4% will unlock each month thereafter, such that all EIGEN will be unlocked in September 2027 (the “**Lockup Schedule**”).

### EMPLOYEE AND INVESTOR STAKING ON EIGENLAYER

It was [previously communicated](https://blog.eigenfoundation.org/announcement/) that Investors and Early Contributors would be on the above Lockup Schedule. We want to clarify Eigen Labs company policies with respect to staking EIGEN and other assets and any EIGEN rewards:

#### Employees:
- **EIGEN staking**: Eigen Labs prohibits its current and former employees from staking any EIGEN received from Eigen Labs on EigenLayer until at least September 30th, 2025. 
- **Other assets staking**: Eigen Labs does not restrict its employees from staking other assets on EigenLayer (including ETH and LSTs), and any rewards received (including EIGEN) from such staking will not be subject to the Lockup Schedule.  
- **Stakedrops**: Eigen Labs employees were not permitted to claim stakedrops.

#### Investors:
- ***EIGEN staking**: Eigen Labs [Investors](https://www.eigenlabs.org/#investors) are not restricted from staking EIGEN on EigenLayer. As such, investors may choose to stake their EIGEN and receive staking rewards the same as any other user. EIGEN provided by Eigen Labs to investors is subject to the Lockup Schedule, but EIGEN investors receive from staking will not be subject to the Lockup Schedule. 
  - Note, as previously communicated, Investors did not receive rewards or airdrop allocation for any staking of EIGEN prior to September 30, 2024. 
- **Other assets staking**: Eigen Labs does not restrict [Investors](https://www.eigenlabs.org/#investors) from staking other assets on EigenLayer (including ETH and LSTs), and any rewards received (including EIGEN) from such staking will not be subject to the Lockup Schedule.
- **Stakedrops**: Investors were not restricted from claiming stakedrops.

*25% programmatic incentives go to EIGEN staking while the remaining 75% go to ETH and ETH-equivalent staking assets.

In addition to the above disclosures, we also encourage you to review our [Privacy Policy](../legal/privacy-policy.md) and our [Terms of Service](../legal/terms-of-service.md).  The above policies and disclosures are subject to change at any time for any reason and without notice.
````

## File: docs/eigenlayer/information-and-transparency/faq.md
````markdown
---
sidebar_position: 1
title: FAQ
---

# FAQ

## General Questions

**Is EigenLayer a DeFi protocol?**

No, EigenLayer is not a DeFi protocol. EigenLayer is a platform to bootstrap new proof of stake (PoS) systems. Through the EigenLayer protocol, users CANNOT engage in any financial activities such as swapping and lending. 

However, for the decentralized services built on top of EigenLayer (we call them AVSs, Autonomous Verifiable Services), these services could be DeFi applications themselves or support key functionalities in other DeFi protocols. These AVSs are external to the EigenLayer contracts.

Moreover, another class of protocols built on top of EigenLayer is called liquid restaking token (LRT). They function similarly to liquid staking tokens. They are permissionlessly built on EigenLayer. We urge users to do their own research before interacting with LRTs.

**Does EigenLayer reuse the same economic security?**

Yes, EigenLayer does reuse the same economic security by enabling different AVSs to share a common economic security base. This concept is known as 'pooled security.' Similar to how various dApps share Ethereum's economic security, EigenLayer allows different decentralized PoS networks share the same economic security base.

Pooling allows various AVSs to contribute to a larger, shared economic security base, enhancing economic safety. This is because the cost to compromise any individual AVS increases significantly. For instance, consider 100 AVSs each with \$1B in economic security versus 100 AVSs sharing \$100B in economic security. The cost to attack any single AVS in the pooled system has effectively multiplied by 100.

In addition to pooled security, EigenLayer will let each AVS acquire additional attributable security. Unlike pooled security, attributable security is specific to each AVS and will only be slashable by one AVS. This arrangement can provide game-theoretic guarantees for AVS customers. 

Imagine a bridge protocol with a shared security base of \$10B and an attributable portion of \$10B. If a malfunction occurs, the protocol can assure the AVS customer that up to \$10B can still be securing the bridge through the attributable part. Attributable security also enables the flexible scaling of economic security. As an AVS, you can adjust your security budget based on user demand, catering to varying user needs at different times.

Furthermore, EigenLayer aims to achieve economies of scale by allowing AVSs to share the same underlying smart contract infrastructure. If a DeFi application uses services from five different AVSs and requests \$100B in economic security, these AVSs can collectively purchase attributable security instead of each buying \$100B individually. This is feasible because if any of them fail, the \$100B could be compensated to the DeFi protocol.

In conclusion, EigenLayer will not only allow AVSs to share economic security but will also enable individual AVSs to acquire additional attributable security. The combination of pooled and attributable security allows EigenLayer to flexibly and efficiently scale economic security.

**Why is EigenLayer's contract licensed under BSL?**

EigenLayer's core contract operates under BSL due to the numerous market forces at play. EigenLayer is not isolated; it operates within a broader ecosystem. Innovators can utilize the comprehensive decentralized trust network of the Ethereum economy with EigenLayer, eliminating the need for new token creation. 

Without the need for a new token, this makes it easy for other entities, particularly those with extensive product distribution, to replicate and utilize the code. This situation applies to AVS building on EigenLayer and EigenLayer itself.

In scenarios where major LSTs have a much larger distribution and no new token is required for new networks, it becomes crucial to establish boundaries to safeguard open innovation. This protection ensures that individual LSTs do not consume open innovation and that existing protocols are not easily copying AVS designs. Consequently, EigenLayer's core operates under the BSL, transitioning to the MIT (Massachusetts Institute of Technology) license after two years, a practice also followed by some AVSs.

We are also exploring new licensing models that combine the benefits of protected licensing, which ensures value accumulation for innovators with permissionless innovation, which allows the creation of derivatives without needing explicit permission. Until we finalize these new models, we will continue using the tried-and-tested BSL model. If Ethereum core developers express an interest in enshrining EigenLayer before the BSL expires, we are more than open to collaboration as well.




# Staker Related
**Is EigenLayer rehypothecating my staked tokens?**

No, EigenLayer does not engage in rehypothecation. 

Rehypothecation is the practice that allows collateral posted by, say, a hedge fund to its prime broker to be used again as collateral by that *prime broker* for its own funding.

Rehypothecation in traditional finance implies:
1) Depositor no longer have direct control of their assets.
2) Depositors have little say in the financial activities conducted by the intermediary.
3) Depositors are subject to substantial liquidity and counterparty risks.

Whereas in EigenLayer:
1) Stakers retain full control over their staked tokens.
2) Stakers have autonomy over the services they wish to validate.
3) Stakers are not exposed to counterparty liquidity risks.

There are clear differences between EigenLayer and rehypothecation.

## AVS Related

**Doesn't EigenLayer remove all the utility from an AVS network token?**

No, EigenLayer doesn't remove the value of any hypothetical AVS tokens, but adds more value to it.

Firstly, Ethereum's L2 networks serve as a greatest counterpoint to this line of logic. Despite outsourcing their network security to Ethereum, L2 tokens maintain significant value within their respective networks.

Secondly, if an individual AVS decides to incorporate its own token for staking and network operations, EigenLayer supports [dual staking](https://www.blog.eigenlayer.xyz/dual-staking/). In this setup, an AVS can be secured by two types of assets, likely one in ETH and one in its native token.

It's important to note that in the dual staking model, AVS governance always has the ability to adjust the balance between AVS's native token and ETH within its network. 

During the initial phase, an AVS may choose to rely more heavily on ETH, giving it greater weight to mitigate the inherent risk of relying solely on its native token, which could potentially lead to a downward spiral. However, as the AVS matures and gains more adoption, the governance can adjust the balance to give more weight to the AVS token, thereby increasing its value.

These points provide individual AVS with more options than the current status quo. Building on EigenLayer, therefore, doesn't negate the utility of an AVS token. Instead, through these additional options, we enhance their value.

**Given the answer above, does it mean EigenLayer provides little value beyond the bootstrapping phase?**

No, EigenLayer offers significant value at any stage of the AVS's economic security phase.

Firstly, the use of ETH as the shared security set inherently reduces the endogenous risk associated with a native AVS token.

Secondly, as the AVS ecosystem matures, interoperability between different AVSs and the resulting synergy will become a crucial factor enabling AVSs to leverage EigenLayer.

Thirdly, the flexible security that EigenLayer provides proves beneficial in handling security demand shocks for AVSs. If an AVS needs to secure a large customer transaction suddenly, it can quickly access the necessary security.

Finally, building on the synergy point, as AVSs begin to serve more customers collectively, the economic scale of insurance will enable AVSs to meet their customers' need for cryptoeconomic security more affordably.

## LRT Related

**What roles does LRTs serve?**

LRTs safeguard EigenLayer and AVSs from external financial risks tied to staked positions. In EigenLayer, stakers don't receive a transferable receipt post-staking. Despite this, we expect some stakers to try and financialize their positions. 

In the absence of LRTs, if a financialized staker's position gets liquidated, the staked tokens must be withdrawn from EigenLayer. This is the case because staked position cannot be transferred, negatively affecting AVS's economic security. However, with LRTs, withdrawals become unnecessary. The liquidator can simply assume the staker's position.

This feature is especially crucial during substantial market downturns. As the price of ETH falls, so does the system's total economic security. If liquidation leads to more ETH leaving EigenLayer, it would worsen the downward spiral. LRTs can significantly mitigate this risk, thereby protecting both AVS and EigenLayer from potential financial risks related to staker positions.


**Is there an LRT looping risk that would potentially cause potential liquidation cascade?**

With the recent Permissionless Token upgrade to the protocol, it is up to the AVSs to determine the risks of the collateral they accept for restaking.

In lending markets, looping LRTs may occur based on the risk analysis of individual LRTs. We advise caution when taking this step as it involves financial leverage and exposes LRT holders to significant risks.

Looping LRTs in lending markets can lead to a cascade of liquidations. This risk is limited to individual lending markets and does not affect the security of EigenLayer. This is similar to the stETH depeg incident in 2022. During the event, the price risk of stETH is contained within the lending market, and Ethereum consensus remains unaffected.

## Ethereum Related


**Does EigenLayer increase centralization pressure on the validator set?**

If EigenLayer imposes additional computational demands on Ethereum validators, it unavoidably becomes a centralization vector for Ethereum. With this principle in mind, EigenLayer discourages the increase in node requirements for Ethereum validators and promotes the use of lightweight AVSs, such as [EigenDA](https://twitter.com/eigen_da).

For more computationally demanding AVSs, EigenLayer's delegation features allow Home stakers to secure Ethereum while delegating the operation of other AVSs to a different operator.

Ongoing research is focused on mechanisms, such as the [Optimistic Delegation Framework](https://research.eigenlayer.xyz/t/optimistic-delegation-framework-idea-to-allow-for-native-restaking-without-delegation/39) and [AVS for PBS](https://www.youtube.com/watch?v=7xCqa_Ufv0A), to reduce the computational load on Ethereum validators in a trust-minimized manner.

From an economic standpoint, we are exploring the possibility of directly incentivizing individual stakers through a rewards sharing scheme. Consider, for instance, allocating a certain percentage (x%) of all rewards processed through EigenLayer to solo validators.

If you are interested in discussing these mechanisms and incentive issues further, please share your thoughts on [research.eigenlayer.xyz](http://research.eigenlayer.xyz)

**Does EigenLayer eat away into the security margin of Ethereum?**

No, EigenLayer does not compromise Ethereum's security margin.

When the stake in EigenLayer is significantly less than the total staked ETH, it does not impact Ethereum's security.

If the stake in EigenLayer makes up a large part of the total staked ETH, EigenLayer doesn't put the entire stake at risk. Instead, it sets aside a portion as a buffer for Ethereum, offering only the remaining stake for security. This approach ensures that we can consistently safeguard Ethereum's security margin.

For a better understanding of the required security level for Ethereum, we suggest readers refer to [StakeSure](https://arxiv.org/abs/2401.05797).

**Does EigenLayer need to self-limit like other liquid staking protocols?**

No, EigenLayer doesn't require self-limiting.

The danger of a liquid staking protocol owning a substantial portion of the network is the potential imposition of a specific validator set on the Ethereum network. This risk doesn't apply to EigenLayer. Even if all staked ETH is restaked through EigenLayer, it doesn't control Ethereum's validator set. That control remains solely with Ethereum's stakers and validators.

If EigenLayer self-limits, the surplus restaking demand will likely revert to liquid staking protocols, increasing centralization pressure at the protocol's base layer. If EigenLayer didn't exist, dominant LST protocols would likely offer restaking services only to their internal stakers. These centralized restaking services could considerably strengthen the dominant LST's central position. More restaking services mean additional yield, leading to further consolidation of the LST. This scenario resembles the situation with MEV, where large stakers, including professional operators and LSTs, can negotiate MEV order flow guarantees that smaller stakers can't participate in.

In contrast to the centralized restaking mechanisms mentioned above, EigenLayer is developing a credibly neutral, decentralized restaking platform. All participants, whether small or large LSTs, small or large node operators, or institutional and home stakers, can all participate in additional validation opportunities. This approach helps prevent a monopoly in LST or LRT, fostering decentralization and neutrality among node operators.

EigenLayer doesn't define an operator set, so it doesn't threaten Ethereum's censorship resistance or operator decentralization. 

Self-limiting EigenLayer offers no significant benefit. Instead, if EigenLayer proves successful in the market, it could even be integrated into the Ethereum protocol as a native service.

**Does EigenLayer experience the principal-agent problem between stakers and operators?**

EigenLayer, as with other PoS protocols, does experience the principal-agent problem. However, our goal is to minimize this issue as much as possible.

It's important to highlight that EigenLayer operators never retain custody of the staker's tokens, which limits the potential for malicious activities to only slashing events.

We have established several strategies to further limit the operator's capacity for harmful actions:
1. Technical Trust: EigenLayer operators can utilize an anti-slasher within a Trusted Execution Environment (TEE) to verify its response to any validation task. The operator then sends the associated TEE certificate with its response for the task, confirming that the anti-slasher was utilized. Another technical solution to address the principal-agent problem is to structure the EigenLayer operator with decentralized DVT nodes.
2. Economic Trust: Similar to RocketPool, a specific LRT protocol might require operators to stake proportionally to the delegation they receive. This approach assumes a certain amount that can be slashed and a probability for that occurrence. By encouraging the operator nodes to behave correctly, the principal-agent problem can be mitigated.
3. Social Trust: A staker always has the option to self-delegate and operate the nodes for AVSs or delegate it to a trusted operator. In this scenario, the staker trusts the operator to perform only the stipulated operations and not engage in any malicious activities. This practice is common in the real world, where service providers like AWS offer services.
````

## File: docs/eigenlayer/legal/_category_.json
````json
{
  "position": 8,
  "label": "Legal"
}
````

## File: docs/eigenlayer/legal/disclaimers.md
````markdown
---
sidebar_position: 3
title: Disclaimers
---

# Disclaimers

***Last Revised on September 30, 2024***

## LEGAL DISCLAIMERS

All Eigen Labs, Inc. (“**Eigen Labs**”) blog posts, social media posts and accounts, forum posts, podcasts, speeches, videos, documentation, website copy, including [www.eigenlayer.xyz](https://www.eigenlayer.xyz), [www.eigenda.xyz](https://www.eigenda.xyz), and [www.eigenlabs.org](https://www.eigenlabs.org), or other content (collectively “**Content**”) are for entertainment and informational purposes only and do not necessarily express the views of Eigen Labs or any of its employees or contractors. The Content may contain hypothetical, forward-looking, incomplete, or incorrect information, which are not guaranteed and are subject to change. No Content, whether oral or written, from Eigen Labs or its employees or contractors, should be construed as a representation or warranty, express or implied, of any kind whatsoever. You should not rely on any Content as advice of any kind, including legal, investment, financial, tax or other professional advice, and the Content is not a substitute for advice from a qualified professional.

Any Content should not be construed as an offer to sell or the solicitation of an offer to purchase any token, financial instrument or security, and is not an offering, advertisement, solicitation, confirmation, statement, or any financial promotion that can be construed as an invitation or inducement to engage in any investment activity or similar.
````

## File: docs/eigenlayer/legal/privacy-policy.md
````markdown
---
sidebar_position: 1
title: Privacy Policy
---

# Privacy Policy

***Last Revised on March 20, 2024***

This Privacy Policy for Eigen Labs, Inc. ("Company", "we", "us" "our") describes how we collect, use and disclose information about users of the Company's website (eigenlayer.xyz), and any related services, tools and features, including the EigenLayer service (collectively, the "Services"). For the purposes of this Privacy Policy, "you" and "your" means you as the user of the Services. ​ Please read this Privacy Policy carefully. By using, accessing, or downloading any of the Services, you agree to the collection, use, and disclosure of your information as described in this Privacy Policy. If you do not agree to this Privacy Policy, please do not use, access or download any of the Services. ​

## UPDATING THIS PRIVACY POLICY

​We may modify this Privacy Policy from time to time in which case we will update the "Last Revised" date at the top of this Privacy Policy. If we make material changes to the way in which we use information we collect, we will use reasonable efforts to notify you (such as by emailing you at the last email address you provided us, by posting notice of such changes on the Services, or by other means consistent with applicable law) and will take additional steps as required by applicable law. If you do not agree to any updates to this Privacy Policy please do not access or continue to use the Services. ​

## COMPANY'S COLLECTION AND USE OF INFORMATION

​ When you access or use the Services, we may collect certain categories of information about you from a variety of sources, which comprises: ​

- The following information about you: name, email address, and Discord Tag. We collect your email address and Discord Tag in order to communicate with you through the Services and through third party platforms, such as Discord.
- Information included in any identity documents you provide to us, including without limitation driver’s license or passport number, date of birth and/or country of residence. We collect this in limited circumstances for the purposes of identification of the jurisdiction of residence of certain users or as otherwise needed to satisfy certain regulatory obligations.
- The following third-party wallet ("Wallet") information: public wallet address and token holdings. We collect third-party Wallet information in order to facilitate your use of the Services. ​
- Any other information you choose to include in communications with us, for example, when sending a message through the Services. ​ 

We also automatically collect certain information about your interaction with the Services ("Usage Data"). To do this, we may use cookies, web beacons/clear gifs and other geolocation tracking technologies ("Tracking Technologies"). Usage Data comprises of: ​
- Device information (e.g., unique device identifier, device type, IP address, operating system) ​
- Browser information (e.g., browser type) ​
- Location information (e.g., approximate geolocation) ​
- Other information regarding your interaction with the Services (e.g., log data, date and time stamps, clickstream data, ​ We use Usage Data to tailor features and content to you and to run analytics and better understand user interaction with the Services. For more information on how we use Tracking Technologies and your choices, see the section below, Cookies and Other Tracking Technologies. ​ In addition to the foregoing, we may use any of the above information to comply with any applicable legal obligations, to enforce any applicable terms of service, and to protect or defend the Services, our rights, and the rights of our users or others. ​

## HOW THE COMPANY SHARES YOUR INFORMATION

​In certain circumstances, the Company may share your information with third parties for legitimate purposes subject to this Privacy Policy. Such circumstances comprise of: ​

- With vendors or other service providers, such as ​
- Blockchain analysis service providers, including Chainanalysis ​
- Data analytics vendors, including Google Analytics ​
- To comply with applicable law or any obligations thereunder, including cooperation with law
  enforcement, judicial orders, and regulatory inquiries ​
- In connection with an asset sale, merger, bankruptcy, or other business transaction ​
- To enforce any applicable terms of service ​
- To ensure the safety and security of the Company and/or its users ​
- When you request us to share certain information with third parties, such as through your use of login integrations ​
- With professional advisors, such as auditors, law firms, or accounting firms ​

## COOKIES AND OTHER TRACKING TECHNOLOGIES

​Do Not Track Signals ​ Your browser settings may allow you to transmit a "Do Not Track" signal when you visit various websites. Like many websites, our website is not designed to respond to "Do Not Track" signals received from browsers. To learn more about "Do Not Track" signals, you can visit [http://www.allaboutdnt.com/.](http://www.allaboutdnt.com) ​ Cookies and Other Tracking Technologies ​ Most browsers accept cookies automatically, but you may be able to control the way in which your devices permit the use of cookies, web beacons/clear gifs, other geolocation tracking technologies. If you so choose, you may block or delete our cookies from your browser; however, blocking or deleting cookies may cause some of the Services, including any portal features and general functionality, to work incorrectly. If you have questions regarding the specific information about you that we process or retain, as well as your choices regarding our collection and use practices, please contact us using the information listed below. ​ To opt out of tracking by Google Analytics, click [here](https://tools.google.com/dlpage/gaoptout). ​ Your browser settings may allow you to transmit a "Do Not Track" signal when you visit various websites. Like many websites, our website is not designed to respond to "Do Not Track" signals received from browsers. To learn more about "Do Not Track" signals, you can visit [http://www.allaboutdnt.com/.](http://www.allaboutdnt.com) ​

## SOCIAL NETWORKS AND OTHER THIRD PARTY WEBSITES AND LINKS

​We may provide links to websites or other online platforms operated by third parties, including third-party social networking platforms, such as Twitter, Discord, or Medium, operated by third parties (such platforms are "Social Networks"). If you follow links to sites not affiliated or controlled by us, you should review their privacy and security policies and other terms and conditions. We do not guarantee and are not responsible for the privacy or security of these sites, including the accuracy, completeness, or reliability of information found on these sites. Information you provide on public or semi-public venues, including information you share or post on Social Networks, may also be accessible or viewable by other users of the Services and/or users of those third-party online platforms without limitation as to its use by us or by a third party. Our inclusion of such links does not, by itself, imply any endorsement of the content on such platforms or of their owners or operators, except as disclosed on the Services. ​

## THIRD PARTY WALLET EXTENSIONS

​Certain transactions conducted via our Services, will require you to connect a Wallet to the Services. By using such Wallet to conduct such transactions via the Services, you agree that your interactions with such third party Wallets are governed by the privacy policy for the applicable Wallet. We expressly disclaim any and all liability for actions arising from your use of third party Wallets, including but without limitation, to actions relating to the use and/or disclosure of personal information by such third party Wallets.

## CHILDREN'S PRIVACY

​Children under the age of 18 are not permitted to use the Services, and we do not seek or knowingly collect any personal information about children under 13 years of age. If we become aware that we have unknowingly collected information about a child under 13 years of age, we will make commercially reasonable efforts to delete such information from our database. ​ If you are the parent or guardian of a child under 13 years of age who has provided us with their personal information, you may contact us using the below information to request that it be deleted. ​

## DATA SECURITY

​Please be aware that, despite our reasonable efforts to protect your information, no security measures are perfect or impenetrable, and we cannot guarantee "perfect security." Please further note that any information you send to us electronically, while using the Services or otherwise interacting with us, may not be secure while in transit. We recommend that you do not use unsecure channels to communicate sensitive or confidential information to us. ​

## HOW TO CONTACT US

Should you have any questions about our privacy practices or this Privacy Policy, please email us at notices@eigenlabs.org or contact us at 15790 Redmond Way #1176, Redmond, WA 98052 .
````

## File: docs/eigenlayer/legal/terms-of-service.md
````markdown
---
sidebar_position: 2
title: Terms of Service
---

# Terms of Service
***Last Revised on August 29, 2024***

These Terms of Service (these "**Terms**") explain the terms and conditions by which you may access and use our websites, www.eigenlayer.xyz (the "**EigenLayer Website**") and www.eigenda.xyz (together with the EigenLayer Website, the "Websites"), operated by or on behalf of Eigen Labs, Inc. ("**Company**", "**we**" or "**us**"). Our Websites, our App (as defined below), our EigenLayer App testnet ("**Testnet**"), our application programming interfaces ("**APIs**"), and any content, tools, documentation, features and functionality offered on or through the Websites or App are collectively referred to herein as the **"Services"**. Effective as of August 29, 2024, the Websites and Services do not include the following subdomains of the EigenLayer Website: forum.eigenlayer.xyz (the **“Forum”**), the research forum available at  research.eigenlayer.xyz  (**"Research"** and collectively with the Forum, and any other subdomains that contain a terms of service indicating the are operated by EigenFoundation, the **"Third-Party Subdomains"**), each including any lower-level domains and any content, tools, documentations, features and functionality offered therein. As of August 29, 2024, the Third-Party Subdomains are operated by or on behalf of EigenFoundation or its subsidiaries, not us, and your access and use of the Foundation Website after such date is governed by the terms and conditions available at docs.eigenfoundation.org/legal/terms-of-service . Notwithstanding the foregoing, all rights and liabilities related to your use of the Third-Party Subdomains prior to August 29, 2024 are governed by these Terms.

These Terms govern your access to and use of the Services. Please read these Terms carefully, as they include important information about your legal rights. By accessing and/or using the Services, you are agreeing to these Terms. If you do not understand or agree to these Terms, please do not use the Services.

For purposes of these Terms, "**you**" and "**your**" means you as the user of the Services. If you use the Services on behalf of a company or other entity then "you" includes you and that entity, and you represent and warrant that (a) you are an authorized representative of the entity with the authority to bind the entity to these Terms, and (b) you agree to these Terms on the entity's behalf.

> SECTION 9 CONTAINS AN ARBITRATION CLAUSE AND CLASS ACTION WAIVER. BY AGREEING TO THESE TERMS, YOU AGREE (A) TO RESOLVE ALL DISPUTES (WITH LIMITED EXCEPTION) RELATED TO THE COMPANY'S SERVICES AND/OR PRODUCTS THROUGH BINDING INDIVIDUAL ARBITRATION, WHICH MEANS THAT YOU WAIVE ANY RIGHT TO HAVE THOSE DISPUTES DECIDED BY A JUDGE OR JURY, AND (B) TO WAIVE YOUR RIGHT TO PARTICIPATE IN CLASS ACTIONS, CLASS ARBITRATIONS, OR REPRESENTATIVE ACTIONS, AS SET FORTH BELOW. YOU HAVE THE RIGHT TO OPT-OUT OF THE ARBITRATION CLAUSE AND THE CLASS ACTION WAIVER AS EXPLAINED IN SECTION 9.

1. **THE SERVICES**
    1. Services. The Services provide a front-end interface (the "**App**") that display data that facilitates users interfacing with a set of decentralized smart contracts that allow for the restaking of digital assets, such as Ether (ETH). These underlying smart contracts are referred to herein as the "**EigenLayer Protocol**”.  The Services may also reference or provide an App or APIs related to smart contracts that provide a data availability service built on top of Ethereum. These underlying smart contracts are referred to herein as the “**EigenDA Protocol**”. Collectively, the EigenLayer Protocol and EigenDA Protocol are referred to herein as the **“Protocols**”). The Services also provide documentation available at [docs.eigenlayer.xyz](https://docs.eigenlayer.xyz) related to the App and Protocol ("**Documentation**"). **The Protocols are not part of the Services, and your use of the Protocols is entirely at your own risk. Additionally, the third party technologies required to be used or interacted with in order to interact with the Protocols, including but not limited to a Wallet (as defined below, and collectively the “Third-Party Tools”), are not part of the Services, and your use of such Third-Party Tools are entirely at your own risk.** The App is separate and distinct from either of the Protocols and any Third-Party Tools. The App is not essential to accessing the Protocols. The App merely displays blockchain data and provides a web application that reduces the complexity of using the Third-Party Tools or otherwise accessing either of the Protocols. All activity on each of the Protocols is run by permissionless smart contracts, and other developers are free to create their own interfaces to function with the Protocols.

    2. Wallets. To use certain of the Services you may need to link a third-party digital wallet (**"Wallet"**) with the Services. By using a Wallet in connection with the Services, you agree that you are using the Wallet under the terms and conditions of the applicable third-party provider of such Wallet. Wallets are not associated with, maintained by, supported by or affiliated with the Company. You acknowledge and agree that we are not party to any transactions conducted while accessing our App, and we do not have possession, custody or control over any digital assets appearing on the Services. When you interact with the App, you retain control over your digital assets at all times. The Company accepts no responsibility or liability to you in connection with your use of a Wallet, and makes no representations and warranties regarding how the Services will operate with any specific Wallet. **The private keys and/or seed phrases necessary to access the assets held in a Wallet are not held by the Company. The Company has no ability to help you access or recover your private keys and/or seed phrases for your Wallet, so please keep them in a safe place.**

    3. Updates; Monitoring. We may make any improvement, modifications or updates to the Services, including but not limited to changes and updates to the underlying software, infrastructure, security protocols, technical configurations or service features (the "**Updates**") from time to time. Your continued access and use of the Services are subject to such Updates and you shall accept any patches, system upgrades, bug fixes, feature modifications, or other maintenance work that arise out of such Updates. We are not liable for any failure by you to accept and use such Updates in the manner specified or required by us. Although the Company is not obligated to monitor access to or participation in the Services, it has the right to do so for the purpose of operating the Services, to ensure compliance with the Terms and to comply with applicable law or other legal requirements.

    4. The Company may charge or pass through fees for some or part of the Services we make available to you, including transaction or processing fees, blockchain gas or similar network fees. We will disclose the amount of fees we will charge or pass through to you for the applicable Service at the time you access, use or otherwise transact with the Services. Although we will attempt to provide accurate fee information, any such information reflects our estimate of fees, which may vary from the fees actually paid to use the Services and interact with the applicable blockchain with which the Services are compatible. Additionally, your external Wallet provider may impose a fee to transact on the Services. We are not responsible for any fees charged by a third party. All transactions processed through the Services are non-refundable. You will be responsible for paying any and all taxes, duties and assessments now or hereafter claimed or imposed by any governmental authority associated with your use of the Services. In certain cases, your transactions through the Services may not be successful due to an error with the blockchain or the Wallet. We accept no responsibility or liability to you for any such failed transactions, or any transaction or gas fees that may be incurred by you in connection with such failed transactions. You acknowledge and agree that all information you provide with respect to transactions on the Services, including, without limitation, credit card, bank account, PayPal or other payment information is accurate, current and complete, and you have the legal right to use such payment method.

    5. Rewards.  In your use of the Services, you may be attributed certain reputation indicators, points, or other intangible rewards (“**Rewards”**). Rewards are not, and may never convert to, accrue to, be used as basis to calculate, or become any other tokens or virtual assets or distribution thereof. Rewards are virtual items with no monetary value. Rewards do not constitute any currency or property of any type and are not redeemable, refundable, or eligible for any fiat or virtual currency or anything else of value. Rewards are not transferable between users, and you may not attempt to sell, trade, or transfer any Rewards, or obtain any manner of credit using any Rewards. Any attempt to sell, trade, or transfer any Rewards or tokens redeemable for or representing any Rewards will be null and void. 

2. **Who May Use the Services**

    1. You must be 18 years of age or older and not be a Prohibited Person to use the Services. A "**Prohibited Person**" is any person or entity that is (a) listed on any U.S. Government list of prohibited or restricted parties, including without limitation the U.S. Treasury Department's list of Specially Designated Nationals or the U.S. Department of Commerce Denied Person's List or Entity List, (b) located or organized in any U.S. embargoed countries or region any country or region that has been designated by the U.S. Government as "terrorist supporting", or (c) owned or controlled by such persons or entities listed in (a)-(b). You acknowledge and agree that you are solely responsible for complying with all applicable laws of the jurisdiction you are located or accessing the Services from in connection with your use of the Services. By using the Services, you represent and warrant that you meet these requirements and will not be using the Services for any illegal activity or to engage in the prohibited activities in Section 5.3.

    2. To use certain of the Services, you may need to create an account or link another account ("**Account**"). You agree to provide us with accurate, complete and updated information for your Account. You are solely responsible for any activity on your Account and maintaining the confidentiality and security of your password. We are not liable for any acts or omissions by you in connection with your Account. You must immediately notify us at [notices@eigenlabs.org](mailto:notices@eigenlabs.org) if you know or have any reason to suspect that your Account or password have been stolen, misappropriated or otherwise compromised, or in case of any actual or suspected unauthorized use of your Account. You agree not to create any Account if we have previously removed yours, or we previously banned you from any of our Services, unless we provide written consent otherwise.

    3. We may require you to provide additional information and documents regarding your use of the Services, including at the request of any competent authority or in case of application of any applicable law or regulation, including laws related to anti-money laundering or for counteracting financing of terrorism. We may also require you to provide additional information or documents in cases where we have reason to believe: (i) that your Wallet is being used for illegal money laundering or for any other illegal activity; (ii) you have concealed or reported false identification information or other details; or (iii) you are a Prohibited Person. You agree that if it is determined in our sole discretion that you may be violating this Section 2 or engaging in any activities prohibited in these Terms, we may disable your ability to use the Services including the App, which may include but is not limited to preventing you from restaking assets or withdrawing previously restaked assets. 

3. **The Testnet**

    1. Purpose and Participation. The Testnet is designed to demonstrate the functionality and features of the App (or any portion thereof) and to improve participant experiences prior to the App's launch. YOUR PARTICIPATION IN THE TESTNET IS ENTIRELY VOLUNTARY, BUT IF YOU ARE PARTICIPATING IN THE TESTNET, YOU MUST STRICTLY ADHERE TO THESE TERMS. We make no representation or warranty that the Testnet will accurately or completely simulate, duplicate or replicate the App.
    2. Duration. The availability of the Testnet will commence on the date prescribed by the Company and continue until terminated by the Company in its sole discretion. Notwithstanding any other information provided by the Company regarding the Testnet (including on the Websites, blog posts or through other communications (such as forums, Telegram, Github, Discord, or other channels)), the Company may change, discontinue, or terminate, temporarily or permanently, all or any part of the Testnet, at any time and without notice, at its sole discretion (including prior to providing any incentives or rewards). The Company may retain control or upgradeability over certain aspects of the Testnet that it will not retain on Mainnet.
    3. The Testnet Eligibility. Your participation in the Testnet (or any portion thereof) may be subject to eligibility criteria determined by the Company in its sole discretion (including, without limitation, geographical distribution and applicant reputation). By applying or registering, there is no promise or guarantee that you will be able to participate in the Testnet. Notwithstanding any other information provided by the Company regarding the Testnet (including on the Websites, blog posts or through other communications (such as forums, Telegram, Github, Discord, or other channels)), the Company may change or modify at any time the number of participants eligible to participate in the Testnet or the requirements of the Testnet and terminate any participant's participation in the Testnet at any time. The Testnet may operate in certain phases. Your selection or participation in any one phase of the Testnet does not imply that you will be selected for any other phases of the Testnet. The Company reserves the right to block your access to the Testnet at any time in its sole discretion.
    4. No Monetary Value. In your use of the Testnet, you may interact with or transfer certain cryptocurrencies or other digital assets on the Testnet ("**Testnet Tokens**"), such as Testnet Tokens obtained through a faucet. Testnet Tokens are not, and shall never convert to or accrue to become any other tokens or virtual assets. Testnet Tokens are virtual items with no monetary value. Testnet Tokens do not constitute any currency or property of any type and are not redeemable, refundable, or eligible for any fiat or digital currency or anything else of value. Testnet Tokens are not transferable between users outside of the Testnet, and you may not attempt to sell, trade, or transfer any Testnet Tokens outside of the Testnet, or obtain any manner of credit using any Testnet Tokens. Any attempt to sell, trade, or transfer any Testnet Tokens outside of the Testnet will be null and void. Testnet Tokens will not be converted into any future rewards offered by the Company.

4. **Location of Our Privacy Policy**

    1. Privacy Policy. Our Privacy Policy describes how we handle the information you provide to us when you use the Services. For an explanation of our privacy practices, please visit our [Privacy Policy here](./privacy-policy.md).
    

5. **Rights We Grant You**

    1. Right to Use Services. We hereby permit you to use the Services for your personal non-commercial use only, provided that you comply with these Terms in connection with all such use. If any software, content or other materials owned or controlled by us are distributed to you as part of your use of the Services, we hereby grant you, a personal, non-assignable, non-sublicensable, non-transferrable, and non-exclusive right and license to access and display such software, content and materials provided to you as part of the Services, in each case for the sole purpose of enabling you to use the Services as permitted by these Terms. Your access and use of the Services may be interrupted from time to time for any of several reasons, including, without limitation, the malfunction of equipment, periodic updating, maintenance or repair of the Service or other actions that Company, in its sole discretion, may elect to take. Certain elements of the Protocols, including the underlying smart contracts, are made available under an open-source or source-available license (e.g., at https://github.com/Layr-Labs and https://github.com/eigenfoundation), and these Terms do not override or supersede the terms of those licenses.

    2. Right to Use Our APIs. Subject to these Terms, we hereby grant you a non-exclusive, non-transferable, non-sublicensable, worldwide, revocable right and license to use our APIs for the limited purposes set forth in the documentation for the Services. Your use of our APIs must comply with the technical documentation, usage guidelines, call volume limits, and other documentation maintained at [https://docs.eigenlayer.xyz/](https://docs.eigenlayer.xyz/) or such other location we may designate from time to time. We may terminate your right to use the API from time to time at any time.
    3. Restrictions On Your Use of the Services. You may not do any of the following in connection with your use of the Services, unless applicable laws or regulations prohibit these restrictions or you have our written permission to do so:
        1. download, modify, copy, distribute, transmit, display, perform, reproduce, duplicate, publish, license, create derivative works from, or offer for sale any information contained on, or obtained from or through, the Services, except for temporary files that are automatically cached by your web browser for display purposes, or as otherwise expressly permitted in these Terms;
        1. duplicate, decompile, reverse engineer, disassemble or decode the Services (including any underlying idea or algorithm), or attempt to do any of the same;
        1. use, reproduce or remove any copyright, trademark, service mark, trade name, slogan, logo, image, or other proprietary notation displayed on or through the Services;
        1. use automation software (bots), hacks, modifications (mods) or any other unauthorized third-party software designed to modify the Services;
        8. exploit the Services for any commercial purpose, including without limitation communicating or facilitating any commercial advertisement or solicitation;
        9. access or use the Services in any manner that could disable, overburden, damage, disrupt or impair the Services or interfere with any other party's access to or use of the Services or use any device, software or routine that causes the same;
        10. attempt to gain unauthorized access to, interfere with, damage or disrupt the Services, accounts registered to other users, or the computer systems, wallets, accounts, protocols or networks connected to the Services;
        11. circumvent, remove, alter, deactivate, degrade or thwart any technological measure or content protections of the Services or the computer systems, wallets, accounts, protocols or networks connected to the Services;
        12. use any robot, spider, crawlers or other automatic device, process, software or queries that intercepts, "mines," scrapes or otherwise accesses the Services to monitor, extract, copy or collect information or data from or through the Services, or engage in any manual process to do the same;
        13. introduce any viruses, trojan horses, worms, logic bombs or other materials that are malicious or technologically harmful into our systems;
        14. submit, transmit, display, perform, post or store any content that is inaccurate, unlawful, defamatory, obscene, lewd, lascivious, filthy, excessively violent, pornographic, invasive of privacy or publicity rights, harassing, threatening, abusive, inflammatory, harmful, hateful, cruel or insensitive, deceptive, or otherwise objectionable, use the Services for illegal, harassing, bullying, unethical or disruptive purposes, or otherwise use the Services in a manner that is obscene, lewd, lascivious, filthy, excessively violent, harassing, harmful, hateful, cruel or insensitive, deceptive, threatening, abusive, inflammatory, pornographic, inciting, organizing, promoting or facilitating violence or criminal or harmful activities, defamatory, obscene or otherwise objectionable;
        15. violate any applicable law or regulation in connection with your access to or use of the Services; or
        16. access or use the Services in any way not expressly permitted by these Terms.
    1. Interactions with Other Users on the Services. You are responsible for your interactions with other users on or through the Services. While we reserve the right to monitor interactions between users, we are not obligated to do so, and we cannot be held liable for your interactions with other users, or for any user's actions or inactions. If you have a dispute with one or more users, you release us (and our affiliates and subsidiaries, and our and their respective officers, directors, employees and agents) from claims, demands and damages (actual and consequential) of every kind and nature, known and unknown, arising out of or in any way connected with such disputes. In entering into this release you expressly waive any protections (whether statutory or otherwise) that would otherwise limit the coverage of this release to include only those claims which you may know or suspect to exist in your favor at the time of agreeing to this release.

6. **Ownership and Content**

    1. Ownership of the Services. The Services, including their "look and feel" (e.g., text, graphics, images, logos), proprietary content, information and other materials, are protected under copyright, trademark and other intellectual property laws. You agree that the Company and/or its licensors own all right, title and interest in and to the Services (including any and all intellectual property rights therein) and you agree not to take any action(s) inconsistent with such ownership interests. We and our licensors reserve all rights in connection with the Services and its content, including, without limitation, the exclusive right to create derivative works.
    2. Ownership of Trademarks. The Company's name, trademarks and logos and all related names, logos, product and service names, designs and slogans are trademarks of the Company or its affiliates or licensors. Other names, logos, product and service names, designs and slogans that appear on the Services are the property of their respective owners, who may or may not be affiliated with, connected to, or sponsored by us.
    3. Ownership of Feedback. We welcome feedback, bug reports, comments and suggestions for improvements to the Services ("**Feedback**"). You acknowledge and expressly agree that any contribution of Feedback does not and will not give or grant you any right, title or interest in the Services or in any such Feedback. All Feedback becomes the sole and exclusive property of the Company, and the Company may use and disclose Feedback in any manner and for any purpose whatsoever without further notice or compensation to you and without retention by you of any proprietary or other right or claim. You hereby assign to the Company any and all right, title and interest (including, but not limited to, any patent, copyright, trade secret, trademark, show-how, know-how, moral rights and any and all other intellectual property right) that you may have in and to any and all Feedback.
    4. Your Content License Grant. In connection with your use of the Services, you may be able to post, upload, or submit content to be made available through the Services ("Your Content"). In order to operate the Service, we must obtain from you certain license rights in Your Content so that actions we take in operating the Service are not considered legal violations. Accordingly, by using the Service and uploading Your Content, you grant us a license to access, use, host, cache, store, reproduce, transmit, display, publish, distribute, and modify (for technical purposes, e.g., making sure content is viewable on smartphones as well as computers and other devices) Your Content but solely as required to be able to operate, improve and provide the Services. You agree that these rights and licenses are royalty free, transferable, sub-licensable, worldwide and irrevocable (for so long as Your Content is stored with us), and include a right for us to make Your Content available to, and pass these rights along to, others with whom we have contractual relationships related to the provision of the Services, solely for the purpose of providing such Services, and to otherwise permit access to or disclose Your Content to third parties if we determine such access is necessary to comply with our legal obligations. As part of the foregoing license grant you agree that the other users of the Services shall have the right to comment on and/or tag Your Content and/or to use, publish, display, modify or include a copy of Your Content as part of their own use of the Services; except that the foregoing shall not apply to any of Your Content that you post privately for non-public display on the Services. To the fullest extent permitted by applicable law, the Company reserves the right, and has absolute discretion, to remove, screen, edit, or delete any of Your Content at any time, for any reason, and without notice. By posting or submitting Your Content through the Services, you represent and warrant that you have, or have obtained, all rights, licenses, consents, permissions, power and/or authority necessary to grant the rights granted herein for Your Content. You agree that Your Content will not contain material subject to copyright or other proprietary rights, unless you have the necessary permission or are otherwise legally entitled to post the material and to grant us the license described above.
    5. Notice of Infringement -- DMCA (Copyright) Policy. 
    
    If you believe that any text, graphics, photos, audio, videos or other materials or works uploaded, downloaded or appearing on the Services have been copied in a way that constitutes copyright infringement, you may submit a notification to our copyright agent in accordance with 17 USC 512(c) of the Digital Millennium Copyright Act (the "**DMCA**"), by providing the following information in writing:
        1. identification of the copyrighted work that is claimed to be infringed;
        2. identification of the allegedly infringing material that is requested to be removed, including a description of where it is located on the Service;
        3. information for our copyright agent to contact you, such as an address, telephone number and e-mail address;
        4. a statement that you have a good faith belief that the identified, allegedly infringing use is not authorized by the copyright owners, its agent or the law;
        5. a statement that the information above is accurate, and under penalty of perjury, that you are the copyright owner or the authorized person to act on behalf of the copyright owner; and
        6. the physical or electronic signature of a person authorized to act on behalf of the owner of the copyright or of an exclusive right that is allegedly infringed.

    Notices of copyright infringement claims should be sent by mail to: Eigen Labs, Inc., Attn: Legal, 15790 Redmond Way #1176 Redmond, WA 98052 ; or by e-mail to [notices@eigenlabs.org](mailto:notices@eigenlabs.org). It is our policy, in appropriate circumstances and at our discretion, to disable or terminate the accounts of users who repeatedly infringe copyrights or intellectual property rights of others.

    A user of the Services who has uploaded or posted materials identified as infringing as described above may supply a counter-notification pursuant to sections 512(g)(2) and (3) of the DMCA. When we receive a counter-notification, we may reinstate the posts or material in question, in our sole discretion. To file a counter-notification with us, you must provide a written communication (by fax or regular mail or by email) that sets forth all of the items required by sections 512(g)(2) and (3) of the DMCA. Please note that you will be liable for damages if you materially misrepresent that content or an activity is not infringing the copyrights of others.

7. **Third Party Services and Materials**

    1. Third Party Services and Materials. The Services may allow you to browse the availability of  certain (i) actively validated services (“**AVSs**”) (ii) operators that offer to run certain AVSs in connection with your delegated restaked digital assets and/or (iii) other services or products developed or run by third parties displayed on the Services, including the App  (“**Third-Party Services**”) and may display, include or make available content, data, information, applications or materials from third parties (“**Third-Party Materials**”) or provide links to certain third party websites. For clarity, effective August 29, 2024, the Third-Party Subdomains are each a Third-Party Service and the content, data, information, applications or materials therein are Third-Party Materials. The Company does not endorse or recommend any Third-Party Materials, the use of any provider of any Third-Party Services, or the restaking or delegation of any assets to any Third-Party Services. You agree that your access and use of such Third-Party Services and Third-Party Materials is governed solely by the terms and conditions of such Third-Party Services and Third-Party Materials, as applicable. The Company is not responsible or liable for, and make no representations as to any aspect of such Third-Party Materials and Third-Party Services, including, without limitation, their content or the manner in which they handle, protect, manage or process data or any interaction between you and the provider of such Third-Party Services. The Company is not responsible for examining or evaluating the content, accuracy, completeness, availability, timeliness, validity, copyright compliance, legality, decency, quality, security or any other aspect of such Third Party Services or Third Party Materials or websites. You irrevocably waive any claim against the Company with respect to such Third-Party Services and Third-Party Materials. We are not liable for and you expressly disclaim any liability with respect to any damage or loss caused or alleged to be caused by or in connection with your enablement, access or use of any such Third-Party Services or Third-Party Materials, or your reliance on the privacy practices, data security processes or other policies of such Third-Party Services, including without limitation, the delegation of any assets to any Third-Party Service or the staking of any assets with any Third-Party Service that results in slashing or any other loss of funds or the integration of any Third-Party Service such as an AVS into your product or service that results in any damages whatsoever. Third-Party Services, Third-Party Materials and links to other websites are provided solely as a convenience to you. Certain Third-Party Services or Third-Party Materials may automatically populate on the Company’s Services. The Company reserves the right to remove any Third-Party Services or Third-Party Materials from the Services, including without limitation, any AVSs or operators for any reason whatsoever. 

8. **Disclaimers, Limitations of Liability and Indemnification**

    1. Disclaimers. Your access to and use of the Services and any Protocols are at your own risk. You understand and agree that the Services are provided to you on an "AS IS" and "AS AVAILABLE" basis. Without limiting the foregoing, to the maximum extent permitted under applicable law, the Company, its parents, affiliates, related companies, officers, directors, employees, agents, representatives, partners and licensors (the "**Company Entities**") and MultiSig Committee Members (as defined below) DISCLAIM ALL WARRANTIES AND CONDITIONS, WHETHER EXPRESS, IMPLIED OR STATUTORY, INCLUDING WITHOUT LIMITATION ANY WARRANTIES RELATING TO TITLE, MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, NON-INFRINGEMENT, USAGE, QUALITY, PERFORMANCE, SUITABILITY OR FITNESS OF THE SERVICES AND THE PROTOCOLS FOR ANY PARTICULAR PURPOSE, OR AS TO THE ACCURACY, QUALITY, SEQUENCE, RELIABILITY, WORKMANSHIP OR TECHNICAL CODING THEREOF, OR THE ABSENCE OF ANY DEFECTS THEREIN WHETHER LATENT OR PATENT. The Company Entities and MultiSig Committee Members make no warranty or representation and disclaim all responsibility and liability for: (a) the completeness, accuracy, availability, timeliness, security or reliability of the Services, and the Protocols; (b) any harm to your computer system, loss of data, or other harm that results from your access to or use of the Services or the Protocols; (c) the operation or compatibility with any other application or any particular system or device, including any Wallets; (d) whether the Services or the Protocols will meet your requirements or be available on an uninterrupted, secure or error-free basis;  (e) whether the Services or the Protocols will protect your assets from theft, hacking, cyber attack or other form of loss caused by third party conduct; (f) loss of funds or value resulting from intentional or unintentional slashing or as a result of a fork of any token such as via a social slashing or intersubjective forking mechanism and (g) the deletion of, or the failure to store or transmit, Your Content and other communications maintained by the Services. No advice or information, whether oral or written, obtained from the Company Entities or the MultiSig Committee Members or through the Services, will create any warranty or representation not expressly made herein. You should not rely on the Services, for advice of any kind, including legal, tax, investment, financial or other professional advice.
    2. Limitations of Liability. TO THE EXTENT NOT PROHIBITED BY LAW, YOU AGREE THAT IN NO EVENT WILL THE COMPANY ENTITIES AND MULTISIG COMMITTEE MEMBERS BE LIABLE (A) FOR DAMAGES OF ANY KIND, INCLUDING INDIRECT, SPECIAL, EXEMPLARY, INCIDENTAL, CONSEQUENTIAL OR PUNITIVE DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES, LOSS OF USE, DATA OR PROFITS, BUSINESS INTERRUPTION OR ANY OTHER DAMAGES OR LOSSES, ARISING OUT OF OR RELATED TO YOUR USE OR INABILITY TO USE THE SERVICES), HOWEVER CAUSED AND UNDER ANY THEORY OF LIABILITY, WHETHER UNDER THESE TERMS OR OTHERWISE ARISING IN ANY WAY IN CONNECTION WITH THE SERVICES OR THESE TERMS AND WHETHER IN CONTRACT, STRICT LIABILITY OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) EVEN IF THE COMPANY ENTITIES HAVE BEEN ADVISED OF THE POSSIBILITY OF SUCH DAMAGE, OR (B) FOR ANY OTHER CLAIM, DEMAND OR DAMAGES WHATSOEVER RESULTING FROM OR ARISING OUT OF OR IN CONNECTION WITH THESE TERMS OR THE DELIVERY, USE OR PERFORMANCE OF THE SERVICES. THE COMPANY ENTITIES' AND MULTISIG COMMITTEE MEMBERS' TOTAL LIABILITY TO YOU FOR ANY DAMAGES FINALLY AWARDED SHALL NOT EXCEED ONE HUNDRED DOLLARS ($100.00) RESPECTIVELY.
    3. SOME JURISDICTIONS (SUCH AS THE STATE OF NEW JERSEY) DO NOT ALLOW LIMITATIONS ON IMPLIED WARRANTIES OR THE EXCLUSION OR LIMITATION OF INCIDENTAL OR CONSEQUENTIAL DAMAGES, SO SOME OR ALL OF THE ABOVE DISCLAIMERS, EXCLUSION OR LIMITATION MAY NOT APPLY TO YOU, AND YOU MAY HAVE ADDITIONAL RIGHTS.
    4. Assumption of Risks.
        1. By using the Services, you represent that you have sufficient knowledge and experience in business and financial matters, including a sufficient understanding of blockchain or cryptographic tokens and technologies and other digital assets, storage mechanisms (such as Wallets), blockchain-based software systems, and blockchain technology, to be able to assess and evaluate the risks and benefits of the Services contemplated hereunder, and will bear the risks thereof, including loss of all amounts paid, and the risk that the tokens may have little or no value. You acknowledge and agree that there are risks associated with purchasing and holding cryptocurrency, using blockchain technology and staking cryptocurrency. These include, but are not limited to, risk of losing access to cryptocurrency due to slashing, loss of private key(s), custodial error or purchaser error, risk of mining or blockchain attacks, risk of hacking and security weaknesses, risk of unfavorable regulatory intervention in one or more jurisdictions, risk related to token taxation, risk of personal information disclosure, risk of uninsured losses, volatility risks, and unanticipated risks. You acknowledge that cryptocurrencies are not (i) deposits of or guaranteed by a bank (ii) insured by the FDIC or by any other governmental agency and (iii) that we do not custody and cannot transfer any cryptocurrency or digital assets you may interact with on the Services or Protocols. 
        1. There are certain multi-signature crypto wallets (the "**MultiSigs**", and the signatories to such MultiSigs, the "**MultiSig Committee Members**") that have certain controls related to one or more of the Protocols, that may include, but are not limited to, the ability to pause certain functionality of the Protocols, reverse or pause slashing, implement or influence upgrades to the Protocols (or any aspect thereof) and certain other controls of the functionality of the Protocols as described in the documentation or in public communications made by us. Certain MultiSigs may be controlled by us or MultiSig Committee Members that are employed or engaged by us, and certain other MultiSigs will be controlled partially or entirely by MultiSig Committee Members that are unaffiliated third parties over which we have no or limited control. We will not be able to control the actions of such MultiSig Committee Members if they are not employed or engaged by us and thus certain MultiSigs will be outside of our control.
        1. The regulatory regime governing blockchain technologies, cryptocurrencies and other digital assets is uncertain, and new regulations or policies may materially adversely affect the potential utility or value of such cryptocurrencies and digital assets. There also exists the risks of new taxation of the purchase or sale of cryptocurrencies and other digital assets.
        1. We cannot control how third-party exchange platforms quote or value cryptocurrencies and other digital assets and we expressly deny and disclaim any liability to you and deny any obligations to indemnify or hold you harmless for any losses you may incur as a result of fluctuations in the value of cryptocurrencies or other digital assets.
        1. Smart contracts execute automatically when certain conditions are met. Since smart contracts typically cannot be stopped or reversed, vulnerabilities in their programming and design or other vulnerabilities that may arise due to hacking or other security incidents can have adverse effects to restaked assets, including but not limited to significant volatility and risk of loss.
        1. Certain protocols and networks subject staked assets to slashing upon certain conditions, including, but not limited to, if a validator or operator engages in harmful or malicious behavior, fails to perform their role as a validator or operator properly, or incorrectly validates a transaction, and we expressly deny and disclaim any liability to you and deny any obligations to indemnify or hold you harmless for any losses you may incur as a result of slashing.
        1. Certain protocols and networks require that a certain amount of staked assets be locked for a certain period of time while staking, and withdrawal of staked assets may be delayed. We do not guarantee the security or functionality of any third-party protocol, software or technology intended to be compatible with restaked assets.
    5. Indemnification. By entering into these Terms and accessing or using the Services, you agree that you shall defend, indemnify and hold the Company Entities and MultiSig Committee Members harmless from and against any and all claims, costs, damages, losses, liabilities and expenses (including attorneys’ fees and costs) incurred by the Company Entities or MultiSig Committee Members arising out of or in connection with: (a) your violation or breach of any term of these Terms or any applicable law or regulation; (b) your violation of any rights of any third party; (c) your misuse of the Services; or (d) your negligence or wilful misconduct; or (e) your Content. If you are obligated to indemnify any Company Entity or MultiSig Committee Members hereunder, then you agree that the Company (or, at its discretion, the applicable Company Entity) or MultiSig Committee Members, as applicable, will have the right, in its sole discretion, to control any action or proceeding and to determine whether the Company or MultiSig Committee Member, as applicable, wishes to settle, and if so, on what terms, and you agree to fully cooperate with the Company or MultiSig Committee Members in the defense or settlement of such claim.
    6. Third Party Beneficiaries. You and the Company acknowledge and agree that the Company Entities (other than the Company) and the MultiSig Committee Members are third party beneficiaries of these Terms, including under Section 8 and 9.

9. **ARBITRATION AND CLASS ACTION WAIVER**

    1. PLEASE READ THIS SECTION CAREFULLY -- IT MAY SIGNIFICANTLY AFFECT YOUR LEGAL RIGHTS, INCLUDING YOUR RIGHT TO FILE A LAWSUIT IN COURT AND TO HAVE A JURY HEAR YOUR CLAIMS. IT CONTAINS PROCEDURES FOR MANDATORY BINDING ARBITRATION AND A CLASS ACTION WAIVER.
    2. Informal Process First. You and the Company agree that in the event of any dispute between you and the Company Entities or the MultiSig Committee Members, either party will first contact the other party and make a good faith sustained effort to resolve the dispute before resorting to more formal means of resolution, including without limitation, any court action, after first allowing the receiving party 30 days in which to respond. Both you and the Company agree that this dispute resolution procedure is a condition precedent which must be satisfied before initiating any arbitration against you, any Company Entity or any MultiSig Committee Members, as applicable.
    3. Arbitration Agreement and Class Action Waiver. After the informal dispute resolution process, any remaining dispute, controversy, or claim (collectively, "**Claim**") relating in any way to the Services, including the App, any use or access or lack of access thereto, and any other usage of the Protocols even if interacted with outside of the Services or App, will be resolved by arbitration, including threshold questions of arbitrability of the Claim. You and the Company agree that any Claim will be settled by final and binding arbitration, using the English language, administered by JAMS under its Comprehensive Arbitration Rules and Procedures (the "**JAMS Rules**") then in effect (those rules are deemed to be incorporated by reference into this section, and as of the date of these Terms). Because your contract with the Company, these Terms, and this Arbitration Agreement concern interstate commerce, the Federal Arbitration Act ("FAA") governs the arbitrability of all disputes. However, the arbitrator will apply applicable substantive law consistent with the FAA and the applicable statute of limitations or condition precedent to suit. Arbitration will be handled by a sole arbitrator in accordance with the JAMS Rules. Judgment on the arbitration award may be entered in any court that has jurisdiction. Any arbitration under these Terms will take place on an individual basis -- class arbitrations and class actions are not permitted. You understand that by agreeing to these Terms, you and the Company are each waiving the right to trial by jury or to participate in a class action or class arbitration.
    4. Batch Arbitration. To increase the efficiency of administration and resolution of arbitrations, you and the Company agree that in the event that there are one-hundred (100) or more individual Claims of a substantially similar nature filed against the Company by or with the assistance of the same law firm, group of law firms, or organizations, then within a thirty (30) day period (or as soon as possible thereafter), JAMS shall (1) administer the arbitration demands in batches of 100 Claims per batch (plus, to the extent there are less than 100 Claims left over after the batching described above, a final batch consisting of the remaining Claims); (2) appoint one arbitrator for each batch; and (3) provide for the resolution of each batch as a single consolidated arbitration with one set of filing and administrative fees due per side per batch, one procedural calendar, one hearing (if any) in a place to be determined by the arbitrator, and one final award (**“Batch Arbitration”**). All parties agree that Claims are of a “substantially similar nature” if they arise out of or relate to the same event or factual scenario and raise the same or similar legal issues and seek the same or similar relief. To the extent the parties disagree on the application of the Batch Arbitration process, the disagreeing party shall advise JAMS, and JAMS shall appoint a sole standing arbitrator to determine the applicability of the Batch Arbitration process (**“Administrative Arbitrator”**). In an effort to expedite resolution of any such dispute by the Administrative Arbitrator, the parties agree the Administrative Arbitrator may set forth such procedures as are necessary to resolve any disputes promptly. The Administrative Arbitrator’s fees shall be paid by the Company. You and the Company agree to cooperate in good faith with JAMS to implement the Batch Arbitration process including the payment of single filing and administrative fees for batches of Claims, as well as any steps to minimize the time and costs of arbitration, which may include: (1) the appointment of a discovery special master to assist the arbitrator in the resolution of discovery disputes; and (2) the adoption of an expedited calendar of the arbitration proceedings. This Batch Arbitration provision shall in no way be interpreted as authorizing a class, collective and/or mass arbitration or action of any kind, or arbitration involving joint or consolidated claims under any circumstances, except as expressly set forth in this provision.

    5. Exceptions. Notwithstanding the foregoing, you and the Company agree that the following types of disputes will be resolved in a court of proper jurisdiction: (i) disputes or claims within the jurisdiction of a small claims court consistent with the jurisdictional and dollar limits that may apply, as long as it is brought and maintained as an individual dispute and not as a class, representative, or consolidated action or proceeding; (ii) disputes or claims where the sole form of relief sought is injunctive relief (including public injunctive relief); or (iii) intellectual property disputes.
    6. Costs of Arbitration. Payment of all filing, administration, and arbitrator costs and expenses will be governed by the JAMS Rules, except that if you demonstrate that any such costs and expenses owed by you under those rules would be prohibitively more expensive than a court proceeding, the Company will pay the amount of any such costs and expenses that the arbitrator determines are necessary to prevent the arbitration from being prohibitively more expensive than a court proceeding (subject to possible reimbursement as set forth below).

        1. Fees and costs may be awarded as provided pursuant to applicable law. If the arbitrator finds that either the substance of your claim or the relief sought in the Demand is frivolous or brought for an improper purpose (as measured by the standards set forth in Federal Rule of Civil Procedure 11(b)), then the payment of all fees will be governed by the JAMS rules. In that case, you agree to reimburse the Company for all monies previously disbursed by it that are otherwise your obligation to pay under the applicable rules. If you prevail in the arbitration and are awarded an amount that is less than the last written settlement amount offered by the Company before the arbitrator was appointed, the Company will pay you the amount it offered in settlement. The arbitrator may make rulings and resolve disputes as to the payment and reimbursement of fees or expenses at any time during the proceeding and upon request from either party made within fourteen (14) days of the arbitrator's ruling on the merits.

    7. **Opt-Out. You have the right to opt-out and not be bound by the arbitration provisions set forth in these Terms by sending written notice of your decision to opt-out to notices@eigenlabs.org. The notice must be sent to the Company within thirty (30) days of your first registering to use the Services or agreeing to these Terms; otherwise you shall be bound to arbitrate disputes on a non-class basis in accordance with these Terms. If you opt out of only the arbitration provisions, and not also the class action waiver, the class action waiver still applies. You may not opt out of only the class action waiver and not also the arbitration provisions. If you opt-out of these arbitration provisions, the Company also will not be bound by them.**
    8. WAIVER OF RIGHT TO BRING CLASS ACTION AND REPRESENTATIVE CLAIMS. TO THE FULLEST EXTENT PERMITTED BY APPLICABLE LAW, YOU AND THE COMPANY EACH AGREE THAT ANY PROCEEDING TO RESOLVE ANY DISPUTE, CLAIM OR CONTROVERSY WILL BE BROUGHT AND CONDUCTED ONLY IN THE RESPECTIVE PARTY'S INDIVIDUAL CAPACITY AND NOT AS PART OF ANY CLASS (OR PURPORTED CLASS), CONSOLIDATED, MULTIPLE-PLAINTIFF, OR REPRESENTATIVE ACTION OR PROCEEDING ("CLASS ACTION"). YOU AND THE COMPANY AGREE TO WAIVE THE RIGHT TO PARTICIPATE AS A PLAINTIFF OR CLASS MEMBER IN ANY CLASS ACTION. YOU AND THE COMPANY EXPRESSLY WAIVE ANY ABILITY TO MAINTAIN A CLASS ACTION IN ANY FORUM. IF THE DISPUTE IS SUBJECT TO ARBITRATION, THE ARBITRATOR WILL NOT HAVE THE AUTHORITY TO COMBINE OR AGGREGATE CLAIMS, CONDUCT A CLASS ACTION, OR MAKE AN AWARD TO ANY PERSON OR ENTITY NOT A PARTY TO THE ARBITRATION. FURTHER, YOU AND THE COMPANY AGREE THAT THE ARBITRATOR MAY NOT CONSOLIDATE PROCEEDINGS FOR MORE THAN ONE PERSON'S CLAIMS, AND IT MAY NOT OTHERWISE PRESIDE OVER ANY FORM OF A CLASS ACTION. FOR THE AVOIDANCE OF DOUBT, HOWEVER, YOU CAN SEEK PUBLIC INJUNCTIVE RELIEF TO THE EXTENT AUTHORIZED BY LAW AND CONSISTENT WITH THE EXCEPTIONS CLAUSE ABOVE. 3. IF THIS CLASS ACTION WAIVER IS LIMITED, VOIDED, OR FOUND UNENFORCEABLE, THEN, UNLESS THE PARTIES MUTUALLY AGREE OTHERWISE, THE PARTIES' AGREEMENT TO ARBITRATE SHALL BE NULL AND VOID WITH RESPECT TO SUCH PROCEEDING SO LONG AS THE PROCEEDING IS PERMITTED TO PROCEED AS A CLASS ACTION. IF A COURT DECIDES THAT THE LIMITATIONS OF THIS PARAGRAPH ARE DEEMED INVALID OR UNENFORCEABLE, ANY PUTATIVE CLASS, PRIVATE ATTORNEY GENERAL OR CONSOLIDATED OR REPRESENTATIVE ACTION MUST BE BROUGHT IN A COURT OF PROPER JURISDICTION AND NOT IN ARBITRATION.

10. **Additional Provisions**

    1. Updating These Terms. We may modify these Terms from time to time in which case we will update the "**Last Revised**" date at the top of these Terms. If we make changes that are material, we will use reasonable efforts to attempt to notify you, such as by e-mail and/or by placing a prominent notice on the first page of the Website. However, it is your sole responsibility to review these Terms from time to time to view any such changes. The updated Terms will be effective as of the time of posting, or such later date as may be specified in the updated Terms. Your continued access or use of the Services after the modifications have become effective will be deemed your acceptance of the modified Terms. No amendment shall apply to a dispute for which an arbitration has been initiated prior to the change in Terms.
    2. Suspension; Termination. If you breach any of the provisions of these Terms, all licenses granted by the Company will terminate automatically. Additionally, the Company may, in its sole discretion, suspend or terminate your Account and/or access to or use of any of the Services, with or without notice, for any or no reason, including, without limitation, (i) if we believe, in our sole discretion, you have engaged in any of the prohibited activities set forth in Section 5.3; (ii) if you provide any incomplete, incorrect or false information to us; (iii) if you have breached any portion of these Terms; (iv) if we suspect you may be a Prohibited Person or any Wallet used to access the Services is linked with any illegal or high-risk activity; and/or (v) if we determine such action is necessary to comply with these Terms, any of our policies, procedures or practices, or any law rule or regulation.  If the Company deletes your Account for any suspected breach of these Terms by you, you are prohibited from re-registering for the Services under a different name. In the event of Account deletion for any reason, the Company may, but is not obligated to, delete any of Your Content. the Company shall not be responsible for the failure to delete or deletion of Your Content. All sections which by their nature should survive the termination of these Terms shall continue in full force and effect subsequent to and notwithstanding any termination of this Agreement by the Company or you. Termination will not limit any of the Company’s other rights or remedies at law or in equity.
    3. Injunctive Relief. You agree that a breach of these Terms will cause irreparable injury to the Company for which monetary damages would not be an adequate remedy and the Company shall be entitled to equitable relief in addition to any remedies it may have hereunder or at law without a bond, other security or proof of damages.
    4. California Residents. If you are a California resident, in accordance with Cal. Civ. Code § 1789.3, you may report complaints to the Complaint Assistance Unit of the Division of Consumer Services of the California Department of Consumer Affairs by contacting them in writing at 1625 North Market Blvd., Suite N 112 Sacramento, CA 95834, or by telephone at (800) 952-5210.
    5. Export Laws. You agree that you will not export or re-export, directly or indirectly, the Services and/or other information or materials provided by the Company hereunder, to any country for which the United States or any other relevant jurisdiction requires any export license or other governmental approval at the time of export without first obtaining such license or approval. In particular, but without limitation, the Services may not be exported or re-exported (a) into any U.S. embargoed countries or any country that has been designated by the U.S. Government as a "terrorist supporting" country, or (b) to anyone listed on any U.S. Government list of prohibited or restricted parties, including the U.S. Treasury Department's list of Specially Designated Nationals or the U.S. Department of Commerce Denied Person's List or Entity List. By using the Services, you represent and warrant that you are not located in any such country or on any such list. You are responsible for and hereby agree to comply at your sole expense with all applicable United States export laws and regulations.
    6. Force Majeure. We will not be liable or responsible to you, nor be deemed to have defaulted under or breached these Terms, for any failure or delay in fulfilling or performing any of our obligations under these Terms or in providing the Services, when and to the extent such failure or delay is caused by or results from any events beyond our ability to control, including acts of God; flood, fire, earthquake, epidemics, pandemics, tsunami, explosion, war, invasion, hostilities (whether war is declared or not), terrorist threats or acts, riot or other civil unrest, government order, law, or action, embargoes or blockades, strikes, labor stoppages or slowdowns or other industrial disturbances, shortage of adequate or suitable Internet connectivity, telecommunication breakdown or shortage of adequate power or electricity, and other similar events beyond our control.
    7. Miscellaneous. If any provision of these Terms shall be unlawful, void or for any reason unenforceable, then that provision shall be deemed severable from these Terms and shall not affect the validity and enforceability of any remaining provisions. These Terms and the licenses granted hereunder may be assigned by the Company but may not be assigned by you without the prior express written consent of the Company. No waiver by either party of any breach or default hereunder shall be deemed to be a waiver of any preceding or subsequent breach or default. The section headings used herein are for reference only and shall not be read to have any legal effect. The Services are operated by us in the United States. Those who choose to access the Services from locations outside the United States do so at their own initiative and are responsible for compliance with applicable local laws. These Terms are governed by the laws of the State of New York, without regard to conflict of laws rules, and the proper venue for any disputes arising out of or relating to any of the same will be the state and federal courts located in New York, New York.
    8. How to Contact Us. You may contact us regarding the Services or these Terms by e-mail at **notices@eigenlabs.org**.
````

## File: docs/eigenlayer/overview/_category_.json
````json
{
  "position": 1,
  "label": "Overview"
}
````

## File: docs/eigenlayer/overview/key-terms.md
````markdown
---
title: Key Terms
sidebar_position: 2
---



- **Autonomous Verifiable Services (AVS):**  a service built externally to EigenLayer that requires active verification by a set of Operators. An AVS deploys its service manager to interact with EigenLayer core contracts that allow for Operator registration to Operator Sets, slashing, and rewards distribution. Once registered, an Operator agrees to run the AVS’s off-chain code.

- **Allocation / Deallocation:** an in-protocol commitment of security to an AVS’s Operator Set by an Operator. The act of allocating demarcates portions of an Operator’s delegated stake as Unique Stake, making it slashable by a single AVS. Deallocation is the same process in reverse, subject to additional time delays that ensure AVSs can appropriately slash for tasks that have occurred in the past.

- **AVS Developer**: development team that builds an AVS service.
- **Cryptoeconomic security:** security model that uses economic incentives and cryptography to ensure the proper functioning and security of a network.
- **Delegation:** the process by which a Staker assigns their staked tokens to a chosen Operator, granting the Operator the authority to use the value of those tokens for validating AVSs. The Operator cannot directly access the delegated tokens, but can subject any delegated tokens to slashing by an AVS. Delegations themselves are the sum of a given Operator’s delegated stake from Stakers.
- **EigenPod:** contract that is deployed on a per-user basis that facilitates native restaking.
- **Free-market governance:** EigenLayer provides an open market mechanism that allows stakers to choose which services to opt into, based on their own risk and reward analysis.
- **Liquid Staking:** a service that enables users to deposit their ETH into a staking pool and receive a liquid staking token. This token represents a claim on their ETH and its staking yield. Liquid staking tokens can be traded in the DeFi ecosystem and redeemed for their underlying ETH value after a waiting period.
- **LST Restaking:** a method where LST holders restake their Liquid Staking Tokens (LSTs) by transferring them into the EigenLayer smart contracts.
- **Magnitude**: The accounting tool used to track Operator allocations to Operator Sets. Represented as \`wads\` in the AllocationManager and \`bips\` in the CLI. Magnitudes represent proportions of an Operator’s delegations for a specific Strategy. The sum of all of an Operator’s Magnitudes cannot exceed the INITIAL\_TOTAL\_MAGNITUDE.
- **Native Restaking:** a method where Ethereum stakers restake their staked ETH natively by pointing their withdrawal credentials to the EigenLayer contracts.
- **On-chain slashing contract:** a smart contract deployed by service modules on EigenLayer that enforces slashing, specifying and penalizing any misbehavior.
- **Operator:** An entity that registers an Operator address on Eigenlayer to receive delegations from Stakers and run AVS infrastructure. Operators allocate their delegated stake to Operator Sets created by an AVS.
- **Operator Set:** a segmentation of Operators created by an AVS that secures a specific set of tasks for the AVS with staked assets that may be reserved for securing that set.
- **Pooled security via restaking:** when multiple parties combine their resources to provide greater security for a system. In EigenLayer, Ethereum stakers can “restake” their ETH or Liquid Staking Tokens (LST) by opting into new services built on EigenLayer.
- **Restaker**: a person who restakes Native or LST ETH to the EigenLayer protocol.
- **Rewards**: Tokens sent by AVSs to Stakers and/or Operators to compensate participation.
- **Slashing:** A penalty for improperly or inaccurately completing tasks assigned in Operator Sets by an AVS. A slashing results in a burning/loss of funds.
- **Staker:** An individual address that directly supplies assets to Eigenlayer. Such an address could be an EOA wallet or a smart contract controlled by an individual or institution.
- **Strategies**: assets that are restaked into the platform.
- **Unique Stake:** Assets made slashable exclusively by one Operator Set. Unique Stake is an accounting tool defined on the level of Operator Sets that ensures AVSs and Operators maintain key safety properties when handling staked security and slashing on EigenLayer. Unique Stake is allocated to different Operator Sets on an opt-in basis by Operators. Unique Stake represents the proportion of an Operator’s delegated stake from Stakers that an AVS can slash.
- **Withdrawal:** The process through which assets are moved out of the EigenLayer protocol after safety delays and with applied slashings to the nominal amounts.
````

## File: docs/eigenlayer/overview/README.md
````markdown
---
title: Intro to EigenLayer
sidebar_position: 1
---


## What is EigenLayer?


Building a new Web3 service comes with significant challenges: bootstrapping crypto-economic security and assembling a reliable network of Operators. Meanwhile, the Web3 ecosystem is rich with opportunities, including a surplus of asset holders eager to earn rewards and skilled Operators seeking to expand into new, value-driven services. EigenLayer bridges this gap, aligning incentives and unlocking untapped potential for both builders and the broader community.

EigenLayer is a protocol built on Ethereum that introduces Restaking, a new primitive for Web3 builders that provides a "marketplace for trust" bringing together Restakers, Operators, and Autonomous Verifiable Services (AVSs). It allows users to stake assets such as Native ETH, Liquid Staking Tokens (LSTs), the EIGEN token, or any ERC20 token into EigenLayer smart contracts, thereby extending Ethereum's cryptoeconomic security to additional applications on the network. It fosters innovation by enabling newer projects to benefit from Ethereum’s robust security guarantees without the need to replicate the costly process of securing their own network.

AVSs have tools to make economic commitments to their end users, such as proper or fair execution of their code run by Operators. The [Rewards v2 (currently in Testnet) upgrade](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md#eigenlayer-improvement-proposal-001-rewards-v2) enables AVSs to issue rewards to Operators and Stakers when the AVS’ services are properly run (the carrot). The [Slashing and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md) (currently in Testnet) upgrade gives AVSs the ability to slash stake in instances where the commitments to properly run their services are broken (the stick).

## Why Build with EigenLayer?

Ethereum is a secure foundation for decentralized applications and has established itself as the best in class infrastructure for smart contract apps. However, many Web3 builders wish to expand beyond Ethereum’s compute capability and offer secured off-chain services for their communities. EigenLayer acts as an additional layer on top of Ethereum, allowing developers to build on this foundation without having to duplicate the cost, complexities, or resources needed to create their own blockchain network and services.

EigenLayer solves the bootstrapping problem for new Web3 services by aggregating a ready-to-deploy network of Operators and Restaked assets that are ready to operate and validate new Web3 services. Instead of requiring every Web3 builder to independently raise capital, establish cryptoeconomic security, and onboard Operators, EigenLayer offers Cryptoeconomic Security as a Service. This approach frees builders to focus on their core differentiators, accelerating innovation without the need to build security frameworks from scratch.

The key benefits of building an AVS on EigenLayer include:

- Security via Restaking: leverage Ethereum’s staking mechanism to secure your service.
- Focus on your project's unique value: spend less time and resources accumulating economic security from scratch.
- Bootstrap your Operator network: quickly access a large network of experienced Operators.
- Decentralization and Trust: build on trust minimized, decentralized infrastructure.
- Composability: seamlessly integrate with the broader Ethereum ecosystem.

## EigenLayer Architecture Overview

The core components of the EigenLayer protocol include:

- **Restaking** enables stakers to restake their Native ETH or Liquid Staking Tokens (LST) to provide security for services in the EigenLayer ecosystem, known as Autonomous Verifiable Services (AVSs).
- **Autonomous Verifiable Services (AVSs)** are services built on the EigenLayer protocol that leverage Ethereum's shared security. AVSs deliver services to users and the broader Web3 ecosystem. 
- **Operators** are entities that run AVS software and perform validation tasks for AVSs built on EigenLayer. They register in EigenLayer and allow stakers to delegate to them, then opt in to provide various services (AVSs) built on top of EigenLayer.
- **Delegation** is the process where stakers delegate their restaked ETH or LSTs to Operators or run validation services themselves, effectively becoming an Operator. This process involves a double opt-in between both parties, ensuring mutual agreement. Restakers retain agency over their stake and choose which AVSs they opt-in to validate for.
- EigenLayer **Rewards** enables AVSs to make rewards distributions to stakers and operators that opt-in to support the AVS. AVSs make RewardsSubmissions to the RewardsCoordinator, a core protocol contract.
- **Slashing** is a penalty for improperly or inaccurately completing tasks assigned in Operator Sets by an AVS. A slashing results in a burning/loss of funds.

<img src="/img/overview/eigenlayer-arch-v2.png" width="75%"
    style={{ margin: '50px'}}>
</img>

## Next Steps
To learn more about EigenLayer, refer to the [Whitepapers](whitepaper.md) or explore the [Learning Resources](../resources/learning-resources.md).

Get started with EigenLayer:
- [Restake on EigenLayer](/restakers/concepts/overview)
- [Register as an Operator](/operators/howto/operator-installation)
- [Build an AVS](/developers/Concepts/avs-developer-guide)
- Join our Ecosystem: [Discord](https://discord.com/invite/eigenlayer), [Twitter](https://twitter.com/eigenlayer)
````

## File: docs/eigenlayer/overview/support.md
````markdown
# Community and EIGEN Claim Support

For any discussion, engagement, and learning about EigenLayer, please join the [EigenLayer Community Discord](https://discord.gg/eigenlayer).

# Restaker, Operator, and AVS Development Support

For issues with the dApp, LST and restaking issues and Operator questions you may send us a question via our AI-enabled chatbot and Support team here:  <a href="javascript:void(0)"  id="intercom_trigger_eldocs" >EigenLayer Support Desk</a>

# EigenLayer Forum

If you are interested in EigenLayer at a deeper level, please check out the [EigenLayer forum](https://forum.eigenlayer.xyz/)! There are groups of researchers, AVS developers, and more contributing their expertise to help build the open verifiable cloud.

# EIGEN Claim Support

For any issues concerning EIGEN including token claims and stakedrop issues, please visit the stakedrop-support channel in the [EigenLayer Community Discord](https://discord.gg/eigenlayer). The Eigen Foundation support team can address your question there. Please see docs.eigenfoundation.org for information related to EIGEN token claims.

Please beware of any fraudulent tokens, dApps, and phishing sites:
- The only site to claim EIGEN is: claims.eigenfoundation.org .
- The EIGEN token contract address is: `0xec53bF9167f50cDEB3Ae105f56099aaaB9061F83` .
- The only support website is: docs.eigenlayer.xyz .
    
For a complete list of links and more information, please visit the: [EigenLayer Community Discord](https://discord.gg/eigenlayer).
````

## File: docs/eigenlayer/overview/whitepaper.md
````markdown
# Whitepapers

**EigenLayer: The Restaking Collective** ([PDF](/pdf/EigenLayer_WhitePaper.pdf) / <a href="/html/EigenLayer_WhitePaper-converted-xodo.html" target="_blank">HTML</a>): the research paper that formed the basis of the EigenLayer protocol development. The document discusses the original architecture of EigenLayer, the Restaking primitive, and the concept of AVSs. Please note that some components of the design have changed since the original conception of the protocol. Use this document for high level guidance. For specific implementation details, please see the respective protocol implementation source code repositories.


**EIGEN The Universal Intersubjective Work Token:**([PDF](/pdf/EIGEN_Token_Whitepaper.pdf) / <a href="/html/EIGEN_Token_Whitepaper-converted-xodo.html" target="_blank">HTML</a>): the research paper that introduces the structure of the EIGEN token, a universal intersubjective work token. We view this intersubjective work token as a first step towards the goal of building the Verifiable Digital Commons.
````

## File: docs/eigenlayer/reference/_category_.json
````json
{
  "position": 7,
  "label": "Reference"
}
````

## File: docs/eigenlayer/reference/allocation-manager-interface.md
````markdown
---
sidebar_position: 4
title: Allocation Manager Interface
---

The `AllocationManager` interface handles all allocation and deallocation signals.

```solidity
interface IAllocationManager {

   /**
    * @notice struct used to modify the allocation of slashable magnitude to an operator set
    * @param operatorSet the operator set to modify the allocation for
    * @param strategies the strategies to modify allocations for
    * @param newMagnitudes the new magnitude to allocate for each strategy to this operator set
    */
   struct AllocateParams {
       OperatorSet operatorSet;
       IStrategy[] strategies;
       uint64[] newMagnitudes;
   }

   /**
     * @notice Called by the delegation manager OR an operator to set an operator's allocation delay.
     * This is set when the operator first registers, and is the number of blocks between an operator
     * allocating magnitude to an operator set, and the magnitude becoming slashable.
     * @param operator The operator to set the delay on behalf of.
     * @param delay the allocation delay in blocks
     */
    function setAllocationDelay(
	address operator, 
	uint32 delay
    ) external;

    /**
     * @notice Modifies the proportions of slashable stake allocated to an operator set  from a list of strategies
     * Note that deallocations remain slashable for DEALLOCATION_DELAY blocks therefore when they are cleared they may
     * free up less allocatable magnitude than initially deallocated.
     * @param operator the operator to modify allocations for
     * @param params array of magnitude adjustments for one or more operator sets
     * @dev Updates encumberedMagnitude for the updated strategies
     * @dev msg.sender is used as operator
     */
    function modifyAllocations(
	address operator, 
	AllocateParams[] calldata params
    ) external;

    /**
     * @notice struct used to modify the allocation of slashable magnitude to an operator set
     * @param operatorSet the operator set to modify the allocation for
     * @param strategies the strategies to modify allocations for
     * @param newMagnitudes the new magnitude to allocate for each strategy to this operator set
     */
    struct AllocateParams {
        OperatorSet operatorSet;
        IStrategy[] strategies;
        uint64[] newMagnitudes;
    }

    /**
     * @notice This function takes a list of strategies and for each strategy, removes from the deallocationQueue
     * all clearable deallocations up to max `numToClear` number of deallocations, updating the encumberedMagnitude
     * of the operator as needed.
     *
     * @param operator address to clear deallocations for
     * @param strategies a list of strategies to clear deallocations for
     * @param numToClear a list of number of pending deallocations to clear for each strategy
     *
     * @dev can be called permissionlessly by anyone
     */
    function clearDeallocationQueue(
        address operator,
        IStrategy[] calldata strategies,
        uint16[] calldata numToClear
    ) external;
}
```
````

## File: docs/eigenlayer/reference/safety-delays-reference.md
````markdown
---
sidebar_position: 4
title: Safety Delays
---

Safety delays for allocations and deallocations are included in the table.

| Parameter | Description                                                                                                                                                                                                                                                                                                                                           | Value | Setter & Configuration |
| :---- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| :---- | :---- |
| `ALLOCATION_CONFIG_DELAY` | Amount of blocks between an Operator queuing an `ALLOCATION_DELAY` change and the change taking effect.                                                                                                                                                                                                                                               | 126000 blocks (~17.5 days) | Core Protocol: Set via governance |
| `ALLOCATION_DELAY` | Amount of blocks it takes for an Operator’s allocation to be live in an Operator Set for a given Strategy. Must be set by the Operator before any allocations and applies globally to all Operator Sets and Strategies.  The protocol provides no constraints on this value. It can be any unsigned integer value and can be changed by the Operator. | Unsigned integer value representing a number of blocks  | Operator: Set via `AllocationManager` Must be set in order to allocate |
| `DEALLOCATION_DELAY` | Amount of blocks between an Operator queuing a deallocation of stake from an Operator Set for a strategy and the deallocation taking effect. This delay also applies to an Operator *deregistering* from an Operator Set, either by their own action or that of the AVS.                                                                              | 100800 blocks (~14 days) | Core Protocol: Set via governance |
| `INITIAL_TOTAL_MAGNITUDE` | Initial value of the monotonically decreasing total magnitude for every Operator for every strategy. Initially set high enough to start out with a large level of precision in magnitude allocations and slashings.                                                                                                                                   | 1e18 | Core Protocol: Constant, unlikely to change |
| `WITHDRAWAL_DELAY` | Amount of blocks between a Staker queueing a withdrawal and the withdrawal becoming non-slashable and completable.                                                                                                                                                                                                                                    | 100800 blocks (~14 days) | Core Protocol: Set via governance |
````

## File: docs/eigenlayer/resources/_category_.json
````json
{
  "position": 11,
  "label": "Resources"
}
````

## File: docs/eigenlayer/resources/apis-and-dashboards.md
````markdown
---
sidebar_position: 2
title: APIs, Dashboards, and Tooling
---

### APIs

- [EigenExplorer API](https://docs.eigenexplorer.com/api-reference/introduction)
- [Dune EigenLayer API](https://docs.dune.com/api-reference/eigenlayer/introduction)

### Dashboards

- [Eigen Economy (maintained by Eigen Labs)](https://economy.eigenlayer.xyz/)
- [EigenExplorer Dashboard](https://dashboard.eigenexplorer.com/)
- [The Ultimate Restaking Dashboard](https://dune.com/hahahash/eigenlayer)
- [AVS Dune Dashboard](https://dune.com/hahahash/avs)
- [EigenLayer Dune dashboard by dyorcrypto](https://dune.com/dyorcrypto/eigenlayer)
- [Validator.info - In-depth real-time EigenLayer analytics](https://validator.info/eigenlayer)
- [Restaking Info by Nethermind](https://restaking.info/)
- [OpenBlock EigenLayer Restaking Dashboard](https://app.openblocklabs.com/app/restaking/eigenlayer)
- [EigenLayer Dashboard](https://daic.capital/projects/eigenlayer)
````

## File: docs/eigenlayer/resources/infinite-hackathon.md
````markdown
---
sidebar_position: 3
title: EigenLayer Infinite Hackathon
---

The EigenLayer Infinite Hackathon encourages submissions from developers building on EigenLayer, EigenLayer AVSs, EigenDA, and rollups built on EigenDA at any hackathon happening anywhere at any time now and in the future, both in-person and online.

Submit your project [here](https://airtable.com/appnYZo360sWuEYLS/shrrnj9BWIPevjc5c).

If you're in the process of hacking in the EigenLayer ecosystem, join us in the EigenLayer Hacker group chat [here](https://airtable.com/appnYZo360sWuEYLS/shrz6Pstds7EXjC5N)!

![EigenLayer Infinite Hackathon](/img/infinite-hackathon.png)

If you are building within the EigenLayer ecosystem as part of your hackathon project, you are qualified to submit and be eligible for prizes in the following categories.

## 1. AVSs and open source AVS reference architectures

Any new AVS or AVS reference architecture. Emphasis on code quality, documentation quality, and real world utility.

1. Ether - $7,500.00
2. Finney - $5,000.00
3. Gwei - $2,500.00
4. Wei - $1,000.00

To learn more about building AVSs, see [Awesome AVS](https://github.com/Layr-Labs/awesome-avs).

## 2. Apps

Any new application that is built on EigenLayer AVSs or EigenDA rollups. Emphasis on code quality, documentation quality, and real world utility.

1. Ether - $7,500.00
2. Finney - $5,000.00
3. Gwei - $2,500.00
4. Wei - $1,000.00

You can see AVSs live on EigenLayer mainnet [here](https://www.eigenlayer.xyz/ecosystem?category=AVS), and rollups live on EigenDA [here](https://www.eigenlayer.xyz/ecosystem?category=EigenDA).

## 3. EigenDA and rollups

Any new tooling for, or rollups built, on EigenDA. Emphasis on code quality, documentation quality, and real world utility.

1. Ether - $7,500.00
2. Finney - $5,000.00
3. Gwei - $2,500.00
4. Wei - $1,000.00

To learn more about EigenDA, see the documentation [here](https://docs.eigenlayer.xyz/eigenda/overview).

### Eligibility Criteria

Participants of hackathons that are already hosted by, or that include prizes offered by, Eigen Labs or Eigen Foundation are not eligible.

Projects must be submitted within 14 days of original hackathon deadline.

Project code must be open source.

Full Hackathon Terms of Service are available [here](https://docs.google.com/document/d/1ZmW_WakvPobyps5XP_fkNs_AEAyXp-bq/edit?usp=sharing&ouid=115843462632021279280&rtpof=true&sd=true)
````

## File: docs/eigenlayer/resources/learning-resources.md
````markdown
---
sidebar_position: 1
title: Learning Resources
---

# EigenLayer Learning Resources

### Start here

* [Boys Club Ep 127: What is EigenLayer?](https://open.spotify.com/episode/2aR83WBag0pj0ldRRm2vZD)
* [You Could've Invented EigenLayer (Video)](https://www.youtube.com/watch?v=fCl_PucMytU)
* [The Three Pillars of Programmable Trust: The EigenLayer End Game](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)
* [Shared Security: The Four Superpowers](https://twitter.com/sreeramkannan/status/1742949397523304600)

### Blog posts

* [EigenLayer Blog](https://www.blog.eigenlayer.xyz/)
* [You Could've Invented EigenLayer (Blog)](https://www.blog.eigenlayer.xyz/ycie/)
* [The EigenLayer Universe: Ideas for Building the Next 15 Unicorns](https://www.blog.eigenlayer.xyz/eigenlayer-universe-15-unicorn-ideas/)
* [Dual Staking](https://www.blog.eigenlayer.xyz/dual-staking/)
* [EigenLayer for Developers](https://nader.substack.com/p/beyond-restaking-eigenlayer-for-developers)
* [EigenLayer: Intersubjective Faults, Token forking, bEIGEN & more](https://mirror.xyz/edatweets.eth/l3QtrWv-27h5DVkrSdFMq96MRJ8AotemvmZIQ23Ew3A)

### Videos and podcasts

* [Official EigenLayer YouTube](https://www.youtube.com/@EigenLayer)
* [Unchained Podcast EigenLayer interview ](https://www.youtube.com/watch?v=16p7YG8S3ec)
* [EigenLayer in 2024](https://www.youtube.com/watch?v=ms94dx9HvL0)
* [EigenLayer: The Endgame Coordination Layer](https://www.youtube.com/watch?v=o9y_pZUr0Nc)
* [EigenLayer Explained: 4th Paradigm in CryptoEconomic Capital Efficiency](https://www.youtube.com/watch?v=iMFscq9Sxdk)

### Developer resources

Please see Developer Learning Resources [here](/docs/developers/Reference/resources.md).

### Community

* [EigenLayer Forum](https://forum.eigenlayer.xyz/)
* [EigenLayer Research Forum](https://research.eigenlayer.xyz/)
* [Build on Eigen group chat](https://airtable.com/appnYZo360sWuEYLS/shrz6Pstds7EXjC5N)
* [Discord](https://discord.com/invite/eigenlayer)
* [EigenLayer Twitter](https://twitter.com/eigenlayer)
* [BuildOnEigen Twitter](https://x.com/buildoneigen)
````

## File: docs/eigenlayer/rewards-claiming/claim-rewards/_category_.json
````json
{
  "position": 2,
  "label": "Claim Rewards"
}
````

## File: docs/eigenlayer/rewards-claiming/claim-rewards/via-cli.mdx
````
---
sidebar_position: 2
title: Claim via CLI
---


import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Using CLI

Follow the instructions below step by step in order to claim your currently available rewards in the EigenLayer web app via EigenLayer CLI. Rewards can be claimed by a Staker or Operator.

## Quick Steps

### Step 1: Install the latest version of EigenLayer CLI

Head over to the [installation instructions](https://github.com/Layr-Labs/eigenlayer-cli#install-eigenlayer-cli-using-a-binary) for getting the binary with your preferred way. 

Below steps assume that you have used the default binary installation command and your binary is installed at `./bin/` location


### Step 2: Check if installation is correct
```bash
./bin/eigenlayer --version
```
This should output the version of EigenLayer CLI installed
```bash
eigenlayer version v<X.X.X>
```

### Step 3: Check if rewards are available to claim
<Tabs groupId="network">
  <TabItem value="mainnet" label="Mainnet">
    ```bash
    ./bin/eigenlayer rewards show \
      --network mainnet \
      --earner-address <earner-address> \
      --claim-type unclaimed

    ```

  </TabItem>
  <TabItem value="holesky" label="Holesky">
    ```bash
    ./bin/eigenlayer rewards show \
      --network holesky \
      --earner-address <earner-address> \
      --claim-type unclaimed
    ```
  </TabItem>
</Tabs>

This will output the token address and the amount of unclaimed rewards available 
```bash
---------------------------------------------------------------------------------------
Token Address                              | Wei Amount
---------------------------------------------------------------------------------------
0x554c393923c753d146aa34608523ad7946b61662 | 6324648267039518
0xdf3b00151bf851e8c4036ceda284d38a2f1d09df | 132817613607829878
---------------------------------------------------------------------------------------
```

### Step 4: Ensure you have your keys in place
Claiming rewards requires you to have access to your wallet keys. 
Supported Key Management options are listed [here](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/README.md#supported-key-management-backends)

:::info
If you already have your keys in either of the supported key management options, you can skip this step.
:::

In case you have your keys in some browser wallet, you can import your keys to local keystore file by using the following command
```bash
./bin/eigenlayer keys import \
  --key-type <key-type> <private-key-hex>
```
This will ask you to set a password to protect your keystore file.

Refer your browser wallet documentation to get the `private-key-hex`.

### Step 5: Claim the rewards

Make sure the keys you are using to claim is the claimer you have set.
<Tabs groupId="network">
  <TabItem value="mainnet" label="Mainnet">
    If you are using local keystore file, use the following command to claim rewards
    
    ```bash
    ./bin/eigenlayer rewards claim \
      --network mainnet \
      --eth-rpc-url <mainnet-eth-rpc-url> \
      --earner-address <earner-address> \
      --recipient-address <address-to-send-rewards-to> \
      --path-to-key-store /path/to/key/store-json \
      --token-addresses <comma-separated-list-of-token-addresses> \
      --broadcast
    ```
    `comma-separated-list-of-token-addresses` - You can get this from output of [Step 3](#step-3-check-if-rewards-are-available-to-claim)

  </TabItem>
  <TabItem value="holesky" label="Holesky">
    If you are using local keystore file, use the following command to claim rewards
    
    ```bash
    ./bin/eigenlayer rewards claim \
      --network holesky \
      --eth-rpc-url <holesky-eth-rpc-url> \
      --earner-address <earner-address> \
      --recipient-address <address-to-send-rewards-to> \
      --path-to-key-store /path/to/key/store-json \
      --token-addresses <comma-separated-list-of-token-addresses> \
      --broadcast
    ```
    `comma-separated-list-of-token-addresses` - You can get this from output of Step 3
  </TabItem>
</Tabs>

If you are using private key hex, fireblocks or web3 signer check the command help for the respective key management backend.
```bash
./bin/eigenlayer rewards claim --help
```

### Step 6: (Optional) Check the rewards parameters
If you want to see the rewards parameters before claiming, you can use the following command
<Tabs groupId="network">
  <TabItem value="mainnet" label="Mainnet">
    If you are using local keystore file, use the following command to claim rewards
    
    ```bash
    ./bin/eigenlayer rewards claim \
      --network holesky \
      --eth-rpc-url <mainnet-eth-rpc-url> \
      --earner-address <earner-address> \
      --token-addresses <comma-separated-list-of-token-addresses>
    ```
    `comma-separated-list-of-token-addresses` - You can get this from output of [Step 3](#step-3-check-if-rewards-are-available-to-claim)

  </TabItem>
  <TabItem value="holesky" label="Holesky">
    If you are using local keystore file, use the following command to claim rewards
    
    ```bash
    ./bin/eigenlayer rewards claim \
      --network holesky \
      --eth-rpc-url <holesky-eth-rpc-url> \
      --earner-address <earner-address> \
      --token-addresses <comma-separated-list-of-token-addresses>
    ```
    `comma-separated-list-of-token-addresses` - You can get this from output of Step 3
  </TabItem>
</Tabs>

## Advanced configurations
### Setting an address to claim rewards on earners behalf
If you want a different address to claim the rewards on earners behalf, you can set the claimer using the following command
```bash
./bin/eigenlayer rewards set-claimer \
  --network mainnet \
  --eth-rpc-url <mainnet-eth-rpc-url> \
  --earner-address <earner-address> \
  --claimer-address <claimer-address> \
  --path-to-key-store /path/to/key/store-json \
  --broadcast
```

### Claiming rewards when earner is a smart contract
If the earner is a smart contract, you can claim rewards by either generating a JSON object with the arguments to the call `processClaim`
method on rewards coordinator contract
or generate a calldata which can be signed and broadcasted.

To generate the JSON object, use the following command
```bash
    ./bin/eigenlayer rewards claim \
      --network mainnet \
      --eth-rpc-url <mainnet-eth-rpc-url> \
      --earner-address <earner-address> \
      --recipient-address <address-to-send-rewards-to> \
      --path-to-key-store /path/to/key/store-json \
      --token-addresses <comma-separated-list-of-token-addresses> \
      --output-type json
```

To generate the calldata, use the following command
```bash
    ./bin/eigenlayer rewards claim \
      --network mainnet \
      --eth-rpc-url <mainnet-eth-rpc-url> \
      --earner-address <earner-address> \
      --recipient-address <address-to-send-rewards-to> \
      --path-to-key-store /path/to/key/store-json \
      --token-addresses <comma-separated-list-of-token-addresses> \
      --output-type calldata
```
````

## File: docs/eigenlayer/rewards-claiming/claim-rewards/via-ui.md
````markdown
---
sidebar_position: 1
title: Claim via WebApp
---

Rewards can be claimed by a Staker or Operator as a claimer or earner. 

Key Definitions:
- Earner Address: an address that is actively earning rewards within the system.
- Claimer Address: an address that has been granted permission to claim rewards on behalf of one or more earning addresses.


## Using the WebApp

Follow the instructions below step by step in order to claim your currently available rewards in the [EigenLayer Web App](https://app.eigenlayer.xyz/). Note that EIGEN token rewards are included in the displayed reward rates.

**Step 1**: Navigate to EigenLayer web app **Dashboard**.

**Step 2**: Click **Claim Rewards** button.

**Step 3**: Select tokens individually you wish to claim rewards for or click **Select All** to claim all token rewards at once.

**Step 4**: Click **Claim Tokens** button. This will initiate a transaction in your Web3 wallet to include claim proofs. **Sign** the transaction.

**Step 5**: View the summary of Rewards claimed successfully.

## Claimer Instructions for Rewards

### Understanding Profiles

In the web app, a "profile" refers to an individual earning address that a Claimer address has been authorized to claim rewards for. A single Claimer address may have permissions for multiple Earner profiles.

When logged in as a Claimer address, you will see a list of all associated Earner profiles. Each profile represents an Earner address for which you have claim privileges.

### Claimer Address Login Instructions

1.	Navigate to the web app and log in with your Claimer address.
2.	Upon login, you will be presented with a list of Earner profiles associated with your Claimer address.
3.	Select a specific Earner profile to view its associated rewards.
4.	The only action enabled for a Claimer address is to claim rewards on behalf of the selected Earner profile.

**Visual Indicators for Claimer Addresses**

To differentiate between Claimer and Earner profiles, the app provides the following cues:
- When acting as a Claimer address, you will only see the Claim Rewards option.
- Switching between Earner profiles is seamless within the same session, allowing you to manage multiple profiles efficiently.

**Performance Note**

When logging in with a Claimer address that is associated with more than 100 Earner profiles, the app may experience a delay of up to 10 seconds while loading. This is considered an edge case, as most Claimer addresses are typically associated with one or two Earner profiles. We are working to optimize this behavior and expect improvements in future updates. In the meantime, if you experience delays, please allow the system time to load all profiles.
````

## File: docs/eigenlayer/rewards-claiming/_category_.json
````json
{
  "position": 4,
  "label": "Rewards"
}
````

## File: docs/eigenlayer/rewards-claiming/rewards-claiming-faq.md
````markdown
---
sidebar_position: 2
title: Rewards Claiming FAQ
---



### When can I claim my rewards?

After a root is posted, rewards are claimable after an activation delay. On mainnet this delay is 1 week and on testnet it is 2 hours.

### What portion of rewards goes to my operator?

Operators get a fixed 10% portion rewards, though this is subject to change in a future release to be variable.

### How can I test reward distributions and claiming on testnet?

#### 1. Programmatic incentives.
To accumulate programmatic incentives, you must be delegated to an operator that is registered to at least one AVS of any type. Programmatic incentives are payed in Testnet EIGEN. Assets that earn programmatic incentives are limited to: EIGEN, LsETH, ETHx, rETH, osETH, cbETH, ankrETH, stETH, WETH, sfrxETH, mETH.

#### 2. Rewards from AVSs
To accumulate testnet rewards from AVSs, you must be delegated to an Operator that is registered to at least one AVS with active rewards.

**Faucet AVS:**
FaucetAVS is designed purely to distribute WETH to staked WETH with no requirements beyond operator registration.

**EigenDA:**
EigenDA distributes rewards to [operators actively participating in EigenDA](https://docs.eigenda.xyz/operator-guides/requirements/). Operators may be ejected if they fail to sign batches or fall below the threshold requirements. Rewards are earned for:
- EIGEN Quorum participation
- ETH Quorum participation including LsETH, ETHx, rETH, osETH, cbETH, ankrETH, stETH, WETH, sfrxETH, mETH and Beacon Chain ETH in EigenPods.


### Are reward distributions based on the amount of work performed by an operator, the Operator's total delegated stake or both?

The current rewards calculation assumes that work done is directly proportional to stake; therefore, rewards are distributed proportional to stake. If an operator does not perform the tasks expected of it, the AVS should eject or "churn" the operator (which we have examples for in our middleware contracts).

### Will the AVS Rewards be distributed using the same ERC20 token used to Stake / Operate (opt-in to) the AVS?

An AVS can distribute any ERC-20 token it chooses in a [RewardSubmission](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/RewardsCoordinator.md#createavsrewardssubmission). These reward token(s) can be different from the list of Strategies (assets) that were originally staked, delegated and opted into by the Restaker, Operator, and AVS.

For example, Restakers could delegate stETH (lido eth) to an Operator. The Operator could opt in to an AVS with the stETH strategy. Then a week later the AVS could pay rewards in USDC. The decision of which ERC20 token to reward to a Strategy is entirely up to the AVS to determine.

### How is the APR calculated?

The UI shows up to a 7-day averaged APR for a given strategy. Due to the 2 day calculation delay, neither APR nor accrual of rewards can be observed until 2 days after a user has restaked and delegated qualifying assets to an Operator that is earning rewards. The APR is given by the following equation:

$$
\frac{E_{\text{earned}, s}}{\sum_{7 \ \text{days}}E_{\text staked, s}}*365\ \text{days}
$$

That is, $$ E_{\text{earned}, s} $$ is the ETH value of all reward tokens earned over the past 7 days from restaking strategy $$ s $$. 
$$ E_{\text staked, s} $$ is the ETH value of tokens staked in restaked strategy $$ s $$ on a given day, excluding any days in which no reward is earned.

ETH values are calculated using the latest price feeds sourced from Coingecko. Reward tokens that do not have a public price available from Coingecko are not included in the calculation. APR is not calculated for staked tokens that do not have a public price available from Coingecko.
````

## File: docs/eigenlayer/rewards-claiming/rewards-claiming-overview.md
````markdown
---
sidebar_position: 1
title: Rewards Claiming Overview
---

# Rewards Overview

## Overview

The EigenLayer Rewards protocol enables AVSs to make rewards to stakers and operators. Operators earn rewards by opting in to AVSs that make `RewardsSubmissions` to the `RewardsCoordinator`, a core protocol contract. Within a single `RewardsSubmission`,  an AVS can specify a time range for which the reward will be distributed, a list of weights for each `Strategy` for the reward, and an ERC20 token to make rewards in.

By default, Operators will earn a flat 10% split on rewards. The rest of the reward is claimable by the operator's delegated Stakers. Rewards are proportional to:
- The amount of stake.
- The AVS's relative weighting of strategies in a rewards submission.


## Rewards Flexibility

The protocol also includes the following features to enable additional flexibility for rewards distribution:
- Operator Directed Rewards: AVSs can now direct performance-based rewards to specific Operators using custom logic. This allows rewards to be distributed based on work completion, quality or other parameters determined by the AVS, allowing flexible and tailored incentives. Operators registered to AVSs for the specified duration are eligible. This approach enables customization and diverse reward mechanisms that can be attributed on-chain, aligning incentives with Operator contributions.
- Variable Operator Fees for AVS Rewards: Operators can now set their per-AVS fee rate on AVS rewards to any amount from 0% to 100%, deviating from the 10% default split. Changes to this split take effect after a 7-day activation delay. The ability to set a variable split per-AVS allows Operators to align their fee structures with their economic needs and the complexity and diversity of AVS demands.
- Variable Operator Splits for Programmatic Incentives: Operators can set their split of Programmatic Incentives to any amount from 0% to 100%, so that Operators have flexibility in determining the appropriate take rate. Changes to this split take effect after a 7-day activation delay. These splits integrate seamlessly with the existing reward distribution model, ensuring that stakers delegating to Operators benefit proportionately.

Please see the [EigenLayer Improvement Proposal-001: Rewards v2](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md#executive-summary) for more detail.


## Rewards Contract Configurations

### Earners 
Operators and Stakers are both categorized as "Earners" when it comes to claiming and are distinct by their addresses. Actual reward calculations are explained further in the [technical docs](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/RewardsCoordinator.md). To summarize, reward calculations are performed daily by snapshotting the on-chain state. Once a week on mainnet and daily on testnet, a Merkle root is posted to the contract that allows earners to claim their updated earnings.


### Setting Designated Claimers
Earners (Stakers and Operators) can set a claimer address that can claim rewards for the tokens they've earned. An Earner is its own claimer by default and only the claimer address can claim rewards. If a new claimer is set, the new address can claim all of the previously unclaimed rewards. The Earner can always change their designated claimer address.  

Note: Earners or their designated claimers do not have to claim weekly against every single Merkle root to receive all their earnings up to that point. Earnings are calculated cumulatively so simply claiming one time against the latest Merkle root posted on the `RewardsCoordinator` contract will reward them with all their cumulative earnings even if there were several roots posted to the contract that were not claimed against.

### Rewards Recipient Address
Not to be confused with the designated claimer address specified above, the recipient address is the address that will receive the ERC20 token rewards. The designated claimer (or the Earner themselves) has the ability to call `RewardsCoordinator.processClaim` for the Earner while also specifying a recipient address to receive all the rewards.

## Reward Calculations

Rewards are calculated via an off-chain process. A Merkle root is posted which represents the cumulative rewards across all earners weekly on Mainnet and daily on Testnet. There is an additional 2 hour delay on testnet and 1 week delay on mainnet after posting in order for the root to be claimable against with a valid Merkle proof. The deterministic calculation of the distribution of rewards is specified in our [technical docs](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/RewardsCoordinator.md). 



## AVS Integrations
Please refer to [AVS Guide: AVS Rewards](/docs/developers/HowTo/build/rewards.md) for complete instructions.
````

## File: docs/eigenlayer/rewards-claiming/rewards-snapshot-data.md
````markdown
---
sidebar_position: 3

---

# Rewards Distribution Data

Rewards snapshot distribution data is available:
* From an [EigenLayer Sidecar](#using-eigenlayer-sidecar).
* Via a [public S3 bucket](#via-s3-bucket). Users may access this data for their own analytics purposes.

## Using EigenLayer Sidecar

The [EigenLayer Sidecar](https://sidecar-docs.eigenlayer.xyz/docs/sidecar/running/getting-started) is an open source, permissionless, verified indexer enabling anyone (for example AVS, Operator) to access 
EigenLayer’s protocol rewards in real-time.

For information on how to install and launch a Sidecar, refer to the [Sidecar documentation](https://sidecar-docs.eigenlayer.xyz/docs/sidecar/running/getting-started).

There are two methods to access the rewards data from a Sidecar:
* Terminal or a bash script with `curl` and `grpcurl`.
* Using the gRPC or HTTP clients published in the [protocol-apis](https://github.com/Layr-Labs/protocol-apis) Go package.

Refer to the [sidecar](https://github.com/Layr-Labs/sidecar) repository for [examples](https://github.com/Layr-Labs/sidecar/blob/master/examples/rewardsData/main.go).

To obtain rewards snapshot distribution data using a EigenLayer Sidecar:

1. List distribution roots. 
   ``` 
   # grpcurl
   grpcurl -plaintext -d '{ }' localhost:7100 eigenlayer.sidecar.v1.rewards.Rewards/ListDistributionRoots | jq '.distributionRoots[0]'

   # curl
   curl -s http://localhost:7101/rewards/v1/distribution-roots

   {
     "root": "0x2888a89a97b1d022688ef24bc2dd731ff5871465339a067874143629d92c9e49",
     "rootIndex": "217",
     "rewardsCalculationEnd": "2025-02-22T00:00:00Z",
     "rewardsCalculationEndUnit": "snapshot",
     "activatedAt": "2025-02-24T19:00:48Z",
     "activatedAtUnit": "timestamp",
     "createdAtBlockNumber": "3418350",
     "transactionHash": "0x769b4efbefb99c6c80738405ae5d082829d8e2e6f97ee20da615fa7073c16d90",
     "blockHeight": "3418350",
     "logIndex": "544"
   }
   ```
2. Use the `rootIndex` to fetch the rewards data.
   ```
   # grpcurl
   grpcurl -plaintext --max-msg-sz 2147483647 -d '{ "rootIndex": 217 }' localhost:7100 eigenlayer.sidecar.v1.rewards.Rewards/GetRewardsForDistributionRoot > rewardsData.json

   # curl
   curl -s http://localhost:7101/rewards/v1/distribution-roots/217/rewards > rewardsData.json

   {
    "rewards": [
     {
      "earner": "0xe44ce641a7cf6d52c06c278694313b08c2b181c0",
      "token": "0x3b78576f7d6837500ba3de27a60c7f594934027e",
      "amount": "130212752259281570",
      "snapshot": "2025-02-22T00:00:00Z"
     },
    // ...
    ]
   }
   ```

## Via S3 Bucket

To obtain rewards snapshot distribution data from the S3 bucket: 

To get a list of snapshot dates from RewardsCoordinator contract:

1. Find the RewardsCoordinator Proxy address for Testnet or Mainnet [here](https://github.com/Layr-Labs/eigenlayer-contracts/?tab=readme-ov-file#deployments).
    1. Get the DistributionRoot(s) needed for the rewards time ranges desired.
       * Call `getCurrentDistributionRoot` to get the most recent root posted. `getCurrentClaimableDistributionRoot` returns the most recent claimable root since there is an activation delay.
       * Find the rewardsCalculationEndTimestamp value as the second value in the [DistributionRoot struct](https://github.com/Layr-Labs/eigenlayer-contracts/blob/b4fa900a11df04f3b0034e225deb1eb42b39f8bc/src/contracts/interfaces/IRewardsCoordinator.sol#L72) resulting tuple.
       * Or Index on the event `DistributionRootSubmitted` which is emitted when a [root is created](https://etherscan.io/tx/0x2aff6f7b0132092c05c8f6f41a5e5eeeb208aa0d95ebcc9022d7823e343dd012#eventlog).
       * Note: the current snapshot cadence is at most once per day for Testnet, weekly for Mainnet if there are new rewards to publish ([more detail here](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/RewardsCoordinator.md#off-chain-calculation)).
   2. Convert this rewardsCalculationEndTimestamp value from unix time stamp integer format to the date format YYYY-MM-DD using a conversion tool ([example here](https://www.unixtimestamp.com/)).

2. Construct the URL to return the claim-amounts.json file for the desired snapshot date in the following format:

`<bucket url>/<environment>/<network>/<snapshot date>/claim-amounts.json`

* bucket_url: 
  * [https://eigenlabs-rewards-testnet-holesky.s3.amazonaws.com](https://eigenlabs-rewards-testnet-holesky.s3.amazonaws.com)
  * [https://eigenlabs-rewards-mainnet-ethereum.s3.amazonaws.com](https://eigenlabs-rewards-mainnet-ethereum.s3.amazonaws.com)
* environment: testnet or mainnet
* network: holesky or ethereum

Example:

`https://eigenlabs-rewards-mainnet-ethereum.s3.amazonaws.com/mainnet/ethereum/2024-08-11/claim-amounts.json`

Extract data from the claim-amounts.json file as needed. Please find the schema here:

```

{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "EigenLayer rewards cumulative earnings",
  "type": "object",
  "properties": {
    "earner": {
      "type": "string",
      "description": "Ethereum address"
    },
    "token": {
      "type": "string",
      "Ethereum address"
    },
    "snapshot": {
      "type": "number",
      "Unix timestamp of the snapshot date in UTC"
    },
    "cumulative_amount": {
      "type": "string",
      "Cumulative amount of tokens earned over time (includes both claimed and unclaimed rewards)"
    }
  },
  "required": [
    "earner",
    "token",
    "snapshot",
    "cumulative_amount"
  ]
}
```

Note: claim-amounts.json file is not a json file, but a json line file where each line is a valid json object.
````

## File: docs/eigenlayer/security/_category_.json
````json
{
  "position": 6,
  "label": "Security"
}
````

## File: docs/eigenlayer/security/audits.md
````markdown
---
sidebar_position: 2
---

# Audits

As a key component of our development process, please see the most recent audits that help assess the robustness and reliability of our systems:
- [Sigma Prime](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/audits/M2%20Mainnet%20-%20Sigma%20Prime%20-%20Feb%202024.pdf)  
- [Dedaub](https://github.com/Layr-Labs/eigenlayer-middleware/blob/dev/audits/M2%20Mainnet%20-%20Dedaub%20-%20Feb%202024.pdf)
- [Cantina](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/audits/M2%20Mainnet%20-%20Cantina%20-%20Apr%202024.pdf)

Please see the following repositories for all current and past audits:
- [EigenLayer-Contracts / Audits](https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/audits)
- [EigenLayer-Middleware / Audits](https://github.com/Layr-Labs/eigenlayer-middleware/tree/dev/audits)

We encourage you to review all audits carefully as they offer an in-depth analysis of our technology's capabilities, security measures, and overall reliability.

Instructions are also available for [Installation and Running Tests / Analyzers](https://github.com/Layr-Labs/eigenlayer-contracts#installation) via the Github repo.
````

## File: docs/eigenlayer/security/bug-bounty.md
````markdown
---
sidebar_position: 3
description: Check out the official bug bounty program for EigenLayer on Immunefi
---

# Bug Bounty

Check out the official bug bounty program for EigenLayer on Immunefi:
[https://immunefi.com/bounty/eigenlayer/](https://immunefi.com/bounty/eigenlayer/)
````

## File: docs/eigenlayer/security/guardrails.md
````markdown
---
sidebar_position: 4
---

# Guardrails

There will be a 7-day [withdrawal delay](withdrawal-delay.md) that will serve as a security measure during the early stages of the EigenLayer mainnet, to optimize for the safety of assets. This withdrawal lag, which is common in staking protocols, is required when AVSs go live, as there is a lag to verify that activity associated with any AVS was completed successfully.
````

## File: docs/eigenlayer/security/multisig-governance.md
````markdown
---
sidebar_position: 1
---

# Governance

Please see [EigenFoundation Governance](https://docs.eigenfoundation.org/category/protocol-governance) for latest information.
````

## File: docs/eigenlayer/security/withdrawal-delay.md
````markdown
---
sidebar_position: 5
---

# Withdrawal Delay

EigenLayer contracts feature a withdrawal delay for LST tokens, EIGEN token, and Native Restaking, a critical security measure for instances of vulnerability disclosure or when anomalous behavior is detected by monitoring systems. The delays serve as a preventive mechanism and also allows, in certain cases, to help mitigate protocol attacks. When contracts are paused and withdrawals disabled, the system enables arbitrary state or code changes to the contracts through upgrades. While technically feasible, such interventions are not a routine practice and should be approached with caution.

The Withdrawal Delay is also referred to as the Escrow Period. Please see Restaking [Escrow Period](/docs/restakers/restaking-guides/testnet/README.md#testnet-vs-mainnet-differences) for details on the specific duration.

There are two main caveats to this system. The first is the potential for a vulnerability that can bypass the withdrawal delay. The second is the risk of a flaw in the code managing requests after they have undergone the delay period.

To mitigate these risks, the approach involves optimizing complex code processes before the delay, while ensuring simpler code operations post-delay. This is coupled with the aim of developing a robust and foolproof delay framework, thereby enhancing the overall security and resilience of the system.
````

## File: docs/eigenlayer/_category_.json
````json
{
  "position": 1,
  "label": "EigenLayer"
}
````

## File: docs/eigenlayer/releases.md
````markdown
---
sidebar_position: 3
title: Releases
---
# Mainnet

The [Rewards v2 release](https://www.blog.eigenlayer.xyz/rewards-v2/) is available on mainnet. The [eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts)
and [eigenlayer-middleware](https://github.com/Layr-Labs/eigenlayer-middleware) include the Rewards v2 release on the `mainnet` branch
in each repository. 

# Testnet 

The [Slashing](https://www.blog.eigenlayer.xyz/introducing-slashing/) and Rewards v2.1 releases are available on the Holesky testnet. 
The [eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts) and [eigenlayer-middleware](https://github.com/Layr-Labs/eigenlayer-middleware)
include the Slashing release on the `testnet-holesky` branch.

:::important 
Unless specified otherwise, this documentation matches the functionality available on the Holesky testnet. For mainnet 
specific documentation, refer to the `/docs` repository on the `mainnet` branch in the [eigenlayer-contracts](https://github.com/Layr-Labs/eigenlayer-contracts)
and [eigenlayer-middleware](https://github.com/Layr-Labs/eigenlayer-middleware) repositories.
:::

## What's changed 

### Operator Sets

The Slashing release on the Holesky testnet introduced Operator Sets. The AllocationManager core contract manages Operator Sets and replaces
the AVSDirectory for registering Operators to an AVS. [The AVSDirectory will be deprecated in a future upgrade](https://docs.eigenlayer.xyz/developers/HowTo/slashing/migrate-to-operatorsets).

### Rewards v2.1

Rewards v2.1 on the Holesky testnet introduced Operator directed rewards for Operator sets. For AVSs using Operator Sets, use `createOperatorDirectedOperatorSetRewardsSubmission`. 
`createAVSRewardsSubmission` and `createOperatorDirectedAVSRewardsSubmission` remain available for use by AVSs that have not yet [migrated to Operator Sets](https://docs.eigenlayer.xyz/developers/HowTo/slashing/migrate-to-operatorsets).

### Release notes 

For complete release notes, refer to the [`eigenlayer-contracts` repository](https://github.com/Layr-Labs/eigenlayer-contracts/releases).

# SDKs

The [EigenLayer Rust SDK](https://github.com/Layr-Labs/eigensdk-rs) supports two bindngs:
* Rewards v2 - Current mainnet release.
* Slashing - Middleware's dev branch latest commit.

The [EigenLayer Go SDK](https://github.com/Layr-Labs/eigensdk-go) supports the Rewards v2.1 release. 

# Samples

The [Hello World AVS](https://github.com/Layr-Labs/hello-world-avs) and [Incredible Squaring](https://github.com/Layr-Labs/incredible-squaring-avs)
samples are available to for development and testing to get familiar with EigenLayer. We are currently updating these to 
include rewards and slashing capabilities.
````

## File: docs/eigenlayer/roadmap.md
````markdown
---
sidebar_position: 2
title: EigenLayer Roadmap
---
# EigenLayer Roadmap

EigenLayer is a protocol and developer platform for building, operating, and securing verifiable applications and services 
using Ethereum’s cryptoeconomic security. Developers can create scalable services that verify off-chain actions and enforce 
trust-based commitments. Stakers can restake ETH, liquid staking tokens (LSTs), and ERC-20 assets to secure these services, 
nominate Operators, and access performance data to make informed decisions. Institutional Stakers can participate through 
custodians or liquid restaking tokens integrated with DeFi.

Operators can secure AVSs (Autonomous Verifiable Services), earn rewards, and manage slashing risks with tools for key management,
role delegation, and secure protocol interactions. AVS builders can deploy services, attract Operators, and use EigenLayer’s SDKs,
middleware, and templates to create scalable, composable solutions. Applications built on EigenLayer benefit from seamless AVS 
integration, EigenDA for high-performance data availability, and tools for bootstrapping adoption.

This roadmap outlines **our key development priorities to make EigenLayer’s marketplace even more transparent, rewarding, 
and accessible** for Stakers, Operators, Developers, and Users alike:


<img src="/img/roadmap.png" width="75%" style={{ margin: '50px'}}>
</img>

## Universal Commitments: EigenLayer is the universal commitment marketplace

EigenLayer is pioneering a Commitment Economy offering cryptoeconomic mechanisms with intuitive user experiences to enable
a secure, functional marketplace for trust. Our roadmap for this marketplace centers on:

1. **Security and Flexibility:** EigenLayer will equip AVS builders with adaptable security models, allowing stakers to delegate
stake to operators across multiple AVS ("pooled security"). Stakers should be able to allocate natively-restaked ETH, any ERC-20
token, or assets from L2s and even non-Ethereum chains. They will be able to redelegate stakes, manage slashing risks, and
commit security long-term for added rewards. AVSs should be able to launch across chains and enforce guarantees via operator 
penalties, fund redistribution, and other offchain mechanisms.

2. **Transparency and Accessibility:** EigenLayer will offer transparency to the ecosystem through a unified dashboard displaying
metrics, monitoring and a simple Marketplace UI for restaking. Stakers and Operators will be able to access verified metadata, 
performance history, and accounting tools. Insights into live AVS activity and restaking opportunities will strengthen coordination
and inform decision making.

3. **Expressive:** EigenLayer will enable programmable incentives, allowing AVSs to distribute rewards and Operators to set reward
splits. Rewards should be flexible and expressive, aligning with AVS goals and utility. These systems will create a competitive
marketplace for high-quality Operators while aligning security and participation goals.

### In Progress Features

* **Slashing:** A key deterrent in EigenLayer, penalizing broken commitments between AVSs, Operators, and users. Unique Stake
ensures slashing mitigates risk without systemic impact.

* **Operator Sets:** Segmented Operator groups created by AVSs for business logic, rewards distribution, slashing, and architectural
organization.

* **Rewards Boost:** Enabling EIGEN to be distributed weekly to stakers and operators, proportional to AVS reward distributions.

## Trusted Primitives: EigenLayer has high-quality primitives with full-stack trust

EigenLayer will enable developers to create high-performance, trusted services and applications, by delivering:

1. **EIGEN token forking for full-stack trust:** Existing tokens like ETH and ERC20 tokens, require staking for validation
and can be penalized (slashed) for rule violations, but they are usually special-purpose (tied to a specific task) and objective
(enforceable only when violations have on-chain proofs). We proposed a novel token design for EIGEN, which expands the utility
of the staking token to be both universal (can be used for arbitrary tasks) and intersubjective (slashing enforceable as long
as the violations have offchain observability).

2. **Scalability via EigenDA:** EigenDA provides mechanisms for chains, services and apps to scale via a highly scalable 
data availability layer. EigenDA is live on mainnet, utilizes EIGEN staking and is able to run at 15 MB/s. EigenDA will 
offer integrations with OP Stack, Arbitrum Orbit, and zkSync and offer robust DA guarantees, slashing functionality through 
intersubjective forking, and flexible bandwidth options. EigenLayer Rollapps will have access to restaked services like DA, 
sequencing, and finality gadgets for optimal performance. 

### In Progress Features

* **DA Performance and Robustness:**  A network upgrade with architectural updates and efficient bridging strategies for
improved performance and robustness is being built.

* **Intersubjective Forking:** The first version of the token is live in production for EigenDA and the extension of the 
intersubjective forking to arbitrary tasks built by AVS developers is currently in development.

* **Liquidity incentivization for multiple chains:** Grow EigenDA Total Value Secured & LRT TVL by shipping our liquidity 
incentive program to multiple new partner chains.

## Verifiable Services: EigenLayer is the leading platform for building cloud services that make verifiable commitments

EigenLayer will deliver a stable, intuitive platform, enabling AVS builders to deploy verifiable cloud services efficiently
and enabling operators to manage them securely and reliably. Our roadmap for this developer platform centers on:

1. **Simplified AVS Development:** Clear documentation, templates, and tools for developers, with standardized contracts, 
pre-built integrations, and built-for-purpose services like EigenDA. Plug-and-play templates will handle business logic, 
operator penalties, and workload liveness. An intuitive UI will help developers manage AVS lifecycles with minimal overhead. 
Operators will have tools for onboarding to AVSs, including role-based access controls (RBAC), automated workflows, and a 
self-service portal for AVS evaluation. As our AVS ecosystem grows, EigenLayer will allow developers to integrate and bundle
multiple services easily, enabling faster development.

2. **Management and Monitoring:** Secure, flexible environment to support both AVS builders and Operators. Developers can 
delegate management roles, while Operators will be able to configure and maintain AVS infrastructure with key management, 
service discovery, and risk monitoring tools. The platform will include lifecycle management tooling for AVS services upgrades, 
versioning, and performance monitoring ensuring seamless operations across multiple AVSs.

3. **Cross-Service Coordination:** Modular middleware, task orchestration, and service coordination to support cross-chain
interoperability for AVS builders. Operators will gain visibility into cross-service interactions and dependencies, allowing
them to manage AVSs effectively across multiple networks. As the ecosystem expands, operators will play a crucial role in 
ensuring cross-chain compatibility and service synchronization.

4. **Expressive Incentives and Monitoring:** AVS builders will be able to distribute rewards programmatically, while operators
can monitor rewards, penalties, and staking opportunities. Customizable dashboards, alerts, and notifications will be available
for critical events. Lifecycle tools will help operators manage role delegation and fulfill AVS commitments efficiently.

### In Progress Features
   
* **AVS Lifecycle Management:** Tools to manage AVS registration, upgrades, and version control, to ensure smooth operations 
and easy governance for both builders and operators.

* **Dev Environments:** Mainnet equivalent testnet and developer sandbox environments for AVS builders and operators to 
safely test and debug services.

* **AVS Templates:** Pre-built templates for business logic integration, operator penalties and rewards.

## Get Involved

Eigen Labs is committed to building the EigenLayer protocol and expanding its services. The purpose of this roadmap is to
drive legibility for all of our users into our priorities and goals. We are committing to updating this view regularly to 
provide awareness of our progress.

While some of this work is already in flight, we are looking for feedback and welcome teams who want to contribute to the 
core protocol. To join the conversation about the future of EigenLayer, visit our [Forum](https://forum.eigenlayer.xyz/t/merged-elip-001-rewards-v2/14196).
You can also [check out our GitHub](https://github.com/eigenfoundation/ELIPs) with historical EigenLayer Improvement Proposals (ELIPs).
````
