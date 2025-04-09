Sourced from: https://github.com/Layr-Labs/eigenlayer-contracts/

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
- Only files matching these patterns are included: docs/**
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Security check has been disabled - content may contain sensitive information

## Additional Info

# Directory Structure
```
docs/
  core/
    accounting/
      DualSlashingEdgeCase.md
      SharesAccounting.md
      SharesAccountingEdgeCases.md
      StrategyBaseAccounting.md
    AllocationManager.md
    AVSDirectory.md
    DelegationManager.md
    EigenPod.md
    EigenPodManager.md
    RewardsCoordinator.md
    StrategyManager.md
  experimental/
    AVS-Guide.md
  images/
    Staker Flow Diagrams/
      diagrams.excalidraw
  permissions/
    PermissionController.md
  README.md

```

# Files

## File: docs/core/accounting/DualSlashingEdgeCase.md
````markdown
# Dual Slashing Edge Case

This document describes edge cases surrounding the slashing of a staker for native ETH by the beacon chain (BC) and an AVS. 

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
* [ELIP-004: Slashing-Aware EigenPods](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-004.md)
* [Shares Accounting](./SharesAccounting.md)

## Scenario

Consider a staker, Alice who is in the following state:

1. Alice has verified a validator. `withdrawable: 32 ETH`
2. Alice's operator is slashed for 75%. `withdrawable: 8 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 32` 
    * `maxMagnitude: 0.25`
    * `BCSF: 1`
    * `DSF: 1`
    * `withdrawable = 32 * 0.25 * 1 * 1 = 8 ETH`
    </details>
3. Alice is slashed by 16 ETH on the beacon chain

## Restaking

We define restaking as **reusing staked ETH as security for AVSs. Thus, the same Native ETH that is securing the BC (beacon chain) can also be slashed by an AVS, with priority burning rights going to the BC.**

In the above scenario, let's say the Alice now proves a checkpoint.

4. A checkpoint of BC state is proven. `withdrawable: 4 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 16`
    * `maxMagnitude: 0.25`
    * `BCSF: 1`
    * `DSF: 1`
    * `withdrawable = 16 * 0.25 * 1 * 1 = 4 ETH`
    </details>

The checkpoint slash has devalued Alice's currently withdrawable assets by 50%. The AVS slashes from what's left due to the BC getting priority burning rights. Thus, AVSs must factor Native ETH (or an LST) being slashed by the beacon chain when designing their slashing conditions. The below diagram illustrates this behavior:

| ![AVS and Beacon Chain Slashing Behavior](../../images/avs-bc-slash.png) |
|:--:|
| *Diagram showing how AVS slashing is applied after Beacon Chain slashing, with BC having priority burning rights* |

Note that the portion that is marked as BC Slash and BC + AVS Slash has priority burning rights by the beacon chain. 12 ETH has been slashed "twice", but this is by design given our definition of restaking.

The behavior of BC and AVS slashings for Native ETH mimics the behavior of slashings for an LST in isolation (see below for an additional edge case). This ensures that Native ETH security is not disadvantaged compared to LST security. ELIP-004 explains this in [more detail](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-004.md#why-do-eigenpods-need-to-upgrade).

## Ordering of EigenPod Actions

**When an AVS slashes, its attributable slashed amount is between 0 and the originally slashed amount. The attributable slashed amount decreases in the event of BC slashes.** We see this behavior in the above example, where the 12 ETH that was attributed to the AVS is less than the original 24 ETH that was slashed. 

However, given the asynchronous nature of the EigenPod proof system, Alice may have a different number of withdrawable shares depending on the ordering of her actions. Note that even in this case, **assets are not overslashed**.

Let's start with our above scenario. 

Scenario A:

4. Alice verifies another validator. `withdrawable: 40 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 64`
    * `maxMagnitude: 0.25`
    * `BCSF: 1`
    * `DSF = 40 / (32 + 32) / 0.25 = 2.5`
    * `withdrawable = 64 * 0.25 * 1 * 2.5 = 40 ETH`
    </details>

5. Alice checkpoints all her pods. `withdrawable: 30 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 64`
    * `maxMagnitude: 0.25`
    * `BCSF = 48 / 64 = 0.75`
    * `DSF: 2.5`
    * `withdrawable = 64 * 0.25 * 0.75 * 2.5 = 30 ETH`
    </details>

In this scenario, 25% of Alice's currently proven assets are slashed. Similarly, the AVSs attributable slashed amount has been decreased by 25% (24 → 18 ETH). 


Scenario B:

4. Alice checkpoints her pod. `withdrawable: 4 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 32`
    * `maxMagnitude: 0.25`
    * `BCSF = 16 / 32 = 0.5`
    * `DSF: 1`
    * `withdrawable = 32 * 0.25 * 0.5 * 1 = 4 ETH`
    </details>

5. Alice verifies another validator. `withdrawable: 40 ETH`
    <details>
    <summary>Calculation</summary>

    * `depositShares: 64`
    * `maxMagnitude: 0.25`
    * `BCSF: 0.5`
    * `DSF = 36 / (32 + 32) / 0.125 = 4.5`
    * `withdrawable = 64 * 0.25 * 0.5 * 4.5 = 36 ETH`
    </details>

In scenario B, 50% of Alice's currently proven assets are slashed, along with a commensurate decrease in the AVSs attributable slashed amount. In both cases Alice's withdrawable shares and the AVSs attributable slashed amount decrease by the same percentage.

We acknowledge this edge case. A benefit of this system is that stakers are incentivized to immediately prove BC slashed. Eigen Labs runs an off-chain process (EigenPod Health Checker) that monitors BC slashings and starts checkpoints as needed. Conversely, when Native-ETH burning is implemented, AVSs are incentivized to immediately exit stakers from the BC to recoup the maximum possible attributable slashed amount.  

This edge case also applies if Alice undelegates after being slashed on the beacon chain, and then continues along with Scenario A, exiting her position fully. See below for details:
<details>
<summary>Scenario</summary>

1. Alice verifies a validator: `withdrawable: 32 ETH`
2. Alice's operator is slashed for 100%. `withdrawable: 0 ETH` 
3. Alice is slashed by 16 ETH on the beacon chain. 
4. Alice undelegates. `depositShares = 0` 
5. Alice verifies another validator. `withdrawable: 32 ETH`. `depositShares: 32 ETH` 
6. Alice checkpoints her slash from step 3. `withdrawable: 24 ETH`
    - `restakedExecutionLayerGwei = 16`. This is the AVSs attributable slashed amount, but it increases once Alice completely exits. 
    - `BCSF= 48/64 = 0.75`
7. Alice completes her withdrawal as shares from undelegation. No affect since the operator's magnitude was 0
8. Alice exits her validator from step 5. `withdrawable: 24 ETH`
    - `restakedExecutionLayerGwei = 48` 
9. Alice queues a withdrawal for all shares. `scaledShares = 32` 
10. Alice completes her withdrawal. Alice receives 24 ETH
    - `scaledShares * slashingFactor = 32 * 0.75 = 24` 
11. There is 24 ETH locked up in the pod. 
</details>
````

## File: docs/core/accounting/SharesAccounting.md
````markdown
[elip-002]: https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md

# Shares Accounting

This document outlines the changes to the staker and operator Shares accounting resulting from the Slashing Upgrade. There are several introduced variables such as the _deposit scaling factor_ ($k_n$), _max magnitude_ ($m_n$), and _beacon chain slashing factor_ ($l_n$). How these interact with the operator and staker events like deposits, slashing, withdrawals will all be described below.

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)

## Pre-Slashing Upgrade

We'll look at the "shares" model as historically defined prior to the Slashing upgrade. Pre-slashing, stakers could receive shares for deposited assets, delegate those shares to operators, and withdraw those shares from the protocol. We can write this a bit more formally:

#### Staker Level

$s_n$ - The amount of shares in the storage of the `StrategyManager`/`EigenPodManager` at time n.

#### Operator Level

$op_n$ - The operator shares in the storage of the `DelegationManager` at time n which can also be rewritten as \
$op_n = \sum_{i=1}^{k} s_{n,i}$ where the operator has $k$ number of stakers delegated to them.


#### Staker Deposits 

Upon each staker deposit of amount $d_n$ at time $n$, the staker's shares and delegated operator's shares are updated as follows:

$$
 s_{n+1} = s_{n} + d_{n}
$$

$$
 op_{n+1} = op_{n} + d_{n}
$$

#### Staker Withdrawals 

Similarly for staker withdrawals, given an amount $w_n$ to withdraw at time $n$, the staker and operator's shares are decremented at the point of the withdrawal being queued:

$$
 s_{n+1} = s_{n} - w_{n}
$$

$$  
 op_{n+1} = op_{n} - w_{n}
$$

Later after the withdrawal delay has passed, the staker can complete their withdrawal to withdraw the full amount $w_n$ of shares. 

---

## Slashing Upgrade Changes

The remaining portions of this document will assume understanding of Allocations/Deallocations, Max Magnitudes, and Operator Sets as described in [ELIP-002][elip-002].

### Terminology

The word "shares" in EigenLayer has historically referred to the amount of shares a staker receives upon depositing assets through the `StrategyManager` or `EigenPodManager`. Outside of some conversion ratios in the `StrategyManager` to account for rebasing tokens, shares roughly correspond 1:1 with deposit amounts (i.e. 1e18 shares in the `beaconChainETHStrategy` corresponds to 1 ETH of assets). When delegating to an operator or queueing a withdrawal, the `DelegationManager` reads deposit shares from the `StrategyManager` or `EigenPodManager` to determine how many shares to delegate (or undelegate).

With the slashing release, there is a need to differentiate "classes" of shares.

**Deposit shares**: 

Formerly known as "shares," these are the same shares used before the slashing release. They continue to be managed by the `StrategyManager` and `EigenPodManager`, and roughly correspond 1:1 with deposited assets.

**Withdrawable shares**: 

When an operator is slashed, the slash is applied to their stakers _asynchronously_ (otherwise, slashing would require iterating over each of an operator's stakers; this is prohibitively expensive). 

The `DelegationManager` must find a common representation for the deposit shares of many stakers, each of which may have experienced different amounts of slashing depending on which operator they are delegated to, and when they delegated. This common representation is achieved in part through a value called the `depositScalingFactor`: a per-staker, per-strategy value that scales a staker's deposit shares as they deposit assets over time.

When a staker does just about anything (changing their delegated operator, queueing/completing a withdrawal, depositing new assets), the `DelegationManager` converts their _deposit shares_ to _withdrawable shares_ by applying the staker's `depositScalingFactor` and the current _slashing factor_ (a per-strategy scalar primarily derived from the amount of slashing an operator has received in the `AllocationManager`).

These _withdrawable shares_ are used to determine how many of a staker's deposit shares are actually able to be withdrawn from the protocol, as well as how many shares can be delegated to an operator. An individual staker's withdrawable shares are not reflected anywhere in storage; they are calculated on-demand.

**Operator shares**:

_Operator shares_ are derivative of _withdrawable shares_. When a staker delegates to an operator, they are delegating their _withdrawable shares_. Thus, an operator's _operator shares_ represent the sum of all of their stakers' _withdrawable shares_. Note that when a staker first delegates to an operator, this is a special case where _deposit shares_ == _withdrawable shares_. If the staker deposits additional assets later, this case will not hold if slashing was experienced in the interim.

---

Each of these definitions can also be applied to the pre-slashing share model, but with the caveat that for all stakers, _withdrawable shares equal deposit shares_. After the slashing upgrade this is not necessarily the case - a staker may not be able to withdraw the amount they deposited if their operator got slashed.

Now let's look at these updated definitions in detail and how the accounting math works with deposits, withdrawals, and slashing.

### Stored Variables

Note that these variables are all defined within the context of a single Strategy. Also note that the concept of "1" used within these equations is represented in the code by the constant `1 WAD`, or `1e18`.

#### Staker Level

$s_n$ - The amount of deposit shares in the storage of the `StrategyManager`/`EigenPodManager` at time $n$. In storage: `StrategyManager.stakerDepositShares` and `EigenPodManager.podOwnerDepositShares`

$k_n$ - The staker's “deposit scaling factor” at time $n$. This is initialized to 1. In storage: `DelegationManager.depositScalingFactor`

$l_n$ - The staker's "beacon chain slashing factor" at time $n$. This is initialized to 1. For any equations concerning non-native ETH strategies, this can be assumed to be 1. In storage: `EigenPodManager.beaconChainSlashingFactor`

#### Operator Level

$m_n$ - The operator magnitude at time n. This is initialized to 1.

$op_n$ - The operator shares in the storage of the `DelegationManager` at time n. In storage: `DelegationManager.operatorShares`

### Conceptual Variables

$a_n = s_n k_n l_n m_n$ - The withdrawable shares that the staker owns at time $n$. Read from view function `DelegationManager.getWithdrawableShares`

Note that $op_n = \sum_{i=1}^{k} a_{n,i}$.

---

### Deposits

For an amount of newly deposited shares $d_n$,

#### Staker Level

Conceptually, the staker's deposit shares and withdrawable shares both increase by the deposited amount $d_n$. Let's work out how this math impacts the deposit scaling factor $k_n$.

$$
a_{n+1} = a_n + d_n
$$

$$
s_{n+1} = s_n +d_n
$$

$$
l_{n+1} = l_n
$$

$$
m_{n+1} = m_n
$$

Expanding the $a_{n+1}$ calculation

$$
s_{n+1} k_{n+1} l_{n+1} m_{n+1} = s_n k_n l_n m_n + d_n
$$

Simplifying yields:

$$
k_{n+1} = \frac{s_n k_n l_n m_n + d_n}{s_{n+1} l_{n+1} m_{n+1}}=\frac{s_n k_n l_n m_n + d_n}{(s_n+d_n)l_nm_n}
$$

Updating the slashing factor is implemented in `SlashingLib.update`.

#### Operator Level

For the operator (if the staker is delegated), the delegated operator shares should increase by the exact amount
the staker just deposited. Therefore $op_n$ is updated as follows:

$$
op_{n+1} = op_n+d_n
$$

See implementation in:
* [`StrategyManager.depositIntoStrategy`](../../../src/contracts/core/StrategyManager.sol)
* [`EigenPodManager.recordBeaconChainETHBalanceUpdate`](../../../src/contracts/pods/EigenPodManager.sol)


---

### Delegation

Suppose we have an undelegated staker who decides to delegate to an operator.
We have the following properties that should be preserved.

#### Operator Level

Operator shares should be increased by the amount of delegatable shares the staker has, this is synonymous to their withdrawable shares $a_n$. Therefore,

$$
op_{n+1} = op_{n} + a_n
$$

$$
= op_{n} + s_n k_n l_n m_n
$$


#### Staker Level

withdrawable shares should remain unchanged

$$
a_{n+1} = a_n
$$

deposit shares should remain unchanged

$$
s_{n+1} = s_n
$$

beaconChainSlashingFactor and maxMagnitude should also remain unchanged. In this case, since the staker is not delegated, then their maxMagnitude should by default be equal to 1.

$$
l_{n+1} = l_n
$$

Now the question is what is the new depositScalingFactor equal to?

$$
a_{n+1} = a_n
$$

$$
=> s_{n+1} k_{n+1} l_{n+1} m_{n+1} = s_n k_n l_n m_n
$$

$$
=> s_{n} k_{n+1} l_{n} m_{n+1} = s_n k_n l_n m_n
$$

$$
=> k_{n+1} = \frac {k_n m_n} { m_{n+1} }
$$

Notice how the staker variables that update $k_{n+1}$ and $m_{n+1}$ do not affect previously queued withdrawals and shares received upon withdrawal completion. This is because the maxMagnitude that is looked up is dependent on the operator at the time of the queued withdrawal and the $k_n$ is effectively stored in the scaled shares field.

---

### Slashing

Given a proportion to slash $p_n = \frac {m_{n+1}}{m_n}$ ,

#### Operator Level

From a conceptual level, operator shares should be decreased by the proportion according to the following:

$$
 op_{n+1} = op_n p_n
$$

$$
 => op_{n+1} = op_n \frac {m_{n+1}} {m_n}
$$ 

Calculating the amount of $sharesToDecrement$:

$$
 sharesToDecrement = op_n - op_{n+1}
$$

$$
 = op_n - op_n \frac {m_{n+1}} {m_n}
$$

This calculation is performed in `SlashingLib.calcSlashedAmount`.

#### Staker Level

From the conceptual level, a staker's withdrawable shares should also be proportionally slashed so the following must be true:

$$
a_{n+1} = a_n p_n
$$

We don't want to update storage at the staker level during slashing as this would be computationally too expensive given an operator has a 1-many relationship with its delegated stakers. Therefore we want to prove $a_{n+1} = a_n p_n$ since withdrawable shares are slashed by $p_n$.

Given the following:

$l_{n+1} = l_n$ \
$k_{n+1} = k_n$ \
$s_{n+1} = s_n$

Expanding the $a_{n+1}$ equation:

$$
a_{n+1} = s_{n+1} k_{n+1} l_{n+1} m_{n+1}
$$

$$
=> s_{n} k_{n} l_{n} m_{n+1}
$$

We know that  $p_n = \frac {m_{n+1}}{m_n}$ =>  $m_{n+1} = m_n p_n$ 

$$
=>  s_n k_n l_n m_n p_n
$$

$$
=> a_n p_n
$$

This means that a staker's withdrawable shares are immediately affected upon their operator's maxMagnitude being decreased via slashing.

---

### Queue Withdrawal

Withdrawals are queued by inputting a `depositShares` amount $x_n <= s_n$. The actual withdrawable amount $w_n$ corresponding to $x_n$ is given by the following:

$$
 w_n = x_n k_n l_n m_n
$$

This conceptually makes sense as the amount being withdrawn $w_n$ is some amount <= $a_n$ which is the total withdrawable shares amount for the staker. 


#### Operator Level

When a staker queues a withdrawal, their operator's shares are reduced accordingly:

$$
 op_{n+1} = op_n - w_n
$$


#### Staker Level

$$
 a_{n+1} = a_n - w_n
$$

$$
 s_{n+1} = s_n - x_n
$$

This means that when queuing a withdrawal, the staker inputs a `depositShares` amount $x_n$. The `DelegationManager` calls the the `EigenPodManager`/`StrategyManager` to decrement their `depositShares` by this amount. Additionally, the `depositShares` are converted to a withdrawable amount $w_n$, which are decremented from the operator's shares.

We want to show that the total withdrawable shares for the staker are decreased accordingly such that $a_{n+1} = a_n - w_n$.

Given the following:

$l_{n+1} = l_n$ \
$k_{n+1} = k_n$ \
$s_{n+1} = s_n$

Expanding the $a_{n+1}$ equation:

$$
 a_{n+1} = s_{n+1} k_{n+1} l_{n+1} m_{n+1}
$$


$$
 =>  (s_{n} - x_n) k_{n+1} l_{n+1} m_{n+1}
$$

$$
 =  (s_{n} - x_n) k_n l_n m_n
$$

$$
 = s_n k_n l_n m_n - x_n k_n l_n m_n
$$

$$
 = a_n - w_n
$$

Note that when a withdrawal is queued, a `Withdrawal` struct is created with _scaled shares_ defined as $q_t = x_t k_t$ where $t$ is the time of the queuing. The reason we define and store scaled shares like this will be clearer in [Complete Withdrawal](#complete-withdrawal) below.

Additionally, we reset the depositScalingFactor when a user queues a withdrawal for all their shares, either through un/redelegation or directly. This is because the DSF at the time of withdrawal is stored in the scaled shares, and any "new" deposits or delegations by the staker should be considered as new. Note that withdrawal completion is treated as a kind of deposit when done as shares, which again will be clearer below.

See implementation in:
* `DelegationManager.queueWithdrawals`
* `SlashingLib.scaleForQueueWithdrawal`

<br>

---

### Complete Withdrawal

Now the staker completes a withdrawal $(q_t, t)$ which was queued at time $t$.

#### Operator Level

If the staker completes the withdrawal _as tokens_, any operator shares remain unchanged. The original operator's shares were decremented when the withdrawal was queued, and a new operator does not receive shares if the staker is withdrawing assets ("as tokens").

However, if the staker completes the withdrawal _as shares_, the shares are added to the staker's current operator according to the formulae in [Deposits](#deposits).

#### Staker Level

<!-- There are no storage updates for the staker outside of needing to calculate the shares to send the staker. -->

Recall from [Queue Withdrawal](#queue-withdrawal) that, when a withdrawal is queued, the `Withdrawal` struct stores _scaled shares_, defined as $q_t = x_t k_t$ where $x_t$ is the deposit share amount requested for withdrawal and $t$ is the time of the queuing.

And, given the formula for calculating withdrawable shares, the withdrawable shares given to the staker are $w_t$:

$$
w_t = q_t m_t l_t = x_t k_t l_t m_t
$$

However, the staker's shares in their withdrawal may have been slashed while the withdrawal was in the queue. Their operator may have been slashed by an AVS, or, if the strategy is the `beaconChainETHStrategy`, the staker's validators may have been slashed/penalized.

The amount of shares they actually receive is proportionally the following:

$$
    \frac{m_{t+delay} l_{now} }{m_t l_t}
$$

So the actual amount of shares withdrawn on completion is calculated to be:

$$
sharesWithdrawn = w_t (\frac{m_{t+delay} l_{now}}{m_t l_t} )
$$

$$ 
= x_t k_t l_t m_t (\frac{m_{t+delay} l_{now}}{m_t l_t} )
$$

$$ 
= x_t k_t m_{t+delay} l_{now}
$$

Now we know that $q_t = x_t k_t$ so we can substitute this value in here. 

$$ 
= q_t m_{t+delay} l_{now}
$$

From the above equations the known values we have during the time of queue withdrawal is $x_t k_t$ and we only know $m_{t+delay} l_{now}$ when the queued withdrawal is completable. This is why we store scaled shares as $q_t = x_t k_t$. The other term ($m_{t+delay} l_{now}$) is read during the completing transaction of the withdrawal.

Note: Reading $m_{t+delay}$ is performed by a historical Snapshot lookup of the max magnitude in the `AllocationManager` while $l_{now}$, the current beacon chain slashing factor, is done through the `EigenPodManager`. Recall that if the strategy in question is not the `beaconChainETHStrategy`, $l_{now}$ will default to "1".

The definition of scaled shares is used solely for handling withdrawals and accounting for slashing that may have occurred (both on EigenLayer and on the beacon chain) during the queue period.

See implementation in:
* `DelegationManager.completeQueuedWithdrawal`
* `SlashingLib.scaleForCompleteWithdrawal`

---

### Handling Beacon Chain Balance Decreases in EigenPods

Beacon chain balance decreases are handled differently after the slashing upgrade with the introduction of $l_n$ the beacon chain slashing factor. 

Prior to the upgrade, any decreases in an `EigenPod` balance for a staker as a result of completing a checkpoint immediately decrements from the staker's shares in the `EigenPodManager`. As an edge case, this meant that a staker's shares could go negative if, for example, they queued a withdrawal for all their shares and then completed a checkpoint on their `EigenPod` showing a balance decrease.

With the introduction of the beacon chain slashing factor, beacon chain balance decreases no longer result in a decrease in deposit shares. Instead, the staker's beacon chain slashing factor is decreased, allowing the system to realize that slash in any existing shares, as well as in any existing queued withdrawals. Effectively, this means that beacon chain slashing is accounted for similarly to EigenLayer-native slashing; _deposit shares remain the same, while withdrawable shares are reduced:_

![.](../../images/slashing-model.png)

Now let's consider how beacon chain balance decreases are handled when they represent a negative share delta for a staker's EigenPod.

#### Added Definitions

$welw$ is `withdrawableExecutionLayerGwei`. This is purely native ETH in the `EigenPod`, attributed via checkpoint and considered withdrawable by the pod (but without factoring in any EigenLayer-native slashing). `DelegationManager.getWithdrawableShares` can be called  to account for both EigenLayer and beacon chain slashing.

$before\text{ }start$ is time just before a checkpoint is started

<!-- $before\text{ }complete$ is time just before a checkpoint is completed -->

$after\text{ }complete$ is the time just after a checkpoint is completed 

As a checkpoint is completed, the total assets represented by the pod's native ETH and beacon chain balances _before_ and _after_ are given by:

$g_n = welw_{before\text{ }start}+\sum_i validator_i.balance_{before\text{ }start}$ \
$h_n = welw_{after\text{ }complete}+\sum_i validator_i.balance_{after\text{ }complete}$

#### Staker Level

Conceptually, the above logic specifies that we decrease the staker's withdrawable shares proportionally to the balance decrease:

$$
a_{n+1} = \frac{h_n}{g_n}a_n
$$

We implement this by setting

$$
l_{n+1}=\frac{h_n}{g_n}l_n
$$

Given:

$m_{n+1}=m_n$ (staker beacon chain slashing does not affect its operator's magnitude)
$s_{n+1} = s_n$ (no subtraction of deposit shares)
$k_{n+1}=k_n$

Then, plugging into the formula for withdrawable shares:

$$
a_{n+1} = s_{n+1}k_{n+1}l_{n+1}m_{n+1}
$$

$$
=s_nk_n\frac{h_n}{g_n}l_nm_n
$$

$$
= \frac{h_n}{g_n}a_n
$$

#### Operator Level

Now we want to update the operator's shares accordingly. At a conceptual level $op_{n+1}$ should be the following:

$$
 op_{n+1} = op_n - a_n + a_{n+1}
$$

We can simplify this further


$$
 =op_{n}-s_nk_nl_nm_n + s_nk_nl_{n+1}m_n
$$


$$
 = op_{n}+s_nk_nm_n(l_{n+1}-l_n)
$$

See implementation in:
* `EigenPodManager.recordBeaconChainETHBalanceUpdate`
* `DelegationManager.decreaseDelegatedShares`

---

## Implementation Details

In practice, we can’t actually have floating values so we will substitute all $k_n, l_n, m_n$ terms with $m_n$/1e18  $\frac{k_n}{1e18},\frac{l_n}{1e18} ,\frac{m_n}{1e18}$ respectively where $k_n, l_n, m_n$ are the values in storage, all initialized to 1e18. This allows us to conceptually have values in the range $[0,1]$.

We make use of OpenZeppelin's Math library and `mulDiv` for calculating $floor(\frac{x \cdot y}{denominator})$ with full precision. Sometimes for specific rounding edge cases, $ceiling(\frac{x \cdot y}{denominator})$ is explicitly used.

#### Multiplication and Division Operations
For all the equations in the above document, we substitute any product operations of $k_n, l_n, m_n$ with the `mulWad` pure function.
```solidity
function mulWad(uint256 x, uint256 y) internal pure returns (uint256) {
    return x.mulDiv(y, WAD);
}
```

Conversely, for any divisions of $k_n, l_n, m_n$ we use the `divWad` pure function.

```solidity
function divWad(uint256 x, uint256 y) internal pure returns (uint256) {
    return x.mulDiv(WAD, y);
}
```
````

## File: docs/core/accounting/SharesAccountingEdgeCases.md
````markdown
[elip-002]: https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md

# Shares Accounting Edge Cases

This document is meant to explore and analyze the different mathematical operations we are performing in the slashing release. Primarily we want to ensure safety on rounding and overflow situations. Prior reading of the [Shares Accounting](./SharesAccounting.md) is required to make sense of this document.

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
* [Shares Accounting](./SharesAccounting.md)


## Fully Slashed for a Strategy

Within the context of a single Strategy, recall that updates to the deposit scaling factor $k_n$ are defined as the following:

$$
k_{n+1} = \frac{s_n k_n m_n + d_n}{s_{n+1} l_{n+1} m_{n+1}}=\frac{s_n k_n l_n m_n + d_n}{(s_n+d_n)l_nm_n}
$$

We can see here that calculating $k_{n+1}$ can give us a divide by 0 error if any of $(s_n + d_n)$, $l_n$, or $m_n$ are equal to 0. The $(s_n + d_n) = 0$ case should not arise because the `EigenPodManager` and `StrategyManager` will not report share increases in this case. However, the other two terms may reach 0:
* When an operator is 100% slashed for a given strategy and their max magnitude $m_n = 0$
* When a staker's `EigenPod` native ETH balance is 0 _and_ their validators have all been slashed such that $l_n = 0$

In these cases, updates to a staker's deposit scaling factor will encounter a division by 0 error. In either case, we know that since either the operator was fully slashed or the staker was fully slashed for the `beaconChainETHStrategy` then their withdrawable shares $a_n = 0$.

In practice, if $m_n = 0$ for a given operator, then:
1. Any staker who is already delegated to this operator _will be unable to deposit additional assets into the corresponding strategy_ 
2. Any staker that currently holds deposit shares in this strategy and is NOT delegated to the operator _will be unable to delegate to the operator_

Note that in the first case, it _is_ possible for the staker to undelegate, queue, and complete withdrawals - though as $a_n = 0$, they will not receive any withdrawable shares as a result.

Additionally, if $l_n = 0$ for a given staker in the beacon chain ETH strategy, then **any further deposits of ETH or restaking of validators will not yield shares in EigenLayer.** This should only occur in extraordinary circumstances, as a beacon chain slashing factor of 0 means that a staker both has ~0 assets in their `EigenPod`, and ALL of their validators have been ~100% slashed on the beacon chain - something that happens only when coordinated groups of validators are slashed. If this case occurs, an `EigenPod` is essentially bricked - the pod owner should NOT send ETH to the pod, and should NOT point additional validators at the pod.

These are all expected edge cases and their occurrences and side effects are within acceptable tolerances.

## Upper Bound on Deposit Scaling Factor $k_n$

Let's examine potential overflow situations with respect to calculating a staker's withdrawable shares.
Below is the function in `SlashingLib.sol` which calculates $a_n = s_nk_nl_nm_n$. \
Note: `slashingFactor` = $l_nm_n$

```solidity
function calcWithdrawable(
    DepositScalingFactor memory dsf,
    uint256 depositShares,
    uint256 slashingFactor
) internal pure returns (uint256) {
    /// forgefmt: disable-next-item
    return depositShares
        .mulWad(dsf.scalingFactor())
        .mulWad(slashingFactor);
}
```

`depositShares` are the staker’s shares $s_n$ in storage. We know this can at max be 1e38 - 1 as this is the max total shares we allow in a strategy. $l_n ≤ 1e18$ and $m_n ≤ 1e18$ as they are montonically decreasing values. So a `mulWad` of the `slashingFactor` operation should never result in a overflow, it will always result in a smaller or equal number.

The question now comes to `depositShares.mulWad(dsf.scalingFactor())` and whether this term will overflow a `uint256`. Let's examine the math behind this. The function `SlashingLib.update` performs the following calculation:

$$
k_{n+1} =\frac{s_n k_n l_n m_n + d_n}{(s_n+d_n)l_nm_n}
$$

Assuming:
- $k_0 = 1$
- 0 < $l_0$ ≤ 1 and is monotonically decreasing but doesn’t reach 0
- 0 < $m_0$ ≤ 1 and is monotonically decreasing but doesn’t reach 0
- 0 ≤ $s_n, {s_{n+1}}$ ≤ 1e38 - 1 (`MAX_TOTAL_SHARES = 1e38 - 1` in StrategyBase.sol)
- 0 < $d_n$ ≤ 1e38 - 1
- ${s_{n+1}}={s_n} + {d_n}$

Rewriting above we can get the following by factoring out the k and cancelling out some terms.

$$
k_{n+1} = k_n\frac{s_n}{s_n + d_n} + \frac{d_n}{(s_n+d_n)l_nm_n}
$$

The first term  $\frac{s_n}{{{s_n} + {d_n}}}$ < 1 so when multiplied with $k_n$ will not contribute to the growth of ${k_{n+1}}$ if only considering this term. 

The second term $\frac{d_n}{({{s_n} + {d_n}}){l_n}{m_n}}$ however can make $k_n$ grow over time depending on how small ${l_n}{m_n}$ becomes and also how large $d_n$ is proportionally compared to $s_n$. We only care about the worst case scenario here so let’s assume the upper bound on the existing shares and new deposit amount by rounding the value up to 1.

Now in practice, the smallest values ${l_n}$ and ${m_n}$ could equal to is 1/1e18. Substituting this in the above second term gives the following:

$$
\frac{d_n}{(s_n+d_n)l_nm_n} = \frac{d_n}{s_n+d_n}*1e18^2
$$

So lets round up the first term $\frac{s_n}{{{s_n} + {d_n}}}$ to 1 and also $\frac{d_n}{{{s_n} + {d_n}}}$ in the second term to 1. We can simplify the recursive definition of k in this worst case scenario as the following.

$$
k_{n+1} = k_n\frac{s_n}{s_n + d_n} + \frac{d_n}{(s_n+d_n)l_nm_n}
$$

$$
=> k_{n+1} = k_n+ \frac{d_n}{(s_n+d_n)l_nm_n}
$$

$$
=> k_{n+1} = k_n + 1e36
$$

Because of the max shares in storage for a strategy is 1e38 - 1 and deposits must be non-zero we can actually come up with an upper bound on ${k_n}$ by having 1e38-1 deposits of amount 1, updating ${k_n}$ each time.

$$
k_{1e38-1} \approx (1e38-1)\cdot 1e36 < 1e74
$$

After 1e38-1 iterations/deposits, the upper bound on k we calculate is 1e74 in the _worst_ case scenario. This is technically possible if as a staker, you are delegated to an operator for the beaconChainStrategy where your operator has been slashed 99.9999999…% for native ETH but also as a staker you have had proportional EigenPod balance decreases up to 99.9999999…..%.

The max shares of 1e38-1 also accommodates the entire supply of ETH as well (only needs 27 bits). For normal StrategyManager strategies,  ${l_n} = 1$ and ${k_n}$ would not grow nearly to the same extent.

Clearly this value of 1e74 for ${k_n}$ fits within a uint256 storage slot.

Bringing this all back to the `calcWithdrawable` method used to calculate your actual withdrawable shares for a staker as well as the actual next ${k_{n+1}}$ value. We can see here that the shares is not expected to overflow given the constraints on all our variables and the use of the depositScalingFactor is safe.


The staker depositScalingFactor is unbounded on how it can increase over time but because of the lower bounds we have  ${l_n}$ and  ${m_n}$ as well as the upper bound on number of shares a strategy has (or amount of ETH in existence w.r.t beaconChainStrategy) we can see that it is infeasble for the deposit scaling factor $k_n$ to overflow in our contracts.  



## Rounding Behavior Considerations

The `SlashingLib.sol` introduces some small rounding precision errors due to the usage of `mulWad`/`divWad` operations in the contracts where we are doing a `x * y / denominator` operation. In Solidity, we round down to the nearest integer introducing an absolute error of up to 1 wei. Taking this into consideration, in certain portions of code, we will explicitly use either take the floor or ceiling value of `x * y / denominator`.

This has implications on several parts of the system. For example, completing a withdrawal as shares and having your updated withdrawable shares being less than what it was originally due to rounding. For stakers having a non-WAD beacon chain slashing factor(BCSF) this is essentially self induced from being penalized/slashed on the BC. For operator's have non-WAD maxMagnitudes for specific strategies, it is also a result of them being slashed by the OperatorSet(s) they are allocated to. Stakers should be wary of delegating to operators of low maxMagnitude for the strategies they they have deposits in. The impact of rounding error can result in a larger discrepancy between what they _should_ have withdrawable vs what they actually can withdraw.

### Rounding up on Slashing

When an operator is slashed by an operatorSet in the `AllocationManager`, we actually want to round up on slashing. Rather than calculating `floor(x * y / denominator)` from mulDiv, we want `ceiling(x * y / denominator)`. This is because we don’t want any kind of DOS scenario where an operatorSet attempting to slash an operator is rounded to 0; potentially possible if an operator registered for their own fake AVS and slashed themselves repeatedly to bring their maxMagnitude to a small enough value. This will ensure an operator is always slashed for some amount from their maxMagnitude which eventually, if they are slashed enough, can reach 0.

`AllocationManager.slashOperator`
```solidity
// 3. Calculate the amount of magnitude being slashed, and subtract from
// the operator's currently-allocated magnitude, as well as the strategy's
// max and encumbered magnitudes
uint64 slashedMagnitude = uint64(uint256(allocation.currentMagnitude).mulWadRoundUp(params.wadsToSlash[i]));
```

### Deposits actually _reducing_ withdrawableShares

There are some very particular edge cases where, due to rounding error, deposits can actually decrease withdrawble shares for a staker which is conceptually wrong.
The unit test `DelegationUnit.t.sol:test_increaseDelegatedShares_depositRepeatedly` exemplifies this where there is an increasing difference over the course of multiple deposits between a staker's withdrawable shares and the staker's delegated operator shares.
Essentially, what’s happening in this test case is that after the very first deposit of a large amount of shares, subsequent deposits of amount 1000 are causing the getWithdrawable shares to actually decrease for the staker.

Since the operatorShares are simply incrementing by the exact depositShares, the operatorShares mapping is increasing as expected. This ends up creating a very big discrepancy/drift between the two values after performing 1000 deposits. The difference between the operatorShares and the staker’s withdrawableShares ends up being `4.418e13`.

Granted the initial deposit amount was `4.418e28` which is magnitudes larger than the discrepancy here but this its important to note the side effects of the redesigned accounting model.
Instead of purely incremented/decremented amounts, we have introduced magnitudes and scaling factor variables which now result in small amounts of rounding error from division in several places. We deem this rounding behavior to be tolerable given the costs associated for the number of transactions to emulate this and the proportional error is very small.

### Slashing and Rounding Up Operator Shares and Rounding down on Staker Withdrawable Shares

As can be observed in the `SlashingLib.sol` library, we round up on the operatorShares when slashing and round down on the staker's withdrawableShares. If we look at a core invariant of the shares accounting model, we ideally want to preserve the following:

$$
op_n = \sum_{i=1}^{k} a_{n,i}
$$

where $op_n$ is the operatorShares at time $n$ and $a_{n,i}$ is the staker's withdrawableShares at time $n$ for the $i^{th}$ staker.

However due to rounding limitations, there will be some error introduced in calculating the amount of operator shares to slash above and also in calculating the staker's withdrawableShares. To prevent a situation where all stakers were to attempt to withdraw and the operatorShares underflows, we round up on the operatorShares when slashing and round down on the staker's withdrawableShares.

So in practice, the above invariant becomes.

$$
op_n \geq \sum_{i=1}^{k} a_{n,i}
$$

Upwards rounding on calculating the amount of operatorShares to give to an operator after slashing is intentionally performed in `SlashingLib.calcSlashedAmount`.
For calculating a staker's withdrawableShares, there are many different factors to consider such as calculating their depositScalingFactor, their slashingFactor, and calculating the amount of withdrawable shares altogether with their depositShares. These variables are all by default rounded down in calculation and is expected behavior for stakers.


## Upper bound on Residual Operator Shares

Related to the above rounding error on deposits, we want to calculate what is the worst case rounding error for a staker depositing shares into EigenLayer.
That is, what is the largest difference between the depositShares deposited and the resulting withdrawableShares? For a staker who initially deposits without getting slashed, these two values should conceptually be equal. Let's examine below.

Below is a code snippet of `SlashingLib.sol`
```solidity
function update(
    DepositScalingFactor storage dsf,
    uint256 prevDepositShares,
    uint256 addedShares,
    uint256 slashingFactor
) internal {
    // If this is the staker's first deposit, set the scaling factor to
    // the inverse of slashingFactor
    if (prevDepositShares == 0) {
        dsf._scalingFactor = uint256(WAD).divWad(slashingFactor);
        return;
    }

...

function calcWithdrawable(
    DepositScalingFactor memory dsf,
    uint256 depositShares,
    uint256 slashingFactor
) internal pure returns (uint256) {
    /// forgefmt: disable-next-item
    return depositShares
        .mulWad(dsf.scalingFactor())
        .mulWad(slashingFactor);
}
```

Mathematically, withdrawable shares can be represented as below

$$
withdrawableShares = d\space\cdot\space \frac{k}{WAD} \space\cdot\space \frac{slashingFactor}{WAD}
$$

Substituting $k$ with `WAD.divWad(slashingFactor)` (see update function above) if the staker only has done one single deposit of amount $d$. Also expanding out slashingFactor which is `maxMagnitude.mulWad(beaconChainScalingFactor)`

$$
= d\space\cdot\space \frac{\frac{WAD\space\cdot \space WAD}{m_{deposit} \cdot l_{deposit}}}{WAD} \space\cdot\space \frac{\frac{m \space\cdot\space l}{WAD}}{WAD}
$$

Above is the real true value of the amount of withdrawable shares a staker has but in practice, there are rounding implications at each division operation. It becomes the following

$$
withdrawableShares (rounded) =
\lfloor
\lfloor 
d \space\cdot\space 
\frac{\lfloor\frac{WAD\space\cdot \space WAD
}{m_{deposit} \space\cdot\space l_{deposit}}
\rfloor }{WAD}
\rfloor 
\space\cdot\space \frac{\lfloor \frac{m \space\cdot\space l}{WAD}\rfloor}{WAD}
\rfloor
$$

Each floor operation can introduce a rounding error of at most 1 wei. Because there are nested divisions however, this error can result in a total error thats larger than just off by 1 wei.
We can rewrite parts of above with epsilon $e$ which is in the range of [0,1].

1. First inner rounded term

$$
\frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit}} = \lfloor \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit}} \rfloor + \epsilon_1
$$

$$
\frac{\lfloor \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit}} \rfloor}{WAD} = \frac{\frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit}} - \epsilon_1}{WAD}
$$

2. Second rounded term

$$
\lfloor d \cdot \frac{\lfloor \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit}} \rfloor}{WAD} \rfloor
$$

$$
= \lfloor d \cdot \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit} \cdot WAD} - d \cdot \frac{\epsilon_1}{WAD} \rfloor
$$

$$
= d \cdot \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit} \cdot WAD} - d \cdot \frac{\epsilon_1}{WAD} - \epsilon_2
$$

3. Third rounded term

$$
\lfloor \frac{m \cdot l}{WAD} \rfloor = \frac{m \cdot l}{WAD}  - \epsilon_3
$$

$$
=>
\frac{\lfloor \frac{m \cdot l}{WAD} \rfloor}{WAD} = \frac{\frac{m \cdot l}{WAD} - \epsilon_3}{WAD}
$$

$$
=>
\frac{\lfloor \frac{m \cdot l}{WAD} \rfloor}{WAD} = \frac{m \cdot l}{WAD^2} - \frac{\epsilon_3}{WAD}
$$

4. Now bringing it all back to the original equation

$$
withdrawableShares (rounded) =
\lfloor
\lfloor 
d \space\cdot\space 
\frac{\lfloor\frac{WAD\space\cdot \space WAD
}{m_{deposit} \space\cdot\space l_{deposit}}
\rfloor }{WAD}
\rfloor 
\space\cdot\space \frac{\lfloor \frac{m \space\cdot\space l}{WAD}\rfloor}{WAD}
\rfloor
$$

$$
= \lfloor\left(d \cdot \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit} \cdot WAD} - d \cdot \frac{\epsilon_1}{WAD} - \epsilon_2\right)\cdot\left(\frac{m \cdot l}{WAD^2} - \frac{\epsilon_3}{WAD}\right)\rfloor
$$

$$
= \left(
d \cdot \frac{WAD \cdot WAD}{m_{deposit} \cdot l_{deposit} \cdot WAD} - d \cdot \frac{\epsilon_1}{WAD} - \epsilon_2
\right)
\cdot
\left(
\frac{m \cdot l}{WAD^2} - \frac{\epsilon_3}{WAD}
\right) - \epsilon_4
$$

After expansion and some simplification

$$
withdrawableShares (rounded) =
d \cdot \frac{m\cdot l}{m_{deposit} \cdot l_{deposit}\cdot WAD} - d \cdot \frac{\epsilon_1 \cdot m \cdot l}{WAD^3} - \frac{\epsilon_2 \cdot m \cdot l}{WAD^2} - d \cdot \frac{\epsilon_3}{m_{deposit} \cdot l_{deposit} } + \text{(higher-order terms)}
$$

Note that (higher-order terms) are the terms with multiple epsilon terms where the amounts become negligible, because each term $e$ is < 1.

The true value term is the following:

$$
withdrawableShares = d\space\cdot\space \frac{\frac{WAD \space\cdot\space WAD}{m_{deposit} \cdot l_{deposit}}}{WAD} \space\cdot\space \frac{\frac{m \space\cdot\space l}{WAD}}{WAD}
$$

$$
= d\space\cdot\space \frac{WAD }{m_{deposit} \cdot l_{deposit}}\space\cdot\space \frac{m \space\cdot\space l}{WAD^2}
$$

$$
d \cdot \frac{m\cdot l}{m_{deposit } \cdot l_{deposit}\cdot WAD}
$$

But we can see this term show in the withdrawableShares(rounded) above in the first term! Then we can see that we can represent the equations as the following. 

$$
withdrawableShares (rounded) =
withdrawableShares - d \cdot \frac{\epsilon_1 \cdot m \cdot l}{WAD^3} - \frac{\epsilon_2 \cdot m \cdot l}{WAD^2} - d \cdot \frac{\epsilon_3 }{m_{deposit} \cdot l_{deposit} } + \text{(higher-order terms)}
$$

This intuitively makes sense as all the rounding error comes from the epsilon terms and how they propagate out from being nested. Therefore the introduced error from rounding are all the rounding terms added up ignoring the higher-order terms.

$$
roundedError =d \cdot \frac{\epsilon_1 \cdot m \cdot l}{WAD^3} + \frac{\epsilon_2 \cdot m \cdot l}{WAD^2} + d \cdot \frac{\epsilon_3 }{m_{\text{deposit}} \cdot l_{deposit} }
$$

Now lets assume the worst case scenario of maximizing this sum above, if each epsilon $e$ is replaced with the value of 1 due to a full wei being rounded off we can get the following.

$$
d \cdot \frac{m \cdot l}{WAD^3} + \frac{ m \cdot l}{WAD^2} + \frac{ d}{m_{\text{deposit}} \cdot l_{deposit}}
$$

Assuming close to max values that results in rounding behaviour, we can maximize this total sum by having $d = 1e38$ ,  $m, m_{deposit}, l, l_{deposit}$ equal to WAD(1e18) then we get the following:

$$
\frac{1e38\cdot WAD^2}{WAD^3} + \frac{ WAD^2}{WAD^2} + \frac{1e38}{1e36}
$$

$$
=> \frac{1e38}{1e18} + 1 + 100
$$

$$
\approx 1e20
$$

Framed in another way, the amount of loss a staker can have is $\frac{1}{1e18}$ th of the deposit amount. This makes sense as a result of having nested flooring operations that are then multiplied against outer terms.
Over time, as stakers deposit and withdraw, they may not receive as many shares as their “real” withdrawable amount as this is rounded down and there could be residual/dust shares amount in the delegated operatorShares mapping AND in the original Strategy contract.
This is known and we specifically round down to avoid underflow of operatorShares if all their delegated stakers were to withdraw.
````

## File: docs/core/accounting/StrategyBaseAccounting.md
````markdown
# StrategyBase Accounting

- [StrategyBase Accounting](#strategybase-accounting)
  - [Overview](#overview)
  - [Why are shares needed?](#why-are-shares-needed)
  - [Can I mess up the accounting? Say, with a direct transfer?](#can-i-mess-up-the-accounting-say-with-a-direct-transfer)
    - [Arbitrary direct transfers](#arbitrary-direct-transfers)
      - [Simple Scenario](#simple-scenario)
      - [Scenario with More Depositors](#scenario-with-more-depositors)
    - [Inflation attacks](#inflation-attacks)
      - [Exploit Scenario](#exploit-scenario)
      - [Mitigation: Virtual Shares](#mitigation-virtual-shares)
      - [Attack Variation: Diluting the Virtual Depositor](#attack-variation-diluting-the-virtual-depositor)
      - [Attack Variation: Flash Loans](#attack-variation-flash-loans)
      - [Mitigation Side Effects](#mitigation-side-effects)
  - [Conclusion](#conclusion)

## Overview

The `StrategyBase` contract is used to manage the accounting of deposit shares for a specific token by collecting tokens and producing shares. Shares represent a proportional claim to the `StrategyBase`'s token balance, ensuring that users can withdraw their intended amount of tokens.

This document serves *specifically* to describe the accounting behavior for the `StrategyBase` contract. General documentation on the `StrategyBase` contract can be found [here](../StrategyManager.md#strategybase). 

## Why are shares needed?

At first glance, one may wonder why we need shares to be minted and burned when a staker deposits and withdraws tokens. Why not just track the token balance directly?

The primary reason is **rebase tokens**. Rebase tokens are tokens whose supply can change. An example of a rebase token's behavior is as follows:

* A user holds 1000 tokens  
* The token undergoes a 2x rebase (doubling what `balanceOf` returns)
* The token balance of the user is now 2000 tokens

If we were to track the token balance directly, then this 2x increase would not be reflected by the user's withdrawable amount. 

Consider the following scenario, where a user deposits a rebase token into EigenLayer:

* A user deposits 1000 tokens, and this value is recorded
* The token undergoes a 2x rebase
* The user's original 1000 tokens are now worth 2000 tokens
* The user *can only withdraw 1000 tokens*, as the recorded deposit is 1000 tokens

**This is where shares come in.** Shares represent a proportional claim to the `StrategyBase`'s token balance, ensuring that users can withdraw their intended amount of tokens. 

When a user deposits tokens, they receive shares proportional to the amount of tokens they deposited. Similarly, when a user withdraws tokens, they receive tokens proportional to the amount of shares they burned. Even though the underlying token balance may change, the number of shares a user holds will always represent their proportion of the underlying `StrategyBase` token balance.

With shares, the above scenario would play out as follows:

* A user deposits 1000 tokens, and they receive 1000 shares. Assume this is the first deposit for this strategy.
* The token undergoes a 2x rebase
* The user's original 1000 tokens are now worth 2000 tokens
* The user's 1000 shares are *also* now worth 2000 tokens
* The user can withdraw 2000 tokens, as expected

In short, shares allow for tracking a user's proportional claim to the `StrategyBase`'s token balance, ensuring that users can withdraw their intended amount of tokens even in the presence of rebase or other token behavior.

## Can I mess up the accounting? Say, with a direct transfer?

*TL;DR: No, you cannot arbitrarily mess up the accounting with a direct transfer -- at least, not without leveraging or destroying much capital to do so with minimal benefit.*

### Arbitrary direct transfers

As mentioned before, shares represent a proportional claim to the `StrategyBase`'s token balance. When a user deposits tokens, they receive shares proportional to the amount of tokens they deposited. Similarly, when a user withdraws tokens, they receive tokens proportional to the amount of shares they burned.

However, one can impact the accounting by sending tokens directly to the `StrategyBase` contract. For example:

* A user deposits 1000 tokens, receiving 1000 shares. Assume this is the first deposit for this strategy.
* The user sends 1000 tokens directly to the `StrategyBase` contract
* The user still has 1000 shares, but the `StrategyBase` now has 2000 tokens
* The user can now withdraw 2000 tokens, as their shares are 100% of the total number of shares, so they can withdraw 100% of the `StrategyBase`'s token balance

In this case, even though the user circumvented the expected flow, they still receive their intended amount of tokens.

This is due to the concept of an ***exchange rate***, which is the ratio of total shares to total tokens. This exchange rate by default is 1:1, but implicitly changes as the number of shares deviates from the number of tokens. In the above scenario, the exchange rate is now 2 tokens : 1 share.

However, can this behavior be used by an attacker to profit, or otherwise impact the accounting? Let's walk through a scenario.

#### Simple Scenario

Consider Alice and Bob, who are both aspirational stakers for EigenLayer. Alice has 1000 tokens, and Bob has 1000 tokens.

Alice deposits 500 tokens into the `StrategyBase` contract for this token, the first staker to do so. She receives 500 shares in return, a 1:1 ratio. Presume for demonstration purposes that this is not a rebase token, and that no other users are interacting with this `StrategyBase` contract.

The current state:
* `StrategyBase` total shares: **500**
* `StrategyBase` token balance: **500**
* Alice's shares: **500**
* Alice's "deserved" token balance: **500**
* Bob's shares: 0
* Bob's "deserved" token balance: 0

Bob is about to deposit 500 tokens into the `StrategyBase` contract, but Alice notices this. To increase her proportion of the shares, she sends 500 tokens *directly* to the `StrategyBase` contract. She does not go through the `deposit` function; she does not receive any additional shares for depositing these tokens into the contract.

The current state:
* `StrategyBase` total shares: 500
* `StrategyBase` token balance: **1000**
* Alice's shares: 500
* Alice's "deserved" token balance: **1000**
* Bob's shares: 0
* Bob's "deserved" token balance: 0

As we can see, Alice deposited a net total of 1000 tokens, and she is entitled to those 1000 tokens. Even though she only has 500 shares, she still has 100% of the total shares, and therefore is entitled to 100% of the `StrategyBase`'s token balance.

Bob continues with his day, depositing 500 tokens into the `StrategyBase` contract. However, he does not receive 500 shares, but instead receives 250 shares. This is fewer shares than he expected -- is this an issue?

The current state:
* `StrategyBase` total shares: **750**
* `StrategyBase` token balance: **1500**
* Alice's shares: 500
* Alice's "deserved" token balance: 1000
* Bob's shares: **250**
* Bob's "deserved" token balance: **500**

Let's investigate the accounting -- specifically, the ratio of shares to tokens.

As we can see, the `StrategyBase` contract for this token has 750 shares, and 1500 tokens. The ratio of tokens to shares is 2:1. Here you'll notice the concept of an ***exchange rate***, which is the ratio of total shares to total tokens. This exchange rate by default is 1:1, but implicitly changes as the number of shares deviates from the number of tokens.

Does this exchange rate correctly reflect what Alice and Bob are entitled to?

* Alice's 500 shares * 2 tokens / 1 share = 1000 tokens. **Correct!**
* Bob's 250 shares * 2 tokens / 1 share = 500 tokens. **Correct!**

Both Alice and Bob are still able to withdraw their "deserved" token amounts, as their shares are still correctly proportional in relation to each other. As such, despite the direct transfer of tokens to the `StrategyBase` contract, the accounting is still correct.

#### Scenario with More Depositors

Consider the same scenario, but with more depositors. Alice and Bob each deposit 500 tokens, receiving 500 shares each.

The current state:
* `StrategyBase` total shares: **1000**
* `StrategyBase` token balance: **1000**
* Alice's shares: **500**
* Alice's "deserved" token balance: **500**
* Bob's shares: **500**
* Bob's "deserved" token balance: **500**

A third entity, Charlie, prepares to deposit 500 tokens into the `StrategyBase` contract. However, Alice notices this. To increase her proportion of the shares, she sends 500 tokens *directly* to the `StrategyBase` contract. She does not go through the `deposit` function; she does not receive any additional shares for depositing these tokens into the contract.

The current state:
* `StrategyBase` total shares: **1500**
* `StrategyBase` token balance: **2000**
* Alice's shares: 500
* Alice's "deserved" token balance: **1000**
* Bob's shares: 500
* Bob's "deserved" token balance: 500
* Charlie's shares: 0
* Charlie's "deserved" token balance: 0

As we can see, the exchange rate is now 4 tokens : 3 shares. 

As such:
* Alice's 500 shares * 4 tokens / 3 shares = 666 tokens. ***Lower* than the expected 1000 tokens.**
* Bob's 500 shares * 4 tokens / 3 shares = 666 tokens. ***Higher* than the expected 500 tokens. (Good for Bob!)**

From the onset, Bob benefits from Alice's direct deposit. He is still entitled to the same proportion of tokens, but the total tokens have increased. Alice, on the other hand, is worse off, as she is entitled to fewer tokens than she overall deposited or transferred.

As Charlie deposits 500 tokens, he receives 375 shares due to the exchange rate, as 500 tokens * 3 shares / 4 tokens = 375 shares.

The current state:
* `StrategyBase` total shares: **1875**
* `StrategyBase` token balance: **2500**
* Alice's shares: 500
* Alice's "deserved" token balance: 1000
* Bob's shares: 500
* Bob's "deserved" token balance: 500
* Charlie's shares: **375**
* Charlie's "deserved" token balance: **500**

As we can see, the exchange rate is now 4 tokens : 3 shares. 

We can see that Charlie's 375 shares are correctly worth 500 tokens, as 375 shares * 4 tokens / 3 shares = 500 tokens. *Note that, since the exchange rate hasn't changed, we do not need to recalculate Alice and Bob's eligible tokens.*

Therefore, even though the exchange rate was not 1:1 prior to Charlie's deposit, the accounting is still correct, and he receives his intended amount of tokens. The only victim here is Alice, who effectively donated tokens to the existing depositors of the `StrategyBase` contract.

### Inflation attacks

An inflation attack is a more *specific* scenario that may impact depositors during the first deposits into a `StrategyBase` contract. This kind of attack is possible when the `StrategyBase` contract is first created, before or after the very first deposit. At this stage, the exchange rate of the `StrategyBase` is highly susceptible to manipulation, giving the first depositor the ability to steal funds from later depositors.

#### Exploit Scenario

Say Alice deposits 1 token into a `StrategyBase` contract, the first depositor to do so, and receives 1 share. This is an intentionally minimal amount so that she can perform an inflation attack.

The current state:
* `StrategyBase` total shares: **1**
* `StrategyBase` token balance: **1**
* Alice's shares: **1**
* Alice's "deserved" token balance: **1**

She notices that Bob is about to deposit 1000 tokens into the `StrategyBase` contract. She wants to manipulate the exchange rate to be so high that, due to rounding, Bob receives no shares for his deposit. In other words, she wants to set the exchange rate to some value where the number of tokens that Bob will be depositing is less than the number of tokens that would be required to receive 1 share. This would leave Bob with no shares, and no way to withdraw his tokens.

For example, say that Alice sends a *million* tokens to the `StrategyBase` contract. She does not go through the `deposit` function; she does not receive any additional shares for depositing these tokens into the contract. The exchange rate is now 1e6 + 1 tokens : 1 share.

The current state:
* `StrategyBase` total shares: 1
* `StrategyBase` token balance: **1e6 + 1**
* Alice's shares: **1**
* Alice's "deserved" token balance: **1e6 + 1**

Note the large difference between the `StrategyBase`'s token balance and the number of shares. As mentioned before, the exchange rate is now 1e6 + 1 tokens : 1 share.

When Bob deposits 1000 tokens, he is depositing less than 1e6 tokens, meaning that *he receives no shares for his deposit*. Calculating Bob's total shares:

* Bob's 1000 tokens * 1 share / 1e6 + 1 tokens = 1e-3 shares = 0 shares

Due to the large divisor, Bob's received shares are now 1e-3, a very small number, which is *rounded down to 0* due to EVM division. Thus, **Bob receives no shares for his token deposit**. This is a problem, as he is entitled to 1000 tokens, but has no shares for withdrawing it.

The current state:
* `StrategyBase` total shares: 1
* `StrategyBase` token balance: **1e6 + 1001**
* Alice's shares: 1
* Alice's "deserved" token balance: 1e6 + 1
* Bob's shares: 0
* Bob's "deserved" token balance: **1000**

As we can see, Bob's token balance increased, but his shares remained at 0, losing his deposited tokens to Alice. Alice has all the shares of the `StrategyBase` contract, and can withdraw all of the tokens, including Bob's 1000 tokens, even though she does not "deserve" those tokens as she is not the rightful owner.

#### Mitigation: Virtual Shares

To mitigate this, **we use a "virtual shares" mechanism**. Every created `StrategyBase` contract is initialized with a certain number of virtual shares (1e3) and virtual tokens (1e3), which simulate the "first deposit" into this `StrategyBase` contract. This prevents a first depositor from manipulating the exchange rate to their benefit, as they lose the advantages typically associated with the first depositor.

Consider Alice trying to perform the same attack as before, but with the virtual shares mechanism. The virtual shares are now 1e3, and the virtual tokens are 1e3. The total shares and token balance reflect this.

The current state:
* `StrategyBase` total shares: **1,000**
* `StrategyBase` token balance: **1,000**
* Alice's shares: 0
* Alice's "deserved" token balance: 0

Alice deposits her 1 token into the `StrategyBase` contract. She receives 1 share, as expected.

The current state:
* `StrategyBase` total shares: **1,001**
* `StrategyBase` token balance: **1,001**
* Alice's shares: **1**
* Alice's "deserved" token balance: **1**

Note immediately that Alice has 1 share, which is less than 0.1% of the total shares. She no longer has 100% of the shares due to this "virtual depositor".

Bob again intends to deposit 100 tokens into the `StrategyBase` contract. However, Alice beats him to it, depositing 1 million tokens into the `StrategyBase` contract. She does not receive any additional shares for depositing these tokens into the contract.

The current state:
* `StrategyBase` total shares: **1,001**
* `StrategyBase` token balance: **1,001,001**
* Alice's shares: **1**
* Alice's "deserved" token balance: **1,000,001**

Remember how Alice only has 1 share? Notice how she cannot withdraw the million tokens she deposited! 

* Alice's 1 share * 1,000,001 tokens / 1,001 shares = 999 tokens. ***Lower* than the expected 1,000,001 tokens.**

Given the virtual depositor, Alice is "donating" her tokens to the `StrategyBase` contract, and is not able to withdraw the majority of the tokens she deposited.

Bob now deposits his 1000 tokens, at an exchange rate of ~1000 tokens : 1 share. Given that the exchange rate is now 1000 tokens : 1 share, Bob receives 1 share for his deposit, as he has deposited enough tokens to not have his shares rounded down to 0.

The current state:
* `StrategyBase` total shares: **1,002**
* `StrategyBase` token balance: **1,002,001**
* Alice's shares: 1
* Alice's "deserved" token balance: 1,000,001
* Bob's shares: **1**
* Bob's "deserved" token balance: **1,000**

Hilariously enough, not only is Bob able to withdraw his 1000 tokens, but Alice is also *only* able to withdraw 1000 tokens! Alice's attack is rendered unsuccessful.

#### Attack Variation: Diluting the Virtual Depositor

What if Alice attempts to dilute the initial 1e3 virtual shares and tokens? Clearly, since she didn't have enough shares with her minimal initial deposit, her attack's capital was dilulted by the "virtual depositor".

Imagine that Alice instead deposits 1 million tokens upfront. She receives 1 million shares, as expected.

The current state:
* `StrategyBase` total shares: **1,001,000**
* `StrategyBase` token balance: **1,001,000**
* Alice's shares: **1,000,000**
* Alice's "deserved" token balance: **1,000,000**

As you can see here, Alice has a vast majority of the shares. She tries again to manipulate the exchange rate before Bob deposits by depositing yet *another* million tokens. Remember that she does not receive any additional shares for depositing these tokens into the contract.

The current state:
* `StrategyBase` total shares: 1,001,000
* `StrategyBase` token balance: **2,001,000**
* Alice's shares: 1,000,000
* Alice's "deserved" token balance: **2,000,000**

The exchange rate is now ~2 tokens : 1 share. More accurately, it is 2,001,000 tokens : 1,001,000 shares, or ~1.999 tokens : 1 share.

So let's ask the question: how many tokens can Alice withdraw given her 1 million shares?

* Alice's 1 million shares * 2,001,000 tokens / 1,001,000 shares = 1,999,000 tokens. **Lower* than the expected 2,000,000 tokens.**

As we can see, Alice is not able to withdraw her intended amount of tokens, even though she has a vast majority of the shares. She actually *loses* 1000 tokens due to the virtual depositor.

Let's see what happens when Bob deposits his 1000 tokens. Given the exchange rate of ~2 tokens : 1 share, we expect him to receive 500 shares for his deposit. We calculate this as follows:

* Bob's 1000 tokens * 1,001,000 shares / 2,001,000 tokens = 500.25 shares = 500 shares (due to rounding)

The current state:
* `StrategyBase` total shares: **1,001,500**
* `StrategyBase` token balance: **2,002,000**
* Alice's shares: 1,000,000
* Alice's "deserved" token balance: 2,000,000
* Bob's shares: **500**
* Bob's "deserved" token balance: **1,000**

As we can see, Bob receives 500 shares for his deposit, as expected. If he attempts to withdraw:

* Bob's 500 shares * 2,002,000 tokens / 1,001,500 shares = 999.5 tokens = 999 tokens (due to rounding). ***Correct!**

Alice, the attempted attacker, is the one who loses in this scenario. Bob loses one token, but Alice loses 1000 tokens, and locks up significant capital for her troubles. An attacker **is not economically incentivized** to perform an inflation attack, as they will lose out in the end.

As such, the virtual depositor is a useful mechanism to protect against inflation attacks, even when the attacker has a vast majority of the shares.

#### Attack Variation: Flash Loans

You may be wondering, what if Alice performs a flash loan attack? These provide large amounts of capital on demand, and perhaps enough capital can make an inflation attack profitable.

First, similar to how Alice lost capital to the virtual depositor in the previous scenario, she will lose capital when performing the flash loan attack. This alone prevents her attack from being profitable, even in the best case scenario.

Say that Alice is, for lack of a better term, "insane" and chooses to disobey economic incentives. Note that typical flash loans only provide capital [within a given transaction](https://aave.com/docs/developers/flash-loans#:~:text=the%20borrowed%20amount%20(and%20a%20fee)%20is%20returned%20before%20the%20end%20of%20the%20transaction), and are not able to be borrowed over any larger unit of time. As such, flash loans are not a viable route for performing this attack in the first place.

#### Mitigation Side Effects

The virtual depositor has a few side effects that are important to note. 

* Rebase dilution: In the event of a token rebase, user token balances will typically increase by the rebase factor. However, the virtual depositor's token balance will not increase by the same factor, as it is a fixed amount. This means that user gains will be mildly diluted over time. 
  * However, as the virtual depositor only has 1e3 shares and tokens, this effect is negligible (estimated to be 1 part in 1e20).
* Negative rebase: In the event of a "negative rebase," where the token balance decreases, not all users may be able to withdraw. The `StrategyBase` contract will have more shares than assets due to this loss of principal. As a result, the last depositor(s) will not be able to withdraw. This is because the virtual depositor's shares and tokens are fixed, and are not subject to the loss of principal.
  * However, this is a rare occurrence, and is not expected to happen in the near future. Moreover, this is easily mitigated by a one-off "donation" of tokens to the `StrategyBase` contract, up to 1000 tokens. Given this minimal impact, we do not consider this a significant issue.

## Conclusion

Shares are a useful mechanism to manage the accounting of a `StrategyBase` contract. They allow for tracking a user's proportional claim to the `StrategyBase`'s token balance, ensuring that users can withdraw their intended amount of tokens even in the presence of rebase or other token behavior.

Typically, this model is vulnerable to an "inflation attack," but the virtual depositor mitigation protects against this. It is a simple and effective mechanism to prevent a first depositor from manipulating the exchange rate to their benefit, as they lose the advantages typically associated with the first depositor. 

Any attacker attempting to perform an inflation attack will lose out in the end. Even if they seek to grief other users, the amount of capital required to perform the attack in the first place is extremely high. Though there are small side effects to the virtual depositor, they are negligible and do not impact the core functionality of the `StrategyBase` contract.
````

## File: docs/core/AllocationManager.md
````markdown
# AllocationManager

| File | Notes |
| -------- | -------- |
| [`AllocationManager.sol`](../../src/contracts/core/AllocationManager.sol) |  |
| [`AllocationManagerStorage.sol`](../../src/contracts/core/AllocationManagerStorage.sol) | state variables |
| [`IAllocationManager.sol`](../../src/contracts/interfaces/IAllocationManager.sol) | interface |

Libraries and Mixins:

| File | Notes |
| -------- | -------- |
| [`PermissionControllerMixin.sol`](../../src/contracts/mixins/PermissionControllerMixin.sol) | account delegation |
| [`Pausable.sol`](../../src/contracts/permissions/Pausable.sol) | |
| [`SlashingLib.sol`](../../src/contracts/libraries/SlashingLib.sol) | slashing math |
| [`OperatorSetLib.sol`](../../src/contracts/libraries/OperatorSetLib.sol) | encode/decode operator sets |
| [`Snapshots.sol`](../../src/contracts/libraries/Snapshots.sol) | historical state |

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)

## Overview

The `AllocationManager` manages AVS metadata registration, registration and deregistration of operators to operator sets, handles allocation and slashing of operators' slashable stake, and is the entry point an AVS uses to slash an operator. The `AllocationManager's` responsibilities are broken down into the following concepts:

* [AVS Metadata](#avs-metadata)
* [Operator Sets](#operator-sets)
* [Allocations and Slashing](#allocations-and-slashing)
* [Config](#config)

## Parameterization

* `ALLOCATION_CONFIGURATION_DELAY`: The delay in blocks before allocations take effect.
    * Mainnet: `126000 blocks` (17.5 days).
    * Testnet: `75 blocks` (15 minutes).
* `DEALLOCATION_DELAY`: The delay in blocks before deallocations take effect.
    * Mainnet: `100800 blocks` (14 days).
    * Testnet: `50 blocks` (10 minutes).

---

## AVS Metadata

AVSs must register their metadata to declare themselves who they are as the first step, before they can create operator sets or register operators into operator sets. `AllocationManager` keeps track of AVSs that have registered metadata.

**Methods:**
* [`updateAVSMetadataURI`](#updateavsmetadatauri)


#### `updateAVSMetadataURI`

```solidity
/**
 *  @notice Called by an AVS to emit an `AVSMetadataURIUpdated` event indicating the information has updated.
 *
 *  @param metadataURI The URI for metadata associated with an AVS.
 *
 *  @dev Note that the `metadataURI` is *never stored* and is only emitted in the `AVSMetadataURIUpdated` event.
 */
function updateAVSMetadataURI(
    address avs, 
    string calldata metadataURI
) 
    external
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

Below is the format AVSs should use when updating their metadata URI initially. This is not validated onchain.

```json
{
    "name": "AVS",
    "website": "https.avs.xyz/",
    "description": "Some description about",
    "logo": "http://github.com/logo.png",
    "twitter": "https://twitter.com/avs",
}
```


Later on, once AVSs have created operator sets, content in their metadata URI can be updated subsequently.

```json
{
    "name": "AVS",
    "website": "https.avs.xyz/",
    "description": "Some description about",
    "logo": "http://github.com/logo.png",
    "twitter": "https://twitter.com/avs",
    "operatorSets": [
        {
            "name": "ETH Set",
            "id": "1", // Note: we use this param to match the opSet id in the Allocation Manager
            "description": "The ETH operatorSet for AVS",
            "software": [
                {
                    "name": "NetworkMonitor",
                    "description": "",
                    "url": "https://link-to-binary-or-github.com"
                },
                {
                    "name": "ValidatorClient",
                    "description": "",
                    "url": "https://link-to-binary-or-github.com"
                }
            ],
            "slashingConditions": ["Condition A", "Condition B"]
        },
        {
            "name": "EIGEN Set",
            "id": "2", // Note: we use this param to match the opSet id in the Allocation Manager
            "description": "The EIGEN operatorSet for AVS",
            "software": [
                {
                    "name": "NetworkMonitor",
                    "description": "",
                    "url": "https://link-to-binary-or-github.com"
                },
                {
                    "name": "ValidatorClient",
                    "description": "",
                    "url": "https://link-to-binary-or-github.com"
                }
            ],
            "slashingConditions": ["Condition A", "Condition B"]
        }
    ]
}
```

*Effects*:
* Emits an `AVSMetadataURIUpdated` event for use in offchain services

*Requirements*:
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))


## Operator Sets

Operator sets, as described in [ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#operator-sets), are useful for AVSs to configure operator groupings which can be assigned different tasks, rewarded based on their strategy allocations, and slashed according to different rules. Operator sets are defined in [`libraries/OperatorSetLib.sol`](../../src/contracts/libraries/OperatorSetLib.sol):

```solidity
/**
 * @notice An operator set identified by the AVS address and an identifier
 * @param avs The address of the AVS this operator set belongs to
 * @param id The unique identifier for the operator set
 */
struct OperatorSet {
    address avs;
    uint32 id;
}
```

The `AllocationManager` tracks operator sets and members of operator sets in the following mappings:

```solidity
/// @dev Lists the operator set ids an AVS has created
mapping(address avs => EnumerableSet.UintSet) internal _operatorSets;

/// @dev Lists the members of an AVS's operator set
mapping(bytes32 operatorSetKey => EnumerableSet.AddressSet) internal _operatorSetMembers;
```

Every `OperatorSet` corresponds to a single AVS, as indicated by the `avs` parameter. On creation, the AVS provides an `id` (unique to that AVS), as well as a list of `strategies` the `OperatorSet` includes. Together, the `avs` and `id` form the `key` that uniquely identifies a given `OperatorSet`. Operators can register to and deregister from operator sets. In combination with allocating slashable magnitude, operator set registration forms the basis of operator slashability (discussed further in [Allocations and Slashing](#allocations-and-slashing)).

**Concepts:**
* [Registration Status](#registration-status)

**Methods:**
* [`createOperatorSets`](#createoperatorsets)
* [`addStrategiesToOperatorSet`](#addstrategiestooperatorset)
* [`removeStrategiesFromOperatorSet`](#removestrategiesfromoperatorset)
* [`registerForOperatorSets`](#registerforoperatorsets)
* [`deregisterFromOperatorSets`](#deregisterfromoperatorsets)

#### Registration Status

Operator registration and deregistration is tracked in the following state variables:

```solidity
/// @dev Lists the operator sets the operator is registered for. Note that an operator
/// can be registered without allocated stake. Likewise, an operator can allocate
/// without being registered.
mapping(address operator => EnumerableSet.Bytes32Set) internal registeredSets;

/**
 * @notice Contains registration details for an operator pertaining to an operator set
 * @param registered Whether the operator is currently registered for the operator set
 * @param slashableUntil If the operator is not registered, they are still slashable until
 * this block is reached.
 */
struct RegistrationStatus {
    bool registered;
    uint32 slashableUntil;
}

/// @dev Contains the operator's registration status for an operator set.
mapping(address operator => mapping(bytes32 operatorSetKey => RegistrationStatus)) internal registrationStatus;
```

For each operator, `registeredSets` keeps a list of `OperatorSet` `keys` for which the operator is currently registered. Each operator registration and deregistration respectively adds and removes the relevant `key` for a given operator. An additional factor in registration is the operator's `RegistrationStatus`.

The `RegistrationStatus.slashableUntil` value is used to ensure an operator remains slashable for a period of time after they initiate deregistration. This is to prevent an operator from committing a slashable offence and immediately deregistering to avoid penalty. This means that when an operator deregisters from an operator set, their `RegistrationStatus.slashableUntil` value is set to `block.number + DEALLOCATION_DELAY`.

#### `createOperatorSets`

```solidity
/**
 * @notice Parameters used by an AVS to create new operator sets
 * @param operatorSetId the id of the operator set to create
 * @param strategies the strategies to add as slashable to the operator set
 */
struct CreateSetParams {
    uint32 operatorSetId;
    IStrategy[] strategies;
}

/**
 * @notice Allows an AVS to create new operator sets, defining strategies that the operator set uses
 */
function createOperatorSets(
    address avs,
    CreateSetParams[] calldata params
)
    external
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

AVSs use this method to create new operator sets. An AVS can create as many operator sets as they desire, depending on their needs. Once created, operators can [allocate slashable stake to](#modifyallocations) and [register for](#registerforoperatorsets) these operator sets.

On creation, the `avs` specifies an `operatorSetId` unique to the AVS. Together, the `avs` address and `operatorSetId` create a `key` that uniquely identifies this operator set throughout the `AllocationManager`.

Optionally, the `avs` can provide a list of `strategies`, specifying which strategies will be slashable for the new operator set. AVSs may create operator sets with various strategies based on their needs - and strategies may be added to more than one operator set.

*Effects*:
* For each `CreateSetParams` element:
    * For each `params.strategies` element:
        * Add `strategy` to `_operatorSetStrategies[operatorSetKey]`
        * Emits `StrategyAddedToOperatorSet` event

*Requirements*:
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* AVS MUST have registered metadata via calling `updateAVSMetadataURI`
* For each `CreateSetParams` element:
    * Each `params.operatorSetId` MUST NOT already exist in `_operatorSets[avs]`
    
#### `addStrategiesToOperatorSet`

```solidity
/**
 * @notice Allows an AVS to add strategies to an operator set
 * @dev Strategies MUST NOT already exist in the operator set
 * @param avs the avs to set strategies for
 * @param operatorSetId the operator set to add strategies to
 * @param strategies the strategies to add
 */
function addStrategiesToOperatorSet(
    address avs,
    uint32 operatorSetId,
    IStrategy[] calldata strategies
)
    external
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

This function allows an AVS to add slashable strategies to a given operator set. If any strategy is already registered for the given operator set, the entire call will fail.

*Effects*:
* For each `strategies` element:
    * Adds the strategy to `_operatorSetStrategies[operatorSetKey]`
    * Emits a `StrategyAddedToOperatorSet` event

*Requirements*:
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* The operator set MUST be registered for the AVS
* Each proposed strategy MUST NOT be registered for the operator set

#### `removeStrategiesFromOperatorSet`

```solidity
/**
 * @notice Allows an AVS to remove strategies from an operator set
 * @dev Strategies MUST already exist in the operator set
 * @param avs the avs to remove strategies for
 * @param operatorSetId the operator set to remove strategies from
 * @param strategies the strategies to remove
 */
function removeStrategiesFromOperatorSet(
    address avs,
    uint32 operatorSetId,
    IStrategy[] calldata strategies
)
    external
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

This function allows an AVS to remove slashable strategies from a given operator set. If any strategy is not registered for the given operator set, the entire call will fail.

*Effects*:
* For each `strategies` element:
    * Removes the strategy from `_operatorSetStrategies[operatorSetKey]`
    * Emits a `StrategyRemovedFromOperatorSet` event

*Requirements*:
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* The operator set MUST be registered for the AVS
* Each proposed strategy MUST be registered for the operator set

#### `registerForOperatorSets`

```solidity
/**
 * @notice Parameters used to register for an AVS's operator sets
 * @param avs the AVS being registered for
 * @param operatorSetIds the operator sets within the AVS to register for
 * @param data extra data to be passed to the AVS to complete registration
 */
struct RegisterParams {
    address avs;
    uint32[] operatorSetIds;
    bytes data;
}

/**
 * @notice Allows an operator to register for one or more operator sets for an AVS. If the operator
 * has any stake allocated to these operator sets, it immediately becomes slashable.
 * @dev After registering within the ALM, this method calls the AVS Registrar's `IAVSRegistrar.
 * registerOperator` method to complete registration. This call MUST succeed in order for 
 * registration to be successful.
 */
function registerForOperatorSets(
    address operator,
    RegisterParams calldata params
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_SET_REGISTRATION_AND_DEREGISTRATION)
    checkCanCall(operator)
```

_Note: this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

An operator may call this function to register for any number of operator sets of a given AVS at once. There are two very important details to know about this method:
1. As part of registration, each operator set is added to the operator's `registeredSets`. Note that for each newly-registered set, **any stake allocations to the operator set become immediately slashable**.
2. Once all sets have been added, the AVS's configured `IAVSRegistrar` is called to confirm and complete registration. _This call MUST NOT revert,_ as **AVSs are expected to use this call to reject ineligible operators** (according to their own custom logic). Note that if the AVS has not configured a registrar, the `avs` itself is called.

This method makes an external call to the `IAVSRegistrar.registerOperator` method, passing in the registering `operator`, the `operatorSetIds` being registered for, and the input `params.data` provided during registration. From [`IAVSRegistrar.sol`](../../src/contracts/interfaces/IAVSRegistrar.sol):

```solidity
/**
 * @notice Called by the AllocationManager when an operator wants to register
 * for one or more operator sets. This method should revert if registration
 * is unsuccessful.
 * @param operator the registering operator
 * @param operatorSetIds the list of operator set ids being registered for
 * @param data arbitrary data the operator can provide as part of registration
 */
function registerOperator(address operator, uint32[] calldata operatorSetIds, bytes calldata data) external;
```

*Effects*:
* Adds the proposed operator sets to the operator's list of registered sets (`registeredSets`)
* Adds the operator to `_operatorSetMembers` for each operator set
* Marks the operator as registered for the given operator sets (in `registrationStatus`)
* Passes the `params` for registration to the AVS's `AVSRegistrar`, which can arbitrarily handle the registration request
* Emits an `OperatorAddedToOperatorSet` event for each operator

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_OPERATOR_SET_REGISTRATION_AND_DEREGISTRATION`
* `operator` MUST be registered as an operator in the `DelegationManager`
* Caller MUST be authorized, either the operator themselves, or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Each `operatorSetId` MUST exist for the given AVS
* Operator MUST NOT already be registered for any proposed operator sets
* If operator has deregistered, operator MUST NOT be slashable anymore (i.e. the `DEALLOCATION_DELAY` must have passed)
* The call to the AVS's configured `IAVSRegistrar` MUST NOT revert

#### `deregisterFromOperatorSets`

```solidity
/**
 * @notice Parameters used to deregister from an AVS's operator sets
 * @param operator the operator being deregistered
 * @param avs the avs being deregistered from
 * @param operatorSetIds the operator sets within the AVS being deregistered from
 */
struct DeregisterParams {
    address operator;
    address avs;
    uint32[] operatorSetIds;
}

/**
 * @notice Allows an operator or AVS to deregister the operator from one or more of the AVS's operator sets.
 * If the operator has any slashable stake allocated to the AVS, it remains slashable until the
 * DEALLOCATION_DELAY has passed.
 * @dev After deregistering within the ALM, this method calls the AVS Registrar's `IAVSRegistrar.
 * deregisterOperator` method to complete deregistration. This call MUST succeed in order for
 * deregistration to be successful.
 */
function deregisterFromOperatorSets(
    DeregisterParams calldata params
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_SET_REGISTRATION_AND_DEREGISTRATION)
```

_Note: this method can be called directly by an operator/AVS, or by a caller authorized by the operator/AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

This method may be called by EITHER an operator OR an AVS to which an operator is registered; it is intended to allow deregistration to be triggered by EITHER party. This method generally inverts the effects of `registerForOperatorSets`, with two specific exceptions:
1. As part of deregistration, each operator set is removed from the operator's `registeredSets`. HOWEVER, **any stake allocations to that operator set will remain slashable for `DEALLOCATION_DELAY` blocks.** The operator will not be allowed to register for the operator set again until this slashable window has passed.
2. Once all sets have been removed, the AVS's configured `IAVSRegistrar` is called to complete deregistration on the AVS side.

This method makes an external call to the `IAVSRegistrar.deregisterOperator` method, passing in the deregistering `operator` and the `operatorSetIds` being deregistered from. From [`IAVSRegistrar.sol`](../../src/contracts/interfaces/IAVSRegistrar.sol):

```solidity
/**
 * @notice Called by the AllocationManager when an operator is deregistered from
 * one or more operator sets. If this method reverts, it is ignored.
 * @param operator the deregistering operator
 * @param operatorSetIds the list of operator set ids being deregistered from
 */
function deregisterOperator(address operator, uint32[] calldata operatorSetIds) external;
```

*Effects*:
* Removes the proposed operator sets from the operator's list of registered sets (`registeredSets`)
* Removes the operator from `_operatorSetMembers` for each operator set
* Updates the operator's `registrationStatus` with:
    * `registered: false`
    * `slashableUntil: block.number + DEALLOCATION_DELAY`
        * As mentioned above, this allows for AVSs to slash deregistered operators until `block.number == slashableUntil`
* Emits an `OperatorRemovedFromOperatorSet` event for each operator
* Passes the `operator` and `operatorSetIds` to the AVS's `AVSRegistrar`, which can arbitrarily handle the deregistration request

*Requirements*:
<!-- * Address MUST be registered as an operator -->
* Pause status MUST NOT be set: `PAUSED_OPERATOR_SET_REGISTRATION_AND_DEREGISTRATION`
* Caller MUST be authorized, either the operator/AVS themselves, or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Each operator set ID MUST exist for the given AVS
* Operator MUST be registered for the given operator sets
* Note that, unlike `registerForOperatorSets`, the AVS's `AVSRegistrar` MAY revert and the deregistration will still succeed

---

## Allocations and Slashing

[Operator set registration](#operator-sets) is one step of preparing to participate in an AVS. When an operator successfully registers for an operator set, it is because the AVS in question is ready to assign them tasks. However, it follows that _before assigning tasks_ to an operator, an AVS will expect operators to allocate slashable stake to the operator set such that the AVS has some economic security.

For this reason, it is expected that many AVSs will require operators to **allocate slashable stake BEFORE registering for an operator set**. This is due to [`registerForOperatorSets`](#registerforoperatorsets) serving in part as an AVS's "consent mechanism," as calling `IAVSRegsitrar.registerOperator` allows the AVS to query the amount of slashable stake the operator can provide when assigned tasks.

It is only once an operator is both _registered for an operator set_ and _has an active allocation to that operator set_ that the associated AVS can slash actual stake from an operator.

See [ELIP-002#Unique Stake Allocation & Deallocation](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#unique-stake-allocation--deallocation) for additional context.

**Concepts:**
* [Max vs Encumbered Magnitude](#max-vs-encumbered-magnitude)
* [Evaluating the "Current" Allocation](#evaluating-the-current-allocation)
* [Evaluating Whether an Allocation is "Slashable"](#evaluating-whether-an-allocation-is-slashable)

**Methods:**
* [`modifyAllocations`](#modifyallocations)
* [`clearDeallocationQueue`](#cleardeallocationqueue)
* [`slashOperator`](#slashoperator)

#### Max vs Encumbered Magnitude

Operators allocate _magnitude_, which represents a proportion of their total stake. For a given strategy, the `AllocationManager` tracks two quantities, _max magnitude_ and _encumbered magnitude_:

```solidity
/**
 * @notice Contains allocation info for a specific strategy
 * @param maxMagnitude the maximum magnitude that can be allocated between all operator sets
 * @param encumberedMagnitude the currently-allocated magnitude for the strategy
 */
struct StrategyInfo {
    uint64 maxMagnitude;
    uint64 encumberedMagnitude;
}

/// @dev Contains a history of the operator's maximum magnitude for a given strategy
mapping(address operator => mapping(IStrategy strategy => Snapshots.DefaultWadHistory)) internal _maxMagnitudeHistory;

/// @dev For a strategy, contains the amount of magnitude an operator has allocated to operator sets
/// @dev This value should be read with caution, as deallocations that are completable but not
///      popped off the queue are still included in the encumbered magnitude
mapping(address operator => mapping(IStrategy strategy => uint64)) public encumberedMagnitude;
```

An operator's max magnitude starts at `1 WAD` (`1e18`), and is decreased when they are slashed. Max magnitude represents "100%" of allocatable magnitude. When an operator allocates magnitude from a strategy to an operator set, their encumbered magnitude for that strategy increases. An operator cannot allocate > 100%; therefore, a strategy's encumbered magnitude can never exceed that strategy's max magnitude.

#### Evaluating the "Current" Allocation

As mentioned in the previous section, allocations and deallocations take place on a delay, and as such the `Allocation` struct has both a `currentMagnitude`, and `pendingDiff` / `effectBlock` fields:

```solidity
/**
 * @notice Defines allocation information from a strategy to an operator set, for an operator
 * @param currentMagnitude the current magnitude allocated from the strategy to the operator set
 * @param pendingDiff a pending change in magnitude, if it exists (0 otherwise)
 * @param effectBlock the block at which the pending magnitude diff will take effect
 */
struct Allocation {
    uint64 currentMagnitude;
    int128 pendingDiff;
    uint32 effectBlock;
}
```

Although the `allocations` mapping can be used to fetch an `Allocation` directly, you'll notice a convention in the `AllocationManager` of using the `_getUpdatedAllocation` helper, instead. This helper reads an existing `Allocation`, then evaluates `block.number` against `Allocation.effectBlock` to determine whether or not to apply the `pendingDiff`. 
* If the diff can be applied, the helper returns an `Allocation` with an updated `currentMagnitude` and zeroed out `pendingDiff` and `effectBlock` fields -- as if the modification has already been completed.
* Otherwise, the `Allocation` is returned from storage unmodified.

Generally, when an `Allocation` is mentioned in this doc (or used within the `AllocationManager.sol` contract), we are referring to the "Current" `Allocation` as defined above.

#### Evaluating Whether an Allocation is "Slashable"

Given an `operator` and an `Allocation` from a `strategy` to an AVS's `OperatorSet`, the `AllocationManager` uses the following criteria to determine whether the operator's allocation is slashable:
1. The `operator` must be registered for the operator set, or if they are deregistered, they must still be slashable (See [Registration Status](#registration-status)).
2. The AVS must have added the `strategy` to the operator set (See [`addStrategiesToOperatorSet`](#addstrategiestooperatorset) and [`removeStrategiesFromOperatorSet`](#removestrategiesfromoperatorset))
3. The existing `Allocation` must have a nonzero `Allocation.currentMagnitude`

If ALL of these are true, the `AllocationManager` will allow the AVS to slash the `operator's` `Allocation`.

#### Evaluating How Much of the Allocation is "Slashable"

The `getMinimumSlashableStake` calculates the minimum amount of stake that will be slashable at a specified future block. This computation accounts for each operator’s allocated stake from different strategies within an operator set. The function considers pending allocation changes that could reduce the slashable stake over time, ensuring a minimum guaranteed value. Because this is a forecast, the slashable stake at any given moment is a discrete value, but when looking ahead to a future block, the function provides the lowest possible amount, factoring in any planned allocation adjustments that will take effect within the specified timeframe.

Please see [IAllocationManager.sol:getMinimumSlashableStake](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/src/contracts/interfaces/IAllocationManager.sol#L577) for more detail.


#### `modifyAllocations`

```solidity
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
 * @notice Modifies the proportions of slashable stake allocated to an operator set from a list of strategies
 * Note that deallocations remain slashable for DEALLOCATION_DELAY blocks therefore when they are cleared they may
 * free up less allocatable magnitude than initially deallocated.
 * @param operator the operator to modify allocations for
 * @param params array of magnitude adjustments for one or more operator sets
 * @dev Updates encumberedMagnitude for the updated strategies
 */
function modifyAllocations(
    address operator, 
    AllocateParams[] calldata params
) 
    external
    onlyWhenNotPaused(PAUSED_MODIFY_ALLOCATIONS)
```

_Note: this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

This function is called by an operator to EITHER increase OR decrease the slashable magnitude allocated from a strategy to an operator set. As input, the operator provides an operator set as the target, and a list of strategies and corresponding `newMagnitudes` to allocate. The `newMagnitude` value is compared against the operator's current `Allocation` for that operator set/strategy:
* If `newMagnitude` is _greater than_ `Allocation.currentMagnitude`, this is an allocation
* If `newMagnitude` is _less than_ `Allocation.currentMagnitude`, this is a deallocation
* If `newMagnitude` is _equal to_ `Allocation.currentMagnitude`, this is invalid (revert)

Allocation modifications play by different rules depending on a few factors. Recall that at all times, the `encumberedMagnitude` for a strategy may not exceed that strategy's `maxMagnitude`. Additionally, note that _before processing a modification for a strategy,_ the `deallocationQueue` for that strategy is first cleared. This ensures any completable deallocations are processed first, freeing up magnitude for allocation. This process is further explained in [`clearDeallocationQueue`](#cleardeallocationqueue). 

Finally, `modifyAllocations` does NOT require an allocation to consider whether its corresponding strategy is relevant to the operator set in question. This is primarily to cut down on complexity. Because [`removeStrategiesFromOperatorSet`](#removestrategiesfromoperatorset) always allows an AVS to _remove_ strategies from consideration, we always need to be sure an operator can initiate a _deallocation_ for such strategies. Although there's not a clear usecase for _allocating_ when a strategy is not included in an operator set, we elected not to check for this. It's possible some AVSs may announce a strategy is being added ahead of time specifically to encourage allocations in advance. **It is expected behavior** that an AVS adding a strategy to an operator set makes any existing allocations to that strategy instantly slashable.

**If we are handling an _increase in magnitude_ (allocation):**

* The increase in magnitude is immediately added to the strategy's `encumberedMagnitude`. This ensures that subsequent _allocations to other operator sets from the same strategy_ will not go above the strategy's `maxMagnitude`.
* The `allocation.pendingDiff` is set, with an `allocation.effectBlock` equal to the current block plus the operator's configured allocation delay.

**If we are handling a _decrease in magnitude_ (deallocation):**

First, evaluate whether the operator's _existing allocation is currently slashable_ by the AVS. This is important because the AVS might be using the existing allocation to secure a task given to this operator. See [Evaluating Whether an Allocation is "Slashable"](#evaluating-whether-an-allocation-is-slashable) for details.

Next, _if the existing allocation IS slashable_:

* The `allocation.pendingDiff` is set, with an `allocation.effectBlock` equal to the current block plus `DEALLOCATION_DELAY + 1`. This means the existing allocation _remains slashable_ for `DEALLOCATION_DELAY` blocks.
* The _operator set_ is pushed to the operator's `deallocationQueue` for that strategy, denoting that there is a pending deallocation for this `(operatorSet, strategy)`. This is an ordered queue that enforces deallocations are processed sequentially and is used both in this method and in [`clearDeallocationQueue`](#cleardeallocationqueue).

Alternatively, _if the existing allocation IS NOT slashable_, the deallocated amount is immediately **freed**. It is subtracted from the strategy's encumbered magnitude and can be used for subsequent allocations. This is the only type of update that does not result in a "pending modification." The rationale here is that if the existing allocation is not slashable, the AVS does not need it to secure tasks, and therefore does not need to enforce a deallocation delay.

Another point of consideration are race conditions involving a slashing event and a deallocation occurring for an operator. Consider the following scenario with an operator having an allocation of 500 magnitude and trying to deallocate setting it to 250. However in the same block _right_ before calling `modifyAllocations` the operator is slashed 100% by the OperatorSet, setting the current magnitude to 0. Now the operator's deallocation is considered an allocation and ends up allocating 250 magnitude when they were trying to _deallocate_. This is a potential griefing vector by malicious AVSs and a known shortcoming. In such scenarios, the operator should simply deallocate all their allocations to 0 so that they don't accidentally allocate more slashable stake. In general for non malicious AVSs, slashing is deemed to be a very occasional occurrence and this race condition to not be impacting to operators.

*Effects*:
* For each `AllocateParams` element:
    * Complete any existing deallocations (See [`clearDeallocationQueue`](#cleardeallocationqueue))
    * Update the operator's `encumberedMagnitude`, `allocations`, and `deallocationQueue` according to the rules described above. Additionally:
        * If `encumberedMagnitude` is updated, emits `EncumberedMagnitudeUpdated`
        * If a pending modification is created:
            * Adds the `strategy` to `allocatedStrategies[operator][operatorSetKey]` (if not present)
            * Adds the `operatorSetKey` to `allocatedSets[operator]` (if not present)
        * If the allocation now has a `currentMagnitude` of 0:
            * Removes `strategy` from the `allocatedStrategies[operator][operatorSetKey]` list
            * If this list now has a length of 0, remove `operatorSetKey` from `allocatedSets[operator]`
    * Emits an `AllocationUpdated` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_MODIFY_ALLOCATIONS`
* Caller MUST be authorized, either as the operator themselves or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Operator MUST have already set an allocation delay (See [`setAllocationDelay`](#setallocationdelay))
* For each `AllocationParams` element:
    * Provided strategies MUST be of equal length to provided magnitudes for a given `AllocateParams` object
    * Operator set MUST exist for each specified AVS
    * Operator MUST NOT have pending, non-completable modifications for any given strategy
    * New magnitudes MUST NOT match existing ones
    * New encumbered magnitude MUST NOT exceed the operator's max magnitude for the given strategy

#### `clearDeallocationQueue`

```solidity
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
)
    external
    onlyWhenNotPaused(PAUSED_MODIFY_ALLOCATIONS)
```

This function is used to complete any eligible pending deallocations for an operator. The function takes an operator, a list of strategies, and a corresponding number of pending deallocations to complete. 

Clearing pending deallocations plays an important role in [`modifyAllocations`](#modifyallocations), as completable deallocations represent magnitude that can be freed for re-allocation to a different operator set. This method exists as a convenience for operators that want to complete pending deallocations as a standalone operation. However, `modifyAllocations` will _automatically_ clear any eligible deallocations when processing an allocation modification for a given strategy.

For each strategy, the method iterates over `deallocationQueue[operator][strategy]`:

```solidity
/// @dev For a strategy, keeps an ordered queue of operator sets that have pending deallocations
/// These must be completed in order to free up magnitude for future allocation
mapping(address operator => mapping(IStrategy strategy => DoubleEndedQueue.Bytes32Deque)) internal deallocationQueue;
```

This queue contains a per-strategy ordered list of operator sets that, due to prior calls by the `operator` to `modifyAllocations`, have a pending decrease in slashable magnitude. For each operator set in the queue, the corresponding allocation for that operator set is evaluated. If its `effectBlock` has been reached, the deallocation is completed, freeing up the deallocated magnitude by subtracting it from `encumberedMagnitude[operator][strategy]`. The corresponding entry is then popped from the front of the queue.

This method stops iterating when: the queue is empty, a deallocation is reached that cannot be completed yet, or when it has cleared `numToClear` entries from the queue.

*Effects*:
* For each `strategy` and _completable_ deallocation in `deallocationQueue[operator][strategy]`:
    * Pops the corresponding operator set from the `deallocationQueue`
    * Reduces `allocation.currentMagnitude` by the deallocated amount
    * Sets `allocation.pendingDiff` and `allocation.effectBlock` to 0
    * Adds the deallocated amount to the strategy's `encumberedMagnitude`
    * Emits `EncumberedMagnitudeUpdated`
    * Additionally, if the deallocation leaves `allocation.currentMagnitude` equal to zero:
        * Removes `strategy` from the `allocatedStrategies[operator][operatorSetKey]` list
        * If this list now has a length of 0, remove `operatorSetKey` from `allocatedSets[operator]`

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_MODIFY_ALLOCATIONS`
* Strategy list MUST be equal length of `numToClear` list

#### `slashOperator`

```solidity
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

/**
 * @notice Called by an AVS to slash an operator in a given operator set. The operator must be registered
 * and have slashable stake allocated to the operator set.
 *
 * @param avs The AVS address initiating the slash.
 * @param params The slashing parameters, containing:
 *  - operator: The operator to slash.
 *  - operatorSetId: The ID of the operator set the operator is being slashed from.
 *  - strategies: Array of strategies to slash allocations from (must be in ascending order).
 *  - wadsToSlash: Array of proportions to slash from each strategy (must be between 0 and 1e18).
 *  - description: Description of why the operator was slashed.
 *
 * @dev For each strategy:
 *      1. Reduces the operator's current allocation magnitude by wadToSlash proportion.
 *      2. Reduces the strategy's max and encumbered magnitudes proportionally.
 *      3. If there is a pending deallocation, reduces it proportionally.
 *      4. Updates the operator's shares in the DelegationManager.
 *
 * @dev Small slashing amounts may not result in actual token burns due to
 *      rounding, which will result in small amounts of tokens locked in the contract
 *      rather than fully burning through the burn mechanism.
 */
function slashOperator(
    address avs,
    SlashingParams calldata params
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_SLASHING)
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

AVSs use slashing as a punitive disincentive for misbehavior. For details and examples of how slashing works, see [ELIP-002#Slashing of Unique Stake](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#slashing-of-unique-stake). Note that whatever slashing criteria an AVS decides on, the only criteria enforced by the `AllocationManager` are those detailed above (see [Evaluating Whether an Allocation is "Slashable"](#evaluating-whether-an-allocation-is-slashable)).

In order to slash an eligible operator, the AVS specifies which operator set the operator belongs to, the `strategies` the operator should be slashed for, and for each strategy, the _proportion of the operator's allocated magnitude_ that should be slashed (given by `wadsToSlash`). An optional `description` string allows the AVS to add context to the slash.

Once triggered in the `AllocationManager`, slashing is instant and irreversible. For each slashed strategy, the operator's `maxMagnitude` and `encumberedMagnitude` are decreased, and the allocation made to the given operator set has its `currentMagnitude` reduced. See [TODO - Accounting Doc]() for details on how slashed amounts are calculated.

There are two edge cases to note for this method:
1. In the process of slashing an `operator` for a given `strategy`, if the `Allocation` being slashed has a `currentMagnitude` of 0, the call will NOT revert. Instead, the `strategy` is skipped and slashing continues with the next `strategy` listed. This is to prevent an edge case where slashing occurs on or around a deallocation's `effectBlock` -- if the call reverted, the entire slash would fail. Skipping allows any valid slashes to be processed without requiring resubmission.
2. If the `operator` has a pending, non-completable deallocation, the deallocation's `pendingDiff` is reduced proportional to the slash. This ensures that when the deallocation is completed, less `encumberedMagnitude` is freed.

Once slashing is processed for a strategy, [slashed stake is burned via the `DelegationManager`](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#burning-of-slashed-funds).

*Effects*:
* Given an `operator` and `operatorSet`, then for each `params.strategies` element and its corresponding `allocation`:
    * Calculates magnitude to slash by multiplying current magnitude by the provided `wadsToSlash`
    * Reduce `allocation.currentMagnitude` by the slashed magnitude
        * Emit an `AllocationUpdated` event
    * Reduce the operator's `encumberedMagnitude` for this strategy by the slashed magnitude
        * Emit an `EncumberedMagnitudeUpdated` event
    * Push an entry to the operator's `maxMagnitudeHistory`, reducing their `maxMagnitude` by the slashed magnitude
        * Emit a `MaxMagnitudeUpdated` event
    * If the `allocation` has a pending, non-completable deallocation, additionally reduce `allocation.pendingDiff` by the same proportion and emit an `AllocationUpdated` event
    * If the `allocation` now has a `currentMagnitude` of 0:
        * Removes `strategy` from the `allocatedStrategies[operator][operatorSetKey]` list
        * If this list now has a length of 0, remove `operatorSetKey` from `allocatedSets[operator]`
    * Calls [`DelegationManager.slashOperatorShares`](./DelegationManager.md#slashoperatorshares)
* Emit an `OperatorSlashed` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_OPERATOR_SLASHING`
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Operator set MUST be registered for the AVS
* Operator MUST BE slashable, i.e.:
    * Operator is registered for the operator set, *OR*
    * The operator's `DEALLOCATION_DELAY` has not yet completed
* `params.strategies` MUST be in ascending order (to ensure no duplicates)
* `params.strategies.length` MUST be equal to `params.wadsToSlash.length`
* For each `params.strategies` element:
    * `wadsToSlash` MUST be within the bounds `(0, 1e18]`
    * Operator set MUST contain the strategy

---

## Config

**Methods:**
* [`setAllocationDelay`](#setallocationdelay)
* [`setAVSRegistrar`](#setavsregistrar)

#### `setAllocationDelay`

```solidity
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
)
    external
```

_Note: IF NOT CALLED BY THE `DelegationManager`, this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

This function sets an operator's allocation delay, in blocks. This delay can be updated by the operator once set. Both the initial setting of this value and any further updates _take `ALLOCATION_CONFIGURATION_DELAY` blocks_ to take effect. Because having a delay is a requirement to allocating slashable stake, this effectively means that once the slashing release goes live, no one will be able to allocate slashable stake for at least `ALLOCATION_CONFIGURATION_DELAY` blocks.

The `DelegationManager` calls this upon operator registration for all new operators created after the slashing release. For operators that existed in the `DelegationManager` _prior_ to the slashing release, **they will need to call this method to configure an allocation delay prior to allocating slashable stake to any AVS**.

The allocation delay's primary purpose is to give stakers delegated to an operator the chance to withdraw their stake before the operator can change the risk profile to something they're not comfortable with. However, operators can choose to configure this delay however they want - including setting it to 0.

*Effects*:
* Sets the operator's `pendingDelay` to the proposed `delay`, and save the `effectBlock` at which the `pendingDelay` can be activated
    * `effectBlock = uint32(block.number) + ALLOCATION_CONFIGURATION_DELAY + 1`
* If the operator has a `pendingDelay`, and if the `effectBlock` has passed, sets the operator's `delay` to the `pendingDelay` value
    * This also sets the `isSet` boolean to `true` to indicate that the operator's `delay`, even if 0, was set intentionally
* Emits an `AllocationDelaySet` event

*Requirements*:
* Caller MUST BE either the DelegationManager, or a registered operator
    * An admin and/or appointee for the operator can also call this function (see [`PermissionController.md`](../permissions/PermissionController.md))

#### `setAVSRegistrar`

```solidity
/**
 * @notice Called by an AVS to configure the address that is called when an operator registers
 * or is deregistered from the AVS's operator sets. If not set (or set to 0), defaults
 * to the AVS's address.
 * @param registrar the new registrar address
 */
function setAVSRegistrar(
    address avs,
    IAVSRegistrar registrar
)
    external
    checkCanCall(avs)
```

_Note: this method can be called directly by an AVS, or by a caller authorized by the AVS. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

Sets the `registrar` for a given `avs`. Note that if the registrar is set to 0, `getAVSRegistrar` will return the AVS's address.

The avs registrar is called when operators register to or deregister from an operator set. From [`IAVSRegistrar.sol`](../../src/contracts/interfaces/IAVSRegistrar.sol), the avs registrar should use the following interface:

```solidity
interface IAVSRegistrar {
    /**
     * @notice Called by the AllocationManager when an operator wants to register
     * for one or more operator sets. This method should revert if registration
     * is unsuccessful.
     * @param operator the registering operator
     * @param operatorSetIds the list of operator set ids being registered for
     * @param data arbitrary data the operator can provide as part of registration
     */
    function registerOperator(address operator, uint32[] calldata operatorSetIds, bytes calldata data) external;

    /**
     * @notice Called by the AllocationManager when an operator is deregistered from
     * one or more operator sets. If this method reverts, it is ignored.
     * @param operator the deregistering operator
     * @param operatorSetIds the list of operator set ids being deregistered from
     */
    function deregisterOperator(address operator, uint32[] calldata operatorSetIds) external;
}
```

Note that when an operator registers, registration will FAIL if the call to `IAVSRegistrar` reverts. However, when an operator deregisters, a revert in `deregisterOperator` is ignored.

*Effects*:
* Sets `_avsRegistrar[avs]` to `registrar`
* Emits an `AVSRegistrarSet` event

*Requirements*:
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
````

## File: docs/core/AVSDirectory.md
````markdown
[middleware-repo]: https://github.com/Layr-Labs/eigenlayer-middleware/

## AVSDirectory

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`AVSDirectory.sol`](../../src/contracts/core/AVSDirectory.sol) | Singleton | Transparent proxy |

*Note: The AVSDirectory is deprecated as of the slashing release. This documentation is kept for historical purposes. AVSs are recommended to use the [AllocationManager](./AllocationManager.md) for the new operatorSet and slashing model.*

The `AVSDirectory` once handled interactions between AVSs and the EigenLayer core contracts. Once registered as an Operator in EigenLayer core (via the `DelegationManager`), Operators can register with one or more AVSs (via the AVS's contracts) to begin providing services to them offchain. As a part of registering with an AVS, the AVS will record this registration in the core contracts by calling into the `AVSDirectory`.

For more information on AVS contracts, see the [middleware repo][middleware-repo].

Currently, the only interactions between AVSs and the core contracts is to track whether Operators are currently registered for the AVS. This is handled by two methods:
<!-- no toc -->
* [`AVSDirectory.registerOperatorToAVS`](#registeroperatortoavs)
* [`AVSDirectory.deregisterOperatorFromAVS`](#deregisteroperatorfromavs)

---

#### `registerOperatorToAVS`

```solidity
function registerOperatorToAVS(
    address operator,
    ISignatureUtils.SignatureWithSaltAndExpiry memory operatorSignature
) 
    external 
    onlyWhenNotPaused(PAUSED_OPERATOR_REGISTER_DEREGISTER_TO_AVS)
```

Allows the caller (an AVS) to register an `operator` with itself, given the provided signature is valid.

*Effects*:
* Sets the `operator`'s status to `REGISTERED` for the AVS

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_OPERATOR_REGISTER_DEREGISTER_TO_AVS`
* `operator` MUST already be a registered Operator (via the `DelegationManager`)
* `operator` MUST NOT already be registered with the AVS
* `operatorSignature` must be a valid, unused, unexpired signature from the `operator`. The signature is an ECDSA signature by the operator over the [`OPERATOR_AVS_REGISTRATION_TYPEHASH`](../../src/contracts/core/DelegationManagerStorage.sol). Expiry is a utc timestamp in seconds. Salt is used only once per signature to prevent replay attacks.

*As of M2*:
* Operator registration/deregistration in the AVSDirectory does not have any sort of consequences for the Operator or its shares. The AllocationManager handles with operator sets, tying in rewards for services and slashing for misbehavior.

#### `deregisterOperatorFromAVS`

```solidity
function deregisterOperatorFromAVS(
    address operator
) 
    external 
    onlyWhenNotPaused(PAUSED_OPERATOR_REGISTER_DEREGISTER_TO_AVS)
```

Allows the caller (an AVS) to deregister an `operator` with itself

*Effects*:
* Sets the `operator's` status to `UNREGISTERED` for the AVS

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_OPERATOR_REGISTER_DEREGISTER_TO_AVS`
* `operator` MUST already be registered with the AVS

*As of M2*:
* Operator registration/deregistration in the AVSDirectory does not have any sort of consequences for the Operator or its shares. The AllocationManager handles this with operator sets, tying in rewards for services and slashing for misbehavior.

#### `cancelSalt`

```solidity
function cancelSalt(bytes32 salt) external
```

Allows the caller (an Operator) to cancel a signature salt before it is used to register for an AVS.

*Effects*:
* Sets `operatorSaltIsSpent[msg.sender][salt]` to `true`

*Requirements*:
* Salt MUST NOT already be cancelled
````

## File: docs/core/DelegationManager.md
````markdown
## DelegationManager

| File | Notes |
| -------- | -------- |
| [`DelegationManager.sol`](../../src/contracts/core/DelegationManager.sol) | |
| [`DelegationManagerStorage.sol`](../../src/contracts/core/DelegationManagerStorage.sol) | state variables |
| [`IDelegationManager.sol`](../../src/contracts/interfaces/IDelegationManager.sol) | interface |

Libraries and Mixins:

| File | Notes |
| -------- | -------- |
| [`PermissionControllerMixin.sol`](../../src/contracts/mixins/PermissionControllerMixin.sol) | account delegation |
| [`SignatureUtils.sol`](../../src/contracts/mixins/SignatureUtils.sol) | signature validation |
| [`Pausable.sol`](../../src/contracts/permissions/Pausable.sol) | |
| [`SlashingLib.sol`](../../src/contracts/libraries/SlashingLib.sol) | slashing math |
| [`Snapshots.sol`](../../src/contracts/libraries/Snapshots.sol) | historical state |

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
* [Shares Accounting](./accounting/SharesAccounting.md)

## Overview

The `DelegationManager` is the intersection between the two sides of the protocol. It (i) allows stakers to delegate/undelegate to/from operators, (ii) handles withdrawals and withdrawal processing for assets in both the `StrategyManager` and `EigenPodManager`, and (iii) manages accounting around slashing for stakers and operators.

When operators are slashed by AVSs, it receives share burning directives from the `AllocationManager`. When stakers deposit assets using the `StrategyManager/EigenPodManager`, it tracks share/delegation accounting changes. The `DelegationManager` combines inputs from both sides of the protocol into a staker's "deposit scaling factor," which serves as the primary conversion vehicle between a staker's _raw deposited assets_ and the _amount they can withdraw_.

The `DelegationManager's` responsibilities can be broken down into the following concepts:
* [Becoming an Operator](#becoming-an-operator)
* [Delegation and Withdrawals](#delegation-and-withdrawals)
* [Slashing and Accounting](#slashing-and-accounting)

## Parameterization

* `MIN_WITHDRAWAL_DELAY_BLOCKS`: The delay in blocks before withdrawals can be completed.
    * Mainnet: `100800 blocks` (14 days).
    * Testnet: `50 blocks` (10 minutes).
* `beaconChainETHStrategy`: a pseudo strategy used to represent beacon chain ETH internally. This is not a real contract!
    * Value: `0xbeaC0eeEeeeeEEeEeEEEEeeEEeEeeeEeeEEBEaC0`

---

## Becoming an Operator

The `DelegationManager` tracks operator-related state in the following mappings:

```solidity
/// @notice Returns the `operator` a `staker` is delegated to, or address(0) if not delegated.
/// Note: operators are delegated to themselves
mapping(address staker => address operator) public delegatedTo;

/// @notice Returns the operator details for a given `operator`.
/// Note: two of the `OperatorDetails` fields are deprecated. The only relevant field
/// is `OperatorDetails.delegationApprover`.
mapping(address operator => OperatorDetails) internal _operatorDetails;

/**
 * @notice Tracks the current balance of shares an `operator` is delegated according to each `strategy`. 
 * Updated by both the `StrategyManager` and `EigenPodManager` when a staker's delegatable balance changes,
 * and by the `AllocationManager` when the `operator` is slashed.
 *
 * @dev The following invariant should hold for each `strategy`:
 *
 * operatorShares[operator] = sum(withdrawable shares of all stakers delegated to operator)
 */
mapping(address operator => mapping(IStrategy strategy => uint256 shares)) public operatorShares;
```

**Methods**:
* [`DelegationManager.registerAsOperator`](#registerasoperator)
* [`DelegationManager.modifyOperatorDetails`](#modifyoperatordetails)
* [`DelegationManager.updateOperatorMetadataURI`](#updateoperatormetadatauri)

#### `registerAsOperator`

```solidity
/**
 * @notice Registers the caller as an operator in EigenLayer.
 * @param initDelegationApprover is an address that, if set, must provide a signature when stakers delegate
 * to an operator.
 * @param allocationDelay The delay before allocations take effect.
 * @param metadataURI is a URI for the operator's metadata, i.e. a link providing more details on the operator.
 *
 * @dev Once an operator is registered, they cannot 'deregister' as an operator, and they will forever be considered "delegated to themself".
 * @dev This function will revert if the caller is already delegated to an operator.
 * @dev Note that the `metadataURI` is *never stored * and is only emitted in the `OperatorMetadataURIUpdated` event
 */
function registerAsOperator(
    address initDelegationApprover,
    uint32 allocationDelay,
    string calldata metadataURI
) external nonReentrant;
```

Registers the caller as an operator in EigenLayer. The new operator provides the following input parameters:
* `address initDelegationApprover`: *(OPTIONAL)* if set to a non-zero address, this address must sign and approve new delegation from stakers to this operator (See [`delegateTo`](#delegateto))
* `uint32 allocationDelay`: the delay (in blocks) before slashable stake allocations will take effect. This is passed to the `AllocationManager` (See [`AllocationManager.md#setAllocationDelay`](./AllocationManager.md#setallocationdelay))
* `string calldata metadataURI`: emits this input in the event `OperatorMetadataURIUpdated`. Does not store the value anywhere.

`registerAsOperator` cements the operator's delegation approver and allocation delay in storage, and self-delegates the operator to themselves - permanently marking the caller as an operator. They cannot "deregister" as an operator - however, if they have deposited funds, they can still withdraw them (See [Delegation and Withdrawals](#delegation-and-withdrawals)).

*Effects*:
* Sets `_operatorDetails[operator].delegationApprover`. Note that the other `OperatorDetails` fields are deprecated; only the `delegationApprover` is used.
* Delegates the operator to themselves
    * Tabulates any deposited shares across the `EigenPodManager` and `StrategyManager`, and delegates these shares to themselves
    * For each strategy in which the operator holds assets, updates the operator's `depositScalingFactor` for that strategy

*Requirements*:
* Caller MUST NOT already be delegated
* Pause status MUST NOT be set: `PAUSED_NEW_DELEGATION`
* For each strategy in which the operator holds assets, their `slashingFactor` for that strategy MUST be non-zero.

#### `modifyOperatorDetails`

```solidity
/**
 * @notice Updates an operator's stored `delegationApprover`.
 * @param operator is the operator to update the delegationApprover for
 * @param newDelegationApprover is the new delegationApprover for the operator
 *
 * @dev The caller must have previously registered as an operator in EigenLayer.
 */
function modifyOperatorDetails(
    address operator, 
    address newDelegationApprover
) 
    external 
    checkCanCall(operator)
    nonReentrant
```

_Note: this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

Allows an operator to update their stored `delegationApprover`.

*Requirements*:
* `address operator` MUST already be an operator.
* Caller MUST be authorized: either the operator themselves, or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))

#### `updateOperatorMetadataURI`

```solidity
/**
 * @notice Called by an operator to emit an `OperatorMetadataURIUpdated` event indicating the information has updated.
 * @param operator The operator to update metadata for
 * @param metadataURI The URI for metadata associated with an operator
 * @dev Note that the `metadataURI` is *never stored * and is only emitted in the `OperatorMetadataURIUpdated` event
 */
function updateOperatorMetadataURI(
    address operator, 
    string calldata metadataURI
) 
    external 
    checkCanCall(operator)
```

_Note: this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

Allows an operator to emit an `OperatorMetadataURIUpdated` event. No other state changes occur.

*Requirements*:
* `address operator` MUST already be an operator.
* Caller MUST be authorized: either the operator themselves, or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))

---

## Delegation and Withdrawals

**Concepts**:
* [Shares Accounting](./accounting/SharesAccounting.md)
* [Legacy and Post-Slashing Withdrawals](#legacy-and-post-slashing-withdrawals)
* [Slashing Factors and Scaling Shares](#slashing-factors-and-scaling-shares)

**Methods**:
* [`DelegationManager.delegateTo`](#delegateto)
* [`DelegationManager.undelegate`](#undelegate)
* [`DelegationManager.redelegate`](#redelegate)
* [`DelegationManager.queueWithdrawals`](#queuewithdrawals)
* [`DelegationManager.completeQueuedWithdrawal`](#completequeuedwithdrawal)
* [`DelegationManager.completeQueuedWithdrawals`](#completequeuedwithdrawals)

#### Legacy and Post-Slashing Withdrawals

The `DelegationManager` tracks withdrawal-related state in the following mappings:

```solidity
/**
 * @dev A struct representing an existing queued withdrawal. After the withdrawal delay has elapsed, this withdrawal can be completed via `completeQueuedWithdrawal`.
 * A `Withdrawal` is created by the `DelegationManager` when `queueWithdrawals` is called. The `withdrawalRoots` hashes returned by `queueWithdrawals` can be used
 * to fetch the corresponding `Withdrawal` from storage (via `queuedWithdrawals`).
 *
 * @param staker The address that queued the withdrawal
 * @param delegatedTo The address that the staker was delegated to at the time the withdrawal was queued. Used to determine if additional slashing occurred before
 * this withdrawal became completable.
 * @param withdrawer The address that will call the contract to complete the withdrawal. Note that this will always equal `staker`; alternate withdrawers are not
 * supported at this time.
 * @param nonce The staker's `cumulativeWithdrawalsQueued` at time of queuing. Used to ensure withdrawals have unique hashes.
 * @param startBlock The block number when the withdrawal was queued.
 * @param strategies The strategies requested for withdrawal when the withdrawal was queued
 * @param scaledShares The staker's deposit shares requested for withdrawal, scaled by the staker's `depositScalingFactor`. Upon completion, these will be
 * scaled by the appropriate slashing factor as of the withdrawal's completable block. The result is what is actually withdrawable.
 */
struct Withdrawal {
    address staker;
    address delegatedTo;
    address withdrawer;
    uint256 nonce;
    uint32 startBlock;
    IStrategy[] strategies;
    uint256[] scaledShares;
}

/// @dev Returns whether a withdrawal is pending for a given `withdrawalRoot`.
/// @dev This variable will be deprecated in the future, values should only be read or deleted.
mapping(bytes32 withdrawalRoot => bool pending) public pendingWithdrawals;

/// @notice Returns the total number of withdrawals that have been queued for a given `staker`.
/// @dev This only increments (doesn't decrement), and is used to help ensure that otherwise identical withdrawals have unique hashes.
mapping(address staker => uint256 totalQueued) public cumulativeWithdrawalsQueued;

/// @notice Returns a list of queued withdrawals for a given `staker`.
/// @dev Entries are removed when the withdrawal is completed.
/// @dev This variable only reflects withdrawals that were made after the slashing release.
mapping(address staker => EnumerableSet.Bytes32Set withdrawalRoots) internal _stakerQueuedWithdrawalRoots;

/// @notice Returns the details of a queued withdrawal given by `withdrawalRoot`.
/// @dev This variable only reflects withdrawals that were made after the slashing release.
mapping(bytes32 withdrawalRoot => Withdrawal withdrawal) public queuedWithdrawals;

/// @notice Contains history of the total cumulative staker withdrawals for an operator and a given strategy.
/// Used to calculate burned StrategyManager shares when an operator is slashed.
/// @dev Stores scaledShares instead of total withdrawn shares to track current slashable shares, dependent on the maxMagnitude
mapping(address operator => mapping(IStrategy strategy => Snapshots.DefaultZeroHistory)) internal
    _cumulativeScaledSharesHistory;
```

Prior to the slashing release, withdrawals were only stored as hashes in the `pendingWithdrawals` mapping. 

With the slashing release, withdrawals are now stored entirely in state, and two new mappings have been added to support this:
* `_stakedQueuedWithdrawalRoots`: a list of all the currently-queued withdrawal hashes belonging to a staker
* `queuedWithdrawals`: maps queued withdrawal hash to `Withdrawal` struct

Legacy withdrawals remain completable using the same methods as new withdrawals. The primary difference between the two is that it is not possible to query the corresponding `Withdrawal` struct for a legacy withdrawal hash. When determining what `Withdrawal` struct to supply to the contract to complete a legacy withdrawal, the caller will need to derive the original `Withdrawal` struct generated when the withdrawal was queued.

#### Slashing Factors and Scaling Shares

_See the [Shares Accounting](./accounting/SharesAccounting.md) doc for a more thorough explanation with examples._

Throughout the `DelegationManager`, a staker's _deposit shares_ can be converted into their current _withdrawable shares_ by applying two factors: the _slashing factor_ and the _deposit scaling factor_. These two values are scaling factors that act as numerators when scaling shares. By default, these values start at `1 WAD` (`1e18`). `1 WAD` also acts as the denominator when scaling.

```solidity
/// @dev All scaling factors have `1e18` as an initial/default value. This value is represented
/// by the constant `WAD`, which is used to preserve precision with uint256 math.
///
/// When applying scaling factors, they are typically multiplied/divided by `WAD`, allowing this
/// constant to act as a "1" in mathematical formulae.
uint64 constant WAD = 1e18;
```

The _deposit scaling factor_ is represented in `DelegationManager` storage, and can be thought of as a way to normalize newly-deposited shares using the _current_ slashing factor, so that _future_ withdrawals can be scaled appropriately if the slashing factor has changed:

```solidity
/*
 * There are 2 types of shares:
 *      1. deposit shares
 *          - These can be converted to an amount of tokens given a strategy
 *              - by calling `sharesToUnderlying` on the strategy address (they're already tokens 
 *              in the case of EigenPods)
 *          - These live in the storage of the EigenPodManager and individual StrategyManager strategies 
 *      2. withdrawable shares
 *          - For a staker, this is the amount of shares that they can withdraw
 *          - For an operator, the shares delegated to them are equal to the sum of their stakers'
 *            withdrawable shares
 *
 * Along with a slashing factor, the DepositScalingFactor is used to convert between the two share types.
 */
struct DepositScalingFactor {
    uint256 _scalingFactor;
}

/// @notice Returns the scaling factor applied to a `staker` for a given `strategy`
mapping(address staker => mapping(IStrategy strategy => DepositScalingFactor)) internal _depositScalingFactor;
```

Calculating the _slashing factor_ varies depending on the strategy in question. _For all strategies_, the slashing factor is the max magnitude of the staker's delegated `operator` in the `AllocationManager` (See [Max vs Encumbered Magnitude](./AllocationManager.md#max-vs-encumbered-magnitude)). If the staker is NOT delegated, this is `WAD` (aka "1").

_For the `beaconChainETHStrategy`_, the slashing factor _also_ includes the staker's `beaconChainSlashingFactor`, which acts like the `operator's` max magnitude, but for a staker's beacon chain assets. This means that, for the `beaconChainETHStrategy` specifically, _slashing factors_ can be applied because of EITHER/BOTH:
* `operator` got slashed for this strategy by an AVS
* `staker` got slashed on the beacon chain

From `DelegationManager.sol`:

```solidity
/// @dev Calculate the amount of slashing to apply to the staker's shares
function _getSlashingFactor(
    address staker,
    IStrategy strategy,
    uint64 operatorMaxMagnitude
) internal view returns (uint256) {
    if (strategy == beaconChainETHStrategy) {
        uint64 beaconChainSlashingFactor = eigenPodManager.beaconChainSlashingFactor(staker);
        return operatorMaxMagnitude.mulWad(beaconChainSlashingFactor);
    }

    return operatorMaxMagnitude;
}
```

#### `delegateTo`

```solidity
// @notice Struct that bundles together a signature and an expiration time for the signature. Used primarily for stack management.
struct SignatureWithExpiry {
    // the signature itself, formatted as a single bytes object
    bytes signature;
    // the expiration timestamp (UTC) of the signature
    uint256 expiry;
}

/**
 * @notice Caller delegates their stake to an operator.
 * @param operator The account (`msg.sender`) is delegating its assets to for use in serving applications built on EigenLayer.
 * @param approverSignatureAndExpiry (optional) Verifies the operator approves of this delegation
 * @param approverSalt (optional) A unique single use value tied to an individual signature.
 * @dev The signature/salt are used ONLY if the operator has configured a delegationApprover.
 * If they have not, these params can be left empty.
 */
function delegateTo(
    address operator, 
    SignatureWithExpiry memory approverSignatureAndExpiry, 
    bytes32 approverSalt
) 
    external
    nonReentrant
```

Allows a staker to delegate their assets to an operator. Delegation is all-or-nothing: when a staker delegates to an operator, they delegate ALL their assets. Stakers can only be delegated to one operator at a time.

For each strategy the staker has deposit shares in, the `DelegationManager` will:
* Query the staker's deposit shares from the `StrategyManager/EigenPodManager`
* Get the slashing factor for this `(staker, operator, strategy)` and use it to update the staker's deposit scaling factor (See [Slashing Factors and Scaling Shares](#slashing-factors-and-scaling-shares))
* Add the deposit shares to the operator's `operatorShares` directly. _Note_ that the initial delegation to an operator is a special case where deposit shares == withdrawable shares.

*Effects*:
* Delegates the caller to the `operator`
    * Tabulates any deposited shares across the `EigenPodManager` and `StrategyManager`, and delegates these shares to the `operator`
    * For each strategy in which the caller holds assets, updates the caller's `depositScalingFactor` for that strategy

*Requirements*:
* The caller MUST NOT already be delegated to an operator
* The `operator` MUST already be an operator
* If the `operator` has a `delegationApprover`, the caller MUST provide a valid `approverSignatureAndExpiry` and `approverSalt`
* Pause status MUST NOT be set: `PAUSED_NEW_DELEGATION`
* For each strategy in which the staker holds assets, the `slashingFactor` for that strategy MUST be non-zero.

#### `undelegate`

```solidity
/**
 * @notice Undelegates the staker from their operator and queues a withdrawal for all of their shares
 * @param staker The account to be undelegated
 * @return withdrawalRoots The roots of the newly queued withdrawals, if a withdrawal was queued. Returns 
 * an empty array if none was queued.
 *
 * @dev Reverts if the `staker` is also an operator, since operators are not allowed to undelegate from themselves.
 * @dev Reverts if the caller is not the staker, nor the operator who the staker is delegated to, nor the operator's specified "delegationApprover"
 * @dev Reverts if the `staker` is not delegated to an operator
 */
function undelegate(
    address staker
) 
    external
    nonReentrant
    returns (bytes32[] memory withdrawalRoots);
```

_Note: this method can be called directly by an operator, or by a caller authorized by the operator. See [`PermissionController.md`](../permissions/PermissionController.md) for details._

`undelegate` can be called EITHER by a staker to undelegate themselves, OR by an operator to force-undelegate a staker from them. Force-undelegation is primarily useful if an operator has a `delegationApprover`, as this role is the only way to prevent a staker from delegating back to the operator once force-undelegated.

Undelegation immediately sets the staker's delegated operator to 0, decreases the prior operator's delegated shares, and queues withdrawals for all of the staker's deposited assets. For UX reasons, one withdrawal is queued for each strategy in which the staker has deposited assets. Queued withdrawals mimic the behavior of the [`queueWithdrawals`](#queuewithdrawals) method; see that method's documentation for details.

Just as with a normal queued withdrawal, these withdrawals can be completed by the staker after `MIN_WITHDRAWAL_DELAY_BLOCKS`. These withdrawals do not require the staker to "fully exit" from the system -- the staker may choose to keep their assets in the system once withdrawals are completed (See [`completeQueuedWithdrawal`](#completequeuedwithdrawal) for details).

*Effects*: 
* The `staker` is undelegated from their operator
* If the `staker` has no deposit shares, there is no withdrawal queued or further effects
* For each strategy held by the `staker`, a `Withdrawal` is queued:
    * _Deposit shares_ are removed from the staker's deposit share balances
        * See [`EigenPodManager.removeDepositShares`](./EigenPodManager.md#removedepositshares)
        * See [`StrategyManager.removeDepositShares`](./StrategyManager.md#removedepositshares)
    * _Deposit shares_ are converted to _withdrawable shares_ (See [Slashing Factors and Scaling Shares](#slashing-factors-and-scaling-shares)). These are decremented from the operator's delegated shares.
    * _Deposit shares_ are converted to _scaled shares_  (See [Shares Accounting - Queue Withdrawals](./accounting/SharesAccounting.md#queue-withdrawal)), which are stored in the `Withdrawal` struct
    * _Scaled shares_ are pushed to `_cumulativeScaledSharesHistory`, which is used for burning slashed shares
    * The `Withdrawal` is saved to storage
        * The hash of the `Withdrawal` is marked as "pending"
        * The hash of the `Withdrawal` is set in a mapping to the `Withdrawal` struct itself
        * The hash of the `Withdrawal` is pushed to `_stakerQueuedWithdrawalRoots`
    * The staker's withdrawal nonce is increased by 1 for each `Withdrawal`

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_ENTER_WITHDRAWAL_QUEUE`
* `staker` MUST be delegated to an operator
* `staker` MUST NOT be an operator
* `staker` parameter MUST NOT be zero
* Caller MUST be authorized: either the `staker` themselves, their operator, their operator's `delegationApprover`, or their operator's admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* See [`EigenPodManager.removeDepositShares`](./EigenPodManager.md#removedepositshares)
* See [`StrategyManager.removeDepositShares`](./StrategyManager.md#removedepositshares)

#### `redelegate`

```solidity
/**
 * @notice Undelegates the staker from their current operator, and redelegates to `newOperator`
 * Queues a withdrawal for all of the staker's withdrawable shares. These shares will only be
 * delegated to `newOperator` AFTER the withdrawal is completed.
 * @dev This method acts like a call to `undelegate`, then `delegateTo`
 * @param newOperator the new operator that will be delegated all assets
 * @dev NOTE: the following 2 params are ONLY checked if `newOperator` has a `delegationApprover`.
 * If not, they can be left empty.
 * @param newOperatorApproverSig A signature from the operator's `delegationApprover`
 * @param approverSalt A unique single use value tied to the approver's signature
 */
 function redelegate(
    address newOperator,
    SignatureWithExpiry memory newOperatorApproverSig,
    bytes32 approverSalt
) 
    external 
    returns (bytes32[] memory withdrawalRoots);
```

`redelegate` is a convenience method that combines the effects of `undelegate` and `delegateTo`. `redelegate` allows a staker to switch their delegated operator to `newOperator` with a single call. **Note**, though, that the staker's assets will not be fully delegated to `newOperator` until the withdrawals queued during the undelegation portion of this method are completed.

*Effects*: 
* See [`delegateTo`](#delegateto) and [`undelegate`](#undelegate)

*Requirements*:
* See [`delegateTo`](#delegateto) and [`undelegate`](#undelegate)

#### `queueWithdrawals`

```solidity
/**
 * @param strategies The strategies to withdraw from
 * @param depositShares For each strategy, the number of deposit shares to withdraw. Deposit shares can
 * be queried via `getDepositedShares`.
 * NOTE: The number of shares ultimately received when a withdrawal is completed may be lower depositShares
 * if the staker or their delegated operator has experienced slashing.
 * @param __deprecated_withdrawer This field is ignored. The only party that may complete a withdrawal
 * is the staker that originally queued it. Alternate withdrawers are not supported.
 */
struct QueuedWithdrawalParams {
    IStrategy[] strategies;
    uint256[] depositShares;
    address __deprecated_withdrawer;
}

/**
 * @notice Allows a staker to queue a withdrawal of their deposit shares. The withdrawal can be 
 * completed after the MIN_WITHDRAWAL_DELAY_BLOCKS via either of the completeQueuedWithdrawal methods.
 * 
 * While in the queue, these shares are removed from the staker's balance, as well as from their operator's
 * delegated share balance (if applicable). Note that while in the queue, deposit shares are still subject
 * to slashing. If any slashing has occurred, the shares received may be less than the queued deposit shares.
 *
 * @dev To view all the staker's strategies/deposit shares that can be queued for withdrawal, see `getDepositedShares`
 * @dev To view the current conversion between a staker's deposit shares and withdrawable shares, see `getWithdrawableShares`
 */
function queueWithdrawals(
    QueuedWithdrawalParams[] calldata queuedWithdrawalParams
) 
    external 
    onlyWhenNotPaused(PAUSED_ENTER_WITHDRAWAL_QUEUE)
    nonReentrant
    returns (bytes32[] memory)
```

Allows the caller to queue their deposit shares for withdrawal across any strategy. Withdrawals can be completed after `MIN_WITHDRAWAL_DELAY_BLOCKS`, by calling [`completeQueuedWithdrawal`](#completequeuedwithdrawal). This method accepts _deposit shares_ as input - however, the amounts received upon completion may be lower if the staker has experienced slashing (See [Shares Accounting](./accounting/SharesAccounting.md) and [Slashing Factors and Scaling Shares](#slashing-factors-and-scaling-shares)).

For each `QueuedWithdrawalParams` passed as input, a `Withdrawal` is created in storage (See [Legacy and Post-Slashing Withdrawals](#legacy-and-post-slashing-withdrawals) for details on structure and querying). Queueing a withdrawal involves multiple transformations to a staker's _deposit shares_, serving a few different purposes:
* The raw _deposit shares_ are removed from the staker's deposit share balance in the corresponding share manager (`EigenPodManager` or `StrategyManager`).
* _Scaled shares_ are calculated by applying the staker's _deposit scaling factor_ to their _deposit shares_. Scaled shares:
    * are stored in the `Withdrawal` itself and used during withdrawal completion
    * are added to the operator's `cumulativeScaledSharesHistory`, where they can be burned if slashing occurs while the withdrawal is in the queue
* _Withdrawable shares_ are calculated by applying both the staker's _deposit scaling factor_ AND any appropriate _slashing factor_ to the staker's _deposit shares_. These "currently withdrawable shares" are removed from the operator's delegated shares (if applicable).

Note that the `QueuedWithdrawalParams.__deprecated_withdrawer` field is ignored. Originally, this was used to create withdrawals that could be completed by a third party. This functionality was removed during the M2 release due to growing concerns over the phish risk this presented. Until the slashing release, this field was explicitly checked for equivalence with `msg.sender`; however, at present it is ignored. All `Withdrawals` are created with `withdrawer == staker` regardless of this field's value.

*Effects*:
* For each `QueuedWithdrawalParams` element:
    * _Deposit shares_ are removed from the staker's deposit share balances
        * See [`EigenPodManager.removeDepositShares`](./EigenPodManager.md#removedepositshares)
        * See [`StrategyManager.removeDepositShares`](./StrategyManager.md#removedepositshares)
    * _Deposit shares_ are converted to _withdrawable shares_ (See [Slashing Factors and Scaling Deposits](#slashing-factors-and-scaling-shares)). These are decremented from their operator's delegated shares (if applicable)
    * _Deposit shares_ are converted to _scaled shares_  (See [Shares Accounting - Queue Withdrawals](./accounting/SharesAccounting.md#queue-withdrawal)), which are stored in the `Withdrawal` struct
    * If the caller is delegated to an operator, _scaled shares_ are pushed to that operator's `_cumulativeScaledSharesHistory`, which may be burned if slashing occurs.
    * The `Withdrawal` is saved to storage
        * The hash of the `Withdrawal` is marked as "pending"
        * The hash of the `Withdrawal` is set in a mapping to the `Withdrawal` struct itself
        * The hash of the `Withdrawal` is pushed to `_stakerQueuedWithdrawalRoots`
    * The staker's withdrawal nonce is increased by 1

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_ENTER_WITHDRAWAL_QUEUE`
* For each `QueuedWithdrawalParams` element:
    * `strategies.length` MUST equal `depositShares.length`
    * The `withdrawer` MUST equal `msg.sender`
    * `strategies.length` MUST NOT be equal to 0
    * See [`EigenPodManager.removeDepositShares`](./EigenPodManager.md#removedepositshares)
    * See [`StrategyManager.removeDepositShares`](./StrategyManager.md#removedepositshares)

#### `completeQueuedWithdrawal`

```solidity
/**
 * @dev A struct representing an existing queued withdrawal. After the withdrawal delay has elapsed, this withdrawal can be completed via `completeQueuedWithdrawal`.
 * A `Withdrawal` is created by the `DelegationManager` when `queueWithdrawals` is called. The `withdrawalRoots` hashes returned by `queueWithdrawals` can be used
 * to fetch the corresponding `Withdrawal` from storage (via `getQueuedWithdrawal`).
 *
 * @param staker The address that queued the withdrawal
 * @param delegatedTo The address that the staker was delegated to at the time the withdrawal was queued. Used to determine if additional slashing occurred before
 * this withdrawal became completable.
 * @param withdrawer The address that will call the contract to complete the withdrawal. Note that this will always equal `staker`; alternate withdrawers are not
 * supported at this time.
 * @param nonce The staker's `cumulativeWithdrawalsQueued` at time of queuing. Used to ensure withdrawals have unique hashes.
 * @param startBlock The block number when the withdrawal was queued.
 * @param strategies The strategies requested for withdrawal when the withdrawal was queued
 * @param scaledShares The staker's deposit shares requested for withdrawal, scaled by the staker's `depositScalingFactor`. Upon completion, these will be
 * scaled by the appropriate slashing factor as of the withdrawal's completable block. The result is what is actually withdrawable.
 */
struct Withdrawal {
    address staker;
    address delegatedTo;
    address withdrawer;
    uint256 nonce;
    uint32 startBlock;
    IStrategy[] strategies;
    uint256[] scaledShares;
}

/**
 * @notice Used to complete a queued withdrawal
 * @param withdrawal The withdrawal to complete
 * @param tokens Array in which the i-th entry specifies the `token` input to the 'withdraw' function of the i-th Strategy in the `withdrawal.strategies` array.
 * @param tokens For each `withdrawal.strategies`, the underlying token of the strategy
 * NOTE: if `receiveAsTokens` is false, the `tokens` array is unused and can be filled with default values. However, `tokens.length` MUST still be equal to `withdrawal.strategies.length`.
 * NOTE: For the `beaconChainETHStrategy`, the corresponding `tokens` value is ignored (can be 0).
 * @param receiveAsTokens If true, withdrawn shares will be converted to tokens and sent to the caller. If false, the caller receives shares that can be delegated to an operator.
 * NOTE: if the caller receives shares and is currently delegated to an operator, the received shares are
 * automatically delegated to the caller's current operator.
 */
function completeQueuedWithdrawal(
    Withdrawal calldata withdrawal,
    IERC20[] calldata tokens,
    bool receiveAsTokens
) 
    external 
    onlyWhenNotPaused(PAUSED_EXIT_WITHDRAWAL_QUEUE)
    nonReentrant
```

`MIN_WITHDRAWAL_DELAY_BLOCKS` after queueing, a staker can complete a `Withdrawal` by calling this method. The staker can elect to receive _either_ tokens OR shares depending on the value of the `receiveAsTokens` parameter. 

Before processing a withdrawal, this method will calculate the slashing factor at the withdrawal's completion block (`withdrawal.startBlock + MIN_WITHDRAWAL_DELAY_BLOCKS`), according to the operator that was delegated to when the withdrawal was queued (`withdrawal.delegatedTo`). This slashing factor is used to determine if any additional slashing occurred while the withdrawal was in the queue. If so, this slashing is applied now.

For each `Withdrawal`, `withdrawal.scaledShares` are converted into _withdrawable shares_, accounting for any slashing that occurred during the withdrawal period (See [Shares Accounting - Complete Withdrawal](./accounting/SharesAccounting.md#complete-withdrawal)).

If the staker chooses to receive the withdrawal _as tokens_, the withdrawable shares are converted to tokens via the corresponding share manager (`EigenPodManager`/`StrategyManager`), and sent to the caller.

If the staker chooses to receive the withdrawal _as shares_, the withdrawable shares are credited to the staker via the corresponding share manager (`EigenPodManager`/`StrategyManager`). Additionally, if the caller is delegated to an operator, the new slashing factor for the given `(staker, operator, strategy)` determines how many shares are awarded to the operator (and how the staker's deposit scaling factor is updated) (See [Slashing Factors and Scaling Shares](#slashing-factors-and-scaling-shares)). In receiving the withdrawal as shares, this amount is credited as deposit shares for the staker. Due to known rounding error, the amount of withdrawable shares after completing the withdrawal may be slightly less than what was originally withdrawable.

**Note:** if the staker (i) receives the withdrawal as shares, (ii) has `MAX_STAKER_STRATEGY_LIST_LENGTH` unique deposit strategies in the `StrategyManager`, and (iii) is withdrawing to a `StrategyManager` strategy in which they do not currently have shares, this will revert. The staker cannot withdraw such that their `stakerStrategyList` length exceeds the maximum; this withdrawal will have to be completed as tokens instead.

**Note:** if the staker receives a `beaconChainETHStrategy` withdrawal as tokens, the staker's `EigenPod` MUST have sufficient `withdrawableExecutionLayerGwei` to honor the withdrawal.

*Effects*:
* The hash of the `Withdrawal` is removed from the pending withdrawals
* The hash of the `Withdrawal` is removed from the enumerable set of staker queued withdrawals
* The `Withdrawal` struct is removed from the queued withdrawals 
* If `receiveAsTokens`:
    * See [`StrategyManager.withdrawSharesAsTokens`](./StrategyManager.md#withdrawsharesastokens)
    * See [`EigenPodManager.withdrawSharesAsTokens`](./EigenPodManager.md#withdrawsharesastokens)
* If `!receiveAsTokens`:
    * Withdrawable shares are awarded to the caller and delegated to the caller's current operator if applicable
    * See [`StrategyManager.addShares`](./StrategyManager.md#addshares)
    * See [`EigenPodManager.addShares`](./EigenPodManager.md#addshares)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_EXIT_WITHDRAWAL_QUEUE`
* `tokens.length` must equal `withdrawal.strategies.length`
* Caller MUST be the `staker/withdrawer` specified in the `Withdrawal`
* At least `MIN_WITHDRAWAL_DELAY_BLOCKS` MUST have passed before `completeQueuedWithdrawal` is called
* The hash of the passed-in `Withdrawal` MUST correspond to a pending withdrawal
* If `receiveAsTokens`:
    * The caller MUST pass in the underlying `IERC20[] tokens` being withdrawn in the appropriate order according to the strategies in the `Withdrawal`.
    * See [`StrategyManager.withdrawSharesAsTokens`](./StrategyManager.md#withdrawsharesastokens)
    * See [`EigenPodManager.withdrawSharesAsTokens`](./EigenPodManager.md#withdrawsharesastokens)
* If `!receiveAsTokens`:
    * See [`StrategyManager.addShares`](./StrategyManager.md#addshares)
    * See [`EigenPodManager.addShares`](./EigenPodManager.md#addshares)

#### `completeQueuedWithdrawals`

```solidity
/**
 * @notice Used to complete multiple queued withdrawals
 * @param withdrawals Array of Withdrawals to complete. See `completeQueuedWithdrawal` for the usage of a single Withdrawal.
 * @param tokens Array of tokens for each Withdrawal. See `completeQueuedWithdrawal` for the usage of a single array.
 * @param receiveAsTokens Whether or not to complete each withdrawal as tokens. See `completeQueuedWithdrawal` for the usage of a single boolean.
 * @dev See `completeQueuedWithdrawal` for relevant dev tags
 */
function completeQueuedWithdrawals(
    Withdrawal[] calldata withdrawals,
    IERC20[][] calldata tokens,
    bool[] calldata receiveAsTokens
) 
    external 
    onlyWhenNotPaused(PAUSED_EXIT_WITHDRAWAL_QUEUE) 
    nonReentrant
```

This method is the plural version of [`completeQueuedWithdrawal`](#completequeuedwithdrawal).

---

## Slashing and Accounting

These methods are all called by other system contracts: the `AllocationManager` calls `slashOperatorShares` during slashing, and the `EigenPodManager/StrategyManager` call `increaseDelegatedShares/decreaseDelegatedShares` when stakers' deposit shares (or when beacon chain balance decreases occur).

**Methods**:
* [`DelegationManager.slashOperatorShares`](#slashoperatorshares)
* [`DelegationManager.increaseDelegatedShares`](#increasedelegatedshares)
* [`DelegationManager.decreaseDelegatedShares`](#decreasedelegatedshares)

#### `slashOperatorShares`

```solidity
/**
 * @notice Decreases the operators shares in storage after a slash and increases the burnable shares by calling
 * into either the StrategyManager or EigenPodManager (if the strategy is beaconChainETH).
 * @param operator The operator to decrease shares for
 * @param strategy The strategy to decrease shares for
 * @param prevMaxMagnitude the previous maxMagnitude of the operator
 * @param newMaxMagnitude the new maxMagnitude of the operator
 * @dev Callable only by the AllocationManager
 * @dev Note: Assumes `prevMaxMagnitude <= newMaxMagnitude`. This invariant is maintained in
 * the AllocationManager.
 */
function slashOperatorShares(
    address operator,
    IStrategy strategy,
    uint64 prevMaxMagnitude,
    uint64 newMaxMagnitude
) 
    external
    onlyAllocationManager
    nonReentrant
```

_See [Shares Accounting - Slashing](https://github.com/Layr-Labs/eigenlayer-contracts/blob/slashing-magnitudes/docs/core/accounting/SharesAccounting.md#slashing) for a description of the accounting in this method._

This method is called by the `AllocationManager` when processing an AVS's slash of an operator. Slashing occurs instantly, with this method directly reducing the operator's delegated shares proportional to the slash.

Additionally, any _slashable shares_ in the withdrawal queue are marked for burning according to the same slashing proportion (shares in the withdrawal queue remain slashable for `MIN_WITHDRAWAL_DELAY_BLOCKS`). For the slashed strategy, the corresponding share manager (`EigenPodManager/StrateyManager`) is called, increasing the burnable shares for that strategy.

**Note**: native ETH does not currently possess a burning mechanism, as this requires Pectra to be able to force exit validators. Currently, slashing for the `beaconChainETHStrategy` is realized by modifying the amount stakers are able to withdraw.

*Effects*:
* The `operator's` `operatorShares` are reduced for the given `strategy`, according to the proportion given by `prevMaxMagnitude` and `newMaxMagnitude`
* Any slashable shares in the withdrawal queue are marked for burning according to the same proportion
* See [`StrategyManager.increaseBurnableShares`](./StrategyManager.md#increaseBurnableShares)
* See [`EigenPodManager.increaseBurnableShares`](./EigenPodManager.md#increaseBurnableShares)


*Requirements*:
* The amount slashed from the operator must not result in underflow of their `operatorShares` for the given `strategy`

#### `increaseDelegatedShares`

```solidity
/**
 * @notice Called by a share manager when a staker's deposit share balance in a strategy increases.
 * This method delegates any new shares to an operator (if applicable), and updates the staker's 
 * deposit scaling factor regardless.
 * @param staker The address whose deposit shares have increased
 * @param strategy The strategy in which shares have been deposited
 * @param prevDepositShares The number of deposit shares the staker had in the strategy prior to the increase
 * @param addedShares The number of deposit shares added by the staker
 *
 * @dev Note that if either the staker's current operator has been slashed 100% for `strategy`, OR the
 * staker has been slashed 100% on the beacon chain such that the calculated slashing factor is 0, this
 * method WILL REVERT.
 */
function increaseDelegatedShares(
    address staker,
    IStrategy strategy,
    uint256 prevDepositShares,
    uint256 addedShares
) 
    external
    onlyStrategyManagerOrEigenPodManager
    nonReentrant
```

Called by either the `StrategyManager` or `EigenPodManager` when a staker's deposit shares for one or more strategies increase.

If the staker is delegated to an operator, the new deposit shares are directly added to that operator's `operatorShares`. Regardless of delegation status, the staker's deposit scaling factor is updated.

**Note** that if either the staker's current operator has been slashed 100% for `strategy`, OR the staker has been slashed 100% on the beacon chain such that the calculated slashing factor is 0, this method WILL REVERT. See [Shares Accounting - Fully Slashed](./accounting/SharesAccountingEdgeCases.md#fully-slashed-for-a-strategy) for details. This doesn't block delegation to an operator if the staker has 0 deposit shares for a strategy which has a slashing factor of 0, but any subsequent deposits that call `increaseDelegatedShares` will revert from the **Fully Slashed** edge case.

*Effects*:
* If the staker is delegated to an operator, `addedShares` are added to the operator's shares
* The staker's deposit scaling factor is updated

*Requirements*:
* Caller MUST be either the `StrategyManager` or `EigenPodManager`

#### `decreaseDelegatedShares`

```solidity
/**
 * @notice If the staker is delegated, decreases its operator's shares in response to
 * a decrease in balance in the beaconChainETHStrategy
 * @param staker the staker whose operator's balance will be decreased
 * @param curDepositShares the current deposit shares held by the staker
 * @param beaconChainSlashingFactorDecrease the amount that the staker's beaconChainSlashingFactor has decreased by
 * @dev Note: `beaconChainSlashingFactorDecrease` are assumed to ALWAYS be < 1 WAD.
 * These invariants are maintained in the EigenPodManager.
 */
function decreaseDelegatedShares(
    address staker,
    uint256 curDepositShares,
    uint64 beaconChainSlashingFactorDecrease
) 
    external
    onlyEigenPodManager
    nonReentrant
```

Called by the `EigenPodManager` when a staker's shares decrease due to a checkpointed balance decrease on the beacon chain. If the staker is delegated to an operator, the operator's shares are decreased in return. Otherwise, this method does nothing.

*Effects*: If the staker in question is delegated to an operator, the operator's shares for the `beaconChainETHStrategy` are decreased by the amount the staker's withdrawable shares have decreased by
* This method is a no-op if the staker is not delegated to an operator.

*Requirements*:
* Caller MUST be the `EigenPodManager`
````

## File: docs/core/EigenPod.md
````markdown
[eip-4788]: https://eips.ethereum.org/EIPS/eip-4788
[custom-types]: https://eth2book.info/capella/part3/config/types/#custom-types
[validator-container]: https://eth2book.info/capella/part3/containers/dependencies/#validator

## EigenPod

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`EigenPod.sol`](../../src/contracts/pods/EigenPod.sol) | Instanced, deployed per-user | Beacon proxy |

An `EigenPod` is deployed via the `EigenPodManager` by a Staker (referred to in this doc as the _Pod Owner_). `EigenPods` allow a Pod Owner to restake one or more beacon chain validators, earning shares which can be delegated to Operators to earn yield. When a Pod Owner begins running a validator on the beacon chain, they choose _withdrawal credentials_ for that validator. Withdrawal credentials are the ETH address to which:
* A validator's _principal_ is sent when the validator exits the beacon chain
* A validator's _consensus rewards_ are sent as the validator proposes/attests to blocks on the beacon chain

Additionally, when running validator node software, a validator is configured with a _fee recipient_. The fee recipient receives:
* _Execution layer rewards_ when the validator proposes a block
* _MEV rewards_ if the validator is running MEV-boost/other custom block proposer software

**An `EigenPod` may serve as EITHER/BOTH the withdrawal credentials OR the fee recipient for your validators.** In prior releases, it was only possible to use an `EigenPod` for withdrawal credentials. However, this is no longer the case!

---

The **primary goal** of the `EigenPod` system is to **ensure that shares are backed 1:1** with ETH that is _either already in the `EigenPod`, or will eventually flow through the `EigenPod`._ To support this goal, `EigenPods`: 
* serve as the withdrawal credentials for one or more beacon chain validators controlled by the Pod Owner
* validate beacon chain state proofs
* interpret these proofs to add or remove shares in the beacon chain ETH strategy

Because beacon chain proofs are processed asynchronously from the beacon chain itself, there is an inherent _lag_ between an event on the beacon chain and a corresponding share update in any affected `EigenPods`. Therefore, the **secondary goals** of the `EigenPod` system are to **minimize lag where possible** and to **ensure various timing windows cannot (i) create unbacked shares or (ii) prevent the withdrawal of existing shares.**

#### High-level Concepts

* [Restaking Beacon Chain ETH](#restaking-beacon-chain-eth)
* [Checkpointing Validators](#checkpointing-validators)
* [Staleness Proofs](#staleness-proofs)
* [Other Methods](#other-methods)

#### Important Definitions

**_Pod Owner_**: A Staker who has deployed an `EigenPod` is a _Pod Owner_. The terms are used interchangeably in this document.
* _Pod Owners_ can only deploy a single `EigenPod`, but can restake any number of beacon chain validators from the same `EigenPod`.
* _Pod Owners_ can delegate their `EigenPodManager` shares to Operators (via `DelegationManager`).
* These shares correspond to the amount of restaked beacon chain ETH held by the _Pod Owner_ via their `EigenPod`.

**_Proof Submitter_**: An address designated by the Pod Owner with permissions to call certain `EigenPod` methods. This role is provided to allow Pod Owners to manage their day-to-day `EigenPod` tasks via hot wallets, rather than the Pod Owner address which controls all funds. The Proof Submitter can call `verifyWithdrawalCredentials` and `startCheckpoint`. See [`setProofSubmitter` docs](#setproofsubmitter) for more details.

**_Active validator set_**: This term is used frequently in this document to describe the set of validators whose withdrawal credentials have been verified to be pointed at an `EigenPod`. The _active validator set_ is used to determine the number of proofs required to complete a checkpoint (see [Checkpointing Validators](#checkpointing-validators)).
* A validator enters the _active validator set_ when their withdrawal credentials are verified (see [`verifyWithdrawalCredentials`](#verifywithdrawalcredentials))
* A validator leaves the _active validator set_ when a checkpoint proof shows they have 0 balance (see [`verifyCheckpointProofs`](#verifycheckpointproofs))

In the implementation, the _active validator set_ is comprised of two state variables:
* `uint256 activeValidatorCount` 
    * incremented by 1 when a validator enters the _active validator set_
    * decremented by 1 when a validator leaves the _active validator set_
* `mapping(bytes32 => ValidatorInfo) _validatorPubkeyHashToInfo` (specifically, the `status` field)
    * `VALIDATOR_STATUS.INACTIVE -> VALIDATOR_STATUS.ACTIVE` when entering the _active validator set_
    * `VALIDATOR_STATUS.ACTIVE -> VALIDATOR_STATUS.WITHDRAWN` when leaving the _active validator set_

**_Checkpoint_**: A snapshot of `EigenPod` and beacon chain state used to update the _Pod Owner's_ shares based on a combination of beacon chain balance and native ETH balance. Checkpoints allow an `EigenPod` to account for validator exits, partial withdrawals of consensus rewards, or execution layer fees earned by their validators. Completing a checkpoint will account for these amounts in the `EigenPod`, enabling the _Pod Owner_ to compound their restaked shares or withdraw accumulated yield.

Only one _checkpoint_ can be active at a time in a given `EigenPod`. The pod's _current checkpoint_ is represented by the following data structure:

```solidity
struct Checkpoint {
    bytes32 beaconBlockRoot;  // proofs are verified against a beacon block root
    uint24 proofsRemaining;   // number of proofs remaining before the checkpoint is completed
    uint64 podBalanceGwei;    // native ETH that will be awarded shares when the checkpoint is completed
    int128 balanceDeltasGwei; // total change in beacon chain balance tracked across submitted proofs
}
```

Checkpoints are completed by submitting one beacon chain proof per validator in the pod's _active validator set_. See [Checkpointing Validators](#checkpointing-validators) for details.

---    

### Restaking Beacon Chain ETH

If a Pod Owner has validators whose withdrawal credentials are an `EigenPod`, the Pod Owner can use `verifyWithdrawalCredentials` to begin restaking ETH while it is still on the beacon chain. Once a validator's withdrawal credentials are verified:
* the Pod Owner receives delegatable shares via `EigenPodManager.podOwnerShares`
* the validator enters the pod's _active validator set_, and must be included in future checkpoint proofs (see [Checkpointing Validators](#checkpointing-validators))

_Methods:_
* [`verifyWithdrawalCredentials`](#verifywithdrawalcredentials)

#### `verifyWithdrawalCredentials`

```solidity
function verifyWithdrawalCredentials(
    uint64 beaconTimestamp,
    BeaconChainProofs.StateRootProof calldata stateRootProof,
    uint40[] calldata validatorIndices,
    bytes[] calldata validatorFieldsProofs,
    bytes32[][] calldata validatorFields
)
    external
    onlyOwnerOrProofSubmitter
    onlyWhenNotPaused(PAUSED_EIGENPODS_VERIFY_CREDENTIALS)

struct StateRootProof {
    bytes32 beaconStateRoot;
    bytes proof;
}
```

This method first verifies a beacon state root against a beacon block root returned by the [EIP-4788 oracle][eip-4788]. Then, it verifies one or more withdrawal credential proofs against the beacon state root. Finally, the Pod Owner is awarded shares according to the sum of the effective balance of each verified validator (via `EigenPodManager.recordBeaconChainETHBalanceUpdate`).

A withdrawal credential proof uses a validator's [`ValidatorIndex`][custom-types] and a merkle proof to prove the existence of a [`Validator` container][validator-container] at a given block. The beacon chain `Validator` container holds important information used in this method:
* `pubkey`: A BLS pubkey hash, used to uniquely identify the validator within the `EigenPod`
* `withdrawal_credentials`: Used to verify that the validator will withdraw its principal to this `EigenPod` if it exits the beacon chain
* `effective_balance`: The balance of the validator, updated once per epoch and capped at 32 ETH. Used to award shares to the Pod Owner
* `activation_epoch`: Initially set to `type(uint64).max`, this value is updated when a validator reaches a balance of at least 32 ETH, designating the validator is ready to become active on the beacon chain. **This method requires that a validator is either already active, or in the process of activating on the beacon chain.**
* `exit_epoch`: Initially set to `type(uint64).max`, this value is updated when a validator initiates exit from the beacon chain. **This method requires that a validator has not initiated an exit from the beacon chain.**
  * If a validator has been exited prior to calling `verifyWithdrawalCredentials`, their ETH can be accounted for, awarded shares, and/or withdrawn via the checkpoint system (see [Checkpointing Validators](#checkpointing-validators)).

_Note that it is not required to verify your validator's withdrawal credentials_, unless you want to receive shares for ETH on the beacon chain. You may choose to use your `EigenPod` without verifying withdrawal credentials; you will still be able to withdraw yield (or receive shares for yield) via the [checkpoint system](#checkpointing-validators).

*Effects*:
* For each set of unique verified withdrawal credentials:
    * `activeValidatorCount` is increased by 1
    * The validator's info is recorded in state (`_validatorPubkeyHashToInfo[pubkeyHash]`):
        * `validatorIndex` is recorded from the passed-in `validatorIndices`
        * `restakedBalanceGwei` is set to the validator's effective balance
        * `lastCheckpointedAt` is set to either the `lastCheckpointTimestamp` or `currentCheckpointTimestamp`
        * `VALIDATOR_STATUS` moves from `INACTIVE` to `ACTIVE`
* The Pod Owner is awarded shares according to the sum of effective balances proven. See [`EigenPodManager.recordBeaconChainETHBalanceUpdate`](./EigenPodManager.md#recordbeaconchainethbalanceupdate)

*Requirements*:
* Caller MUST be EITHER the Pod Owner or Proof Submitter
* Pause status MUST NOT be set: `PAUSED_EIGENPODS_VERIFY_CREDENTIALS`
* Input array lengths MUST be equal
* `beaconTimestamp`:
    * MUST be greater than `currentCheckpointTimestamp`
    * MUST be queryable via the [EIP-4788 oracle][eip-4788]. Generally, this means `beaconTimestamp` corresponds to a valid beacon block created within the last 8192 blocks (~27 hours).
* `stateRootProof` MUST verify a `beaconStateRoot` against the `beaconBlockRoot` returned from the EIP-4788 oracle
* For each validator:
    * The validator MUST NOT have been previously-verified (`VALIDATOR_STATUS` should be `INACTIVE`)
    * The validator's `activation_epoch` MUST NOT equal `type(uint64).max` (aka `FAR_FUTURE_EPOCH`)
    * The validator's `exit_epoch` MUST equal `type(uint64).max` (aka `FAR_FUTURE_EPOCH`)
    * The validator's `withdrawal_credentials` MUST be pointed to the `EigenPod`
    * `validatorFieldsProof` MUST be a valid merkle proof of the corresponding `validatorFields` under the `beaconStateRoot` at the given `validatorIndex`
* See [`EigenPodManager.recordBeaconChainETHBalanceUpdate`](./EigenPodManager.md#recordbeaconchainethbalanceupdate)

---

### Checkpointing Validators

Checkpoint proofs comprise the bulk of proofs submitted to an `EigenPod`. Completing a checkpoint means submitting _one checkpoint proof for each validator_ in the pod's _active validator set._

`EigenPods` use checkpoints to detect:
* when validators have exited from the beacon chain, leaving the pod's _active validator set_
* when the pod has accumulated fees / partial withdrawals from validators
* whether any validators on the beacon chain have increased/decreased in balance

When a checkpoint is completed, shares are updated accordingly for each of these events. OwnedShares can be withdrawn via the `DelegationManager` withdrawal queue (see [DelegationManager: Undelegating and Withdrawing](./DelegationManager.md#undelegating-and-withdrawing)), which means an `EigenPod's` checkpoint proofs also play an important role in allowing Pod Owners to exit funds from the system.

_Important Notes:_
* `EigenPods` can only have **one active checkpoint** at a given time, and once started, checkpoints **cannot be cancelled** (only completed)
* Checkpoint proofs are based entirely off of _current balance_ proofs. Even though partial/full withdrawals are processed via checkpoint proofs, this system does NOT use withdrawal proofs.

_Methods:_
* [`startCheckpoint`](#startcheckpoint)
* [`verifyCheckpointProofs`](#verifycheckpointproofs)

#### `startCheckpoint`

```solidity
function startCheckpoint(bool revertIfNoBalance)
    external
    onlyOwnerOrProofSubmitter() 
    onlyWhenNotPaused(PAUSED_START_CHECKPOINT) 
```

This method allows a Pod Owner (or Proof Submitter) to start a checkpoint, beginning the process of proving a pod's _active validator set_. `startCheckpoint` takes a snapshot of three things:
* `podBalanceGwei`: the `EigenPod's` native ETH balance, minus any balance already credited with shares through previous checkpoints
    * Note: if `revertIfNoBalance == true`, this method will revert if `podBalanceGwei == 0`. This is to allow a Pod Owner to avoid creating a checkpoint unintentionally.
* `activeValidatorCount`: the number of validators in the pod's _active validator set_, aka the number of validators with verified withdrawal credentials who have NOT been proven exited via a previous checkpoint
    * This becomes the checkpoint's `proofsRemaining`, or the number of proofs that need to be submitted to `verifyCheckpointProofs` to complete the checkpoint
* `beaconBlockRoot`: the beacon block root of the previous slot, fetched by querying the [EIP-4788 oracle][eip-4788] with the current `block.timestamp`
    * This is used as the single source of truth for all proofs submitted for this checkpoint

`startCheckpoint` plays a very important role in the security of the checkpoint process: it guarantees that _the pod's native ETH balance and any beacon balances proven in the checkpoint are 100% distinct_. That is: if a partial/full exit is processed in the block before `startCheckpoint` is called, then:
* The withdrawn ETH is already in the pod when `startCheckpoint` is called, and is factored into `podBalanceGwei`
* A proof of the validator's current balance against `beaconBlockRoot` will NOT include the withdrawn ETH

This guarantee means that, if we use the checkpoint to sum up the beacon chain balance of the pod's _active validator set_, **we can award guaranteed-backed shares** according to the sum of the pod's beacon chain balance and its native ETH balance.

*Effects*:
* Sets `currentCheckpointTimestamp` to `block.timestamp`
* Creates a new `Checkpoint`:
    * `beaconBlockRoot`: set to the current block's parent beacon block root, fetched by querying the [EIP-4788 oracle][eip-4788] using `block.timestamp` as input.
    * `proofsRemaining`: set to the current value of `activeValidatorCount` (note that this value MAY be 0)
    * `podBalanceGwei`: set to the pod's native ETH balance, minus any balance already accounted for in previous checkpoints
    * `balanceDeltasGwei`: set to 0 initially
* If `checkpoint.proofsRemaining == 0`, the new checkpoint is auto-completed:
    * `withdrawableRestakedExecutionLayerGwei` is increased by `checkpoint.podBalanceGwei`
    * `lastCheckpointTimestamp` is set to `currentCheckpointTimestamp`
    * `currentCheckpointTimestamp` and `_currentCheckpoint` are deleted
    * The Pod Owner's shares are updated (see [`EigenPodManager.recordBeaconChainETHBalanceUpdate`](./EigenPodManager.md#recordbeaconchainethbalanceupdate))

*Requirements*:
* Caller MUST be EITHER the Pod Owner or Proof Submitter
* Pause status MUST NOT be set: `PAUSED_START_CHECKPOINT`
* A checkpoint MUST NOT be active (`currentCheckpointTimestamp == 0`)
* The last checkpoint completed MUST NOT have been started in the current block (`lastCheckpointTimestamp != block.timestamp`)
* If `revertIfNoBalance == true`, the pod's native ETH balance MUST contain some nonzero value not already accounted for in the _Pod Owner's_ shares

#### `verifyCheckpointProofs`

```solidity
function verifyCheckpointProofs(
    BeaconChainProofs.BalanceContainerProof calldata balanceContainerProof,
    BeaconChainProofs.BalanceProof[] calldata proofs
)
    external 
    onlyWhenNotPaused(PAUSED_EIGENPODS_VERIFY_CHECKPOINT_PROOFS) 

struct BalanceContainerProof {
    bytes32 balanceContainerRoot;
    bytes proof;
}

struct BalanceProof {
    bytes32 pubkeyHash;
    bytes32 balanceRoot;
    bytes proof;
}
```

`verifyCheckpointProofs` is used to make progress on (or complete) the pod's current checkpoint. This method accepts one or more merkle proofs of validators' _current balances_ against a `balanceContainerRoot`. Additionally, a `balanceContainerProof` verifies this `balanceContainerRoot` against the _current checkpoint's_ `beaconBlockRoot`.

Proofs submitted to this method concern a validator's _current balance,_ NOT their _effective balance_. The _current balance_ is updated every slot, while _effective balances_ are updated roughly once per epoch. Current balances are stored in the [`BeaconState.balances` field](https://eth2book.info/capella/part3/containers/state/#beaconstate).

For each validator submitted via `proofs`:
* The validator's `status` should be `ACTIVE`. That is, its withdrawal credentials are verified (see [`verifyWithdrawalCredentials`](#verifywithdrawalcredentials)), and it has a nonzero balance as of the last time it was seen in a checkpoint proof.
* The validator's `lastCheckpointedAt` should be less than `currentCheckpointTimestamp`. This is to prevent a validator from counting towards a checkpoint's progression more than once.

If either of these two conditions is not met, _the proof will be skipped but execution will continue_. Execution continues without reverting to prevent a potential griefing vector where anyone could frontrun a batch of proofs, submit one proof from the batch, and cause the batch to revert.

Each valid proof submitted decreases the _current checkpoint's_ `proofsRemaining` by 1. If `proofsRemaining` hits 0 the checkpoint is automatically completed, updating the Pod Owner's shares accordingly.

*Effects*:
* For each validator successfully checkpointed:
    * The number of proofs remaining in the checkpoint is decreased (`checkpoint.proofsRemaining--`)
    * A balance delta is calculated using the validator's previous `restakedBalanceGwei`. This delta is added to `checkpoint.balanceDeltasGwei` to track the total beacon chain balance delta.
    * The validator's `restakedBalanceGwei` and `lastCheckpointedAt` fields are updated. Additionally, if the proof shows that the validator has a balance of 0, the validator's status is moved to `VALIDATOR_STATUS.WITHDRAWN` and the pod's `activeValidatorCount` is decreased.
* If the checkpoint's `proofsRemaining` drops to 0, the checkpoint is automatically completed:
    * `checkpoint.podBalanceGwei` is added to `withdrawableRestakedExecutionLayerGwei`, rendering it accounted for in future checkpoints
    * `lastCheckpointTimestamp` is set to `currentCheckpointTimestamp`, and both `_currentCheckpoint` and `currentCheckpointTimestamp` are deleted.
    * The Pod Owner's total share delta is calculated as the sum of `checkpoint.podBalanceGwei` and `checkpoint.balanceDeltasGwei`, and forwarded to the `EigenPodManager` (see [`EigenPodManager.recordBeaconChainETHBalanceUpdate`](./EigenPodManager.md#recordbeaconchainethbalanceupdate))

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_EIGENPODS_VERIFY_CHECKPOINT_PROOFS`
* A checkpoint MUST currently be active (`currentCheckpointTimestamp != 0`)
* `balanceContainerProof` MUST contain a valid merkle proof of the beacon chain's balances container against `_currentCheckpoint.beaconBlockRoot`
* Each `proof` in `proofs` MUST contain a valid merkle proof of the validator's `balanceRoot` against `balanceContainerProof.balanceContainerRoot`

---

### Staleness Proofs

Regular checkpointing of validators plays an important role in the health of the system, as a completed checkpoint ensures that the pod's shares and backing assets are up to date.

Typically, checkpoints can only be started by the Pod Owner (see [`startCheckpoint`](#startcheckpoint)). This is because completing a checkpoint with a lot of validators has the potential to be an expensive operation, so gating `startCheckpoint` to only be callable by the Pod Owner prevents a griefing vector where anyone can cheaply force the Pod Owner to perform a checkpoint.

In most cases, Pod Owners are incentivized to perform their own regular checkpoints, as completing checkpoints is the only way to access yield sent to the pod. However, if beacon chain validators are slashed, it's possible that a Pod Owner no longer has an incentive to start/complete a checkpoint. After all, they would be losing shares equal to the slashed amount. Unless they have enough unclaimed yield in the pod to make up for this, they only stand to lose by completing a checkpoint.

In this case, `verifyStaleBalance` can be used to allow a third party to start a checkpoint on the Pod Owner's behalf.

*Methods*:
* [`verifyStaleBalance`](#verifystalebalance)

#### `verifyStaleBalance`

```solidity
function verifyStaleBalance(
    uint64 beaconTimestamp,
    BeaconChainProofs.StateRootProof calldata stateRootProof,
    BeaconChainProofs.ValidatorProof calldata proof
)
    external
    onlyWhenNotPaused(PAUSED_START_CHECKPOINT) 
    onlyWhenNotPaused(PAUSED_VERIFY_STALE_BALANCE)
```

Allows anyone to prove that a validator in the pod's _active validator set_ was slashed on the beacon chain. A successful proof allows the caller to start a checkpoint. Note that if the pod currently has an active checkpoint, the existing checkpoint needs to be completed before `verifyStaleBalance` can start a checkpoint.

A valid proof has the following requirements:
* The `beaconTimestamp` MUST be newer than the timestamp the validator was last checkpointed at
* The validator in question MUST be in the _active validator set_ (have the status `VALIDATOR_STATUS.ACTIVE`)
* The proof MUST show that the validator has been slashed

If these requirements are met and the proofs are valid against a beacon block root given by `beaconTimestamp`, a checkpoint is started.

*Effects*:
* Sets `currentCheckpointTimestamp` to `block.timestamp`
* Creates a new `Checkpoint`:
    * `beaconBlockRoot`: set to the current block's parent beacon block root, fetched by querying the [EIP-4788 oracle][eip-4788] using `block.timestamp` as input.
    * `proofsRemaining`: set to the current value of `activeValidatorCount`
    * `podBalanceGwei`: set to the pod's native ETH balance, minus any balance already accounted for in previous checkpoints
    * `balanceDeltasGwei`: set to 0 initially

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_START_CHECKPOINT`
* Pause status MUST NOT be set: `PAUSED_VERIFY_STALE_BALANCE`
* A checkpoint MUST NOT be active (`currentCheckpointTimestamp == 0`)
* The last checkpoint completed MUST NOT be the current block
* For the validator given by `proof.validatorFields`:
    * `beaconTimestamp` MUST be greater than `validatorInfo.lastCheckpointedAt`
    * `validatorInfo.status` MUST be `VALIDATOR_STATUS.ACTIVE`
    * `proof.validatorFields` MUST show that the validator is slashed
* `stateRootProof` MUST verify a `beaconStateRoot` against the `beaconBlockRoot` returned from the EIP-4788 oracle
* The `ValidatorProof` MUST contain a valid merkle proof of the corresponding `validatorFields` under the `beaconStateRoot` at `validatorInfo.validatorIndex`

---

### Other Methods

Minor methods that do not fit well into other sections:
* [`setProofSubmitter`](#setproofsubmitter)
* [`stake`](#stake)
* [`withdrawRestakedBeaconChainETH`](#withdrawrestakedbeaconchaineth)
* [`recoverTokens`](#recovertokens)

#### `setProofSubmitter`

```solidity
function setProofSubmitter(address newProofSubmitter) external onlyEigenPodOwner
```

Allows the Pod Owner to update the Proof Submitter address for the `EigenPod`. The Proof Submitter can call `verifyWithdrawalCredentials` and `startCheckpoint` just like the Pod Owner. This is intended to allow the Pod Owner to create a hot wallet to manage calls to these methods.

If set, EITHER the Pod Owner OR Proof Submitter may call `verifyWithdrawalCredentials`/`startCheckpoint`.

The Pod Owner can call this with `newProofSubmitter == 0` to remove the current Proof Submitter. If there is no designated Proof Submitter, ONLY the Pod Owner can call `verifyWithdrawalCredentials`/`startCheckpoint`.

*Effects*:
* Updates `proofSubmitter` to `newProofSubmitter`

*Requirements*:
* Caller MUST be the Pod Owner

#### `stake`

```solidity
function stake(
    bytes calldata pubkey,
    bytes calldata signature,
    bytes32 depositDataRoot
)
    external 
    payable 
    onlyEigenPodManager
```

Handles the call to the beacon chain deposit contract. Only called via `EigenPodManager.stake`.

*Effects*:
* Deposits 32 ETH into the beacon chain deposit contract, and provides the pod's address as the deposit's withdrawal credentials

*Requirements*:
* Caller MUST be the `EigenPodManager`
* Call value MUST be 32 ETH
* Deposit contract `deposit` method MUST succeed given the provided `pubkey`, `signature`, and `depositDataRoot`

#### `withdrawRestakedBeaconChainETH`

```solidity
function withdrawRestakedBeaconChainETH(
    address recipient, 
    uint256 amountWei
)
    external 
    onlyEigenPodManager
```

The `EigenPodManager` calls this method when withdrawing a Pod Owner's shares as tokens (native ETH). The input `amountWei` is converted to Gwei and subtracted from `withdrawableRestakedExecutionLayerGwei`, which tracks native ETH balance that has been accounted for in a checkpoint (see [Checkpointing Validators](#checkpointing-validators)).

If the `EigenPod` does not have `amountWei` available to transfer, this method will revert

*Effects*:
* Decreases the pod's `withdrawableRestakedExecutionLayerGwei` by `amountWei / GWEI_TO_WEI`
* Sends `amountWei` ETH to `recipient`

*Requirements*:
* `amountWei / GWEI_TO_WEI` MUST NOT be greater than the proven `withdrawableRestakedExecutionLayerGwei`
* Pod MUST have at least `amountWei` ETH balance
* `recipient` MUST NOT revert when transferred `amountWei`
* `amountWei` MUST be a whole Gwei amount

#### `recoverTokens`

```solidity
function recoverTokens(
    IERC20[] memory tokenList,
    uint256[] memory amountsToWithdraw,
    address recipient
) 
    external 
    onlyEigenPodOwner 
    onlyWhenNotPaused(PAUSED_NON_PROOF_WITHDRAWALS)
```

Allows the Pod Owner to rescue ERC20 tokens accidentally sent to the `EigenPod`.

*Effects:*
* Calls `transfer` on each of the ERC20's in `tokenList`, sending the corresponding `amountsToWithdraw` to the `recipient`

*Requirements:*
* Caller MUST be the Pod Owner
* Pause status MUST NOT be set: `PAUSED_NON_PROOF_WITHDRAWALS`
* `tokenList` and `amountsToWithdraw` MUST have equal lengths
````

## File: docs/core/EigenPodManager.md
````markdown
## EigenPodManager

| File | Notes |
| -------- | -------- |
| [`EigenPodManager.sol`](../../src/contracts/pods/EigenPodManager.sol) | |
| [`EigenPodManagerStorage.sol`](../../src/contracts/pods/EigenPodManagerStorage.sol) | state variables |
| [`IEigenPodManager.sol`](../../src/contracts/interfaces/IEigenPodManager.sol) | interface |

Libraries and Mixins:
| File | Notes |
| -------- | -------- |
| [`EigenPodPausingConstants.sol`](../../src/contracts/pods/EigenPodPausingConstants.sol) | pause values for `EigenPod/EigenPodManager` methods |

## Prior Reading

* [Shares Accounting](./accounting/SharesAccounting.md), especially [_Handling Beacon Chain Balance Decreases in EigenPods_](./accounting/SharesAccounting.md#handling-beacon-chain-balance-decreases-in-eigenpods)

## Overview

The `EigenPodManager` manages the "beacon chain ETH strategy", a virtual strategy that stakers can hold delegatable shares in, similar to the strategies managed by the `StrategyManager`. Whereas the `StrategyManager's` strategy shares are backed by deposited ERC20 tokens, beacon chain ETH strategy shares are backed either by beacon chain validators or native ETH attributed to `EigenPods`.

The `EigenPodManager` allows any staker to deploy an `EigenPod`. `EigenPods` contains beacon chain state proof logic that enable a staker to point either/both a validator's _withdrawal credentials_ and/or _fee recipient_ addresses to their pod. After submitting beacon chain state proofs to their pod, the staker is awarded deposit shares in the beacon chain ETH strategy, which are then delegated to their operator in the `DelegationManager` (if applicable). For more details, see [`EigenPod.md`](./EigenPod.md).

As an `EigenPod` receives balance updates, they are forwarded to the `EigenPodManager`. Balance increases resulting from validator activity on the beacon chain or ETH received in the `EigenPod` will result in an increase in the staker's _deposit shares_ for the beacon chain ETH strategy.

Balance decreases resulting from validator inactivity or beacon chain slashing do NOT decrease the staker's deposit shares. Instead, they result in a _decrease_ to the staker's _beacon chain slashing factor_. Among other factors, the `DelegationManager` uses the beacon chain slashing factor when determining:
* How many of a staker's _deposit shares_ can actually be withdrawn
* How many of a staker's _deposit shares_ can be delegated to an operator

Note that the number of _withdrawable shares_ a staker's _deposit shares_ represent can be queried using `DelegationManager.getWithdrawableShares(staker, strategies)`.

The `EigenPodManager's` responsibilities can be broken down into the following concepts:
* [Depositing Into EigenLayer](#depositing-into-eigenlayer)
* [Withdrawal Processing](#withdrawal-processing)
* [Other Methods](#other-methods)

## Parameterization

* `beaconChainETHStrategy = 0xbeaC0eeEeeeeEEeEeEEEEeeEEeEeeeEeeEEBEaC0`
    * A virtual strategy used to represent beacon chain ETH internally. The `DelegationManager` uses this address to denote the beacon chain ETH strategy managed by the `EigenPodManager`. However, it does not correspond to an actual contract!
* `ethPOS = 0x00000000219ab540356cBB839Cbe05303d7705Fa`
    * The address of the beacon chain deposit contract
* `beaconProxyBytecode` (defined in `EigenPodManagerStorage.sol`)
    * `EigenPods` are deployed using a beacon proxy. This bytecode is a constant, containing the _creation bytecode_ calculated by compiling [OpenZeppelin's `BeaconProxy` contract at version 4.7.1](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.7.1/contracts/proxy/beacon/BeaconProxy.sol). Compilation used solc version `0.8.12`, optimization enabled, 200 runs. Example verified contract [here](https://etherscan.io/address/0xA6f93249580EC3F08016cD3d4154AADD70aC3C96#code).

---

## Depositing Into EigenLayer

Before a staker begins restaking beacon chain ETH, they need to deploy an `EigenPod`, stake, and start a beacon chain validator:
* [`createPod`](#createpod)
* [`stake`](#stake)

#### `createPod`

```solidity
/**
 * @notice Creates an EigenPod for the sender.
 * @dev Function will revert if the `msg.sender` already has an EigenPod.
 * @dev Returns EigenPod address
 */
function createPod()
    external
    onlyWhenNotPaused(PAUSED_NEW_EIGENPODS)
    returns (address)
```

Allows a staker to deploy an `EigenPod` instance, if they have not done so already.

Each staker can only deploy a single `EigenPod` instance, but a single `EigenPod` can serve as the fee recipient / withdrawal credentials for any number of beacon chain validators. Each `EigenPod` is created using Create2 and the beacon proxy pattern, using the staker's address as the Create2 salt.

As part of the `EigenPod` deployment process, the staker is made the Pod Owner, a permissioned role within the `EigenPod`.

*Effects*:
* Create2 deploys `EigenPodManager.beaconProxyBytecode`, appending the `eigenPodBeacon` address as a constructor argument. `bytes32(msg.sender)` is used as the salt.
    * `address eigenPodBeacon` is an [OpenZeppelin v4.7.1 `UpgradableBeacon`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.7.1/contracts/proxy/beacon/UpgradeableBeacon.sol), whose implementation address points to the current `EigenPod` implementation
    * `beaconProxyBytecode` is the constructor code for an [OpenZeppelin v4.7.1 `BeaconProxy`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.7.1/contracts/proxy/beacon/BeaconProxy.sol)
* `EigenPod.initialize(msg.sender)`: initializes the pod, setting the caller as the Pod Owner and activating restaking for any validators pointed at the pod.
* Maps the new pod to the Pod Owner (each address can only deploy a single `EigenPod`)

*Requirements*:
* Caller MUST NOT have deployed an `EigenPod` already
* Pause status MUST NOT be set: `PAUSED_NEW_EIGENPODS`

#### `stake`

```solidity
/**
 * @notice Stakes for a new beacon chain validator on the sender's EigenPod.
 * Also creates an EigenPod for the sender if they don't have one already.
 * @param pubkey The 48 bytes public key of the beacon chain validator.
 * @param signature The validator's signature of the deposit data.
 * @param depositDataRoot The root/hash of the deposit data for the validator's deposit.
 */
function stake(
    bytes calldata pubkey,
    bytes calldata signature,
    bytes32 depositDataRoot
)
    external
    payable
    onlyWhenNotPaused(PAUSED_NEW_EIGENPODS)
```

Allows a staker to deposit 32 ETH into the beacon chain deposit contract, providing the credentials for the staker's beacon chain validator. The `EigenPod.stake` method is called, which automatically calculates the correct withdrawal credentials for the pod and passes these to the deposit contract along with the 32 ETH.

*Effects*:
* Deploys and initializes an `EigenPod` on behalf of staker, if this has not been done already
* See [`EigenPod.stake`](./EigenPod.md#stake)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_NEW_EIGENPODS`
* See [`EigenPod.stake`](./EigenPod.md#stake)

---

## Withdrawal Processing

These methods are callable ONLY by the DelegationManager, and are used when processing undelegations and withdrawals.

**Concepts**:
* [Shares Accounting - Handling Beacon Chain Balance Decreases](./accounting/SharesAccounting.md#handling-beacon-chain-balance-decreases-in-eigenpods)
* [Deposit Shares and Beacon Chain Slashing](#deposit-shares-and-beacon-chain-slashing)

**Methods**:
* [`removeDepositShares`](#removeDepositShares)
* [`addShares`](#addshares)
* [`withdrawSharesAsTokens`](#withdrawsharesastokens)

#### Deposit Shares and Beacon Chain Slashing

The `EigenPodManager` tracks a staker's _deposit shares_ and _beacon chain slashing factor_ using the following state variables:

```solidity
/**
 * @notice mapping from pod owner to the deposit shares they have in the virtual beacon chain ETH strategy
 * 
 * @dev When an EigenPod registers a balance increase, deposit shares are increased. When registering a balance
 * decrease, however, deposit shares are NOT decreased. Instead, the pod owner's beacon chain slashing factor
 * is decreased proportional to the balance decrease. This impacts the number of shares that will be withdrawn
 * when the deposit shares are queued for withdrawal in the DelegationManager.
 * 
 * Note that prior to the slashing release, deposit shares were decreased when balance decreases occurred.
 * In certain cases, a combination of queueing a withdrawal plus registering a balance decrease could result
 * in a staker having negative deposit shares in this mapping. This negative value would be corrected when the
 * staker completes a withdrawal (as tokens or as shares).
 *
 * With the slashing release, negative shares are no longer possible. However, a staker can still have negative
 * shares if they met the conditions for them before the slashing release. If this is the case, that staker
 * should complete any outstanding queued withdrawal in the DelegationManager ("as shares"). This will correct
 * the negative share count and allow the staker to continue using their pod as normal.
 */
mapping(address podOwner => int256 shares) public podOwnerDepositShares;

/**
 * @notice The amount of beacon chain slashing experienced by a pod owner as a proportion of WAD
 * @param isSet whether the slashingFactor has ever been updated. Used to distinguish between
 * a value of "0" and an uninitialized value.
 * @param slashingFactor the proportion of the pod owner's balance that has been decreased due to
 * slashing or other beacon chain balance decreases.
 * @dev NOTE: if !isSet, `slashingFactor` should be treated as WAD. `slashingFactor` is monotonically
 * decreasing and can hit 0 if fully slashed.
 */
struct BeaconChainSlashingFactor {
    bool isSet;
    uint64 slashingFactor;
}

/// @notice Returns the slashing factor applied to the `staker` for the `beaconChainETHStrategy`
/// Note: this value starts at 1 WAD (1e18) for all stakers, and is updated when a staker's pod registers
/// a balance decrease.
mapping(address staker => BeaconChainSlashingFactor) internal _beaconChainSlashingFactor;
```

#### `removeDepositShares`

```solidity
/**
 * @notice Used by the DelegationManager to remove a pod owner's deposit shares when they enter the withdrawal queue.
 * Simply decreases the `podOwner`'s shares by `shares`, down to a minimum of zero.
 * @dev This function reverts if it would result in `podOwnerDepositShares[podOwner]` being less than zero, i.e. it is forbidden for this function to
 * result in the `podOwner` incurring a "share deficit". This behavior prevents a Staker from queuing a withdrawal which improperly removes excessive
 * shares from the operator to whom the staker is delegated.
 * @dev The delegation manager validates that the podOwner is not address(0)
 */
function removeDepositShares(
    address podOwner,
    IStrategy strategy,
    uint256 depositSharesToRemove
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when a staker queues a withdrawal (or undelegates, which also queues a withdrawal). The shares are removed while the withdrawal is in the queue, and when the queue completes, the shares will either be re-awarded or withdrawn as tokens (`addShares` and `withdrawSharesAsTokens`, respectively).

The staker's share balance is decreased by `depositSharesToRemove`.

This method is not allowed to cause the `staker's` balance to go negative. This prevents a staker from queueing a withdrawal for more shares than they have (or more shares than they delegated to an operator).

Note that the amount of deposit shares removed while in the withdrawal queue may not equal the amount credited when the withdrawal is completed. The staker may receive fewer if slashing occurred; see [`DelegationManager.md`](./DelegationManager.md) for details.

*Effects*:
* Removes `depositSharesToRemove` from `podOwner` share balance in `podOwnerDepositShares`
* Emits a `NewTotalShares` event

*Requirements*:
* Caller MUST be the `DelegationManager`
* `strategy` MUST be `beaconChainETHStrategy`
* `staker` MUST NOT be zero
* `depositSharesToRemove` MUST be less than `staker` share balance in `podOwnerDepositShares`

#### `addShares`

```solidity
/**
 * @notice Increases the `podOwner`'s shares by `shares`, paying off negative shares if needed.
 * Used by the DelegationManager to award a pod owner shares on exiting the withdrawal queue
 * @return existingDepositShares the pod owner's shares prior to any additions. Returns 0 if negative
 * @return addedShares the number of shares added to the staker's balance above 0. This means that if,
 * after shares are added, the staker's balance is non-positive, this will return 0.
 */
function addShares(
    address staker,
    IStrategy strategy,
    uint256 shares
)
    external
    onlyDelegationManager
    returns (uint256, uint256)
```

The `DelegationManager` calls this method when a queued withdrawal is completed and the withdrawer specifies that they want to receive the withdrawal "as shares" (rather than as the underlying tokens). A staker might want to do this in order to change their delegated operator without needing to fully exit their validators.

This method credits the input deposit shares to the staker. In most cases, the input `shares` equal the same shares originally removed when the withdrawal was queued. However, if the staker's operator was slashed (or the staker experienced beacon chain slashing), they may receive less. See [`DelegationManager.md`](./DelegationManager.md) for details.

If the staker has a share deficit (negative shares), the deficit is repaid out of the added `shares`. If the Pod Owner's positive share count increases, this change is returned to the `DelegationManager` to be delegated to the staker's operator (if they have one).

*Effects*:
* Increases `staker`'s deposit share balance in `podOwnerDepositShares` by `shares`

*Requirements*:
* Caller MUST be the `DelegationManager`
* `strategy` MUST be `beaconChainETHStrategy`
* `staker` MUST NOT be `address(0)`
* `shares` MUST NOT be negative when converted to an `int256`
* Emits `PodSharesUpdated` and `NewTotalShares` events

#### `withdrawSharesAsTokens`

```solidity
/**
 * @notice Used by the DelegationManager to complete a withdrawal, sending tokens to the pod owner
 * @dev Prioritizes decreasing the podOwner's share deficit, if they have one
 * @dev This function assumes that `removeShares` has already been called by the delegationManager, hence why
 *      we do not need to update the podOwnerDepositShares if `currentpodOwnerDepositShares` is positive
 */
function withdrawSharesAsTokens(
    address podOwner,
    IStrategy strategy,
    IERC20,
    uint256 shares
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when a queued withdrawal is completed and the withdrawer specifies that they want to receive the withdrawal as the tokens underlying the shares. This can be used to "fully exit" some amount of native ETH and send it to the pod owner (via `EigenPod.withdrawRestakedBeaconChainETH`).

Note that because this method entails withdrawing and sending native ETH, two conditions must be met for this method to succeed: (i) the ETH being withdrawn should already be in the `EigenPod`, and (ii) the amount being withdrawn should be accounted for in `EigenPod.withdrawableExecutionLayerGwei`. This latter condition can be achieved by completing an `EigenPod` checkpoint just prior to completing a queued `DelegationManager` withdrawal (see [EigenPod: Checkpointing Validators](./EigenPod.md#checkpointing-validators) for details).

Also note that, like `addShares`, if the original Pod Owner has a share deficit (negative shares), the deficit is repaid out of the withdrawn `shares` before any native ETH is withdrawn.

*Effects*:
* If `staker`'s share balance in `podOwnerDepositShares` is negative (i.e. the staker has a *deficit*):
    * If `shares` is greater than the current deficit:
        * Sets `staker` balance in `podOwnerDepositShares` to 0
        * Subtracts `|deficit|` from `shares` and converts remaining shares 1:1 to ETH (see [`EigenPod.withdrawRestakedBeaconChainETH`](./EigenPod.md#withdrawrestakedbeaconchaineth))
    * If `shares` is less than or equal to the current deficit:
        * Increases `staker` negative balance in `podOwnerDepositShares` by `shares`, bringing it closer to 0
        * Does *not* withdraw any shares
* Emits `PodSharesUpdated` and `NewTotalShares` events

*Requirements*:
* Caller MUST be the `DelegationManager`
* `strategy` MUST be `beaconChainETHStrategy`
* `staker` MUST NOT be `address(0)`
* `shares` MUST NOT be negative when converted to an `int256`
* See [`EigenPod.withdrawRestakedBeaconChainETH`](./EigenPod.md#withdrawrestakedbeaconchaineth)

---

## Other Methods

**Methods**:
* [`recordBeaconChainETHBalanceUpdate`](#recordbeaconchainethbalanceupdate)
* [`increaseBurnableShares`](#increaseburnableshares)

#### `recordBeaconChainETHBalanceUpdate`

```solidity
/**
 * @notice Adds any positive share delta to the pod owner's deposit shares, and delegates them to the pod
 * owner's operator (if applicable). A negative share delta does NOT impact the pod owner's deposit shares,
 * but will reduce their beacon chain slashing factor and delegated shares accordingly.
 * @param podOwner is the pod owner whose balance is being updated.
 * @param prevRestakedBalanceWei is the total amount restaked through the pod before the balance update, including
 * any amount currently in the withdrawal queue.
 * @param balanceDeltaWei is the amount the balance changed
 * @dev Callable only by the podOwner's EigenPod contract.
 * @dev Reverts if `sharesDelta` is not a whole Gwei amount
 */
function recordBeaconChainETHBalanceUpdate(
    address podOwner,
    uint256 prevRestakedBalanceWei,
    int256 balanceDeltaWei
)
    external
    onlyEigenPod(podOwner)
    nonReentrant
```

This method is called by an `EigenPod` to report a change in its pod owner's shares. It accepts a positive or negative `balanceDeltaWei`. A positive delta is added to the pod owner's _deposit shares,_ and delegated to their operator if applicable. A negative delta is NOT removed from the pod owner's deposit shares. Instead, the proportion of the balance decrease is used to update the pod owner's beacon chain slashing factor and decrease the number of shares delegated to their operator (if applicable). A zero delta results in no change. 

**Note** that prior to the slashing release, negative balance deltas subtracted from the pod owner's shares, and could, in certain cases, result in a negative share balance. As of the slashing release, negative balance deltas no longer subtract from share balances, updating the beacon chain slashing factor instead. 

If a staker has negative shares as of the slashing release, this method will REVERT, preventing any further balance updates from their pod while the negative share balance persists. In order to fix this and restore the use of their pod, the staker should complete any outstanding withdrawals in the `DelegationManager` "as shares," which will correct the share deficit.

*Effects*:
* If `balanceDeltaWei` is zero, do nothing
* If `balanceDeltaWei` is positive:
  * Adds `shares` to `podOwnerDepositShares` for `podOwner`
  * Emits `PodSharesUpdated` and `NewTotalShares` events
  * Calls [`DelegationManager.increaseDelegatedShares`](./DelegationManager.md#increasedelegatedshares)
* If `balanceDeltaWei` is negative:
  * Reduces slashing factor proportional to relative balance decrease
  * Emits `BeaconChainSlashingFactorDecreased` event
  * Calls [`DelegationManager.decreaseDelegatedShares`](./DelegationManager.md#decreasedelegatedshares)

*Requirements*:
* Caller MUST be the `EigenPod` associated with the passed-in `podOwner`
* `podOwner` MUST NOT be `address(0)`
* `balanceDeltaWei` MUST be a whole Gwei amount
* Legacy withdrawals MUST be complete (i.e. `currentDepositShares >= 0`)

#### `increaseBurnableShares`

```solidity
/**
 * @notice Increase the amount of burnable shares for a given Strategy. This is called by the DelegationManager
 * when an operator is slashed in EigenLayer.
 * @param strategy The strategy to burn shares in.
 * @param addedSharesToBurn The amount of added shares to burn.
 * @dev This function is only called by the DelegationManager when an operator is slashed.
 */
function increaseBurnableShares(
    IStrategy strategy, 
    uint256 addedSharesToBurn
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when an operator is slashed, calculating the number of slashable shares and marking them for burning here.

Unlike in the `StrategyManager`, there is no current mechanism to burn these shares, as burning requires the Pectra hard fork to be able to eject validators. This will be added in a future update.

*Effects*:
* Increases `burnableShares` for the beacon chain ETH strategy by `addedSharesToBurn`

*Requirements*:
* Can only be called by the `DelegationManager`
````

## File: docs/core/RewardsCoordinator.md
````markdown
## RewardsCoordinator

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`RewardsCoordinator.sol`](../../src/contracts/core/RewardsCoordinator.sol) | Singleton | Transparent proxy |


<!-- The primary functions of the `RewardsCoordinator` contract are to (i) accept ERC20 rewards from AVSs (Actively Validated Services) to their Operators and delegated Stakers for a given time range; (ii) enable the protocol to provide ERC20 tokens to all stakers over a specified time range; and (iii) allow stakers and operators to claim their accumulated earnings. -->

The `RewardsCoordinator` accepts ERC20s from AVSs alongside rewards submission requests made out to Operators who, during a specified time range, were registered to the AVS in the core [`AllocationManager`](./AllocationManager.md) contract.

There are two forms of rewards:
* Rewards v1, also known as rewards submissions.
* Rewards v2, also known as operator-directed rewards submissions. See the [ELIP](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-001.md) for additional context on this rewards type.

*Off-chain*, the trusted *rewards updater* calculates a rewards distribution over some rewards submission's time range, depending on the rewards type. For a **v1 rewards submission**, it is based on: (i) the relative stake weight of each Operator's Stakers and (ii) a default split given to the Operator. For a **v2 rewards submission**, it is based on: (i) an AVS's custom rewards logic, (ii) the per-operator splits.

*On-chain*, the rewards updater sends the `RewardsCoordinator` a merkle root of each earner's cumulative earnings. Earners provide merkle proofs to the `RewardsCoordinator` to claim rewards against these roots.

The typical user flow is as follows:
1. An AVS submits a *rewards submission*, either a `RewardsSubmission` (v1) or `OperatorDirectedRewardsSubmission` (v2), to the `RewardsCoordinator` contract, which specifies a time range (`startTimestamp` and `duration`) and `token`. The rewards submission also specifies the relative reward weights of strategies (i.e. "distribute 80% out to holders of X strategy, and 20% to holders of strategy Y").
   * Note that **v1 rewards** specify a total `amount`, whereas **v2 rewards** specify a per-operator reward (due to customizable rewards logic). **v2 rewards** also allow for adding a `description` of the rewards submission's purpose.
2. Off-chain, the rewards submissions are used to calculate reward distributions, which are periodically consolidated into a merkle tree.
3. The root of this tree (aka the `DistributionRoot`) is posted on-chain by the *rewards updater*. A `DistributionRoot` becomes active for claims after some globally-configured `activationDelay`.
4. Stakers and Operators (or their configured "claimers") can claim their accumulated earnings by providing a merkle proof against any previously-posted `DistributionRoot`.

This entire flow will repeat periodically as AVSs submit rewards submissions, `DistributionRoots` are submitted, and Stakers/Operators claim their accumulated earnings. Note that `DistributionRoots` contain *cumulative earnings*, meaning Stakers/Operators aren't required to claim against every root - simply claiming against the most recent root will claim anything not yet claimed.

**NOTE: Use caution when using reward tokens that do not strictly conform to ERC20 standards. Please DYOR if your token falls outside of ERC20 norms.** Specific things to look out for include (but are not limited to): exotic rebasing tokens, fee-on-transfer tokens, tokens that support reentrant behavior (like ERC-777), and other nonstandard ERC20 derivatives.

#### High-level Concepts

This document is organized according to the following themes (click each to be taken to the relevant section):
* [Submitting Rewards Requests](#submitting-rewards-requests)
* [Distributing and Claiming Rewards](#distributing-and-claiming-rewards)
* [System Configuration](#system-configuration)
* [Rewards Merkle Tree Structure](#rewards-merkle-tree-structure)
* [Off Chain Calculation](#off-chain-calculation)

#### Important state variables

* `DistributionRoot[] public distributionRoots`:
    * `distributionRoots` stores historic reward merkle tree roots submitted by the rewards updater. For each earner, the rewards merkle tree stores cumulative earnings per ERC20 reward token. For more details on merkle tree structure see [Rewards Merkle Tree Structure](#rewards-merkle-tree-structure) below.
* `mapping(address => address) public claimerFor`: earner => claimer
    * Stakers and Operators can designate a "claimer" who can claim rewards via on their behalf via `processClaim`. If a claimer is not set in `claimerFor`, the earner will have to call `processClaim` themselves.
    * Note that the claimer isn't necessarily the reward recipient, but they do have the authority to specify the recipient when calling `processClaim` on the earner's behalf.
* `mapping(address => mapping(IERC20 => uint256)) public cumulativeClaimed`: earner => token => total amount claimed to date
    * Mapping for earners(Stakers/Operators) to track their total claimed earnings per reward token. This mapping is used to calculate the difference between the cumulativeEarnings stored in the merkle tree and the previous total claimed amount. This difference is then transferred to the specified destination address.
* `uint16 public defaultOperatorSplitBips`: *Used off-chain* by the rewards updater to calculate an Operator's split for a specific reward.
    * This is expected to be a flat 10% rate for the initial rewards release. Expressed in basis points, this is `1000`.
* `mapping(address => mapping(address => OperatorSplit)) internal _operatorAVSSplitBips`: operator => AVS => `OperatorSplit`
  * Operators specify their custom split for a given AVS for each `OperatorDirectedRewardsSubmission`, where Stakers receive a relative proportion (by stake weight) of the remaining amount.
* `mapping(address => OperatorSplit) internal _operatorPISplitBips`: operator => `OperatorSplit`
  * Operators may also specify their custom split for [programmatic incentives](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/), where Stakers similarly receive a relative proportion (by stake weight) of the remaining amount.
* `mapping(address operator => mapping(bytes32 operatorSetKey => OperatorSplit split)) internal _operatorSetSplitBips`: operator => Operator Set Key => `OperatorSplit`
  * Operators may specify their custom split for a given Operator Set, which is more granular than an overarching AVS split

#### Helpful definitions

* **AVS** (Autonomous Verifiable Service) refers to the contract entity that is submitting rewards to the `RewardsCoordinator`.
  * This is assumed to be a customized `ServiceManager` contract of some kind that is interfacing with the EigenLayer protocol. See the `ServiceManagerBase` docs here: [`eigenlayer-middleware/docs/ServiceManagerBase.md`](https://github.com/Layr-Labs/eigenlayer-middleware/blob/dev/docs/ServiceManagerBase.md).
* An **Operator Set** refers to a collection of registered operators and strategies. See [ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md#operator-sets) for more details.
* An **Operator Set Key** describes the tuple of an AVS address and an ID that uniquely identifies an Operator Set. See the [AllocationManager](./AllocationManager.md#operator-sets) for details.
* A **rewards submission** includes, unless specified otherwise, both the v1 `RewardsSubmission` and the v2 `OperatorDirectedRewardsSubmission` types.
* The internal function `_checkClaim(RewardsMerkleClaim calldata claim, DistributionRoot memory root)` checks the merkle inclusion of a claim against a `DistributionRoot`
    * It reverts if any of the following are true:
        * mismatch input param lengths: tokenIndices, tokenTreeProofs, tokenLeaves
        * earner proof reverting from calling `_verifyEarnerClaimProof`
        * any of the token proofs reverting from calling `_verifyTokenClaimProof`

---

### Submitting Rewards Requests

Rewards are initially submitted to the contract to be distributed to Operators and Stakers by the following functions:

* [`RewardsCoordinator.createAVSRewardsSubmission`](#createavsrewardssubmission)
* [`RewardsCoordinator.createRewardsForAllSubmission`](#createrewardsforallsubmission)
* [`RewardsCoordinator.createRewardsForAllEarners`](#createrewardsforallearners)
* [`RewardsCoordinator.createOperatorDirectedAVSRewardsSubmission`](#createoperatordirectedavsrewardssubmission)
* [`RewardsCoordinator.createOperatorDirectedOperatorSetRewardsSubmission`](#createoperatordirectedoperatorsetrewardssubmission)

#### `createAVSRewardsSubmission`

```solidity
function createAVSRewardsSubmission(
    RewardsSubmission[] calldata RewardsSubmissions
)
    external
    onlyWhenNotPaused(PAUSED_AVS_REWARDS_SUBMISSION)
    nonReentrant
```

Called by an AVS to submit a list of `RewardsSubmission`s to be distributed across all registered Operators (and Stakers delegated to each Operator). A `RewardsSubmission` consists of the following fields:
* `IERC20 token`: the address of the ERC20 token being used for reward submission
* `uint256 amount`: amount of `token` to transfer to the `RewardsCoordinator`
* `uint32 startTimestamp`: the start of the submission time range
* `uint32 duration`: the duration of the submission time range, in seconds
* `StrategyAndMultiplier[] strategiesAndMultipliers`: an array of `StrategyAndMultiplier` structs that define a linear combination of EigenLayer strategies the AVS is considering eligible for rewards. Each `StrategyAndMultiplier` contains:
    * `IStrategy strategy`: address of the strategy against which a Staker/Operator's relative shares are weighted in order to determine their reward amount
    * `uint96 multiplier`: the relative weighting of the strategy in the linear combination. (Recommended use here is to use 1e18 as the base multiplier and adjust the relative weightings accordingly)

For each submitted `RewardsSubmission`, this method performs a `transferFrom` to transfer the specified reward `token` and `amount` from the caller to the `RewardsCoordinator`.

*Eligibility*:

In order to be eligible to claim a `createAVSRewardsSubmission` reward, the Operator should be registered for the AVS in the `AVSDirectory` during the time period over which the reward is being made (see docs for [`AVSDirectory.registerOperatorToAVS`](./AVSDirectory.md#registeroperatortoavs)). If an Operator is ineligible, any Stakers delegated to the Operator are also ineligible.

In addition, the AVS ServiceManager contract must also implement the interfaces `ServiceManager.getRestakeableStrategies` and `ServiceManager.getOperatorRestakedStrategies` to have their rewards be successfully distributed as these view functions are called offchain as part of the rewards distribution process. This is by default implemented in the `ServiceManagerBase` contract but is important to note if the base contract is not being inherited from.
See the `ServiceManagerBase` abstract contract here: [`ServiceManagerBase.sol`](https://github.com/Layr-Labs/eigenlayer-middleware/blob/dev/src/ServiceManagerBase.sol)

*Rewards Distribution*:

The rewards distribution amongst the AVS's Operators and delegated Stakers is determined offchain using the strategies and multipliers provided in the `RewardsSubmission` struct as well as the actual shares for those defined strategies over the `RewardsSubmission`'s time range. These shares are read from the [`EigenPodManager`](./EigenPodManager.md) (in the case of the Beacon Chain ETH strategy), or the [`StrategyManager`](./StrategyManager.md) for any other strategy. Note that Stakers' shares specifically are what determines rewards distribution; Operators earn based on a combination of their own deposited shares and a configured `defaultOperatorSplitBips`.

*Effects*:
* For each `RewardsSubmission` element
    * Transfers `amount` of `token` from the msg.sender (AVS) to the `RewardsCoordinator`
    * Hashes msg.sender(AVS), nonce, and `RewardsSubmission` struct to create a unique rewards hash and sets this value to `true` in the `isAVSRewardsSubmissionHash` mapping
    * Increments `submissionNonce[msg.sender]`
    * Emits a `AVSRewardsSubmissionCreated` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_AVS_REWARDS_SUBMISSION`
* Function call is not reentered
* For each `RewardsSubmission` element
    * Requirements from calling internal function `_validateRewardsSubmission()`
        * `rewardsSubmission.strategiesAndMultipliers.length > 0`
        * `rewardsSubmission.amount > 0`
        * `rewardsSubmission.amount <= MAX_REWARDS_AMOUNT`
        * `rewardsSubmission.duration <= MAX_REWARDS_DURATION`
        * `rewardsSubmission.duration % calculationIntervalSeconds == 0`
        * `rewardsSubmission.duration > 0`
        * `rewardsSubmission.startTimestamp % calculationIntervalSeconds == 0`
        * `block.timestamp - MAX_RETROACTIVE_LENGTH <= rewardsSubmission.startTimestamp`
        * `GENESIS_REWARDS_TIMESTAMP <= rewardsSubmission.startTimestamp`
        * `rewardsSubmission.startTimestamp <= block.timestamp + MAX_FUTURE_LENGTH`
        * Requirements for `rewardsSubmission.strategiesAndMultipliers`
            * Each `strategy` is whitelisted for deposit in the StrategyManager or is the `beaconChainETHStrategy`
            * `rewardsSubmission.strategiesAndMultipliers` is sorted by ascending strategy address to prevent duplicate strategies
    * `transferFrom` MUST succeed in transferring `amount` of `token` from `msg.sender` to the `RewardsCoordinator`

The text diagram below better visualizes a valid start timestamp for a `RewardsSubmission`
```
Sliding Window for valid RewardsSubmission startTimestamp

Scenario A: GENESIS_REWARDS_TIMESTAMP IS WITHIN RANGE
        <-----MAX_RETROACTIVE_LENGTH-----> t (block.timestamp) <---MAX_FUTURE_LENGTH--->
            <--------------------valid range for startTimestamp------------------------>
            ^
        GENESIS_REWARDS_TIMESTAMP


Scenario B: GENESIS_REWARDS_TIMESTAMP IS OUT OF RANGE
        <-----MAX_RETROACTIVE_LENGTH-----> t (block.timestamp) <---MAX_FUTURE_LENGTH--->
        <------------------------valid range for startTimestamp------------------------>
    ^
GENESIS_REWARDS_TIMESTAMP
```

#### `createRewardsForAllSubmission`

```solidity
function createRewardsForAllSubmission(
    RewardsSubmission[] calldata RewardsSubmissions
)
    external
    onlyWhenNotPaused(PAUSED_REWARDS_FOR_ALL_SUBMISSION)
    onlyRewardsForAllSubmitter
    nonReentrant
```

This method is identical in function to [`createAVSRewardsSubmission`](#createavsrewardssubmission) above, except:
* It can only be called by a whitelisted "rewards for all submitter"
* ALL Stakers are eligible for rewards, instead of those specifically registered for a given AVS

*Effects*:
* See [`createAVSRewardsSubmission`](#createavsrewardssubmission) above. The only differences are that:
    * Each rewards submission hash is stored in the `isRewardsSubmissionForAllHash` mapping
    * A `RewardsSubmissionForAllCreated` event is emitted

*Requirements*:
* See [`createAVSRewardsSubmission`](#createavsrewardssubmission) above. The only difference is that each calculated rewards submission hash MUST NOT already exist in the `isRewardsSubmissionForAllHash` mapping.

#### `createRewardsForAllEarners`

```solidity
function createRewardsForAllEarners(
    RewardsSubmission[] calldata RewardsSubmissions
)
    external
    onlyWhenNotPaused(PAUSED_REWARDS_FOR_ALL_SUBMISSION)
    onlyRewardsForAllSubmitter
    nonReentrant
```

This method is identical in function to [`createAVSRewardsSubmission`](#createavsrewardssubmission) above, except:
* It can only be called by a whitelisted "rewards for all submitter"
* Only operators who have opted into at least one AVS and the operator's delegated stakers are eligible for rewards

*Effects*:
* See [`createAVSRewardsSubmission`](#createavsrewardssubmission) above. The only differences are that:
    * Each rewards submission hash is stored in the `isRewardsSubmissionForAllEarnersHash` mapping
    * Emits a `RewardsSubmissionForAllEarnersCreated` event

*Requirements*:
* See [`createAVSRewardsSubmission`](#createavsrewardssubmission) above. The only difference is that each calculated rewards submission hash MUST NOT already exist in the `isRewardsSubmissionForAllEarnersHash` mapping.

#### `createOperatorDirectedAVSRewardsSubmission`

```solidity
function createOperatorDirectedAVSRewardsSubmission(
    address avs,
    OperatorDirectedRewardsSubmission[] calldata operatorDirectedRewardsSubmissions
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_DIRECTED_AVS_REWARDS_SUBMISSION)
    checkCanCall(avs)
    nonReentrant
```

AVS may make Rewards v2 submissions by calling `createOperatorDirectedAVSRewardsSubmission()` with any custom on-chain or off-chain logic to determine their rewards distribution strategy. This can be custom to the work performed by Operators during a certain period of time, can be a flat reward rate, or some other structure based on the AVS’s economic model. This would enable AVSs' flexibility in rewarding different operators for performance and other variables while maintaining the same easily calculable reward rate for stakers delegating to the same operator and strategy. The AVS can submit multiple performance-based rewards denominated in different tokens for even more flexibility.

*Effects*:
* For each `OperatorDirectedRewardsSubmission` element
  * Transfers `amount` of `token` from `msg.sender` to the `RewardsCoordinator`
  * Hashes `AVS`, `nonce`, and `OperatorDirectedRewardsSubmission` struct to create a unique rewards hash and sets this value to `true` in the `isOperatorDirectedAVSRewardsSubmissionHash` mapping
  * Increments `submissionNonce[avs]`
  * Emits an `OperatorDirectedAVSRewardsSubmissionCreated` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_OPERATOR_DIRECTED_AVS_REWARDS_SUBMISSION`
* Caller MUST be authorized, either as the AVS itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Function call is not reentered
* For each `OperatorDirectedRewardsSubmission` element:
  * Requirements from calling internal function `_validateOperatorDirectedRewardsSubmission()`
    * `operatorDirectedRewardsSubmission.strategiesAndMultipliers.length > 0`
    * `operatorDirectedRewardsSubmission.duration <= MAX_REWARDS_DURATION`
    * `operatorDirectedRewardsSubmission.duration % calculationIntervalSeconds == 0`
    * `operatorDirectedRewardsSubmission.duration > 0`
    * `operatorDirectedRewardsSubmission.startTimestamp % calculationIntervalSeconds == 0`
    * `block.timestamp - MAX_RETROACTIVE_LENGTH <= operatorDirectedRewardsSubmission.startTimestamp`
    * `GENESIS_REWARDS_TIMESTAMP <= operatorDirectedRewardsSubmission.startTimestamp`
    * For each `operatorDirectedRewardsSubmission.strategiesAndMultipliers` element:
      * Each `strategy` is whitelisted for deposit in the StrategyManager or is the `beaconChainETHStrategy`
      * `rewardsSubmission.strategiesAndMultipliers` is sorted by ascending strategy address to prevent duplicate strategies
    * `operatorDirectedRewardsSubmission.operatorRewards.length > 0`
    * For each `operatorDirectedRewardsSubmission.operatorRewards` element:
      * `operatorReward.operator != address(0)`
      * `currOperatorAddress < operatorReward.operator`
      * `operatorReward.amount > 0`
    * `totalAmount <= MAX_REWARDS_AMOUNT`, where `totalAmount` is the sum of every `operatorReward.amount`
    * `operatorDirectedRewardsSubmission.startTimestamp + operatorDirectedRewardsSubmission.duration < block.timestamp`, enforcing strictly retoractive rewards submissions
  * `transferFrom` MUST succeed in transferring `amount` of `token` from `msg.sender` to the `RewardsCoordinator`

#### `createOperatorDirectedOperatorSetRewardsSubmission`

```solidity
function createOperatorDirectedOperatorSetRewardsSubmission(
    OperatorSet calldata operatorSet,
    OperatorDirectedRewardsSubmission[] calldata operatorDirectedRewardsSubmissions
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_DIRECTED_OPERATOR_SET_REWARDS_SUBMISSION)
    checkCanCall(operatorSet.avs)
    nonReentrant
```

This function allows AVSs to make rewards submissions to specific operator sets, allowing for more granularly targeted rewards based on tasks assigned to a specific operator set, or any other custom AVS logic. Its functionality is almost identical to [`createOperatorDirectedAVSRewardsSubmission`](#createoperatordirectedavsrewardssubmission), save for some operator-set specific requirements, state variables, and events.

Note that an AVS must specify an operator set registered to the AVS; in other words, an operator set belonging to a different AVS, or an unregistered operator set, will cause this function to revert.

Also note that making this reward submission with a duration extending prior to the slashing release will result in those reward snapshots, prior to the slashing release, being refunded to the AVS (This is handled in the Sidecar rewards calculation logic).

*Effects*:
* See [`createOperatorDirectedAVSRewardsSubmission`](#createoperatordirectedavsrewardssubmission) above. The only differences are that:
  * Each rewards submission is stored in the `isOperatorDirectedOperatorSetRewardsSubmissionHash` mapping
  * An `OperatorDirectedOperatorSetRewardsSubmissionCreated` event is emitted

*Requirements*:
* See [`createOperatorDirectedAVSRewardsSubmission`](#createoperatordirectedavsrewardssubmission) above. The only differences are that:
  * `operatorSet` MUST be a [registered operator set](./AllocationManager.md#createoperatorsets) for the given AVS as according to `allocationManager.isOperatorSet()`
  * Pause status is instead: `PAUSED_OPERATOR_DIRECTED_OPERATOR_SET_REWARDS_SUBMISSION`

---

### Distributing and Claiming Rewards

The *rewards updater* calculates rewards distributions and submit claimable roots through the following function `submitRoot`. They can also disable the root if it has not yet been activated:

* [`RewardsCoordinator.submitRoot`](#submitroot)
* [`RewardsCoordinator.disableRoot`](#disableroot)

Earners configure and claim these rewards using the following functions:

* [`RewardsCoordinator.setClaimerFor`](#setclaimerfor)
* [`RewardsCoordinator.processClaim`](#processclaim)
* [`RewardsCoordinator.processClaims`](#processclaims)

#### `submitRoot`

```solidity
function submitRoot(
    bytes32 root,
    uint32 rewardsCalculationEndTimestamp
)
    external
    onlyWhenNotPaused(PAUSED_SUBMIT_DISABLE_ROOTS)
    onlyRewardsUpdater
```

Called only by the `rewardsUpdater` address to create a new `DistributionRoot` in the RewardsCoordinator. The `DistributionRoot` struct contains the following fields:
* `bytes32 root`: the merkle root of the rewards merkle tree
* `uint32 rewardsCalculationEndTimestamp`: the end of the rewards time range for which the `DistributionRoot` is being submitted
* `uint32 activatedAt`: the timestamp in seconds when the `DistributionRoot` is activated and can be claimed against

`submitRoot` pushes a new `DistributionRoot` to the `distributionRoots` array. The `DistributionRoot.activatedAt` timestamp is set to `block.timestamp + activationDelay()` to allow for a delay before claims can be processed. Once this delay has passed, the root can be used to verify merkle proofs of rewards made out to Stakers/Operators.

*Effects*:
* Pushes a new `DistributionRoot` to the `distributionRoots` array
* Sets `currRewardsCalculationEndTimestamp` to the param `rewardsCalculationEndTimestamp`
* Emits a `DistributionRootSubmitted` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_SUBMIT_DISABLE_ROOTS`
* `msg.sender` MUST be the `rewardsUpdater`
* `rewardsCalculationEndTimestamp > currRewardsCalculationEndTimestamp`
* `rewardsCalculationEndTimestamp < block.timestamp`

#### `disableRoot`

```solidity
function disableRoot(
    uint32 rootIndex
)
    external
    onlyWhenNotPaused(PAUSED_SUBMIT_DISABLE_ROOTS)
    onlyRewardsUpdater
```

Called only by the `rewardsUpdater` address to disable a pending `DistributionRoot` that has not yet been activated (activatedAt timestamp hasn't been reached yet) in the RewardsCoordinator. Once the activatedAt timestamp has been reached, a root can no longer be disabled and is deemed finalized and claimable against.
This is to add additional measures to prevent invalid roots posted to the contract, either from error or potentially malicious roots posted.

*Effects*:
* Sets the `disabled` field to True for the corresponding `DistributionRoot`
* `DistributionRoot` can no longer be claimed against in `processClaim`
* Emits a `DistributionRootDisabled` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_SUBMIT_DISABLE_ROOTS`
* `msg.sender` MUST be the `rewardsUpdater`
* `rootIndex < distributionRoots.length`
* `root.disabled == False`
* `block.timestamp < root.activatedAt`
* `rewardsCalculationEndTimestamp < block.timestamp`


#### `setClaimerFor`

```solidity
function setClaimerFor(address claimer) external
```

Called by an earner (Staker/Operator) to set a claimer address that can call `processClaim` on their behalf. If the claimer is not set (`claimerFor[earner] == address(0)`), the earner themselves can call `processClaim` directly.

*Effects*:
* Sets the `claimerFor[msg.sender]` to the input param `claimer`
* Emits a `ClaimerForSet` event

#### `processClaim`

```solidity
function processClaim(
    RewardsMerkleClaim calldata claim,
    address recipient
)
    external
    onlyWhenNotPaused(PAUSED_PROCESS_CLAIM)
    nonReentrant
```

Called an earner (Staker/Operator) to claim their accumulated earnings by providing a merkle proof against a posted `DistributionRoot`. If the earner has configured a claimer (via `setClaimerFor`), the claimer must call this method instead.

The `RewardsMerkleClaim` struct contains the following fields (see [Rewards Merkle Tree Structure](#rewards-merkle-tree-structure) for further details):
* `uint32 rootIndex`: the index of the `DistributionRoot` in `distributionRoots` to prove against
* `uint32 earnerIndex`: the index of the earner's account root in the merkle tree
* `bytes earnerTreeProof`: the proof of the earner's `EarnerTreeMerkleLeaf` against the `DistributionRoot`
* `EarnerTreeMerkleLeaf earnerLeaf`: the earner's address and token subtree root
    * `address earner`: the address of the earner
    * `bytes32 earnerTokenRoot`: the merkle root of the earner's token merkle tree
* `uint32[] tokenIndices`: the indices of the token leaves in the earner's subtree
* `bytes[] tokenTreeProofs`: the proofs of the token leaves against the earner's `earnerTokenRoot`
* `TokenTreeMerkleLeaf[] tokenLeaves`: the token leaves to be claimed:
    * `IERC20 token`: the ERC20 token to be claimed
    * `uint256 amount`: the amount of the ERC20 token to be claimed

`processClaim` is a simple wrapper function which calls out to the internal function `_processClaim`, which holds all of the necessary logic.

`_processClaim` will first call `_checkClaim` to verify the merkle proofs against the `DistributionRoot` at the specified `rootIndex`. This is done by first performing a merkle proof verification of the earner's `EarnerTreeMerkleLeaf` against the `DistributionRoot` and then for each tokenIndex, verifying each token leaf against the earner's `earnerTokenRoot`.

The caller must be the set claimer address in the `claimerFor` mapping or the earner themselves if the claimer is not set.

After the claim is verified, for each token leaf, the difference between the cumulative earnings in the merkle tree and the previous total claimed amount last stored in the contract is calculated and transferred from the `RewardsCoordinator` contract to the address `recipient`.

*Effects*:
* For each `claim.tokenLeaves`:
    * Calculates `uint claimAmount = tokenLeaf.cumulativeEarnings - cumulativeClaimed[earner][tokenLeaf.token]`
        * Transfers `claimAmount` of `tokenLeaf.token` to the specified `recipient`
    * Updates the `cumulativeClaimed` mapping for the earner and token
    * Emits a `RewardsClaimed` event

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_PROCESS_CLAIM`
* The `claim` must have valid proofs against a valid `DistributionRoot`:
    * For the `DistributionRoot` given by `claim.rootIndex`, the root MUST be active (`block.timestamp >= root.activatedAt`)
    * `claim.tokenIndices` MUST equal the lengths of `claim.TokenTreeProofs` AND `claim.tokenLeaves`
    * `claim.earnerTreeProof` MUST validate `claim.earnerLeaf` against the `DistributionRoot`
    * For each `claim.tokenIndices[i]`:
        * `claim.tokenTreeProofs[i]` MUST validate `claim.tokenLeaves[i]` against `claim.earnerLeaf.earnerTokenRoot`
* If the `earner` specified in `claim.earnerLeaf.earner` has a designated `claimer` in `claimerFor[earner]`, `msg.sender` MUST be the `claimer`
    * Otherwise, `msg.sender` MUST be the `earner`
* For each `TokenTreeMerkleLeaf`,
    * `tokenLeaf.cumulativeEarnings > cumulativeClaimed[earner][token]`: cumulativeEarnings must be gt than cumulativeClaimed. Trying to reclaim with the same proofs will revert because the claimed and earnings values will equal, breaking this requirement.
    * `tokenLeaf.token.safeTransfer(recipient, claimAmount)` MUST succeed

#### `processClaims`

```solidity
function processClaims(
        RewardsMerkleClaim[] calldata claims,
        address recipient
)
    external
    onlyWhenNotPaused(PAUSED_PROCESS_CLAIM)
    nonReentrant
```

`processClaims` is a simple wrapper function around `_processClaim`, calling it once for each claim provided.

*Effects*:
* For each `RewardsMerkleClaim` element: see [`processClaim`](#processclaim) above.

*Requirements*
* See [`processClaim`](#processclaim) above.

---

### System Configuration

* [`RewardsCoordinator.setActivationDelay`](#setactivationdelay)
* [`RewardsCoordinator.setDefaultOperatorSplit`](#setdefaultoperatorsplit)
* [`RewardsCoordinator.setRewardsUpdater`](#setrewardsupdater)
* [`RewardsCoordinator.setRewardsForAllSubmitter`](#setrewardsforallsubmitter)
* [`RewardsCoordinator.setOperatorAVSSplit`](#setoperatoravssplit)
* [`RewardsCoordinator.setOperatorPISplit`](#setoperatorpisplit)
* [`RewardsCoordinator.setOperatorSetSplit`](#setoperatorsetsplit)

#### `setActivationDelay`

```solidity
function setActivationDelay(uint32 _activationDelay) external onlyOwner
```

Allows the Owner to set the global `activationDelay`. The activation delay is the time in seconds after a `DistributionRoot` is submitted before it can be claimed against. This delay is to allow for interested parties to perform verification of the root before claiming begins.

*Effects*:
* Sets the global `activationDelay`
* Emits a `ActivationDelaySet` event

*Requirements*:
* Caller MUST be the Owner

#### `setDefaultOperatorSplit`

```solidity
function setDefaultOperatorSplit(uint16 split) external onlyOwner
```

Allows the Owner to set the default operator split in basis points.

This split is *used off-chain* when calculating Operator earnings for a given rewards distribution. Operator split is calculated as a percentage of the reward amount made out to each Operator. This split is deducted from the reward amount, after which the remainder is used to calculate rewards made to any Stakers delegated to the Operator.

*Effects*:
* Sets the `defaultOperatorSplitBips`
* Emits a `DefaultOperatorSplitBipsSet` event

*Requirements*:
* Caller MUST be the Owner

#### `setRewardsUpdater`

```solidity
function setRewardsUpdater(address _rewardsUpdater) external onlyOwner
```

Allows the Owner to set the `rewardsUpdater` address. The `rewardsUpdater` is the singleton address that can submit new `DistributionRoots` to the `RewardsCoordinator`. The `rewardsUpdater` is a trusted entity that performs the bulk of the calculations and merkle tree structuring described in this document.

*Effects*:
* Sets the global `rewardsUpdater` address
* Emits a `RewardsUpdaterSet` event

*Requirements*:
* Caller MUST be the Owner

#### `setRewardsForAllSubmitter`

```solidity
function setRewardsForAllSubmitter(address _submitter, bool _newValue) external onlyOwner
```

Allows the Owner to update the `_submitter's` permissions in the `isRewardsForAllSubmitter` mapping. This mapping is used to determine if a given address is a valid submitter for the `createRewardsForAllSubmission` method.

*Effects*:
* Sets the `isRewardsForAllSubmitter` mapping for the address `_submitter` to the bool `_newValue`
* Emits a `RewardsForAllSubmitterSet` event

*Requirements*:
* Caller MUST be the Owner

#### `setOperatorAVSsplit`

```solidity
function setOperatorAVSSplit(
    address operator,
    address avs,
    uint16 split
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_AVS_SPLIT)
    checkCanCall(operator)
```

An Operator may, for a given AVS, set a split which will determine what percent of their attributed rewards are allocated to themselves. The remaining percentage will go to Stakers.

The split will take effect after an `activationDelay` set by the contract owner. Note that once an operator initiates a split update, the `activationDelay` must pass before a new split update can be initiated.

*Effects*:
* Updates `operatorSplit.activatedAt` to `block.timestamp + activationDelay`
* If the operator has not initialized yet, sets  `operatorSplit.oldSplitBips` to `defaultOperatorSplitBips`. Else sets `operatorSplit.oldSplitBips` to the current `newSplitBips`
* Updates `operatorSplit.newSplitBips` to `split`
* Emits an `OperatorAVSSplitBipsSet` event

*Requirements*:
* Caller MUST be authorized, either as the operator itself or an admin/appointee (see [`PermissionController.md`](../permissions/PermissionController.md))
* Split MUST BE <= 10,000 bips (100%)
* Current `block.timestamp` MUST BE greater than current `operatorSplit.activatedAt`.
  * Any pending split must have already completed prior to setting a new split.

#### `setOperatorPIsplit`

```solidity
function setOperatorPISplit(
    address operator,
    uint16 split
)
    external
    onlyWhenNotPaused(PAUSED_OPERATOR_PI_SPLIT)
    checkCanCall(operator)
```

Similar to [`setOperatorAVSSplit`](#setoperatoravssplit), Operators may set their split for [programmatic incentives](https://www.blog.eigenlayer.xyz/introducing-programmatic-incentives-v1/), allowing them to specify what percent of these rewards they will maintain and what percent will go to their Stakers. The `allocationDelay` also applies here, as well as the inability to reinitiate a split update before the delay passes.

*Effects*:
* See [`setOperatorAVSSplit`](#setoperatoravssplit) above. The only differences are that:
  * The split is stored within `_operatorPISplitBips` instead of `_operatorAVSSplitBips`.
  * An `OperatorPISplitBipsSet` event is emitted

*Requirements*:
* See [`setOperatorAVSSplit`](#setoperatoravssplit) above. The only difference is that:
  * Pause status is instead: `PAUSED_OPERATOR_PI_SPLIT`

#### `setOperatorSetSplit`

```solidity
function setOperatorSetSplit(
    address operator,
    OperatorSet calldata operatorSet,
    uint16 split
) 
    external 
    onlyWhenNotPaused(PAUSED_OPERATOR_SET_SPLIT) 
    checkCanCall(operator)
```

*Effects*:
* See [`setOperatorAVSSplit`](#setoperatoravssplit) above. The only difference is that:
  * The split is stored within `_operatorSetSplitBips` instead of `_operatorAVSSplitBips`
  * An `OperatorSetSplitBipsSet` event is emitted 

*Requirements*:
* See [`setOperatorAVSSplit`](#setoperatoravssplit) above. The only differences are that:
  * `operatorSet` MUST be a [registered operator set](./AllocationManager.md#createoperatorsets) for the given AVS as according to `allocationManager.isOperatorSet()`
  * Pause status is instead: `PAUSED_OPERATOR_SET_SPLIT`

---

### Rewards Merkle Tree Structure

This merkle tree is used to verify the claims against a `DistributionRoot`.

When submitting a new `DistributionRoot`, the rewards updater consolidates all `RewardsSubmissions` submitted by AVSs since the previously submitted `DistributionRoot` into a merkle tree comprised of earners and their cumulative earnings for their respective reward tokens distributed.

When an earner or their designated claimer calls `processClaim`, they must provide a `RewardsMerkleClaim` struct that contains the necessary information to verify their claim against the latest `DistributionRoot`. The merkle proof verification is done in the internal `_checkClaim` helper function. This function verifies the merkle proof of the earner's `EarnerTreeMerkleLeaf` against the `DistributionRoot` and then for each tokenIndex, verifies each token leaf against the earner's `earnerTokenRoot`.

Claimers can selectively choose which token leaves to prove against and claim accumulated earnings. Each token reward claimed in a `processClaim` call will send tokens to the `recipient` address specified in the call.

The rewards merkle tree is structured in the diagram below:

![.](../images/RewardsCoordinator_Merkle_Tree.png)

---

### Off Chain Calculation

Rewards are calculated via an off-chain data pipeline. The pipeline takes snapshots of core contract state at the `SNAPSHOT_CADENCE`, currently set to once per day. It then combines these snapshots with any active rewards to calculate what the single daily reward of an earner is. Every `CALCULATION_INTERVAL_SECONDS` rewards are accumulated up to `lastRewardsTimestamp + CALCULATION_INTERVAL_SECONDS` and posted on-chain by the entity with the `rewardsUpdater` role.

`MAX_REWARDS_AMOUNT` is set to `1e38-1` given the precision bounds of the off-chain pipeline. An in-depth overview of the off-chain calculation can be found [here](https://github.com/Layr-Labs/sidecar/blob/master/docs/docs/sidecar/rewards/calculation.md)
````

## File: docs/core/StrategyManager.md
````markdown
## StrategyManager

| File | Notes |
| -------- | -------- |
| [`StrategyManager.sol`](../../src/contracts/core/StrategyManager.sol) | singleton share manager hooked into core |
| [`StrategyManagerStorage.sol`](../../src/contracts/core/StrategyManagerStorage.sol) | state variables |
| [`IStrategyManager.sol`](../../src/contracts/interfaces/IStrategyManager.sol) | interface |

StrategyFactory:

| File | Notes |
| -------- | -------- |
| [`StrategyFactory.sol`](../../src/contracts/core/StrategyFactory.sol) | allows deployment of `StrategyBase` for ERC20 tokens |
| [`StrategyBase.sol`](../../src/contracts/strategies/StrategyBase.sol) | deployed as a beacon proxy via `StrategyFactory` |

Individual strategies:

| File | Notes |
| -------- | -------- |
| [`StrategyBaseTVLLimits.sol`](../../src/contracts/strategies/StrategyBaseTVLLimits.sol) | Pre-StrategyFactory, deployed for certain LSTs. Each instances uses a transparent proxy pattern |
| [`EigenStrategy.sol`](../../src/contracts/strategies/EigenStrategy.sol) | One-off strategy deployed to support EIGEN/bEIGEN |

## Overview

The primary function of the `StrategyManager` is to handle _deposit share_ accounting for individual stakers as they deposit and withdraw supported tokens from their corresponding strategies. Note that the `StrategyManager` only handles _deposit shares_. When the word _shares_ is used in this document, it refers to _deposit shares,_ specifically. For an explanation of other share types, see [Shares Accounting - Terminology](./accounting/SharesAccounting.md#terminology).

The `StrategyManager` is responsible for (i) allowing stakers to deposit tokens into the corresponding strategy, (ii) allowing the `DelegationManager` to remove deposit shares when a staker queues a withdrawal, and (iii) allowing the `DelegationManager` to complete a withdrawal by either adding deposit shares back to the staker or withdrawing the deposit shares as tokens via the corresponding strategy.

Any ERC20-compatible token can be supported by deploying a `StrategyBase` instance from the `StrategyFactory`. Under the hood, the `StrategyFactory` uses the beacon proxy pattern and only allows a strategy to be deployed once per token. Deployed strategies are automatically whitelists for deposit in the `StrategyManager`. For details, see [Strategies](#strategies) below.

**Note**: for the EIGEN/bEIGEN token specifically, the `EigenStrategy` contract is used instead of `StrategyBase`. Additionally, the EIGEN/bEIGEN token are blacklisted within the `StrategyFactory` to prevent duplicate strategies from being deployed for these tokens.

**Note**: for certain LST tokens, the `StrategyBaseTVLLimits` contract is used instead of `StrategyBase`. These strategies were deployed before the `StrategyFactory` allowed arbitrary ERC20 strategies. Unlike strategies deployed through the `StrategyFactory`, these `StrategyBaseTVLLimits` contracts use the transparent proxy pattern. For all intents and purposes, these instances behave the same as `StrategyBase` instances deployed from the `StrategyFactory`. The "TVL Limits" capability of these instances has never been used. Any tokens using one of these instances are blacklisted in the `StrategyFactory` to prevent duplicate strategies from being deployed for these tokens.

The `StrategyManager's` responsibilities can be broken down into the following concepts:
* [Depositing Into Strategies](#depositing-into-strategies)
* [Withdrawal Processing](#withdrawal-processing)
* [Burning Slashed Shares](#burning-slashed-shares)
* [Strategies](#strategies)
* [System Configuration](#system-configuration)

## Parameterization

* `MAX_TOTAL_SHARES = 1e38 - 1`
    * The maximum total shares a single strategy can handle. This maximum prevents overflow in offchain services. Deposits that would increase a strategy's total shares beyond this value will revert.
* `MAX_STAKER_STRATEGY_LIST_LENGTH = 32`
    * The maximum number of unique `StrategyManager` strategies a staker can have deposits in. Any deposits that cause this number to be exceeded will revert.
* `DEFAULT_BURN_ADDRESS = 0x00000000000000000000000000000000000E16E4`
    * When slashed shares are burned, they are converted to tokens and transferred to this address, where they are unrecoverable.

---

## Depositing Into Strategies

The following methods are called by stakers as they (i) deposit ERC20 tokens into strategies to receive deposit shares:

* [`StrategyManager.depositIntoStrategy`](#depositintostrategy)
* [`StrategyManager.depositIntoStrategyWithSignature`](#depositintostrategywithsignature)

Withdrawals are performed through the `DelegationManager` (see [`DelegationManager.md`](./DelegationManager.md)).

#### `depositIntoStrategy`

```solidity
/**
 * @notice Deposits `amount` of `token` into the specified `strategy` and credits shares to the caller
 * @param strategy the strategy that handles `token`
 * @param token the token from which the `amount` will be transferred
 * @param amount the number of tokens to deposit
 * @return depositShares the number of deposit shares credited to the caller
 * @dev The caller must have previously approved this contract to transfer at least `amount` of `token` on their behalf.
 * 
 * WARNING: Be extremely cautious when depositing tokens that do not strictly adhere to ERC20 standards.
 * Tokens that diverge significantly from ERC20 norms can cause unexpected behavior in token balances for
 * that strategy, e.g. ERC-777 tokens allowing cross-contract reentrancy.
 */
function depositIntoStrategy(
    IStrategy strategy,
    IERC20 token,
    uint256 amount
)
    external
    onlyWhenNotPaused(PAUSED_DEPOSITS)
    nonReentrant
    returns (uint256 depositShares)
```

Allows a staker to deposit some `amount` of `token` into the specified `strategy` in exchange for deposit shares in that strategy. The underlying `strategy` must be whitelisted for deposits, meaning it has either been deployed via the `StrategyFactory`, or is an existing `StrategyBaseTVLLimits/EigenStrategy` instance. The `token` parameter should correspond to the strategy's supported token.

The number of shares received is calculated by the `strategy` using an internal exchange rate that depends on the previous number of tokens deposited.

After processing a deposit, the `StrategyManager` forwards the deposit information to the `DelegationManager`, which updates the staker's deposit scaling factor and delegates shares to the staker's operator (if applicable). See [`DelegationManager.increaseDelegatedShares`](./DelegationManager.md#increasedelegatedshares) for details.

*Effects*:
* `token.safeTransferFrom`: Transfers `amount` of `token` to `strategy` on behalf of the caller.
* `StrategyManager` awards the staker with the newly-created deposit shares
* See [`StrategyBase.deposit`](#strategybasedeposit)
* See [`DelegationManager.increaseDelegatedShares`](./DelegationManager.md#increasedelegatedshares)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_DEPOSITS`
* Caller MUST allow at least `amount` of `token` to be transferred by `StrategyManager` to the strategy
* `strategy` in question MUST be whitelisted for deposits.
* See [`StrategyBaseTVLLimits.deposit`](#strategybasedeposit)

#### `depositIntoStrategyWithSignature`

```solidity
/**
 * @notice Deposits `amount` of `token` into the specified `strategy` and credits shares to the `staker`
 * Note tokens are transferred from `msg.sender`, NOT from `staker`. This method allows the caller, using a
 * signature, to deposit their tokens to another staker's balance.
 * @param strategy the strategy that handles `token`
 * @param token the token from which the `amount` will be transferred
 * @param amount the number of tokens to transfer from the caller to the strategy
 * @param staker the staker that the deposited assets will be credited to
 * @param expiry the timestamp at which the signature expires
 * @param signature a valid ECDSA or EIP-1271 signature from `staker`
 * @return depositShares the number of deposit shares credited to `staker`
 * @dev The caller must have previously approved this contract to transfer at least `amount` of `token` on their behalf.
 *
 * WARNING: Be extremely cautious when depositing tokens that do not strictly adhere to ERC20 standards.
 * Tokens that diverge significantly from ERC20 norms can cause unexpected behavior in token balances for
 * that strategy, e.g. ERC-777 tokens allowing cross-contract reentrancy.
 */
function depositIntoStrategyWithSignature(
    IStrategy strategy,
    IERC20 token,
    uint256 amount,
    address staker,
    uint256 expiry,
    bytes memory signature
)
    external
    onlyWhenNotPaused(PAUSED_DEPOSITS)
    nonReentrant
    returns (uint256 depositShares)
```

This method works like `depositIntoStrategy()`, transferring tokens _from the caller_ to the `strategy` contract. Unlike `depositIntoStrategy`, the resulting deposit shares are credited to the passed-in `staker` address, which must sign off on this intent.

*Effects*: See `depositIntoStrategy` above. Additionally:
* The staker's nonce is incremented

*Requirements*: See `depositIntoStrategy` above. Additionally:
* Caller MUST provide a valid, unexpired signature over the correct fields

---

## Withdrawal Processing

These methods are callable ONLY by the `DelegationManager`, and are used when processing undelegations and withdrawals:
* [`StrategyManager.removeDepositShares`](#removedepositshares)
* [`StrategyManager.addShares`](#addshares)
* [`StrategyManager.withdrawSharesAsTokens`](#withdrawsharesastokens)

See [`DelegationManager.md`](./DelegationManager.md) for more context on how these methods are used.

#### `removeDepositShares`

```solidity
/// @notice Used by the DelegationManager to remove a Staker's shares from a particular strategy when entering the withdrawal queue
/// @dev strategy must be beaconChainETH when talking to the EigenPodManager
function removeDepositShares(
    address staker,
    IStrategy strategy,
    uint256 depositSharesToRemove
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when a staker queues a withdrawal (or undelegates, which also queues a withdrawal). The staker's deposit shares are removed while the withdrawal is in the queue, and when the withdrawal is completed, the staker can choose whether to be re-awarded the shares, or to convert and receive them as tokens (`addShares` and `withdrawSharesAsTokens`, respectively).

The staker's deposit share balance for the `strategy` is decreased by the removed `depositSharesToRemove`. If this causes the staker's share balance to hit zero, the `strategy` is removed from the staker's strategy list.

Note that the amount of deposit shares removed while in the withdrawal queue may not equal the amount credited when the withdrawal is completed. The staker may receive fewer if slashing occurred; see [`DelegationManager.md`](./DelegationManager.md) for details.

*Effects*:
* Decrease the staker's deposit share balance for the given `strategy` by the given `depositSharesToRemove`
    * If this causes the balance to hit zero, the `strategy` is removed from the staker's strategy list

*Requirements*:
* Caller MUST be the `DelegationManager`
* `depositSharesToRemove` parameter MUST NOT be zero
* `staker` MUST have at least `depositSharesToRemove` balance for the given `strategy`

#### `addShares`

```solidity
/// @notice Used by the DelegationManager to award a Staker some shares that have passed through the withdrawal queue
/// @dev strategy must be beaconChainETH when talking to the EigenPodManager
/// @return existingDepositShares the shares the staker had before any were added
/// @return addedShares the new shares added to the staker's balance
function addShares(
    address staker,
    IStrategy strategy,
    uint256 shares
)
    external
    onlyDelegationManager
    returns (uint256, uint256)
```

The `DelegationManager` calls this method when a queued withdrawal is completed and the withdrawer specifies that they want to receive the withdrawal "as shares" (rather than as the underlying tokens). 

This method credits the input deposit shares to the staker. In most cases, the input `shares` equal the same shares originally removed when the withdrawal was queued. However, if the staker's operator was slashed, they may receive less. See [`DelegationManager.md`](./DelegationManager.md) for details.

**Note** that if the staker has deposits in `MAX_STAKER_STRATEGY_LIST_LENGTH` unique strategies (and the input `strategy` is not among them), this method will revert. The staker can still choose to complete the withdrawal "as tokens" (See [`DelegationManager.completeQueuedWithdrawal`](./DelegationManager.md#completequeuedwithdrawal)).

*Effects*:
* Increase the `staker's` deposit share balance for the given `strategy` by `shares`
    * If the prior balance was zero, the `strategy` is added to the `staker's` strategy list
* Emit a `Deposit` event

*Requirements*:
* Caller MUST be the `DelegationManager`
* `staker` parameter MUST NOT be zero
* `shares` parameter MUST NOT be zero
* Length of `stakerStrategyList` for the `staker` MUST NOT exceed `MAX_STAKER_STRATEGY_LIST_LENGTH`

#### `withdrawSharesAsTokens`

```solidity
/// @notice Used by the DelegationManager to convert deposit shares to tokens and send them to a staker
/// @dev strategy must be beaconChainETH when talking to the EigenPodManager
/// @dev token is not validated when talking to the EigenPodManager
function withdrawSharesAsTokens(
    address staker,
    IStrategy strategy,
    IERC20 token,
    uint256 shares
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when a queued withdrawal is completed and the withdrawer specifies that they want to receive the withdrawal as the tokens underlying the shares. 

This method directs the `strategy` to convert the input deposit shares to tokens and send them to the `staker`. In most cases, the input `shares` equal the same shares originally removed when the withdrawal was queued. However, if the staker's operator was slashed, they may receive less. See [`DelegationManager.md`](./DelegationManager.md) for details.

*Effects*:
* Calls [`StrategyBase.withdraw`](#strategybasewithdraw)

*Requirements*:
* Caller MUST be the `DelegationManager`
* See [`StrategyBase.withdraw`](#strategybasewithdraw)

---

## Burning Slashed Shares

Slashed shares are marked as burnable, and anyone can call `burnShares` to transfer them to the default burn address. Burnable shares are stored in `burnableShares`, an [EnumerableMap](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v4.9.0/contracts/utils/structs/EnumerableMap.sol) with strategy contract addresses as keys and associated view functions. The following methods handle burning of slashed shares:
* [`StrategyManager.increaseBurnableShares`](#increaseburnableshares)
* [`StrategyManager.burnShares`](#burnshares)

#### `increaseBurnableShares`

```solidity
/**
 * @notice Increase the amount of burnable shares for a given Strategy. This is called by the DelegationManager
 * when an operator is slashed in EigenLayer.
 * @param strategy The strategy to burn shares in.
 * @param addedSharesToBurn The amount of added shares to burn.
 * @dev This function is only called by the DelegationManager when an operator is slashed.
 */
function increaseBurnableShares(
    IStrategy strategy, 
    uint256 addedSharesToBurn
)
    external
    onlyDelegationManager
```

The `DelegationManager` calls this method when an operator is slashed, calculating the number of slashable shares and marking them for burning here.

Anyone can then convert the shares to tokens and trigger a burn via `burnShares`. This asynchronous burning method was added to mitigate potential DoS vectors when slashing.

*Effects*:
* Increases `burnableShares` for the given `strategy` by `addedSharesToBurn`

*Requirements*:
* Can only be called by the `DelegationManager`

#### `burnShares`

```solidity
/**
 * @notice Burns Strategy shares for the given strategy by calling into the strategy to transfer
 * to the default burn address.
 * @param strategy The strategy to burn shares in.
 */
function burnShares(
    IStrategy strategy
)
    external
```

Anyone can call this method to burn slashed shares previously added by the `DelegationManager` via `increaseBurnableShares`. This method resets the strategy's burnable shares to 0, and directs the corresponding `strategy` to convert the shares to tokens and transfer them to `DEFAULT_BURN_ADDRESS`, rendering them unrecoverable.

The `strategy` is not called if the strategy had no burnable shares.

*Effects*:
* Resets the strategy's burnable shares to 0
* Calls `withdraw` on the `strategy`, withdrawing shares and sending a corresponding amount of tokens to the `DEFAULT_BURN_ADDRESS`

---

## Strategies

**Concepts**:
* [StrategyBase vs StrategyBaseTVLLimits](#strategybase-vs-strategybasetvllimits)

**Methods**:
* [`StrategyBase.deposit`](#strategybasedeposit)
* [`StrategyBase.withdraw`](#strategybasewithdraw)
* [`StrategyFactory.deployNewStrategy`](#strategyfactorydeploynewstrategy)
* [`StrategyFactory.blacklistTokens`](#strategyfactoryblacklisttokens)
* [`StrategyFactory.whitelistStrategies`](#strategyfactorywhiteliststrategies)
* [`StrategyFactory.removeStrategiesFromWhitelist`](#strategyfactoryremovestrategiesfromwhitelist)

#### `StrategyBase` vs `StrategyBaseTVLLimits`

Before the introduction of the `StrategyFactory`, strategies were manually deployed and whitelisted in the `StrategyManager`. These strategies used `StrategyBaseTVLLimits.sol`, and were deployed using the transparent proxy pattern. With the introduction of the `StrategyFactory`, anyone can create a depositable strategy for any ERC20 (provided it does not have a deployed strategy yet). The `StrategyFactory` deploys beacon proxies, each of which points at a single implementation of `StrategyBase.sol`.

Though these are two different contracts, `StrategyBaseTVLLimits` inherits all its basic functionality from `StrategyBase`, and only implements a "TVL limits" capability on top of them. In short, this additional functionality checks, before each deposit, whether:
1. the deposit amount exceeds a configured `maxPerDeposit`
2. the total token balance after the deposit exceeds a configured `maxTotalDeposits`

To this date, however, these "TVL limits" capabilities have _never_ been used. The values for both of the variables mentioned above have been set to `type(uint).max` since deployment, and there is no plan to change these. Effectively, all instances of `StrategyBaseTVLLimits` behave identically to instances of `StrategyBase` - with the exception being that the former uses a transparent proxy, and the latter a beacon proxy.

#### `StrategyBase.deposit`

```solidity
/**
 * @notice Used to deposit tokens into this Strategy
 * @param token is the ERC20 token being deposited
 * @param amount is the amount of token being deposited
 * @dev This function is only callable by the strategyManager contract. It is invoked inside of the strategyManager's
 * `depositIntoStrategy` function, and individual share balances are recorded in the strategyManager as well.
 * @dev Note that the assumption is made that `amount` of `token` has already been transferred directly to this contract
 * (as performed in the StrategyManager's deposit functions). In particular, setting the `underlyingToken` of this contract
 * to be a fee-on-transfer token will break the assumption that the amount this contract *received* of the token is equal to
 * the amount that was input when the transfer was performed (i.e. the amount transferred 'out' of the depositor's balance).
 * @dev Note that any validation of `token` is done inside `_beforeDeposit`. This can be overridden if needed.
 * @return newShares is the number of new shares issued at the current exchange ratio.
 */
function deposit(
    IERC20 token,
    uint256 amount
)
    external
    onlyWhenNotPaused(PAUSED_DEPOSITS)
    onlyStrategyManager
    returns (uint256 newShares)
```

The `StrategyManager` calls this method when stakers deposit ERC20 tokens into a strategy. At the time this method is called, the tokens have already been transferred to the strategy. The role of this method is to (i) calculate the number of deposit shares the tokens represent according to the exchange rate, and (ii) add the new deposit shares to the strategy's recorded total shares.

The number of new shares created are returned to the `StrategyManager` to be added to the staker's strategy share balance.

*Effects*:
* `StrategyBaseTVLLimits.totalShares` is increased to account for the new shares created by the deposit

*Requirements*:
* Caller MUST be the `StrategyManager`
* Pause status MUST NOT be set: `PAUSED_DEPOSITS`
* The passed-in `token` MUST match the strategy's `underlyingToken`
* The token amount being deposited MUST NOT exceed the per-deposit cap
* When converted to shares via the strategy's exchange rate:
    * The `amount` of `token` deposited MUST represent at least 1 new share for the depositor
    * The new total shares awarded by the strategy MUST NOT exceed `MAX_TOTAL_SHARES`


#### `StrategyBase.withdraw`

```solidity
/**
 * @notice Used to withdraw tokens from this Strategy, to the `recipient`'s address
 * @param recipient is the address to receive the withdrawn funds
 * @param token is the ERC20 token being transferred out
 * @param amountShares is the amount of shares being withdrawn
 * @dev This function is only callable by the strategyManager contract. It is invoked inside of the strategyManager's
 * other functions, and individual share balances are recorded in the strategyManager as well.
 * @dev Note that any validation of `token` is done inside `_beforeWithdrawal`. This can be overridden if needed.
 */
function withdraw(
    address recipient,
    IERC20 token,
    uint256 amountShares
)
    external
    onlyWhenNotPaused(PAUSED_WITHDRAWALS)
    onlyStrategyManager
```

The `StrategyManager` calls this method to convert a number of deposit shares to tokens, and transfer them to a `recipient`. Typically, this method is invoked as part of the withdrawal completion flow (see [`DelegationManager.completeQueuedWithdrawal`](./DelegationManager.md#completequeuedwithdrawal)). However, this method may also be invoked during the share burning flow (see [`StrategyManager.burnShares`](#burnshares)).

This method converts the deposit shares back into tokens using the strategy's exchange rate. The strategy's total shares are decreased to reflect the withdrawal before transferring the tokens to the `recipient`.

*Effects*:
* `StrategyBaseTVLLimits.totalShares` is decreased to account for the shares being withdrawn
* `underlyingToken.safeTransfer` is called to transfer the tokens to the `recipient`

*Requirements*:
* Caller MUST be the `StrategyManager`
* Pause status MUST NOT be set: `PAUSED_WITHDRAWALS`
* The passed-in `token` MUST match the strategy's `underlyingToken`
* The `amountShares` being withdrawn MUST NOT exceed the `totalShares` in the strategy
* The tokens represented by `amountShares` MUST NOT exceed the strategy's token balance

#### `StrategyFactory.deployNewStrategy`

```solidity
/**
 * @notice Deploy a new StrategyBase contract for the ERC20 token, using a beacon proxy
 * @dev A strategy contract must not yet exist for the token.
 * @dev Immense caution is warranted for non-standard ERC20 tokens, particularly "reentrant" tokens
 * like those that conform to ERC777.
 */
function deployNewStrategy(IERC20 token)
    external
    onlyWhenNotPaused(PAUSED_NEW_STRATEGIES)
    returns (IStrategy newStrategy)
```

Allows anyone to deploy a new `StrategyBase` instance that supports deposits/withdrawals using the provided `token`. As part of calling this method, the `StrategyFactory` automatically whitelists the new strategy for deposits via the `StrategyManager`.

Note that the `StrategyFactory` only permits ONE strategy deployment per `token`. Once a `token` has an associated strategy deployed via this method, `deployNewStrategy` cannot be used to deploy a strategy for `token` again. Additionally, `deployNewStrategy` will reject any `token` placed onto the `StrategyFactory` blacklist. This feature was added to prevent the deployment of strategies that existed _before_ the `StrategyFactory` was created. For details, see [`StrategyFactory.blacklistTokens`](#strategyfactoryblacklisttokens).

**NOTE: Use caution when deploying strategies for tokens that do not strictly conform to ERC20 standards. Rebasing tokens similar to already-whitelisted LSTs should be supported, but please DYOR if your token falls outside of ERC20 norms.** Specific things to look out for include (but are not limited to): exotic rebasing tokens, tokens that support reentrant behavior (like ERC-777), and other nonstandard ERC20 derivatives.

*Effects*:
* Deploys a new `BeaconProxy` for the `token`, which references the current `StrategyBase` implementation
* Updates the `tokenStrategy` mapping for the `token`, preventing a second strategy deployment for the same token
* See `StrategyManager.addStrategiesToDepositWhitelist`

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_NEW_STRATEGIES`
* `token` MUST NOT be blacklisted within `StrategyFactory`
* `StrategyFactory` MUST NOT have been used to deploy a strategy for `token` already
* See `StrategyManager.addStrategiesToDepositWhitelist`

#### `StrategyFactory.blacklistTokens`

```solidity
/**
 * @notice Owner-only function to prevent strategies from being created for given tokens.
 * @param tokens An array of token addresses to blacklist.
 */
function blacklistTokens(IERC20[] calldata tokens) 
    external 
    onlyOwner
```

Allows the owner to prevent certain `tokens` from having strategies deployed via `StrategyFactory.deployNewStrategy`. This method was added to prevent the deployment of strategies for tokens that already have strategies deployed/whitelisted through other means.

Note that once the owner adds tokens to the blacklist, they cannot be removed. This is a known limitation of the `StrategyFactory`, and can be addressed by upgrading the factory if needed.

*Effects*:
* Adds each token in `tokens` to the `isBlacklisted` mapping

*Requirements*:
* Caller MUST be the owner
* Each passed in `token` MUST NOT already be blacklisted

#### `StrategyFactory.whitelistStrategies`

```solidity
/**
 * @notice Owner-only function to pass through a call to `StrategyManager.addStrategiesToDepositWhitelist`
 */
function whitelistStrategies(
    IStrategy[] calldata strategiesToWhitelist
)
    external
    onlyOwner
```

Allows the owner to explicitly whitelist strategies in the `StrategyManager`. This method is used as a passthrough for the `StrategyManager.addStrategiesToDepositWhitelist`, in case the owner needs to whitelist strategies not deployed via the `StrategyFactory`.

*Effects*:
* See `StrategyManager.addStrategiesToDepositWhitelist`

*Requirements*:
* Caller MUST be the owner
* See `StrategyManager.addStrategiesToDepositWhitelist`

#### `StrategyFactory.removeStrategiesFromWhitelist`

```solidity
/**
 * @notice Owner-only function to pass through a call to `StrategyManager.removeStrategiesFromDepositWhitelist`
 */
function removeStrategiesFromWhitelist(
    IStrategy[] calldata strategiesToRemoveFromWhitelist
) 
    external
    onlyOwner
```

Allows the owner to remove strategies from the `StrategyManager` strategy whitelist. This method is used as a passthrough for the `StrategyManager.removeStrategiesFromDepositWhitelist`, in case the owner needs to access this method.

*Effects*:
* See `StrategyManager.removeStrategiesFromDepositWhitelist`

*Requirements*:
* Caller MUST be the owner
* See `StrategyManager.removeStrategiesFromDepositWhitelist`

---

## System Configuration

The Strategy Whitelister role has the ability to permit/remove strategies from being depositable via the `StrategyManager`. This role is held by the `StrategyFactory` (which is fully documented in [Strategies](#strategies)). The following methods concern the Strategy Whitelister role and its abilities within the `StrategyManager`:
* [`StrategyManager.setStrategyWhitelister`](#setstrategywhitelister)
* [`StrategyManager.addStrategiesToDepositWhitelist`](#addstrategiestodepositwhitelist)
* [`StrategyManager.removeStrategiesFromDepositWhitelist`](#removestrategiesfromdepositwhitelist)

#### `setStrategyWhitelister`

```solidity
/**
 * @notice Owner-only function to change the `strategyWhitelister` address.
 * @param newStrategyWhitelister new address for the `strategyWhitelister`.
 */
function setStrategyWhitelister(address newStrategyWhitelister) external onlyOwner
```

Allows the `owner` to update the Strategy Whitelister address. Currently, the Strategy Whitelister role is held by the `StrategyFactory`. See [Strategies](#strategies) for more details.

*Effects*:
* Updates `StrategyManager.strategyWhitelister`

*Requirements*:
* Caller MUST be the `owner`

#### `addStrategiesToDepositWhitelist`

```solidity
/**
 * @notice Owner-only function that adds the provided Strategies to the 'whitelist' of strategies that stakers can deposit into
 * @param strategiesToWhitelist Strategies that will be added to the `strategyIsWhitelistedForDeposit` mapping (if they aren't in it already)
 */
function addStrategiesToDepositWhitelist(
    IStrategy[] calldata strategiesToWhitelist
)
    external
    onlyStrategyWhitelister
```

Allows the Strategy Whitelister to add any number of strategies to the `StrategyManager` whitelist, and configure whether third party transfers are enabled or disabled for each. Strategies on the whitelist are eligible for deposit via `depositIntoStrategy`.

*Effects*:
* Adds entries to `StrategyManager.strategyIsWhitelistedForDeposit`

*Requirements*:
* Caller MUST be the `strategyWhitelister`

#### `removeStrategiesFromDepositWhitelist`

```solidity
/**
 * @notice Owner-only function that removes the provided Strategies from the 'whitelist' of strategies that stakers can deposit into
 * @param strategiesToRemoveFromWhitelist Strategies that will be removed to the `strategyIsWhitelistedForDeposit` mapping (if they are in it)
 */
function removeStrategiesFromDepositWhitelist(
    IStrategy[] calldata strategiesToRemoveFromWhitelist
)
    external
    onlyStrategyWhitelister
```

Allows the Strategy Whitelister to remove any number of strategies from the `StrategyManager` whitelist. The removed strategies will no longer be eligible for deposit via `depositIntoStrategy`. However, withdrawals for previously-whitelisted strategies may still be initiated and completed, as long as the staker has shares to withdraw.

*Effects*:
* Removes entries from `StrategyManager.strategyIsWhitelistedForDeposit`

*Requirements*:
* Caller MUST be the `strategyWhitelister`
````

## File: docs/experimental/AVS-Guide.md
````markdown
[middleware-folder-link]: https://github.com/Layr-Labs/eigenlayer-contracts/tree/master/src/contracts/middleware
[middleware-guide-link]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/master/docs/AVS-Guide.md#quick-start-guide-to-build-avs-contracts
# Purpose
This document aims to describe and summarize how actively validated services (AVSs) building on EigenLayer interact with the core EigenLayer protocol. Currently, this doc explains how AVS developers can use the current** APIs for: 
- enabling operators to opt-in to the AVS,
- enabling operators to opt-out (withdraw stake) from the AVS,
- enabling operators to continuously update their commitments to middlewares, and
- enabling AVS to freeze operators for the purpose of slashing (the corresponding unfreeze actions are determined by the veto committee).

<p align="center"><b><font size="+1">
🚧 ** The Slasher contract is under active development and its interface is expected to change. We recommend writing slashing logic without integrating with the Slasher at this point in time. 🚧
</font></b><p>

We are currently in the process of implementing the API for rewards flow from AVSs to operators in EigenLayer. Details of this API will be added to this document in the near future.

The following figure summarizes the scope of this document: 
![Doc Outline](../images/middleware_outline_doc.png)


# Introduction
In designing EigenLayer, the EigenLabs team aspired to make minimal assumptions about the structure of AVSs built on top of it. If you are getting started looking at EigenLayer's codebase, the `Slasher.sol` contains most of the logic that actually mediates the interactions between EigenLayer and AVSs. Additionally, there is a general-purpose [/middleware/ folder][middleware-folder-link], which contains code that can be extended, used directly, or consulted as a reference in building an AVS on top of EigenLayer. Note that there will be a single, EigenLayer-owned, `Slasher.sol` contract, but all the `middleware` contracts are AVS-specific and need to be deployed separately by AVS teams.

## Important Terminology
- **Tasks** - A task in EigenLayer is the smallest unit of work that operators commit to perform when serving an AVS. These tasks may be associated with one or more slashing conditions applicable to the AVS. 
- **Strategies** - A strategy in EigenLayer is a contract that holds staker deposits, i.e. it controls one or more asset(s) that can be restaked. At launch EigenLayer will feature only simple strategies which may hold a single token. However, EigenLayer's strategy design is flexible and open, and in the future strategies could be deployed which implement more complex logic, including DeFi integrations. 
- **Quorums** - A quorum in EigenLayer is a grouping of specific kinds of stake who opt into an AVS while satisfying a particular trait. Examples of such traits could be stETH stakers or native stakers.  The purpose of having a quorum is that an AVS can customize the makeup of their security offering by choosing which kinds of stake/security they would like to utilize.  

# Key Design Considerations
1. *Decomposition into "Tasks"*: <br/>
    EigenLayer assumes that an AVS manages tasks that are executed over time by a registered operator. Each task is associated with the time period during which the AVS's operators' stakes are placed "at stake", i.e. potentially subject to slashing. Examples of tasks could be:
    - Hosting and serving a “DataStore” in the context of EigenDA
    - Posting a state root of another blockchain for a bridge service

2. *Stake is "At Stake" on Tasks for a Finite Duration*: <br/>
    It is assumed that every task (eventually) resolves. Each operator places their stake in EigenLayer “at stake” on the tasks that they perform. In order to “release” the stake (e.g. so the operator can withdraw their funds), these tasks need to eventually resolve. It is RECOMMENDED, but not required that a predefined duration is specified in the AVS contract for each task. As a guideline, the EigenLabs team believes that the duration of a task should be aligned with the longest reasonable duration that would be acceptable for an operator to keep funds “at stake”. An AVS builder should recognize that extending the duration of a task may impose significant negative externalities on the stakers of EigenLayer, and may disincentivize operators from opting-in to serving their application (so that they can attract more delegated stake).

3. *Services Slash Only Objectively Attributable Behavior*: <br/>
    EigenLayer is built to support slashing as a result of an on-chain-checkable, objectively attributable action. An AVS SHOULD slash in EigenLayer only for such provable and attributable behavior. It is expected that operators will be very hesitant to opt-in to services that slash for other types of behavior, and other services may even choose to exclude operators who have opted-in to serving one or more AVSs with such “subjective slashing conditions”, as these slashing conditions present a significant challenge for risk modeling, and may be perceived as more dangerous in general. Some examples of on-chain-checkable, objectively attributable behavior: 
    - double-signing a block in Ethereum, but NOT inactivity leak; 
    - proofs-of-custody in EigenDA, but NOT a node ceasing to serve data; 
    - a node in a light-node-bridge AVS signing an invalid block from another chain.

4. *Single Point-of-Interaction for Services and EigenLayer*: <br/>
    It is assumed that services have a single contract that coordinates the service’s communications sent to EigenLayer. This contract – referred to as the ServiceManager – informs EigenLayer of operator registration, updates, and deregistration, as well as signaling to EigenLayer when an operator should be slashed (frozen). An AVS has full control over how it splits the actual logic involved, but is expected to route all calls to EigenLayer through a single contract. While technically possible, an AVS SHOULD NOT use multiple contracts to interact with EigenLayer. An AVS architecture using multiple contracts to interact with EigenLayer will impose additional burden on stakers in EigenLayer when withdrawing stake.

## Integration with EigenLayer Contracts:
In this section, we will explain various API interfaces that EigenLayer provides which are essential for AVSs to integrate with EigenLayer. 

### *Operators Opting into AVS*
In order for any EigenLayer operator to be able to opt-in to an AVS, EigenLayer provides two interfaces: `optIntoSlashing(..)` and `recordFirstStakeUpdate(..)`. The sequential flow for opting into an AVS using these functions is as follows:
1. The operator first opts into slashing by calling  `Slasher.optIntoSlashing(..)`, where it has to specify the address of the AVS's ServiceManager contract in the argument. This step results in the operator giving permission to the AVS's ServiceManager contract to slash the operator via EigenLayer, if the operator is ever proven to have engaged in adversarial behavior while responding to the AVS's task. A successful call to  `Slasher.optIntoSlashing(..)` emits the `OptedIntoSlashing(..)` event.
2. Next, the operator needs to register with the AVS on chain via an AVS-specific registry contract (see [this][middleware-guide-link] section for examples). To integrate with EigenLayer, the AVS's Registry contract provides a registration endpoint that calls on the AVS's `ServiceManager.recordFirstStakeUpdate(..)` which in turn calls `Slasher.recordFirstStakeUpdate(..)`. On successful execution of this function call, the event `MiddlewareTimesAdded(..)` is emitted and the operator has to start serving the tasks from the AVS.

The following figure illustrates the above flow: 
![Operator opting-in](../images/operator_opting.png)

### *Staker Delegation to an Operator: Which Opts-In to AVSs*

A staker does not restake into AVSs. A staker delegates to an operator and it is the operator that registers for new AVSs (with the staker having option to opt-out).

By delegating to a specific operator, stakers are implicitly agreeing to the AVSs they support. If desired, operators can pursue off-chain consensus with stakers prior to modifying their AVSs. Moreover, stakers will have a grace period to withdraw their delegation should an operator introduce an AVS that doesn't align with their objectives. This grace period is configurable on an operator level.

### *AVS Visibility and Control*

An AVS registration function can blacklist another AVS contract and during registration check that the operator is not registered in that AVS. Or it can check that the operator has not given permission to that AVS's service manager to slash it.

An AVS registry contract should define quorums (eth LST quorum, erc20 quorum, etc.) and allow (or prefer) operators having a minimum amount of restaked assets in each of those quorums to register with the AVS.

### *Recording Stake Updates*
EigenLayer is a dynamic system where stakers and operators are constantly adjusting amounts of stake delegated via the system. It is therefore imperative for an AVS to be aware of any changes to stake delegated to its operators. In order to facilitate this, EigenLayer offers the `Slasher.recordStakeUpdate(..)`.

Let us illustrate the usage of this facility with an example: A staker has delegated to an operator, who has opted-in to serving an AVS. Whenever the staker withdraws some or all of its stake from EigenLayer, this withdrawal affects all the AVSs uniformly that the staker's delegated operator is participating in. The series of steps for withdrawing stake is as follows:
 - The staker queues their withdrawal request with EigenLayer. The staker can place this request by calling  `StrategyManager.queueWithdrawal(..)`.
 - The operator, noticing an upcoming change in their delegated stake, notifies the AVS about this change. To do this, the operator triggers the AVS to call the `ServiceManager.recordStakeUpdate(..)` which in turn accesses `Slasher.recordStakeUpdate(..)`.  On successful execution of this call, the event `MiddlewareTimesAdded(..)` is emitted.
- The AVS provider now is aware of the change in stake, and the staker can eventually complete their withdrawal.  Refer [here](https://github.com/Layr-Labs/eigenlayer-contracts/blob/master/docs/EigenLayer-withdrawal-flow.md) for more details

The following figure illustrates the above flow: 
![Stake update](../images/staker_withdrawing.png)

### *Deregistering from AVS*
In order for any EigenLayer operator to be able to de-register from an AVS, EigenLayer provides the interface `Slasher.recordLastStakeUpdateAndRevokeSlashingAbility(..)`. Essentially, in order for an operator to deregister from an AVS, the operator has to call `Slasher.recordLastStakeUpdateAndRevokeSlashingAbility(..)`  via the AVS's ServiceManager contract. It is important to note that the latest block number until which the operator is required to serve tasks for the service must be known by the service and included in the ServiceManager's call to `Slasher.recordLastStakeUpdateAndRevokeSlashingAbility`.

The following figure illustrates the above flow in which the operator calls the `deregister(..)` function in a sample Registry contract.
![Operator deregistering](../images/operator_deregister.png)

### *Slashing*
As mentioned above, EigenLayer is built to support slashing as a result of an on-chain-checkable, objectively attributable action. In order for an AVS to be able to slash an operator in an objective manner, the AVS needs to deploy a DisputeResolution contract which anyone can call to raise a challenge against an EigenLayer operator for its adversarial action. On successful challenge, the DisputeResolution contract calls `ServiceManager.freezeOperator(..)`; the ServiceManager in turn calls `Slasher.freezeOperator(..)` to freeze the operator in EigenLayer. EigenLayer's Slasher contract emits a `OperatorFrozen(..)` event whenever an operator is (successfully) frozen

The following figure illustrates the above flow: 
![Slashing](../images/slashing.png)


## Quick Start Guide to Build AVS Contracts:
The EigenLayer team has built a set of reusable and extensible contracts for use in AVSs built on top of EigenLayer. These are contained in the general-purpose [/middleware/ folder][middleware-folder-link], which contains code that can be extended, used directly, or consulted as a reference in building AVS on top of EigenLayer. There are several basic contracts that all AVS-specific contracts can be built on:
- The *VoteWeigherBase contract* tracks an operator’s “weight” in a given quorum, across all strategies that are associated with that quorum.  This contract also manages which strategies are in each quorum - this includes functionalities for both adding and removing strategies, as well as changing strategy weights.  
- The *RegistryBase contract* is a basic registry contract that can be used to track operators opted-into running an AVS.  Importantly, this base registry contract assumes a maximum of two quorums, where each quorum represents an aggregation of a certain type of stake. 

Furthermore, it’s expected that many AVSs will require a quorum of registered operators to sign on commitments.  To this end, the EigenLabs team has developed a set of contracts designed to optimize the cost of checking signatures through the use of a BLS aggregate signature scheme:
### BLSPublicKeyCompendium
This contract allows each Ethereum address to register a unique BLS public key; a single BLSPublicKeyCompendium contract can be shared amongst all AVSs using BLS signatures. <br/>
### BLSRegistry
This contract builds upon lower-level (RegistryBase and VoteWeigherBase) contracts, to allow users of EigenLayer to register as operators for a single AVS. Each AVS’s BLSRegistry keeps a historic record of the Aggregate Public Key (APK) of all operators of the AVS. To allow proper encoding of data and aggregation of signatures while avoiding race conditions (e.g. from operators registering or deregistering, causing the current APK to change), each task defines a referenceBlockNumber, which may be briefly in the past. The BLSRegistry defines an optional whitelister role, which controls whether or not the whitelist is enabled and can edit the whitelist. If the whitelist is enabled, then only addresses that have been whitelisted may opt-in to serving the AVS. <br/>   

In addition, the BLSRegistry (technically the lower-level RegistryBase which the BLSRegistry inherits from) defines a “minimum stake” for the quorum(s). An operator can only register for the AVS if they meet the minimum requirement for at least one quorum. By default the `ServiceManager.owner()` has the ability to change the minimum stake requirement(s). Each BLSRegistry defines one or two “quorums”; each operator for the AVS may have stake in EigenLayer that falls into either (or both) quorum(s). Each quorum is essentially defined by two vectors: a vector of “Strategies” of interest (in practice this ends up being tokens of interest) and a vector of “weights” or “multipliers”, which define whether certain strategies are weighed more heavily than others within the quorum (e.g. if the AVS desires to give 2x power to a specific token over another token). In the contract code these vectors are condensed into a single array of `StrategyAndWeightingMultiplier` structs. The `ServiceManager.owner()` has the ability to edit these arrays at will.

### BLSSignatureChecker
When signatures have been aggregated, they can be submitted to the BLSSignatureChecker, an optimized contract designed expressly for verifying quorums of BLS signers. The caller MUST provide a small amount of data corresponding to the task to be confirmed, the aggregate signature itself, and a bit of data for each non-signer, that is, the caller MUST provide data for each operator registered for the service for whom their signature has not been aggregated. The BLSSignatureChecker ultimately returns both the total stake that was present at the specified block number (i.e. the sum of all operator’s stakes) and the total stake that signed; these amounts can then be checked against a quorum condition (e.g. requiring ⅔ stake to sign) before the task is ultimately confirmed.
````

## File: docs/images/Staker Flow Diagrams/diagrams.excalidraw
````
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://excalidraw.com",
  "elements": [
    {
      "type": "rectangle",
      "version": 1801,
      "versionNonce": 1412022419,
      "index": "b1y0G",
      "isDeleted": false,
      "id": "d7pRFde2mvhA75ZinOGRD",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1007.4642857142851,
      "y": 227.21428571428572,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 455.8571428571431,
      "height": 114.99999999999997,
      "seed": 1964018507,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "pr1eJhZG8MQGJxWd2tB0Y"
        },
        {
          "id": "LDZjamuIcsv5-Wrln_O_d",
          "type": "arrow"
        },
        {
          "id": "lS4fRSLZHIKN77q4fwuWo",
          "type": "arrow"
        },
        {
          "id": "pQuPzVRh8WjIFdRY2eKF5",
          "type": "arrow"
        }
      ],
      "updated": 1734360900715,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1535,
      "versionNonce": 1316959251,
      "index": "b1y0V",
      "isDeleted": false,
      "id": "pr1eJhZG8MQGJxWd2tB0Y",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1155.6345258440285,
      "y": 275.1142857142857,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.51666259765625,
      "height": 19.2,
      "seed": 1520221355,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734113298835,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "DelegationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "d7pRFde2mvhA75ZinOGRD",
      "originalText": "DelegationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1595,
      "versionNonce": 1462533565,
      "index": "b1y1",
      "isDeleted": false,
      "id": "BjZJSI_MJK0uSe18cWEsM",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 512.3474413621626,
      "y": 599.8323612009546,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 663150021,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "SAKX-tjznJ9nB3OnPBiJo"
        },
        {
          "id": "LDZjamuIcsv5-Wrln_O_d",
          "type": "arrow"
        },
        {
          "id": "IVDpX9ZNHIWc1DEu-_MSJ",
          "type": "arrow"
        },
        {
          "id": "LzGvT9mueNtOL43AADrvA",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1296,
      "versionNonce": 531611165,
      "index": "b1y1V",
      "isDeleted": false,
      "id": "SAKX-tjznJ9nB3OnPBiJo",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 553.4724413621626,
      "y": 647.7323612009545,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 215014693,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPodManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "BjZJSI_MJK0uSe18cWEsM",
      "originalText": "EigenPodManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 671,
      "versionNonce": 108445629,
      "index": "b1y2",
      "isDeleted": false,
      "id": "hu48jItWXAGznv2MYPlfa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 739.7857142857142,
      "y": -686.6751012333061,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1017310757,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "k29t3JUZtaF8eQ7ad9dvG"
        }
      ],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 408,
      "versionNonce": 811712541,
      "index": "b1y2G",
      "isDeleted": false,
      "id": "k29t3JUZtaF8eQ7ad9dvG",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 780.9107142857142,
      "y": -638.7751012333061,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1489756549,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "hu48jItWXAGznv2MYPlfa",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 2120,
      "versionNonce": 2064992157,
      "index": "b1y2V",
      "isDeleted": false,
      "id": "-WvgRiaL0QVDivEZ7r6Xc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 1117.635055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 847130949,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2050,
      "versionNonce": 1134840829,
      "index": "b1y3",
      "isDeleted": false,
      "id": "tleLAbz09IPjqR2KmnNm8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 1082.635055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1955709093,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1999,
      "versionNonce": 1083425885,
      "index": "b1y3V",
      "isDeleted": false,
      "id": "aobl_RC8TILQJlluTfRQL",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 1047.135055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 3957765,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1925,
      "versionNonce": 1974808765,
      "index": "b1y4",
      "isDeleted": false,
      "id": "rIBNqVkwT_U14Ee5tTTXy",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 1012.135055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 964402021,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1990,
      "versionNonce": 1890193693,
      "index": "b1y4G",
      "isDeleted": false,
      "id": "vCqGpKBTk41RnVpK56dHk",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 981.385055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 866810155,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "vNXvHpaNh61uq1U8du8yF",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1920,
      "versionNonce": 61091293,
      "index": "b1y4V",
      "isDeleted": false,
      "id": "IQBALGzyvaO99XKwSkIRH",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 946.385055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 2113412043,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1865,
      "versionNonce": 503081533,
      "index": "b1y5",
      "isDeleted": false,
      "id": "ApHGGX2G2KlFQna4mgGyW",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 910.885055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 906093349,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "Erd3ee2INdOqhV26KPfNR",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1798,
      "versionNonce": 1322130077,
      "index": "b1y5V",
      "isDeleted": false,
      "id": "pO-cMQXuP3FqEK-VO2nH-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.4580244801324,
      "y": 875.885055046703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 620990117,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "Erd3ee2INdOqhV26KPfNR",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1717,
      "versionNonce": 1858780925,
      "index": "b1y6",
      "isDeleted": false,
      "id": "3B57LAKyPYCAagMZIplZn",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 638.8330244801324,
      "y": 836.4544994911473,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1867190251,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "t-Fb9iJCrieEmdEJ8_Rze"
        },
        {
          "id": "Erd3ee2INdOqhV26KPfNR",
          "type": "arrow"
        },
        {
          "id": "vNXvHpaNh61uq1U8du8yF",
          "type": "arrow"
        },
        {
          "id": "LzGvT9mueNtOL43AADrvA",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1455,
      "versionNonce": 1148442461,
      "index": "b1y6V",
      "isDeleted": false,
      "id": "t-Fb9iJCrieEmdEJ8_Rze",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 650.0330252430718,
      "y": 874.7544994911473,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 112.5999984741211,
      "height": 38.4,
      "seed": 249027211,
      "groupIds": [
        "txawL17FR69jm-jT8VmVM"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPods\n(1 per user)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "3B57LAKyPYCAagMZIplZn",
      "originalText": "EigenPods\n(1 per user)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1675,
      "versionNonce": 1349154813,
      "index": "b1y7",
      "isDeleted": false,
      "id": "iN5d7A4GZ0ti1VZf0ejqo",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1009.5251238798216,
      "y": 892.1062215430579,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 266.00000000000017,
      "height": 114.99999999999997,
      "seed": 1219437925,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "A2WEASUqMm8xFmepnTzDB"
        }
      ],
      "updated": 1734360280457,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1576,
      "versionNonce": 733245533,
      "index": "b1y7V",
      "isDeleted": false,
      "id": "A2WEASUqMm8xFmepnTzDB",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1072.1501238798216,
      "y": 940.0062215430579,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1601149643,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360280457,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EIP-4788 Oracle",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "iN5d7A4GZ0ti1VZf0ejqo",
      "originalText": "EIP-4788 Oracle",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1013,
      "versionNonce": 2043524221,
      "index": "b1y8",
      "isDeleted": false,
      "id": "uiNRUWY9ZI4zldFqQX2ve",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1112.5873015873017,
      "y": -618.2822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1320455557,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 963,
      "versionNonce": 779885789,
      "index": "b1y8G",
      "isDeleted": false,
      "id": "VYDyjsUXdkxIKjcQeYM0U",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1111.5873015873017,
      "y": -652.2822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 707957989,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 959,
      "versionNonce": 637434173,
      "index": "b1y8V",
      "isDeleted": false,
      "id": "GbnSO6e5w-zEyXp65Rnum",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1110.0873015873017,
      "y": -685.2822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1042837419,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 909,
      "versionNonce": 1236683165,
      "index": "b1y9",
      "isDeleted": false,
      "id": "kV7S0-ol5pVjW8kyCPtrs",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1109.0873015873017,
      "y": -719.2822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 179466635,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 748,
      "versionNonce": 1523912189,
      "index": "b1y9V",
      "isDeleted": false,
      "id": "cbvowATXbHy47R6SwBFKv",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1109.0873015873017,
      "y": -754.2822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1541881157,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "k4w0cZ79L66VZfHSBBqgn"
        }
      ],
      "updated": 1734360836083,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 576,
      "versionNonce": 2019313245,
      "index": "b1yA",
      "isDeleted": false,
      "id": "k4w0cZ79L66VZfHSBBqgn",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1116.2956374637665,
      "y": -715.9822440904488,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 234.5833282470703,
      "height": 38.4,
      "seed": 620317861,
      "groupIds": [
        "L8wotQa46OELUi4G_L_sv"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360836083,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "cbvowATXbHy47R6SwBFKv",
      "originalText": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 583,
      "versionNonce": 2113318749,
      "index": "b1yAG",
      "isDeleted": false,
      "id": "76JG-p1ALcSUWf9ES9oHk",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 198.71351960124616,
      "y": -655.8585003996734,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1785752133,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "yWIuKnaZ9EYu8nW_E24gu"
        }
      ],
      "updated": 1734360838904,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 307,
      "versionNonce": 2105719741,
      "index": "b1yAV",
      "isDeleted": false,
      "id": "yWIuKnaZ9EYu8nW_E24gu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 253.91352036418562,
      "y": -607.9585003996734,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 112.5999984741211,
      "height": 19.2,
      "seed": 1138022821,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360838904,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AVSDirectory",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "76JG-p1ALcSUWf9ES9oHk",
      "originalText": "AVSDirectory",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 1384,
      "versionNonce": 294900467,
      "index": "b1yB",
      "isDeleted": false,
      "id": "Erd3ee2INdOqhV26KPfNR",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 781.4119809434505,
      "y": 951.712889169279,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 208.02818639382463,
      "height": 0.6787821649787702,
      "seed": 257411774,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734360277648,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "3B57LAKyPYCAagMZIplZn",
        "focus": 0.9964165782256912,
        "gap": 7.578956463317979,
        "fixedPoint": null
      },
      "endBinding": null,
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          208.02818639382463,
          0.6787821649787702
        ]
      ]
    },
    {
      "type": "text",
      "version": 472,
      "versionNonce": 157466845,
      "index": "b1yBV",
      "isDeleted": false,
      "id": "FQ0Osc5fg3FuRYzXUG7wf",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 788.0115959087037,
      "y": 857.3314836181316,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 215.9166717529297,
      "height": 75,
      "seed": 126625186,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "Erd3ee2INdOqhV26KPfNR",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Proof validated vs\nrecent beacon block\nheader",
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "3. Proof validated vs\nrecent beacon block\nheader",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 2434,
      "versionNonce": 1411187837,
      "index": "b1yC",
      "isDeleted": false,
      "id": "LzGvT9mueNtOL43AADrvA",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 705.4173021012218,
      "y": 827.5462212097732,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 87.80438961267384,
      "height": 106.31901873897732,
      "seed": 1825768382,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "9LXoq9DwZzpa_Fyv9Teu0"
        }
      ],
      "updated": 1734360312556,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "3B57LAKyPYCAagMZIplZn",
        "focus": 0.4689934907333594,
        "gap": 8.908278281374123,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "BjZJSI_MJK0uSe18cWEsM",
        "focus": 0.3711161723740525,
        "gap": 6.394841269841322,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -87.80438961267384,
          -106.31901873897732
        ]
      ]
    },
    {
      "type": "text",
      "version": 189,
      "versionNonce": 1700401085,
      "index": "b1yCG",
      "isDeleted": false,
      "id": "9LXoq9DwZzpa_Fyv9Teu0",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 568.7551738232053,
      "y": 749.3867118402845,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 185.51986694335938,
      "height": 50,
      "seed": 518652962,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360311034,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Staker awarded\ndeposit shares",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "LzGvT9mueNtOL43AADrvA",
      "originalText": "4. Staker awarded  deposit shares",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "ellipse",
      "version": 861,
      "versionNonce": 239332765,
      "index": "b1yCV",
      "isDeleted": false,
      "id": "aNLyHhgEK3ibcdWPwAb6V",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 147.4758816229895,
      "y": 1025.724340760989,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 1412352510,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "id": "vNXvHpaNh61uq1U8du8yF",
          "type": "arrow"
        },
        {
          "type": "text",
          "id": "pvdmt5DJrZ55bLVP2537b"
        },
        {
          "id": "IVDpX9ZNHIWc1DEu-_MSJ",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 618,
      "versionNonce": 101593597,
      "index": "b1yD",
      "isDeleted": false,
      "id": "pvdmt5DJrZ55bLVP2537b",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 173.2686171897942,
      "y": 1066.601234748695,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 298627006,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "aNLyHhgEK3ibcdWPwAb6V",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 1890,
      "versionNonce": 1710398227,
      "index": "b1yDV",
      "isDeleted": false,
      "id": "vNXvHpaNh61uq1U8du8yF",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 269.3765893531465,
      "y": 1102.7423950802493,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 367.24347365786707,
      "height": 0.05830618139498256,
      "seed": 1119593122,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734360277648,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "aNLyHhgEK3ibcdWPwAb6V",
        "focus": 0.4672147590179739,
        "gap": 17.6893676636725,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "vCqGpKBTk41RnVpK56dHk",
        "focus": -1.1091502390745562,
        "gap": 6.299033852151297,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          367.24347365786707,
          -0.05830618139498256
        ]
      ]
    },
    {
      "type": "text",
      "version": 1038,
      "versionNonce": 1378536317,
      "index": "b1yE",
      "isDeleted": false,
      "id": "jcaRATQnUsA8qMWkGunCX",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0.0026774312006994094,
      "x": 287.9730286654003,
      "y": 985.9959243795562,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 311.5333251953125,
      "height": 100,
      "seed": 1748858018,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "vNXvHpaNh61uq1U8du8yF",
          "type": "arrow"
        }
      ],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Supply proof of beacon chain\nvalidator with withdrawal\ncredentials pointed at Staker's\nEigenPod",
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "2. Supply proof of beacon chain\nvalidator with withdrawal\ncredentials pointed at Staker's\nEigenPod",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 275,
      "versionNonce": 1226724317,
      "index": "b1yEV",
      "isDeleted": false,
      "id": "uQmR5duxL2YFGZTLLdDKu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 277.6544530515599,
      "y": 1121.1707693324174,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 316.3500061035156,
      "height": 24,
      "seed": 623129890,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "verifyWithdrawalCredentials",
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "verifyWithdrawalCredentials",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 911,
      "versionNonce": 1699748733,
      "index": "b1yF",
      "isDeleted": false,
      "id": "LDZjamuIcsv5-Wrln_O_d",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 670.8071239588882,
      "y": 593.3081513249739,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 338.2683875544691,
      "height": 248.48315485174834,
      "seed": 846354786,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "xcaW5b597i8KFqrKbe9_q"
        }
      ],
      "updated": 1734361148717,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "BjZJSI_MJK0uSe18cWEsM",
        "focus": -0.2118205902239549,
        "gap": 6.5242098759806595,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "d7pRFde2mvhA75ZinOGRD",
        "focus": 0.4718620660053835,
        "gap": 2.6107107589399163,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          338.2683875544691,
          -248.48315485174834
        ]
      ]
    },
    {
      "type": "text",
      "version": 320,
      "versionNonce": 1237168019,
      "index": "b1yFV",
      "isDeleted": false,
      "id": "xcaW5b597i8KFqrKbe9_q",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 734.981417833779,
      "y": 394.0665738990997,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 209.9197998046875,
      "height": 150,
      "seed": 1848652862,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361147616,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "5. If Staker is\ndelegated, update\nOperator's delegated\nshares based on\nstaker's increased\ndeposit shares",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "LDZjamuIcsv5-Wrln_O_d",
      "originalText": "5. If Staker is delegated, update Operator's delegated shares based on staker's increased deposit shares",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 594,
      "versionNonce": 1027723325,
      "index": "b1yG",
      "isDeleted": false,
      "id": "pgNm6zGufuhDI1ptkmCnq",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 245.77111158020585,
      "y": 442.6588645705128,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 281.1000061035156,
      "height": 64.39999999999999,
      "seed": 1380464190,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360277639,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nDepositing Native ETH",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nDepositing Native ETH",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 1278,
      "versionNonce": 1635725907,
      "index": "b1yGG",
      "isDeleted": false,
      "id": "IVDpX9ZNHIWc1DEu-_MSJ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 170.51159590870293,
      "y": 1022.5993428059394,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 327.30210637174764,
      "height": 324.01338450211006,
      "seed": 503264766,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "7X5j5XWcrfqZunL5LVn8P"
        }
      ],
      "updated": 1734360277648,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "aNLyHhgEK3ibcdWPwAb6V",
        "focus": -0.973783988334446,
        "gap": 10.907898277340259,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "BjZJSI_MJK0uSe18cWEsM",
        "focus": -0.11991131074358848,
        "gap": 14.533739081712099,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          135.43746691313794,
          -276.72474110745
        ],
        [
          327.30210637174764,
          -324.01338450211006
        ]
      ]
    },
    {
      "type": "text",
      "version": 271,
      "versionNonce": 1433013523,
      "index": "b1yGV",
      "isDeleted": false,
      "id": "7X5j5XWcrfqZunL5LVn8P",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 221.2623046390588,
      "y": 634.6785714285717,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 220.91983032226562,
      "height": 150,
      "seed": 1517705406,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734359522384,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Deploy EigenPod and\nstart up one or more\nbeacon chain\nvalidators with\nwithdrawal credentials\npointed at pod.",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "IVDpX9ZNHIWc1DEu-_MSJ",
      "originalText": "1. Deploy EigenPod and start up one or more beacon chain validators with withdrawal credentials pointed at pod.",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 2123,
      "versionNonce": 1818185405,
      "index": "b1yH",
      "isDeleted": false,
      "id": "OoHImjROrg7WnQ4xzkpcv",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 651.7118827829199,
      "y": 1717.428037405244,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 455.8571428571431,
      "height": 114.99999999999997,
      "seed": 1897941602,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "27hGMnQNsPM5Ow8ganeGX"
        },
        {
          "id": "UShknM2c1yF75IS4pc0Bs",
          "type": "arrow"
        },
        {
          "id": "5nPp_KJjwxHRLaooYuN1o",
          "type": "arrow"
        },
        {
          "id": "3paqcsWlpb_FIiY6I8Lko",
          "type": "arrow"
        },
        {
          "id": "uZdrle8wzDudz1Ky8EmR2",
          "type": "arrow"
        }
      ],
      "updated": 1734361800476,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1852,
      "versionNonce": 33812339,
      "index": "b1yHV",
      "isDeleted": false,
      "id": "27hGMnQNsPM5Ow8ganeGX",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 799.8821229126634,
      "y": 1765.328037405244,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.51666259765625,
      "height": 19.2,
      "seed": 1697426978,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361205390,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "DelegationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "OoHImjROrg7WnQ4xzkpcv",
      "originalText": "DelegationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1652,
      "versionNonce": 1205018099,
      "index": "b1yI",
      "isDeleted": false,
      "id": "y-bDpBMyaFEbqM1dAyygO",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 564.219819290857,
      "y": 2031.3248628020692,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 695638498,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "OYjXOfgD4G99D0UzjCUa4"
        },
        {
          "id": "5nPp_KJjwxHRLaooYuN1o",
          "type": "arrow"
        }
      ],
      "updated": 1734361205390,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1350,
      "versionNonce": 1312876435,
      "index": "b1yIG",
      "isDeleted": false,
      "id": "OYjXOfgD4G99D0UzjCUa4",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 605.344819290857,
      "y": 2079.224862802069,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1829572002,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361205390,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPodManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "y-bDpBMyaFEbqM1dAyygO",
      "originalText": "EigenPodManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "ellipse",
      "version": 1288,
      "versionNonce": 132376797,
      "index": "b1yIV",
      "isDeleted": false,
      "id": "_lflU-chf5oKUcaJ6h4iP",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 83.42227628906892,
      "y": 1758.9697040719104,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 877827298,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "dKgP0x4gTtpzyQ0k3fx7X"
        },
        {
          "id": "UShknM2c1yF75IS4pc0Bs",
          "type": "arrow"
        }
      ],
      "updated": 1734361240894,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1042,
      "versionNonce": 411878717,
      "index": "b1yJ",
      "isDeleted": false,
      "id": "dKgP0x4gTtpzyQ0k3fx7X",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 109.21501185587363,
      "y": 1799.8465980596168,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 1353820322,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361240894,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "_lflU-chf5oKUcaJ6h4iP",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 967,
      "versionNonce": 1354681267,
      "index": "b1yJV",
      "isDeleted": false,
      "id": "ldqJxq6Ymhs0S0M3Oeo2T",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 188.83013064765873,
      "y": 1946.3208945481013,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 290.1166687011719,
      "height": 64.39999999999999,
      "seed": 1058490146,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361205390,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nQueueing a Withdrawal",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nQueueing a Withdrawal",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 533,
      "versionNonce": 1822102941,
      "index": "b1yK",
      "isDeleted": false,
      "id": "UShknM2c1yF75IS4pc0Bs",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 203.76971928374613,
      "y": 1823.2340121234956,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 444.4312508007623,
      "height": 2.283485978642375,
      "seed": 1955708926,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734361240895,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "_lflU-chf5oKUcaJ6h4iP",
        "focus": 0.21762247876367694,
        "gap": 12.698369542443928,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "OoHImjROrg7WnQ4xzkpcv",
        "focus": -0.8826906454351978,
        "gap": 3.51091269841163,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          444.4312508007623,
          2.283485978642375
        ]
      ]
    },
    {
      "type": "text",
      "version": 224,
      "versionNonce": 1882176243,
      "index": "b1yKG",
      "isDeleted": false,
      "id": "-p_5qWmgqYtbWtGR7iAeW",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 285.9276139074964,
      "y": 1837.487561214768,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 281.25,
      "height": 48,
      "seed": 1827303778,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361205390,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "undelegate, redelegate, \nor queueWithdrawals",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "undelegate, redelegate, \nor queueWithdrawals",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 233,
      "versionNonce": 1721284221,
      "index": "b1yKV",
      "isDeleted": false,
      "id": "KzAAaAeuALvdgojxNjbs7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 218.01781375312828,
      "y": 1754.6780181142065,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 393.69964599609375,
      "height": 50,
      "seed": 1121468350,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361237884,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Initiate withdrawal of deposit shares\nfor LSTs, Native ETH, or both",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. Initiate withdrawal of deposit shares\nfor LSTs, Native ETH, or both",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 312,
      "versionNonce": 1014126707,
      "index": "b1yL",
      "isDeleted": false,
      "id": "5nPp_KJjwxHRLaooYuN1o",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 742.5341158452547,
      "y": 1840.820894548101,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 110.84284788508944,
      "height": 183.33333333333303,
      "seed": 2071062882,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "oX28umVZ3JPHOhJbR9hcB"
        }
      ],
      "updated": 1734362077371,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "OoHImjROrg7WnQ4xzkpcv",
        "focus": 0.37027146266844296,
        "gap": 8.39285714285711,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "y-bDpBMyaFEbqM1dAyygO",
        "focus": -0.5683425061782671,
        "gap": 7.170634920635166,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -110.84284788508944,
          183.33333333333303
        ]
      ]
    },
    {
      "type": "text",
      "version": 117,
      "versionNonce": 915396477,
      "index": "b1yLV",
      "isDeleted": false,
      "id": "oX28umVZ3JPHOhJbR9hcB",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 578.5127730752955,
      "y": 1882.4875612147675,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 217.19983765482903,
      "height": 100,
      "seed": 30379646,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362076301,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Remove\ncorresponding deposit\nshares while\nwithdrawal is in queue",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "5nPp_KJjwxHRLaooYuN1o",
      "originalText": "4. Remove corresponding deposit shares while withdrawal is in queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 762,
      "versionNonce": 1735007187,
      "index": "b1yM",
      "isDeleted": false,
      "id": "fgr-vhlkAfB4aNKWl1vBo",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 977.3001764337146,
      "y": 2034.5947040719113,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1155351458,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "h0lFC_qYXovD547xr7zCV"
        },
        {
          "id": "3paqcsWlpb_FIiY6I8Lko",
          "type": "arrow"
        }
      ],
      "updated": 1734361205390,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 498,
      "versionNonce": 1107110259,
      "index": "b1yMV",
      "isDeleted": false,
      "id": "h0lFC_qYXovD547xr7zCV",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1018.4251764337146,
      "y": 2082.4947040719117,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1098415970,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361205390,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "fgr-vhlkAfB4aNKWl1vBo",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 379,
      "versionNonce": 600938013,
      "index": "b1yN",
      "isDeleted": false,
      "id": "3paqcsWlpb_FIiY6I8Lko",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 992.9125326314522,
      "y": 1839.453635696875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 152.49048544824382,
      "height": 186.66666666666606,
      "seed": 512599010,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "zeElqxBoaWoookvFWk7QE"
        }
      ],
      "updated": 1734362080654,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "OoHImjROrg7WnQ4xzkpcv",
        "focus": -0.22029835777170426,
        "gap": 7.025598291631013,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "fgr-vhlkAfB4aNKWl1vBo",
        "focus": 0.6972704914860178,
        "gap": 8.474401708370237,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          152.49048544824382,
          186.66666666666606
        ]
      ]
    },
    {
      "type": "text",
      "version": 124,
      "versionNonce": 1464141555,
      "index": "b1yNV",
      "isDeleted": false,
      "id": "zeElqxBoaWoookvFWk7QE",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 960.5578565281596,
      "y": 1882.786969030208,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 217.19983765482903,
      "height": 100,
      "seed": 2125649826,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362079457,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Remove\ncorresponding deposit\nshares while\nwithdrawal is in queue",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "3paqcsWlpb_FIiY6I8Lko",
      "originalText": "4. Remove corresponding deposit shares while withdrawal is in queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 2236,
      "versionNonce": 219456563,
      "index": "b1yO",
      "isDeleted": false,
      "id": "HMOTYhgmt2kUR8Zl1vEhQ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 597.8047585425198,
      "y": 2620.875138540512,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 455.8571428571431,
      "height": 114.99999999999997,
      "seed": 1749542178,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "9Vck9P2kgcYLnAHDKMCOz"
        },
        {
          "id": "eDi3aeq3shWTkAvqzeyHF",
          "type": "arrow"
        },
        {
          "id": "TTmKNlPiKnQPMyN-Awpoc",
          "type": "arrow"
        },
        {
          "id": "8oDhid4AzUldXlmXntLbl",
          "type": "arrow"
        }
      ],
      "updated": 1734361190878,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1966,
      "versionNonce": 1979164115,
      "index": "b1yOG",
      "isDeleted": false,
      "id": "9Vck9P2kgcYLnAHDKMCOz",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 745.9749986722633,
      "y": 2668.7751385405127,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.51666259765625,
      "height": 19.2,
      "seed": 840102114,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "DelegationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "HMOTYhgmt2kUR8Zl1vEhQ",
      "originalText": "DelegationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1766,
      "versionNonce": 1216914515,
      "index": "b1yOV",
      "isDeleted": false,
      "id": "miN4w27CQFNxPcL7apF3C",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 510.3126950504569,
      "y": 2934.771963937337,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1845920930,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "LH_Dxlusq3BFFI0jYoPF_"
        },
        {
          "id": "TTmKNlPiKnQPMyN-Awpoc",
          "type": "arrow"
        }
      ],
      "updated": 1734361190878,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1464,
      "versionNonce": 2134286835,
      "index": "b1yP",
      "isDeleted": false,
      "id": "LH_Dxlusq3BFFI0jYoPF_",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 551.4376950504569,
      "y": 2982.6719639373378,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1439801442,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPodManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "miN4w27CQFNxPcL7apF3C",
      "originalText": "EigenPodManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "ellipse",
      "version": 1364,
      "versionNonce": 316080435,
      "index": "b1yPV",
      "isDeleted": false,
      "id": "lciwwPyWIpXIstSEYo0zY",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 59.88610774887013,
      "y": 2662.416805207179,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 356729890,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "oRexr89IPDmIIbDsjlB_S"
        },
        {
          "id": "eDi3aeq3shWTkAvqzeyHF",
          "type": "arrow"
        }
      ],
      "updated": 1734361190878,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1118,
      "versionNonce": 761684691,
      "index": "b1yQ",
      "isDeleted": false,
      "id": "oRexr89IPDmIIbDsjlB_S",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 85.67884331567484,
      "y": 2703.2936991948854,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 105541602,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "lciwwPyWIpXIstSEYo0zY",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 1165,
      "versionNonce": 1223952915,
      "index": "b1yQG",
      "isDeleted": false,
      "id": "JoojGAxv1vLPsyFreqRYg",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 60.16467510843063,
      "y": 2854.76799568337,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 419.6333312988281,
      "height": 64.39999999999999,
      "seed": 1828070306,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nCompleting Withdrawal as Shares",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nCompleting Withdrawal as Shares",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 849,
      "versionNonce": 275206333,
      "index": "b1yQV",
      "isDeleted": false,
      "id": "eDi3aeq3shWTkAvqzeyHF",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 180.22970805664283,
      "y": 2726.7025563613306,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 414.0641377874654,
      "height": 2.2620428760758386,
      "seed": 863330146,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734361190885,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "lciwwPyWIpXIstSEYo0zY",
        "focus": 0.21762247876367327,
        "gap": 12.698369542443949,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "HMOTYhgmt2kUR8Zl1vEhQ",
        "focus": -0.8826906454351975,
        "gap": 3.5109126984114596,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          414.0641377874654,
          2.2620428760758386
        ]
      ]
    },
    {
      "type": "text",
      "version": 314,
      "versionNonce": 942727507,
      "index": "b1yR",
      "isDeleted": false,
      "id": "KPgJz2-1NWphbqelE6-3Z",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 226.16885093037138,
      "y": 2740.934662350036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 292.9166564941406,
      "height": 24,
      "seed": 639050530,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "completeQueuedWithdrawals",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "completeQueuedWithdrawals",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 411,
      "versionNonce": 684670707,
      "index": "b1yRV",
      "isDeleted": false,
      "id": "cEyESCGLOGOYOFv2oE-WX",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 190.66051963154325,
      "y": 2657.601329016703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 343.9333190917969,
      "height": 50,
      "seed": 793407202,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. After withdrawal delay, complete\nwithdrawal \"as shares\"",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. After withdrawal delay, complete\nwithdrawal \"as shares\"",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 664,
      "versionNonce": 513224691,
      "index": "b1yS",
      "isDeleted": false,
      "id": "TTmKNlPiKnQPMyN-Awpoc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 689.0054083910524,
      "y": 2744.267995683369,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 110.84284788508944,
      "height": 183.33333333333303,
      "seed": 329138850,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "hJNGWB18ZIUSw7JVTgc2K"
        }
      ],
      "updated": 1734363719393,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "HMOTYhgmt2kUR8Zl1vEhQ",
        "focus": 0.36883093359979874,
        "gap": 8.392857142856883,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "miN4w27CQFNxPcL7apF3C",
        "focus": -0.5657552947899162,
        "gap": 7.170634920635166,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -110.84284788508944,
          183.33333333333303
        ]
      ]
    },
    {
      "type": "text",
      "version": 180,
      "versionNonce": 2100058109,
      "index": "b1ySG",
      "isDeleted": false,
      "id": "hJNGWB18ZIUSw7JVTgc2K",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 534.7340656210931,
      "y": 2785.9346623500355,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 197.69983765482903,
      "height": 100,
      "seed": 767663714,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363718346,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Re-add deposit\nshares (by\nwithdrawable shares\namount)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "TTmKNlPiKnQPMyN-Awpoc",
      "originalText": "4. Re-add deposit shares (by withdrawable shares amount)",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 876,
      "versionNonce": 447503923,
      "index": "b1ySV",
      "isDeleted": false,
      "id": "FkjP9vIB5dinD6UIBMgml",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 923.3930521933146,
      "y": 2938.04180520718,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 713261602,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "5FKrWFg1DqsWunCD6mjB9"
        },
        {
          "id": "8oDhid4AzUldXlmXntLbl",
          "type": "arrow"
        }
      ],
      "updated": 1734361190878,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 612,
      "versionNonce": 747118547,
      "index": "b1yT",
      "isDeleted": false,
      "id": "5FKrWFg1DqsWunCD6mjB9",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 964.5180521933146,
      "y": 2985.9418052071796,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1154674146,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361190878,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "FkjP9vIB5dinD6UIBMgml",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 738,
      "versionNonce": 385988243,
      "index": "b1yTV",
      "isDeleted": false,
      "id": "8oDhid4AzUldXlmXntLbl",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 939.8126988370193,
      "y": 2742.9007368321436,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 152.49048544824382,
      "height": 186.66666666666606,
      "seed": 29286818,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "D41UobT9h1xxCvSzfzvM6"
        }
      ],
      "updated": 1734363721965,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "HMOTYhgmt2kUR8Zl1vEhQ",
        "focus": -0.2232350156608123,
        "gap": 7.025598291631468,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "FkjP9vIB5dinD6UIBMgml",
        "focus": 0.702364689952395,
        "gap": 8.474401708370351,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          152.49048544824382,
          186.66666666666606
        ]
      ]
    },
    {
      "type": "text",
      "version": 138,
      "versionNonce": 1892574045,
      "index": "b1yU",
      "isDeleted": false,
      "id": "D41UobT9h1xxCvSzfzvM6",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 917.2080227337267,
      "y": 2786.2340701654766,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 197.69983765482903,
      "height": 100,
      "seed": 1819696482,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363721257,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Re-add deposit\nshares (by\nwithdrawable shares\namount)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "8oDhid4AzUldXlmXntLbl",
      "originalText": "4. Re-add deposit shares (by withdrawable shares amount)",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 2427,
      "versionNonce": 393682109,
      "index": "b1yUG",
      "isDeleted": false,
      "id": "PQ83u0WAkad1B1o1C1iNi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 977.3043324551686,
      "y": 4051.854354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 455.8571428571431,
      "height": 114.99999999999997,
      "seed": 715186850,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "WxM9bgcZ3WGnjASzoklsN"
        },
        {
          "id": "AxtNwGQnRh8fqQAYXPcB8",
          "type": "arrow"
        },
        {
          "id": "-2NlzuMn6PNtlfulLdjA7",
          "type": "arrow"
        },
        {
          "id": "BEr_TOZmKQdO6AcwFrKWb",
          "type": "arrow"
        },
        {
          "id": "1YDohtzXZZQMtHn9rNLZR",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 2148,
      "versionNonce": 1531870493,
      "index": "b1yUV",
      "isDeleted": false,
      "id": "WxM9bgcZ3WGnjASzoklsN",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1125.474572584912,
      "y": 4099.754354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.51666259765625,
      "height": 19.2,
      "seed": 191905378,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "DelegationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "PQ83u0WAkad1B1o1C1iNi",
      "originalText": "DelegationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "ellipse",
      "version": 1466,
      "versionNonce": 153856765,
      "index": "b1yUl",
      "isDeleted": false,
      "id": "NQxdmwNFdM60dm5-JugHD",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 266.05234832818553,
      "y": 4100.062688282228,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 846299554,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "cor2pTfIUrALyeGhEkHT3"
        },
        {
          "id": "AxtNwGQnRh8fqQAYXPcB8",
          "type": "arrow"
        },
        {
          "id": "6wphfCZUznb3CeuzUuOTg",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1218,
      "versionNonce": 1003963229,
      "index": "b1yV",
      "isDeleted": false,
      "id": "cor2pTfIUrALyeGhEkHT3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 291.84508389499024,
      "y": 4140.939582269934,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 768933218,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "NQxdmwNFdM60dm5-JugHD",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 2048,
      "versionNonce": 306431101,
      "index": "b1yVG",
      "isDeleted": false,
      "id": "LsXGoQ1gtpREiel6zNv0A",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 367.58091823087784,
      "y": 4294.080545425084,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 250.46665954589844,
      "height": 128.79999999999998,
      "seed": 785580322,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nCompleting a Native\nETH Withdrawal\nAs Tokens",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nCompleting a Native\nETH Withdrawal\nAs Tokens",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 1381,
      "versionNonce": 287695827,
      "index": "b1yVV",
      "isDeleted": false,
      "id": "AxtNwGQnRh8fqQAYXPcB8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 386.5051575541728,
      "y": 4163.754307029607,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 587.2882622025842,
      "height": 2.0989984505267785,
      "seed": 819629282,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560521,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "NQxdmwNFdM60dm5-JugHD",
        "focus": 0.21767058936200775,
        "gap": 12.698184619725637,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "PQ83u0WAkad1B1o1C1iNi",
        "focus": -0.882690645435168,
        "gap": 3.51091269841163,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          587.2882622025842,
          -2.0989984505267785
        ]
      ]
    },
    {
      "type": "text",
      "version": 410,
      "versionNonce": 70289725,
      "index": "b1yW",
      "isDeleted": false,
      "id": "i-HUeFuIj_CNiK4aocZiK",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 452.3350915096869,
      "y": 4180.247212091752,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 292.9166564941406,
      "height": 24,
      "seed": 1769644194,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "completeQueuedWithdrawals",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "completeQueuedWithdrawals",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 531,
      "versionNonce": 925260189,
      "index": "b1yWV",
      "isDeleted": false,
      "id": "J8KNcgeeIIyPrjuezAJWw",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 433.4934268775253,
      "y": 4101.91387875842,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 343.9333190917969,
      "height": 50,
      "seed": 73546850,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. After withdrawal delay, complete\nwithdrawal \"as tokens\"",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. After withdrawal delay, complete\nwithdrawal \"as tokens\"",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1514,
      "versionNonce": 1069751805,
      "index": "b1yX",
      "isDeleted": false,
      "id": "kQ1fgu7rlXnxTswNeY2sY",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 666.9913364234233,
      "y": 4356.830183280432,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 800379198,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "Q4WYiLsmSd2ANtW4Fzf5C"
        },
        {
          "id": "-2NlzuMn6PNtlfulLdjA7",
          "type": "arrow"
        },
        {
          "id": "sEaeau6v0HW9Bw5m2-QLc",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1210,
      "versionNonce": 889738845,
      "index": "b1yXG",
      "isDeleted": false,
      "id": "Q4WYiLsmSd2ANtW4Fzf5C",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 708.1163364234233,
      "y": 4404.730183280432,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 96729470,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPodManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "kQ1fgu7rlXnxTswNeY2sY",
      "originalText": "EigenPodManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 2229,
      "versionNonce": 67745661,
      "index": "b1yXV",
      "isDeleted": false,
      "id": "-ByAz-Fdfvva8JaCOwxPA",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4896.3857388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1480858046,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2159,
      "versionNonce": 943701981,
      "index": "b1yY",
      "isDeleted": false,
      "id": "eMRf6JiZPdxP9G7OwFmLL",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4861.3857388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1582707198,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2108,
      "versionNonce": 492158013,
      "index": "b1yYV",
      "isDeleted": false,
      "id": "-3YXnHeLhfBTkpAIrjWHi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4825.8857388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1123762750,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2035,
      "versionNonce": 2066558109,
      "index": "b1yZ",
      "isDeleted": false,
      "id": "OMQvAoE8on6TXfPaYPM7k",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4790.8857388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 689195646,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2100,
      "versionNonce": 561484029,
      "index": "b1yZG",
      "isDeleted": false,
      "id": "lWvf1FuH4IJqchS8LLjgi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4760.1357388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 794970814,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2029,
      "versionNonce": 1840671069,
      "index": "b1yZV",
      "isDeleted": false,
      "id": "T3ieDPp5QgyBQ8v2i4_Hc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4725.1357388359875,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1954956030,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1974,
      "versionNonce": 289093053,
      "index": "b1ya",
      "isDeleted": false,
      "id": "oFgAfaubZA5gBAcdWlWX2",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4689.635738835988,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 453063486,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1909,
      "versionNonce": 388462109,
      "index": "b1yaV",
      "isDeleted": false,
      "id": "sJgKoBo4od3axAJJzTbFl",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.5468919789791,
      "y": 4654.635738835988,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1295260542,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "6wphfCZUznb3CeuzUuOTg",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1832,
      "versionNonce": 466375389,
      "index": "b1yb",
      "isDeleted": false,
      "id": "UicKTv5RzQFz_sRAvYCSx",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.9218919789791,
      "y": 4615.205183280432,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1096809406,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "FS4_skLwKgXHbZaPDZncL"
        },
        {
          "id": "sEaeau6v0HW9Bw5m2-QLc",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1565,
      "versionNonce": 668462909,
      "index": "b1ybV",
      "isDeleted": false,
      "id": "FS4_skLwKgXHbZaPDZncL",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 679.1218927419186,
      "y": 4653.505183280433,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 112.5999984741211,
      "height": 38.4,
      "seed": 1034277886,
      "groupIds": [
        "5pa49v7yiW1ejxPXqXnpp"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPods\n(1 per user)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "UicKTv5RzQFz_sRAvYCSx",
      "originalText": "EigenPods\n(1 per user)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 748,
      "versionNonce": 685733469,
      "index": "b1yd",
      "isDeleted": false,
      "id": "-2NlzuMn6PNtlfulLdjA7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 991.1236247335738,
      "y": 4171.271021615562,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 223.9610718898632,
      "height": 176.66666666666652,
      "seed": 1035097662,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "oRz68yZNH2lbKSZF1y-hq"
        }
      ],
      "updated": 1734363735302,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "PQ83u0WAkad1B1o1C1iNi",
        "focus": 0.4508229435372423,
        "gap": 4.41666666666697,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "kQ1fgu7rlXnxTswNeY2sY",
        "focus": -0.5178875121387579,
        "gap": 8.892494998202892,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -223.9610718898632,
          176.66666666666652
        ]
      ]
    },
    {
      "type": "text",
      "version": 112,
      "versionNonce": 1545484979,
      "index": "b1ydG",
      "isDeleted": false,
      "id": "oRz68yZNH2lbKSZF1y-hq",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 784.3631586738961,
      "y": 4234.604354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 189.5598602294922,
      "height": 50,
      "seed": 969993890,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363734241,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Withdraw shares\nas native eth",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "-2NlzuMn6PNtlfulLdjA7",
      "originalText": "3. Withdraw shares as native eth",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 487,
      "versionNonce": 1107673693,
      "index": "b1ydV",
      "isDeleted": false,
      "id": "sEaeau6v0HW9Bw5m2-QLc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 714.3058205504078,
      "y": 4474.604354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 30,
      "height": 131.66666666666652,
      "seed": 857568610,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "PGqyWD0wYyKYxhwnFlq99"
        }
      ],
      "updated": 1734363753816,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "kQ1fgu7rlXnxTswNeY2sY",
        "focus": 0.40490889053038814,
        "gap": 2.7741716684631683,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "UicKTv5RzQFz_sRAvYCSx",
        "focus": -0.8219842250215641,
        "gap": 8.934161664870771,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -30,
          131.66666666666652
        ]
      ]
    },
    {
      "type": "text",
      "version": 150,
      "versionNonce": 1797238451,
      "index": "b1ye",
      "isDeleted": false,
      "id": "PGqyWD0wYyKYxhwnFlq99",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 590.8558998961109,
      "y": 4502.937688282228,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 216.89984130859375,
      "height": 75,
      "seed": 376107390,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363752712,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. Withdraw ETH from\nproven \"withdrawable\"\nETH",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "sEaeau6v0HW9Bw5m2-QLc",
      "originalText": "4. Withdraw ETH from proven \"withdrawable\" ETH",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 2283,
      "versionNonce": 1483513213,
      "index": "b1yeV",
      "isDeleted": false,
      "id": "ov74QuZuHeQk5snpuex4j",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5672.694632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1321608034,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2214,
      "versionNonce": 190178781,
      "index": "b1yf",
      "isDeleted": false,
      "id": "1K3KAKEZc7hub6QqnfFkO",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5637.694632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 581920546,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "mGMTzes2F3Dpy73P0aXca",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2164,
      "versionNonce": 917971517,
      "index": "b1yfG",
      "isDeleted": false,
      "id": "KbInyH-gvwVMsVBnNRlsP",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5602.194632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 2017080034,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2091,
      "versionNonce": 1869730461,
      "index": "b1yfV",
      "isDeleted": false,
      "id": "ic5dIZ-8ueKkFPMVrRg3q",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5567.194632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 838174370,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "vj4BSnZm75z5L1-Bd7wmA",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2154,
      "versionNonce": 842495741,
      "index": "b1yg",
      "isDeleted": false,
      "id": "w8AHt8u9f13X41cx2ch_o",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5536.444632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 143680098,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2089,
      "versionNonce": 1770622813,
      "index": "b1ygV",
      "isDeleted": false,
      "id": "GEAj2lmmidRmUlCzXn9xj",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5501.444632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1188677154,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2028,
      "versionNonce": 867802045,
      "index": "b1yh",
      "isDeleted": false,
      "id": "k40q4mWXkGfhuTKm0wtdK",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5465.944632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 96514530,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "wl2N1yzHJY0w9n2-O6uBK",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1961,
      "versionNonce": 1760107549,
      "index": "b1yhG",
      "isDeleted": false,
      "id": "mCl_YXyLYkCML2D84GHb_",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.3638562646935,
      "y": 5430.944632726673,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1750345122,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "wl2N1yzHJY0w9n2-O6uBK",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1883,
      "versionNonce": 1628016765,
      "index": "b1yhV",
      "isDeleted": false,
      "id": "EIr7yDWcf5XOCl3LVxa0f",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 508.7388562646935,
      "y": 5391.5140771711185,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 1472405858,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "G35L9DJWDWkC_VH8bd05f"
        },
        {
          "id": "wl2N1yzHJY0w9n2-O6uBK",
          "type": "arrow"
        },
        {
          "id": "vj4BSnZm75z5L1-Bd7wmA",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1619,
      "versionNonce": 2044491997,
      "index": "b1yi",
      "isDeleted": false,
      "id": "G35L9DJWDWkC_VH8bd05f",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 519.9388570276329,
      "y": 5429.814077171118,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 112.5999984741211,
      "height": 38.4,
      "seed": 1063036194,
      "groupIds": [
        "-a95jHDTnDXw5D9TZzWPv"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPods\n(1 per user)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "EIr7yDWcf5XOCl3LVxa0f",
      "originalText": "EigenPods\n(1 per user)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1866,
      "versionNonce": 121825789,
      "index": "b1yiV",
      "isDeleted": false,
      "id": "6dE977xJsK71ptktUgxTH",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 916.8638562646933,
      "y": 5437.527966060007,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 266.00000000000017,
      "height": 114.99999999999997,
      "seed": 69099618,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "LpzmdEWZoAuxFhQksffXs"
        },
        {
          "id": "wl2N1yzHJY0w9n2-O6uBK",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1765,
      "versionNonce": 313434717,
      "index": "b1yj",
      "isDeleted": false,
      "id": "LpzmdEWZoAuxFhQksffXs",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 979.4888562646934,
      "y": 5485.427966060007,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1542980642,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EIP-4788 Oracle",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "6dE977xJsK71ptktUgxTH",
      "originalText": "EIP-4788 Oracle",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 2118,
      "versionNonce": 1971038611,
      "index": "b1yjV",
      "isDeleted": false,
      "id": "wl2N1yzHJY0w9n2-O6uBK",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 651.2829108238828,
      "y": 5506.268772499644,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 258.0630882979534,
      "height": 0.06671593128066888,
      "seed": 409415650,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560521,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "EIr7yDWcf5XOCl3LVxa0f",
        "focus": 0.9957690346543716,
        "gap": 7.544054559189192,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "6dE977xJsK71ptktUgxTH",
        "focus": -0.19358446686072192,
        "gap": 7.5178571428570535,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          258.0630882979534,
          -0.06671593128066888
        ]
      ]
    },
    {
      "type": "text",
      "version": 767,
      "versionNonce": 390300541,
      "index": "b1yk",
      "isDeleted": false,
      "id": "suYlO5trpiXH7DPcsPva-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 659.5840943599316,
      "y": 5439.057727964769,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 241.89999389648438,
      "height": 50,
      "seed": 494592930,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "wl2N1yzHJY0w9n2-O6uBK",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1.5 Parent beacon block\nroot queried from oracle",
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1.5 Parent beacon block\nroot queried from oracle",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "ellipse",
      "version": 1588,
      "versionNonce": 1516243933,
      "index": "b1ykV",
      "isDeleted": false,
      "id": "fu5u9qb7f5q6a-Xge9KFW",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 39.081118169455294,
      "y": 5470.079915506246,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 1969364834,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "zoX-NdDWsLZeKla7uZsJa"
        },
        {
          "id": "vj4BSnZm75z5L1-Bd7wmA",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1336,
      "versionNonce": 764184637,
      "index": "b1yl",
      "isDeleted": false,
      "id": "zoX-NdDWsLZeKla7uZsJa",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 64.87385373626,
      "y": 5510.956809493952,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 208397090,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "fu5u9qb7f5q6a-Xge9KFW",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 908,
      "versionNonce": 670474451,
      "index": "b1ylG",
      "isDeleted": false,
      "id": "vj4BSnZm75z5L1-Bd7wmA",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 149.63115028729248,
      "y": 5490.368685685094,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 356.86961092156184,
      "height": 1.7933079596450625,
      "seed": 1829187298,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560521,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "fu5u9qb7f5q6a-Xge9KFW",
        "focus": -0.6081621304143124,
        "gap": 10.858235443211711,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "EIr7yDWcf5XOCl3LVxa0f",
        "focus": -0.677928857055987,
        "gap": 2.23809505583921,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          356.86961092156184,
          -1.7933079596450625
        ]
      ]
    },
    {
      "type": "text",
      "version": 366,
      "versionNonce": 1196471645,
      "index": "b1ylV",
      "isDeleted": false,
      "id": "8BahsLtuF7IbIbOLx2-Mx",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 227.2477848361218,
      "y": 5494.246582172914,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 175.75,
      "height": 24,
      "seed": 2076377762,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "vj4BSnZm75z5L1-Bd7wmA",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "startCheckpoint",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "startCheckpoint",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 569,
      "versionNonce": 1504444861,
      "index": "b1ym",
      "isDeleted": false,
      "id": "_4N_XcbOGQl5IcAcywLwz",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 220.72278331024324,
      "y": 5392.579915506248,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 168.8000030517578,
      "height": 25,
      "seed": 860773986,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "0. Exit validator",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "0. Exit validator",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 2120,
      "versionNonce": 798933533,
      "index": "b1ymV",
      "isDeleted": false,
      "id": "d72WGNKRuYgAsUjbtzORG",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 71.25582258491318,
      "y": 5302.404354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 322.76666259765625,
      "height": 64.39999999999999,
      "seed": 1006649378,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nProcessing Validator Exits",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nProcessing Validator Exits",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 579,
      "versionNonce": 1807838109,
      "index": "b1yn",
      "isDeleted": false,
      "id": "6wphfCZUznb3CeuzUuOTg",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 661.8188413837411,
      "y": 4783.347844532228,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 339.50691489593646,
      "height": 566.5762032002181,
      "seed": 827744894,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "g7WtYgMLCoPIyK2GZ6v0S"
        }
      ],
      "updated": 1734363759259,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "sJgKoBo4od3axAJJzTbFl",
        "focus": -1.171032254566488,
        "gap": 13.712105696239632,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "NQxdmwNFdM60dm5-JugHD",
        "focus": 0.030507353609042952,
        "gap": 11.73505335639934,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -320,
          -213.33333333333348
        ],
        [
          -339.50691489593646,
          -566.5762032002181
        ]
      ]
    },
    {
      "type": "text",
      "version": 57,
      "versionNonce": 1123936627,
      "index": "b1ynG",
      "isDeleted": false,
      "id": "g7WtYgMLCoPIyK2GZ6v0S",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 223.50893168239372,
      "y": 4545.014511198895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 236.6198194026947,
      "height": 50,
      "seed": 958894142,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363758637,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "5. Send ETH directly to\nStaker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "6wphfCZUznb3CeuzUuOTg",
      "originalText": "5. Send ETH directly to Staker",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 939,
      "versionNonce": 285232563,
      "index": "b1ynV",
      "isDeleted": false,
      "id": "mGMTzes2F3Dpy73P0aXca",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 667.4588196812585,
      "y": 5737.719971933378,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 174.472005955412,
      "height": 1.7402432100288934,
      "seed": 2136924002,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560522,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": {
        "elementId": "i9Mz6DQh6ztmSH01qUZ0E",
        "focus": -0.1511269098788724,
        "gap": 12.049992879231922,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          174.472005955412,
          -1.7402432100288934
        ]
      ]
    },
    {
      "type": "text",
      "version": 479,
      "versionNonce": 998423357,
      "index": "b1yo",
      "isDeleted": false,
      "id": "i9Mz6DQh6ztmSH01qUZ0E",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 853.9808185159025,
      "y": 5676.271021615562,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 353.98333740234375,
      "height": 100,
      "seed": 8192610,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "mGMTzes2F3Dpy73P0aXca",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Any native ETH in pod is\nawarded shares and is made\n\"withdrawable\" via DelegationManger\nwithdrawal queue",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "3. Any native ETH in pod is\nawarded shares and is made\n\"withdrawable\" via DelegationManger\nwithdrawal queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "ellipse",
      "version": 1225,
      "versionNonce": 577240733,
      "index": "b1yoV",
      "isDeleted": false,
      "id": "xZf-MkC7-tDuT1_y0E2RJ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2251.5302860009665,
      "y": 622.5598244224238,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 616463010,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "E5Mvud8IjUlm4Xgjw3ocp"
        },
        {
          "id": "B_6S76kMy7S19TN-Xpgxi",
          "type": "arrow"
        }
      ],
      "updated": 1734360257984,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 980,
      "versionNonce": 2106073853,
      "index": "b1yp",
      "isDeleted": false,
      "id": "E5Mvud8IjUlm4Xgjw3ocp",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2277.3230215677713,
      "y": 663.4367184101302,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 505075298,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360257984,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "xZf-MkC7-tDuT1_y0E2RJ",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 559,
      "versionNonce": 368012669,
      "index": "b1ypG",
      "isDeleted": false,
      "id": "K4Gd0EEsZCk0uKQWEsgtd",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1865.175987485854,
      "y": 336.226190476191,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 201.31666564941406,
      "height": 64.39999999999999,
      "seed": 1488135394,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734113298835,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nDepositing LSTs",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nDepositing LSTs",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "rectangle",
      "version": 1248,
      "versionNonce": 880640061,
      "index": "b1ypV",
      "isDeleted": false,
      "id": "wVMAbbDeEbMp5p52aabU-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1652.7864327916523,
      "y": 615.4846062981054,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1175012030,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "ZlMmZXV0dGL0ZOKWCdMOu"
        },
        {
          "id": "B_6S76kMy7S19TN-Xpgxi",
          "type": "arrow"
        },
        {
          "id": "Bj6zp7ALVlRBocmVnGWMH",
          "type": "arrow"
        },
        {
          "id": "Who6V4OKBA7YOdSKks5WU",
          "type": "arrow"
        },
        {
          "id": "lS4fRSLZHIKN77q4fwuWo",
          "type": "arrow"
        }
      ],
      "updated": 1734360230418,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 980,
      "versionNonce": 1622962333,
      "index": "b1yq",
      "isDeleted": false,
      "id": "ZlMmZXV0dGL0ZOKWCdMOu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1693.9114327916523,
      "y": 663.3846062981054,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 193870590,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360230418,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "wVMAbbDeEbMp5p52aabU-",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1612,
      "versionNonce": 291578333,
      "index": "b1yqV",
      "isDeleted": false,
      "id": "PzimP3lWTrcDzRjSEXUDX",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1596.2627015786109,
      "y": 1046.5568984271035,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 2118789950,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360231907,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1562,
      "versionNonce": 1341840957,
      "index": "b1yr",
      "isDeleted": false,
      "id": "VYih8IVbgosYSpf1fsC7r",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1595.2627015786109,
      "y": 1012.5568984271036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1285428094,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360231907,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1558,
      "versionNonce": 551423645,
      "index": "b1yrV",
      "isDeleted": false,
      "id": "66oUWESs2szy5Tu2IR57k",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1593.7627015786109,
      "y": 979.5568984271036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 818607038,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360231907,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1508,
      "versionNonce": 1656500989,
      "index": "b1ys",
      "isDeleted": false,
      "id": "0stGgUmnEj0FJWSOQa2_X",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1592.7627015786109,
      "y": 945.5568984271036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1962003454,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734360231907,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1351,
      "versionNonce": 571919197,
      "index": "b1ysV",
      "isDeleted": false,
      "id": "oUz79H06A1yzhMam5WMvD",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1592.7627015786109,
      "y": 910.5568984271036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1997773886,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "gDqcJw586XS90Eux8eqy7"
        },
        {
          "id": "Bj6zp7ALVlRBocmVnGWMH",
          "type": "arrow"
        },
        {
          "id": "Who6V4OKBA7YOdSKks5WU",
          "type": "arrow"
        }
      ],
      "updated": 1734360231907,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1175,
      "versionNonce": 1597986749,
      "index": "b1yt",
      "isDeleted": false,
      "id": "gDqcJw586XS90Eux8eqy7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1599.9710374550757,
      "y": 948.8568984271036,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 234.5833282470703,
      "height": 38.4,
      "seed": 756114558,
      "groupIds": [
        "KsktPBVCh0m-8eJ1ax8wi"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360231907,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "oUz79H06A1yzhMam5WMvD",
      "originalText": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 454,
      "versionNonce": 1654092627,
      "index": "b1ytG",
      "isDeleted": false,
      "id": "c1SY7VGafpSUMHtyH25Xr",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1817.580154661147,
      "y": 426.1428571428572,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 313.4666748046875,
      "height": 50,
      "seed": 1218017826,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734113298835,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "0. Approve StrategyManager to\ntransfer deposit amounts",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "0. Approve StrategyManager to\ntransfer deposit amounts",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 1191,
      "versionNonce": 1575271261,
      "index": "b1ytV",
      "isDeleted": false,
      "id": "B_6S76kMy7S19TN-Xpgxi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2244.2656001597725,
      "y": 671.1629088833433,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 360.5783737173265,
      "height": 3.2202684628167617,
      "seed": 1077347490,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734360257984,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "xZf-MkC7-tDuT1_y0E2RJ",
        "focus": 0.08470894448090276,
        "gap": 7.39556321554889
      },
      "endBinding": {
        "elementId": "wVMAbbDeEbMp5p52aabU-",
        "focus": 0.16494087647740363,
        "gap": 7.90079365079373
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -360.5783737173265,
          3.2202684628167617
        ]
      ]
    },
    {
      "type": "text",
      "version": 240,
      "versionNonce": 1219520061,
      "index": "b1yu",
      "isDeleted": false,
      "id": "OR7_5x1L3ZEbhEVKLMUwR",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1961.8665323648602,
      "y": 694.4699893236942,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 222.61666870117188,
      "height": 24,
      "seed": 953797474,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360260040,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "depositIntoStrategy",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "depositIntoStrategy",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 265,
      "versionNonce": 272327741,
      "index": "b1yuV",
      "isDeleted": false,
      "id": "dPD4J6PBBhuD02bxds9p7",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1989.9982692537926,
      "y": 635.0022298479423,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 156.03334045410156,
      "height": 25,
      "seed": 1544827362,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360262644,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Deposit LSTs",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. Deposit LSTs",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 1334,
      "versionNonce": 341268509,
      "index": "b1yv",
      "isDeleted": false,
      "id": "Bj6zp7ALVlRBocmVnGWMH",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1731.593808711268,
      "y": 737.9171459806447,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 117.08400107865964,
      "height": 163.37387943058638,
      "seed": 801795390,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "eP99TQkpI2x9f7BJ2cKb-"
        }
      ],
      "updated": 1734360231907,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "wVMAbbDeEbMp5p52aabU-",
        "focus": -0.16112741540408798,
        "gap": 7.4325396825393
      },
      "endBinding": {
        "elementId": "oUz79H06A1yzhMam5WMvD",
        "focus": -0.908836098861225,
        "gap": 9.265873015872558
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -117.08400107865964,
          163.37387943058638
        ]
      ]
    },
    {
      "type": "text",
      "version": 51,
      "versionNonce": 1519847901,
      "index": "b1yvG",
      "isDeleted": false,
      "id": "eP99TQkpI2x9f7BJ2cKb-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1746.2220671702282,
      "y": 743.9464285714288,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 191.61984622478485,
      "height": 50,
      "seed": 72803874,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360226997,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Transfer tokens\nto strategy",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "Bj6zp7ALVlRBocmVnGWMH",
      "originalText": "2. Transfer tokens to strategy",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 671,
      "versionNonce": 1692118035,
      "index": "b1yvV",
      "isDeleted": false,
      "id": "Who6V4OKBA7YOdSKks5WU",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1850.9902149301315,
      "y": 921.4976590025567,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 116.88516834619963,
      "height": 200.6323634923193,
      "seed": 1310791522,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "TSrje3lowDMZwQ1PERCdD"
        }
      ],
      "updated": 1734360323660,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "oUz79H06A1yzhMam5WMvD",
        "focus": 0.34684392252796814,
        "gap": 9.227513351520656,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "wVMAbbDeEbMp5p52aabU-",
        "focus": -0.39267292129709724,
        "gap": 1,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          116.88516834619963,
          -85.84845852014746
        ],
        [
          17.863239999640427,
          -200.6323634923193
        ]
      ]
    },
    {
      "type": "text",
      "version": 152,
      "versionNonce": 1615275485,
      "index": "b1yw",
      "isDeleted": false,
      "id": "TSrje3lowDMZwQ1PERCdD",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1874.705446142542,
      "y": 810.6492004824092,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 186.33987426757812,
      "height": 50,
      "seed": 559498850,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360322544,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Staker awarded\ndeposit shares",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "Who6V4OKBA7YOdSKks5WU",
      "originalText": "3. Staker awarded deposit shares",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 317,
      "versionNonce": 1888925363,
      "index": "b1ywV",
      "isDeleted": false,
      "id": "lS4fRSLZHIKN77q4fwuWo",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1703.2798349482075,
      "y": 606.7107967742961,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 244.89245252218734,
      "height": 258.48460629810546,
      "seed": 1253663550,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "6PJ_9NCrM6WpCh8MNp_fj"
        }
      ],
      "updated": 1734361144598,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "wVMAbbDeEbMp5p52aabU-",
        "focus": 0.010738090831398414,
        "gap": 8.773809523809291,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "d7pRFde2mvhA75ZinOGRD",
        "focus": -0.5765561345526261,
        "gap": 6.011904761905015,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -244.89245252218734,
          -258.48460629810546
        ]
      ]
    },
    {
      "type": "text",
      "version": 139,
      "versionNonce": 215084349,
      "index": "b1yx",
      "isDeleted": false,
      "id": "6PJ_9NCrM6WpCh8MNp_fj",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1475.87370878477,
      "y": 402.46849362524335,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 209.9197998046875,
      "height": 150,
      "seed": 1985941538,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361142777,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. If Staker is\ndelegated, update\nOperator's delegated\nshares based on\nstaker's increased\ndeposit shares",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "lS4fRSLZHIKN77q4fwuWo",
      "originalText": "4. If Staker is delegated, update Operator's delegated shares based on staker's increased deposit shares",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 2191,
      "versionNonce": 450935581,
      "index": "b1yxG",
      "isDeleted": false,
      "id": "Njo4VT6aJS-BOBxW_rIMR",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2502.4901691502105,
      "y": 2039.71548848204,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 455.8571428571431,
      "height": 114.99999999999997,
      "seed": 684382434,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "S7nHBIf33C-pylj1fs2dG"
        },
        {
          "id": "hNnYwYsWakliX5fkmwJVb",
          "type": "arrow"
        },
        {
          "id": "b0llJrpSAPPddIptJHKYn",
          "type": "arrow"
        },
        {
          "id": "cGXQvdtzHLMM12Ot3tcts",
          "type": "arrow"
        }
      ],
      "updated": 1734361197598,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1922,
      "versionNonce": 1354059645,
      "index": "b1yxV",
      "isDeleted": false,
      "id": "S7nHBIf33C-pylj1fs2dG",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2650.660409279954,
      "y": 2087.61548848204,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.51666259765625,
      "height": 19.2,
      "seed": 360803490,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "DelegationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "Njo4VT6aJS-BOBxW_rIMR",
      "originalText": "DelegationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1721,
      "versionNonce": 1335306493,
      "index": "b1yy",
      "isDeleted": false,
      "id": "fMrtOCuZyykx1UE2GS84u",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2414.998105658148,
      "y": 2353.6123138788653,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 498042978,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "LNX2NfmyO99AzTqRwXqZk"
        },
        {
          "id": "b0llJrpSAPPddIptJHKYn",
          "type": "arrow"
        }
      ],
      "updated": 1734361197598,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1420,
      "versionNonce": 1680015709,
      "index": "b1yyV",
      "isDeleted": false,
      "id": "LNX2NfmyO99AzTqRwXqZk",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2456.123105658148,
      "y": 2401.512313878865,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 2032413730,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPodManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "fMrtOCuZyykx1UE2GS84u",
      "originalText": "EigenPodManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "ellipse",
      "version": 1334,
      "versionNonce": 491654685,
      "index": "b1yz",
      "isDeleted": false,
      "id": "pYX7-scbCCY5JSKmZ1I-q",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1994.5715183565608,
      "y": 2081.2571551487063,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 231626722,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "7ExJPMtL5cEUiKWn321Sh"
        },
        {
          "id": "hNnYwYsWakliX5fkmwJVb",
          "type": "arrow"
        }
      ],
      "updated": 1734361197598,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1088,
      "versionNonce": 1610629757,
      "index": "b1yzG",
      "isDeleted": false,
      "id": "7ExJPMtL5cEUiKWn321Sh",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2020.3642539233656,
      "y": 2122.1340491364126,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 733809570,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "pYX7-scbCCY5JSKmZ1I-q",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 1068,
      "versionNonce": 258311997,
      "index": "b1yzV",
      "isDeleted": false,
      "id": "ctarScP_mj0QeE5OZNAlr",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2021.2500897851316,
      "y": 2268.608345624897,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 320.1666564941406,
      "height": 64.39999999999999,
      "seed": 1226786658,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nDelegating to an Operator",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nDelegating to an Operator",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 763,
      "versionNonce": 1016613107,
      "index": "b1yzl",
      "isDeleted": false,
      "id": "hNnYwYsWakliX5fkmwJVb",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2114.9106506143335,
      "y": 2145.5654144347063,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 379.0686058374654,
      "height": 2.7707857282800887,
      "seed": 1044646690,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734361197728,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "pYX7-scbCCY5JSKmZ1I-q",
        "focus": 0.23409655429971704,
        "gap": 12.698164422728148,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "Njo4VT6aJS-BOBxW_rIMR",
        "focus": -0.7411498268808536,
        "gap": 8.51091269841163,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          379.0686058374654,
          -2.7707857282800887
        ]
      ]
    },
    {
      "type": "text",
      "version": 253,
      "versionNonce": 1683004413,
      "index": "b1z",
      "isDeleted": false,
      "id": "pjE8l-z9Co8dDH2mNSC-0",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2218.7292577233648,
      "y": 2159.775012291564,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 117.16666412353516,
      "height": 24,
      "seed": 26892002,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "delegateTo",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "delegateTo",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 319,
      "versionNonce": 657855581,
      "index": "b20",
      "isDeleted": false,
      "id": "3yhssR2VfkcGJ9mtnM6dJ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2129.2042574690518,
      "y": 2076.441678958231,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 272.8833312988281,
      "height": 50,
      "seed": 1672555170,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Delegate to a registered\nOperator",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. Delegate to a registered\nOperator",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 522,
      "versionNonce": 1664804915,
      "index": "b21",
      "isDeleted": false,
      "id": "b0llJrpSAPPddIptJHKYn",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2594.0753392814922,
      "y": 2163.108345624897,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 110.84284788508944,
      "height": 183.33333333333303,
      "seed": 1261930082,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "dIrPhtpgOibEaRGdpQA3n"
        }
      ],
      "updated": 1734361197728,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "Njo4VT6aJS-BOBxW_rIMR",
        "focus": 0.36736717019133747,
        "gap": 8.392857142857451,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "fMrtOCuZyykx1UE2GS84u",
        "focus": -0.5631263541856226,
        "gap": 7.170634920635166,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -110.84284788508944,
          183.33333333333303
        ]
      ]
    },
    {
      "type": "text",
      "version": 161,
      "versionNonce": 1197778419,
      "index": "b22",
      "isDeleted": false,
      "id": "dIrPhtpgOibEaRGdpQA3n",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1947.605846592538,
      "y": 1558.0357142857135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 191.73980712890625,
      "height": 75,
      "seed": 1895590434,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360637084,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Query number of\ndeposit shares held\nby Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "b0llJrpSAPPddIptJHKYn",
      "originalText": "2. Query number of deposit shares held by Staker",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 830,
      "versionNonce": 788029725,
      "index": "b23",
      "isDeleted": false,
      "id": "iF4Go9CXS_-O1rd3mGeIe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2828.078462801005,
      "y": 2356.882155148707,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 468637154,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "dOm_3_POehmCJNweKFBRR"
        },
        {
          "id": "cGXQvdtzHLMM12Ot3tcts",
          "type": "arrow"
        }
      ],
      "updated": 1734361197598,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 567,
      "versionNonce": 250099069,
      "index": "b24",
      "isDeleted": false,
      "id": "dOm_3_POehmCJNweKFBRR",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2869.203462801005,
      "y": 2404.782155148707,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1562233250,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361197598,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "iF4Go9CXS_-O1rd3mGeIe",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 590,
      "versionNonce": 1722726259,
      "index": "b25",
      "isDeleted": false,
      "id": "cGXQvdtzHLMM12Ot3tcts",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2844.0631322883887,
      "y": 2161.7410867736708,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 152.49048544824382,
      "height": 186.66666666666606,
      "seed": 1270367586,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "FnnLkQFKdQqwVYvgZ3Chu"
        }
      ],
      "updated": 1734361197728,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "Njo4VT6aJS-BOBxW_rIMR",
        "focus": -0.22165271141817774,
        "gap": 7.025598291631127,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "iF4Go9CXS_-O1rd3mGeIe",
        "focus": 0.6996198786224589,
        "gap": 8.474401708370351,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          152.49048544824382,
          186.66666666666606
        ]
      ]
    },
    {
      "type": "text",
      "version": 157,
      "versionNonce": 1058245821,
      "index": "b26",
      "isDeleted": false,
      "id": "FnnLkQFKdQqwVYvgZ3Chu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2329.260306266101,
      "y": 1558.3351221011537,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 191.73980712890625,
      "height": 75,
      "seed": 1091389730,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734360640718,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Query number of\ndeposit shares held\nby Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "cGXQvdtzHLMM12Ot3tcts",
      "originalText": "2. Query number of deposit shares held by Staker",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 612,
      "versionNonce": 1407208029,
      "index": "b27",
      "isDeleted": false,
      "id": "VT_j7-YBBoCet0xKP8qhO",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2988.406633433543,
      "y": 2059.002177039263,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 411.15966796875,
      "height": 100,
      "seed": 1915095486,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734364477445,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. If staker is delegated, add delegated\nshares to Operator\n(increased by deposit shares amount)\n",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "4. If staker is delegated, add delegated\nshares to Operator\n(increased by deposit shares amount)\n",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "ellipse",
      "version": 1439,
      "versionNonce": 1773195261,
      "index": "b28",
      "isDeleted": false,
      "id": "ww6cJfMczV8T_u_QeDNDu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1930.4582723589,
      "y": 4085.798557781826,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 793558526,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "f3pJyffpDIooPa2lvpCDJ"
        },
        {
          "id": "BEr_TOZmKQdO6AcwFrKWb",
          "type": "arrow"
        },
        {
          "id": "VxgaSm93O4OlqrZMY57Iu",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1192,
      "versionNonce": 2054146141,
      "index": "b29",
      "isDeleted": false,
      "id": "f3pJyffpDIooPa2lvpCDJ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1956.251007925705,
      "y": 4126.675451769532,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 1296059966,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "ww6cJfMczV8T_u_QeDNDu",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "text",
      "version": 926,
      "versionNonce": 1485728125,
      "index": "b2A",
      "isDeleted": false,
      "id": "n5kMVYG-Ovvk81SmcoVgI",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1645.111834632196,
      "y": 4283.149748258017,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 274.2166748046875,
      "height": 96.6,
      "seed": 1054332542,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nCompleting an LST\nWithdrawal As Tokens",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nCompleting an LST\nWithdrawal As Tokens",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "rectangle",
      "version": 1439,
      "versionNonce": 1029583325,
      "index": "b2B",
      "isDeleted": false,
      "id": "YEevKYXKSnAcEpK4SoXAN",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1375.6318834700096,
      "y": 4369.756891115159,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1557419710,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "2_63cFNXHKARWqWOScRsc"
        },
        {
          "id": "1YDohtzXZZQMtHn9rNLZR",
          "type": "arrow"
        },
        {
          "id": "LKXfIwH6k9YoTbRX_b9gI",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1166,
      "versionNonce": 433450557,
      "index": "b2C",
      "isDeleted": false,
      "id": "2_63cFNXHKARWqWOScRsc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1416.7568834700096,
      "y": 4417.656891115159,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1071042302,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "YEevKYXKSnAcEpK4SoXAN",
      "originalText": "StrategyManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1885,
      "versionNonce": 520870749,
      "index": "b2D",
      "isDeleted": false,
      "id": "znJmTtSHOG4nKL_lIS9OI",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1620.1001374382645,
      "y": 4791.48308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 1147158334,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1835,
      "versionNonce": 454191037,
      "index": "b2E",
      "isDeleted": false,
      "id": "TnoxboQXJOxVczGEBb3hq",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1619.1001374382645,
      "y": 4757.48308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 248620926,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1831,
      "versionNonce": 55076893,
      "index": "b2F",
      "isDeleted": false,
      "id": "rk_4mrycfQe0nkdZf3KYv",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1617.6001374382645,
      "y": 4724.48308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 258738110,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1781,
      "versionNonce": 161438845,
      "index": "b2G",
      "isDeleted": false,
      "id": "TAs2gzucG2h_8VB50-1i1",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1616.6001374382645,
      "y": 4690.48308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 105930750,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1628,
      "versionNonce": 634963165,
      "index": "b2H",
      "isDeleted": false,
      "id": "GofJhEttiaSCMMbeZPpHJ",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1616.6001374382645,
      "y": 4655.48308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#a5d8ff",
      "width": 249.00000000000009,
      "height": 114.99999999999997,
      "seed": 125808702,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "yWGNhTGz9Jvd0dUba1m5W"
        },
        {
          "id": "LKXfIwH6k9YoTbRX_b9gI",
          "type": "arrow"
        },
        {
          "id": "VxgaSm93O4OlqrZMY57Iu",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1448,
      "versionNonce": 1535516989,
      "index": "b2I",
      "isDeleted": false,
      "id": "yWGNhTGz9Jvd0dUba1m5W",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1623.8084733147293,
      "y": 4693.78308159135,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 234.5833282470703,
      "height": 38.4,
      "seed": 2128279678,
      "groupIds": [
        "TqASbmNKtAb95Cc8uANEn"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "GofJhEttiaSCMMbeZPpHJ",
      "originalText": "StrategyBaseWithTVLLimits\n(1 per LST)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 810,
      "versionNonce": 192462067,
      "index": "b2J",
      "isDeleted": false,
      "id": "BEr_TOZmKQdO6AcwFrKWb",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1921.2082144161936,
      "y": 4154.944773749827,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 483.5690605324512,
      "height": 0.030639649032764282,
      "seed": 1794559294,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560522,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "ww6cJfMczV8T_u_QeDNDu",
        "focus": -0.31714756780341136,
        "gap": 11.516308661733518,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "PQ83u0WAkad1B1o1C1iNi",
        "focus": 0.7918889943459513,
        "gap": 4.477678571430715,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          -483.5690605324512,
          -0.030639649032764282
        ]
      ]
    },
    {
      "type": "text",
      "version": 250,
      "versionNonce": 1704008381,
      "index": "b2K",
      "isDeleted": false,
      "id": "kJlCKO6JhXQwzcWVF_58Z",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1530.672494337844,
      "y": 4091.2710216155624,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 343.9333190917969,
      "height": 50,
      "seed": 1863111650,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. After withdrawal delay, complete\nwithdrawal \"as tokens\"",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. After withdrawal delay, complete\nwithdrawal \"as tokens\"",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 234,
      "versionNonce": 293124893,
      "index": "b2L",
      "isDeleted": false,
      "id": "ElfgCTQ44t8xdLsJ7WzOF",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1551.1808256366717,
      "y": 4172.937688282229,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 292.9166564941406,
      "height": 24,
      "seed": 412788578,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "completeQueuedWithdrawals",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "completeQueuedWithdrawals",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 707,
      "versionNonce": 724323485,
      "index": "b2M",
      "isDeleted": false,
      "id": "1YDohtzXZZQMtHn9rNLZR",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1376.7635585898329,
      "y": 4174.604354948896,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 99.6956610325094,
      "height": 188.33333333333303,
      "seed": 1957123774,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "BBzLnTxvEeFogjjEpl2V8"
        }
      ],
      "updated": 1734363737996,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "PQ83u0WAkad1B1o1C1iNi",
        "focus": -0.5302158402110009,
        "gap": 7.7500000000009095,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "YEevKYXKSnAcEpK4SoXAN",
        "focus": 0.16468584082485693,
        "gap": 6.819202832930387,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          99.6956610325094,
          188.33333333333303
        ]
      ]
    },
    {
      "type": "text",
      "version": 41,
      "versionNonce": 1354586227,
      "index": "b2N",
      "isDeleted": false,
      "id": "BBzLnTxvEeFogjjEpl2V8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1331.8314589913416,
      "y": 4243.771021615563,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 189.5598602294922,
      "height": 50,
      "seed": 1202388322,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363737061,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Withdraw shares\nas LST tokens",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "1YDohtzXZZQMtHn9rNLZR",
      "originalText": "3. Withdraw shares as LST tokens",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 701,
      "versionNonce": 712547421,
      "index": "b2O",
      "isDeleted": false,
      "id": "LKXfIwH6k9YoTbRX_b9gI",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1489.0907593444526,
      "y": 4492.937688282229,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 119.11315367029124,
      "height": 159.7789287411406,
      "seed": 1486747326,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "N0XoOQw-ax-DpKJhTrhTS"
        }
      ],
      "updated": 1734363743439,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "YEevKYXKSnAcEpK4SoXAN",
        "focus": 0.3045066137617944,
        "gap": 8.180797167069613,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "GofJhEttiaSCMMbeZPpHJ",
        "focus": -0.5256061429217742,
        "gap": 8.396224423520607,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          119.11315367029124,
          159.7789287411406
        ]
      ]
    },
    {
      "type": "text",
      "version": 103,
      "versionNonce": 410580147,
      "index": "b2P",
      "isDeleted": false,
      "id": "N0XoOQw-ax-DpKJhTrhTS",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1439.2874048447452,
      "y": 4535.327152652799,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 218.71986266970634,
      "height": 75,
      "seed": 1358753598,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363742254,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "4. For each LST being\nwithdrawn, withdraw\nLST tokens",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "LKXfIwH6k9YoTbRX_b9gI",
      "originalText": "4. For each LST being withdrawn, withdraw LST tokens",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 536,
      "versionNonce": 1018931859,
      "index": "b2Q",
      "isDeleted": false,
      "id": "VxgaSm93O4OlqrZMY57Iu",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1839.7029559670757,
      "y": 4642.937688282229,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 143.33333333333348,
      "height": 435,
      "seed": 2030782014,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "9xUkxgnYx5-Dk5fyPHOh8"
        }
      ],
      "updated": 1734363747910,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "GofJhEttiaSCMMbeZPpHJ",
        "focus": 0.2691884539496283,
        "gap": 12.54539330912121,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "ww6cJfMczV8T_u_QeDNDu",
        "focus": 0.03304796429408594,
        "gap": 17.16110751978089,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          143.33333333333348,
          -188.33333333333348
        ],
        [
          143.33333333333348,
          -435
        ]
      ]
    },
    {
      "type": "text",
      "version": 51,
      "versionNonce": 1509994333,
      "index": "b2R",
      "isDeleted": false,
      "id": "9xUkxgnYx5-Dk5fyPHOh8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1892.8263631515326,
      "y": 4429.604354948895,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 180.4198522977531,
      "height": 50,
      "seed": 83823010,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363747174,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "5. Transfer LSTs\ndirectly to Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "VxgaSm93O4OlqrZMY57Iu",
      "originalText": "5. Transfer LSTs directly to Staker",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 518,
      "versionNonce": 715656371,
      "index": "b2S",
      "isDeleted": false,
      "id": "YHs0Gqhmn3yfxnBDiszoT",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1062.3339933390657,
      "y": 2644.963326508974,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 466.41960448026657,
      "height": 75,
      "seed": 608723646,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363349119,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. If Staker is delegated, add\ndelegated shares to Operator\n(increased by calculated withdrwawable shares)",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "3. If Staker is delegated, add\ndelegated shares to Operator\n(increased by calculated withdrwawable shares)",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 392,
      "versionNonce": 571875891,
      "index": "b2T",
      "isDeleted": false,
      "id": "TGtwXKQ5hDALWDo7cShaP",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1117.9934149732499,
      "y": 1742.4736186430125,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 429.6596252322197,
      "height": 75,
      "seed": 640231614,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362188035,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. If Staker is delegated, remove\ndelegated Shares from Operator\n(decreased by withdrawable shares amount)",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "3. If Staker is delegated, remove\ndelegated Shares from Operator\n(decreased by withdrawable shares amount)",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 659,
      "versionNonce": 100123805,
      "index": "b2U",
      "isDeleted": false,
      "id": "0zAO6U6LbidesdIKI4H-Q",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 176.06574627159222,
      "y": 5424.65197399651,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 265.4166564941406,
      "height": 50,
      "seed": 480087746,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Once validator is exited,\nstart a checkpoint",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. Once validator is exited,\nstart a checkpoint",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 660,
      "versionNonce": 657402109,
      "index": "b2W",
      "isDeleted": false,
      "id": "wndJ7JHlbbZx0Z_hDBLF-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 140.08458328748145,
      "y": 5660.5854539636575,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 356.67490285130594,
      "height": 2.266134773139129,
      "seed": 1708621442,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": null,
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          356.67490285130594,
          2.266134773139129
        ]
      ]
    },
    {
      "type": "text",
      "version": 854,
      "versionNonce": 146473309,
      "index": "b2X",
      "isDeleted": false,
      "id": "9sXM5xj6xYZRin-ElRo03",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 151.87408062217824,
      "y": 5555.4853073298445,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 323.79998779296875,
      "height": 100,
      "seed": 2147457822,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Submit one balance proof\nper validator pointed at pod.\nProofs validated against beacon\nblock root.",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "2. Submit one balance proof\nper validator pointed at pod.\nProofs validated against beacon\nblock root.",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 430,
      "versionNonce": 267177405,
      "index": "b2Y",
      "isDeleted": false,
      "id": "wp_KB7xRBNL1sK5m_Ddox",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 184.89074321983446,
      "y": 5678.485307329843,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 257.76666259765625,
      "height": 24,
      "seed": 346173890,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "verifyCheckpointProofs",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "verifyCheckpointProofs",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 2349,
      "versionNonce": 2078674461,
      "index": "b2Z",
      "isDeleted": false,
      "id": "cQmwh38jtJYD4EgAa2dzN",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5676.463779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 128488834,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2280,
      "versionNonce": 731398781,
      "index": "b2a",
      "isDeleted": false,
      "id": "VHQnnDAIGDndzyKOQ5TFC",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5641.463779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 381588802,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "CzL7G-cd1IeUb87tAORbe",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2231,
      "versionNonce": 600107741,
      "index": "b2b",
      "isDeleted": false,
      "id": "0kiRb8vKovbzXx8BfMr3w",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5605.963779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 444505346,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "CzL7G-cd1IeUb87tAORbe",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2157,
      "versionNonce": 564369309,
      "index": "b2c",
      "isDeleted": false,
      "id": "8Wai9QCuDoTCvrcKRXP0_",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5570.963779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 6794434,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "SpltLNf3GnK7lnsNrn-k3",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2220,
      "versionNonce": 602446845,
      "index": "b2d",
      "isDeleted": false,
      "id": "JEw4y43UVJO53IimI8gMq",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5540.213779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 773375106,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2155,
      "versionNonce": 869143645,
      "index": "b2e",
      "isDeleted": false,
      "id": "oOSfSCvtN3AqNQyEKTVGL",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5505.213779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 2102453314,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2094,
      "versionNonce": 757432509,
      "index": "b2f",
      "isDeleted": false,
      "id": "tLaVL1NOFzKG4pX5tsQyx",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5469.713779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 938413058,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "7JkoDbLx2twPP37CoCYe2",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 2027,
      "versionNonce": 1972396317,
      "index": "b2g",
      "isDeleted": false,
      "id": "UEC4OLrCnh8Ml32lVj6Mo",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1832.8321102329483,
      "y": 5434.713779552066,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 334642114,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "id": "7JkoDbLx2twPP37CoCYe2",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "rectangle",
      "version": 1949,
      "versionNonce": 1115698557,
      "index": "b2h",
      "isDeleted": false,
      "id": "nqbSVKCt0DHaSFjh5axoc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1833.2071102329483,
      "y": 5395.283223996511,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 135.00000000000006,
      "height": 114.99999999999997,
      "seed": 796488578,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "c8vLK_s-l9sT6HdL0kOUm"
        },
        {
          "id": "7JkoDbLx2twPP37CoCYe2",
          "type": "arrow"
        },
        {
          "id": "SpltLNf3GnK7lnsNrn-k3",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1685,
      "versionNonce": 523664861,
      "index": "b2i",
      "isDeleted": false,
      "id": "c8vLK_s-l9sT6HdL0kOUm",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1844.4071109958877,
      "y": 5433.58322399651,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 112.5999984741211,
      "height": 38.4,
      "seed": 214152002,
      "groupIds": [
        "beGEQyYEtc22TsXoZTryG"
      ],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EigenPods\n(1 per user)",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "nqbSVKCt0DHaSFjh5axoc",
      "originalText": "EigenPods\n(1 per user)",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1926,
      "versionNonce": 163516157,
      "index": "b2j",
      "isDeleted": false,
      "id": "3fXcLTF50p0EN1JpZNp4z",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2241.332110232948,
      "y": 5441.2971128854,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 266.00000000000017,
      "height": 114.99999999999997,
      "seed": 238025474,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "oqCfZlQ2XDeWsc466d8Q_"
        },
        {
          "id": "7JkoDbLx2twPP37CoCYe2",
          "type": "arrow"
        }
      ],
      "updated": 1734363560512,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1826,
      "versionNonce": 475932509,
      "index": "b2k",
      "isDeleted": false,
      "id": "oqCfZlQ2XDeWsc466d8Q_",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2303.957110232948,
      "y": 5489.197112885399,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.75,
      "height": 19.2,
      "seed": 1916619458,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560512,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EIP-4788 Oracle",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "3fXcLTF50p0EN1JpZNp4z",
      "originalText": "EIP-4788 Oracle",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 2321,
      "versionNonce": 218520051,
      "index": "b2l",
      "isDeleted": false,
      "id": "7JkoDbLx2twPP37CoCYe2",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1975.7511647921374,
      "y": 5510.0379193250365,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 258.0630882979535,
      "height": 0.06671593127975939,
      "seed": 1334485634,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560522,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "nqbSVKCt0DHaSFjh5axoc",
        "focus": 0.995769034654371,
        "gap": 7.544054559189135,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "3fXcLTF50p0EN1JpZNp4z",
        "focus": -0.19358446686074796,
        "gap": 7.517857142856883,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          258.0630882979535,
          -0.06671593127975939
        ]
      ]
    },
    {
      "type": "text",
      "version": 827,
      "versionNonce": 1275406461,
      "index": "b2m",
      "isDeleted": false,
      "id": "3Ef8Ob6E-5IDU6m-8ALi8",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1984.0523483281863,
      "y": 5442.826874790161,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffec99",
      "width": 241.89999389648438,
      "height": 50,
      "seed": 282526274,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "7JkoDbLx2twPP37CoCYe2",
          "type": "arrow"
        }
      ],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1.5 Parent beacon block\nroot queried from oracle",
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1.5 Parent beacon block\nroot queried from oracle",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "ellipse",
      "version": 1648,
      "versionNonce": 832901341,
      "index": "b2n",
      "isDeleted": false,
      "id": "3xnU6mBXla8WhiFUdP3Jk",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1363.54937213771,
      "y": 5473.849062331638,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 108.75,
      "height": 105,
      "seed": 1895918082,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "H1IqgL54dRz4weNzer4uF"
        },
        {
          "id": "SpltLNf3GnK7lnsNrn-k3",
          "type": "arrow"
        }
      ],
      "updated": 1734363560513,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1396,
      "versionNonce": 1072229693,
      "index": "b2o",
      "isDeleted": false,
      "id": "H1IqgL54dRz4weNzer4uF",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1389.3421077045145,
      "y": 5514.725956319345,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 57.266666412353516,
      "height": 23,
      "seed": 1997068738,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 2,
      "text": "Staker",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "3xnU6mBXla8WhiFUdP3Jk",
      "originalText": "Staker",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 1111,
      "versionNonce": 1243134259,
      "index": "b2p",
      "isDeleted": false,
      "id": "SpltLNf3GnK7lnsNrn-k3",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1474.0981463283924,
      "y": 5494.137838831703,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 356.87086884871655,
      "height": 1.793314280863342,
      "seed": 1913409922,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560522,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "3xnU6mBXla8WhiFUdP3Jk",
        "focus": -0.6081621304143341,
        "gap": 10.8571572156769,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "nqbSVKCt0DHaSFjh5axoc",
        "focus": -0.6779288570559726,
        "gap": 2.238095055839267,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          356.87086884871655,
          -1.793314280863342
        ]
      ]
    },
    {
      "type": "text",
      "version": 426,
      "versionNonce": 896219741,
      "index": "b2q",
      "isDeleted": false,
      "id": "uqfWBDwr7iAProLhv7jCi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1551.7160388043765,
      "y": 5498.015728998306,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 175.75,
      "height": 24,
      "seed": 1967837506,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "SpltLNf3GnK7lnsNrn-k3",
          "type": "arrow"
        }
      ],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "startCheckpoint",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "startCheckpoint",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "text",
      "version": 738,
      "versionNonce": 204334781,
      "index": "b2r",
      "isDeleted": false,
      "id": "BSMrX7GVsCBtAowF2fzk-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1456.6243792584785,
      "y": 5393.015728998306,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 365.9333190917969,
      "height": 50,
      "seed": 1411602690,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "0. Consensus or execution layer yield\nenters pod",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "0. Consensus or execution layer yield\nenters pod",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 2229,
      "versionNonce": 1654392605,
      "index": "b2s",
      "isDeleted": false,
      "id": "AGPGFNNkrSQ_HtPilmtyi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1394.9740765531678,
      "y": 5306.173501774288,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 324.26666259765625,
      "height": 64.39999999999999,
      "seed": 504686786,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 28,
      "fontFamily": 2,
      "text": "Staker Flow:\nProcessing Validator Yield",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "Staker Flow:\nProcessing Validator Yield",
      "autoResize": true,
      "lineHeight": 1.15
    },
    {
      "type": "arrow",
      "version": 1014,
      "versionNonce": 1527531635,
      "index": "b2t",
      "isDeleted": false,
      "id": "CzL7G-cd1IeUb87tAORbe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1975.2604069828478,
      "y": 5739.822452092104,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 191.13867262207737,
      "height": 0.07357654336101405,
      "seed": 448692354,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560522,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "0kiRb8vKovbzXx8BfMr3w",
        "focus": 1.3278784800611976,
        "gap": 18.858672540038242,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "cXR7t0HmweM2M4wSaVIkI",
        "focus": -0.192456511262002,
        "gap": 12.049992879231922,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          191.13867262207737,
          -0.07357654336101405
        ]
      ]
    },
    {
      "type": "text",
      "version": 426,
      "versionNonce": 1309713373,
      "index": "b2u",
      "isDeleted": false,
      "id": "cXR7t0HmweM2M4wSaVIkI",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2178.449072484157,
      "y": 5680.040168440954,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 353.98333740234375,
      "height": 100,
      "seed": 1172250690,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [
        {
          "id": "CzL7G-cd1IeUb87tAORbe",
          "type": "arrow"
        }
      ],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "3. Any native ETH in pod is\nawarded shares and is made\n\"withdrawable\" via DelegationManger\nwithdrawal queue",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "3. Any native ETH in pod is\nawarded shares and is made\n\"withdrawable\" via DelegationManger\nwithdrawal queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 784,
      "versionNonce": 1964314781,
      "index": "b2v",
      "isDeleted": false,
      "id": "_jv2opmrvR8FQED8d_KRL",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1541.1506638547555,
      "y": 5450.087787488568,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 207.51666259765625,
      "height": 25,
      "seed": 1263912962,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "1. Start a checkpoint",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "1. Start a checkpoint",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "arrow",
      "version": 727,
      "versionNonce": 1556280573,
      "index": "b2w",
      "isDeleted": false,
      "id": "XBnxCUl55FOBij1xdDAGb",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1466.2195039224025,
      "y": 5666.021267455718,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 356.67490285130594,
      "height": 2.266134773139129,
      "seed": 52524994,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": null,
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          356.67490285130594,
          2.266134773139129
        ]
      ]
    },
    {
      "type": "text",
      "version": 915,
      "versionNonce": 1911901533,
      "index": "b2x",
      "isDeleted": false,
      "id": "8y_pjMDCTlROHnbxnOVTe",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1476.342334590433,
      "y": 5559.254454155237,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 323.79998779296875,
      "height": 100,
      "seed": 1835085698,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "2. Submit one balance proof\nper validator pointed at pod.\nProofs validated against beacon\nblock root.",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "2. Submit one balance proof\nper validator pointed at pod.\nProofs validated against beacon\nblock root.",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "text",
      "version": 491,
      "versionNonce": 1945999805,
      "index": "b2y",
      "isDeleted": false,
      "id": "Z7P_tOk7DuCZFZ5E_Bl7O",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1509.3589971880892,
      "y": 5682.254454155233,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 257.76666259765625,
      "height": 24,
      "seed": 777252674,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363560513,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 3,
      "text": "verifyCheckpointProofs",
      "textAlign": "center",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "verifyCheckpointProofs",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1406,
      "versionNonce": 756005725,
      "index": "b33",
      "isDeleted": false,
      "id": "CX2I4G3Tx7B460R8LiO1n",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1736.9283928755115,
      "y": -683.360616382829,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 702426237,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "cpMdOQ5aa34VGghGaCa_o"
        }
      ],
      "updated": 1734361209401,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1157,
      "versionNonce": 2107262909,
      "index": "b34",
      "isDeleted": false,
      "id": "cpMdOQ5aa34VGghGaCa_o",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1768.7408928755115,
      "y": -635.460616382829,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 1975358685,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361209401,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "CX2I4G3Tx7B460R8LiO1n",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "rectangle",
      "version": 1876,
      "versionNonce": 248928925,
      "index": "b35",
      "isDeleted": false,
      "id": "LB1RAC-R424nw3plgg1kh",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1135.6703214253794,
      "y": -116.37449800762263,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1046129075,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "qH4cHb6aOV_E5qS8vJw4H"
        },
        {
          "id": "pQuPzVRh8WjIFdRY2eKF5",
          "type": "arrow"
        }
      ],
      "updated": 1734363588296,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1627,
      "versionNonce": 63635197,
      "index": "b36",
      "isDeleted": false,
      "id": "qH4cHb6aOV_E5qS8vJw4H",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1167.4828214253794,
      "y": -68.47449800762266,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 1801517907,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363588297,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "LB1RAC-R424nw3plgg1kh",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 1452,
      "versionNonce": 461404627,
      "index": "b37",
      "isDeleted": false,
      "id": "pQuPzVRh8WjIFdRY2eKF5",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1242.1729864888496,
      "y": 211.26227843242594,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 2.7711085123346493,
      "height": 203.15858945976518,
      "seed": 799750461,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "-8p09am_7qW9OnaDRk8ia"
        }
      ],
      "updated": 1734363588308,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "d7pRFde2mvhA75ZinOGRD",
        "focus": 0.02526414269227943,
        "gap": 15.95200728185975,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "LB1RAC-R424nw3plgg1kh",
        "focus": 0.011690285389073253,
        "gap": 9.478186980283425,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          2.7711085123346493,
          -203.15858945976518
        ]
      ]
    },
    {
      "type": "text",
      "version": 469,
      "versionNonce": 1813289843,
      "index": "b38",
      "isDeleted": false,
      "id": "-8p09am_7qW9OnaDRk8ia",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1103.7635179447445,
      "y": 48.403683012858025,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 217.41982972621918,
      "height": 125,
      "seed": 2144012701,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362486641,
      "link": null,
      "locked": false,
      "fontSize": 20,
      "fontFamily": 1,
      "text": "6/5. Query staker's\nmaxMagnitude for the\nStrategy to update\nstaker's\ndepositScalingFactor",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "pQuPzVRh8WjIFdRY2eKF5",
      "originalText": "6/5. Query staker's maxMagnitude for the Strategy to update staker's depositScalingFactor",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1681,
      "versionNonce": 1969129949,
      "index": "b3B",
      "isDeleted": false,
      "id": "uBOfg8AY3O3q1TzoE0AYc",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 774.7144431608883,
      "y": 1368.5611244983547,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 585934045,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "T2lTCf315-7KP3ycNMXWW"
        },
        {
          "id": "uZdrle8wzDudz1Ky8EmR2",
          "type": "arrow"
        }
      ],
      "updated": 1734361966514,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1432,
      "versionNonce": 537609789,
      "index": "b3C",
      "isDeleted": false,
      "id": "T2lTCf315-7KP3ycNMXWW",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 806.5269431608883,
      "y": 1416.4611244983548,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 1260257597,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734361966514,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "uBOfg8AY3O3q1TzoE0AYc",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 916,
      "versionNonce": 110574493,
      "index": "b3D",
      "isDeleted": false,
      "id": "uZdrle8wzDudz1Ky8EmR2",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 888.8812762825158,
      "y": 1698.5800261707745,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 0.03834289140161218,
      "height": 212.30934927812245,
      "seed": 218690429,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "pAc8FXOW3tNpocQKsPVxn"
        }
      ],
      "updated": 1734362046016,
      "link": null,
      "locked": false,
      "startBinding": {
        "elementId": "OoHImjROrg7WnQ4xzkpcv",
        "focus": 0.04048028406652341,
        "gap": 18.848011234469368,
        "fixedPoint": null
      },
      "endBinding": {
        "elementId": "uBOfg8AY3O3q1TzoE0AYc",
        "focus": -0.024356922642854672,
        "gap": 2.7095523942973614,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          0.03834289140161218,
          -212.30934927812245
        ]
      ]
    },
    {
      "id": "pAc8FXOW3tNpocQKsPVxn",
      "type": "text",
      "x": 790.0505289008022,
      "y": 1542.4253515317132,
      "width": 197.69983765482903,
      "height": 100,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "b3DV",
      "roundness": null,
      "seed": 1205146525,
      "version": 224,
      "versionNonce": 412461885,
      "isDeleted": false,
      "boundElements": null,
      "updated": 1734362040720,
      "link": null,
      "locked": false,
      "text": "2. Query\nmaxMagnitude to\ncalculate staker's\nwithdrawable shares",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "uZdrle8wzDudz1Ky8EmR2",
      "originalText": "2. Query maxMagnitude to calculate staker's withdrawable shares",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1778,
      "versionNonce": 896093651,
      "index": "b3F",
      "isDeleted": false,
      "id": "6FNSXtSCK4i9vGJHUD_Gd",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2622.909053060372,
      "y": 1700.057029928533,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 846589651,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "EKob_oXQJUmOYJUD_1iZg"
        },
        {
          "id": "6dpTPKRZLEdmh9dN3ZD96",
          "type": "arrow"
        }
      ],
      "updated": 1734362199220,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1529,
      "versionNonce": 759569267,
      "index": "b3G",
      "isDeleted": false,
      "id": "EKob_oXQJUmOYJUD_1iZg",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2654.721553060372,
      "y": 1747.957029928533,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 35036275,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362199220,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "6FNSXtSCK4i9vGJHUD_Gd",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 1115,
      "versionNonce": 339791603,
      "index": "b3H",
      "isDeleted": false,
      "id": "6dpTPKRZLEdmh9dN3ZD96",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 2737.075886182,
      "y": 2030.0759316009528,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 0.03834289140161218,
      "height": 212.30934927812245,
      "seed": 546346515,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "stENef2b2kfL0tlWSytw2"
        }
      ],
      "updated": 1734362278797,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": {
        "elementId": "6FNSXtSCK4i9vGJHUD_Gd",
        "focus": -0.02435692264285643,
        "gap": 2.7095523942973614,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          0.03834289140161218,
          -212.30934927812245
        ]
      ]
    },
    {
      "id": "stENef2b2kfL0tlWSytw2",
      "type": "text",
      "x": 2628.1951595549963,
      "y": 1873.9212569618917,
      "width": 217.79979614540935,
      "height": 100,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "b3I",
      "roundness": null,
      "seed": 2094685107,
      "version": 278,
      "versionNonce": 1119906045,
      "isDeleted": false,
      "boundElements": [],
      "updated": 1734362277899,
      "link": null,
      "locked": false,
      "text": "3. Query\nmaxMagnitude to\nupdate staker's\ndeposit scaling factor",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "6dpTPKRZLEdmh9dN3ZD96",
      "originalText": "3. Query maxMagnitude to update staker's deposit scaling factor",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1838,
      "versionNonce": 1109827805,
      "index": "b3J",
      "isDeleted": false,
      "id": "0a8N_e6gkwu5-k8KmBL_-",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 710.5859275076249,
      "y": 2268.2620413972554,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 1306495869,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "2SpNu7VlX4wkRxeUwbP2L"
        },
        {
          "id": "130BriWp6I2pxy6aKtw0G",
          "type": "arrow"
        }
      ],
      "updated": 1734362372290,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1589,
      "versionNonce": 1773698365,
      "index": "b3K",
      "isDeleted": false,
      "id": "2SpNu7VlX4wkRxeUwbP2L",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 742.3984275076249,
      "y": 2316.1620413972555,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 1882919901,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734362372290,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "0a8N_e6gkwu5-k8KmBL_-",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 1205,
      "versionNonce": 1180063027,
      "index": "b3L",
      "isDeleted": false,
      "id": "130BriWp6I2pxy6aKtw0G",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 825.1694272959191,
      "y": 2598.2809430696752,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 0.03834289140161218,
      "height": 212.30934927812223,
      "seed": 1657180221,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "OfHErzEAswtHwUQ0Olnsf"
        }
      ],
      "updated": 1734363604361,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": {
        "elementId": "0a8N_e6gkwu5-k8KmBL_-",
        "focus": -0.028093495417795003,
        "gap": 2.7095523942975888,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          0.03834289140161218,
          -212.30934927812223
        ]
      ]
    },
    {
      "id": "OfHErzEAswtHwUQ0Olnsf",
      "type": "text",
      "x": 724.3686786935023,
      "y": 2417.126268430614,
      "width": 201.63984009623528,
      "height": 150,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "b3M",
      "roundness": null,
      "seed": 56852637,
      "version": 324,
      "versionNonce": 1891300851,
      "isDeleted": false,
      "boundElements": [],
      "updated": 1734363601320,
      "link": null,
      "locked": false,
      "text": "2. Query\nmaxMagnitude to\ncalculate whether\nwithdrawable shares\nwere slashed while in\nqueue",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "130BriWp6I2pxy6aKtw0G",
      "originalText": "2. Query maxMagnitude to calculate whether withdrawable shares were slashed while in queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1925,
      "versionNonce": 1307885843,
      "index": "b3N",
      "isDeleted": false,
      "id": "AK7dgZlDTH-2Oikgm8ydH",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1091.7531389138244,
      "y": 3713.033937529639,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "width": 223.00000000000006,
      "height": 114.99999999999997,
      "seed": 560396019,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "Nz-REXSoxDrDSOd5LCxhq"
        },
        {
          "id": "07rBBHWqecyJpfZJMK1_M",
          "type": "arrow"
        }
      ],
      "updated": 1734363571520,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1677,
      "versionNonce": 1652631219,
      "index": "b3O",
      "isDeleted": false,
      "id": "Nz-REXSoxDrDSOd5LCxhq",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1123.5656389138244,
      "y": 3760.933937529639,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 159.375,
      "height": 19.2,
      "seed": 1509007507,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363571520,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "AllocationManager",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "AK7dgZlDTH-2Oikgm8ydH",
      "originalText": "AllocationManager",
      "autoResize": true,
      "lineHeight": 1.2
    },
    {
      "type": "arrow",
      "version": 1377,
      "versionNonce": 446149171,
      "index": "b3P",
      "isDeleted": false,
      "id": "07rBBHWqecyJpfZJMK1_M",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1205.4707532854518,
      "y": 4042.1544017020587,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#ffc9c9",
      "width": 0.03834289140161218,
      "height": 212.30934927812223,
      "seed": 716576307,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 2
      },
      "boundElements": [
        {
          "type": "text",
          "id": "sw19jzLIy905vlo-X89BW"
        }
      ],
      "updated": 1734363685756,
      "link": null,
      "locked": false,
      "startBinding": null,
      "endBinding": {
        "elementId": "AK7dgZlDTH-2Oikgm8ydH",
        "focus": -0.02032697503509013,
        "gap": 1.8111148942975888,
        "fixedPoint": null
      },
      "lastCommittedPoint": null,
      "startArrowhead": null,
      "endArrowhead": "triangle",
      "points": [
        [
          0,
          0
        ],
        [
          0.03834289140161218,
          -212.30934927812223
        ]
      ]
    },
    {
      "id": "sw19jzLIy905vlo-X89BW",
      "type": "text",
      "x": 1105.119223433035,
      "y": 3861.8981645629974,
      "width": 201.63984009623528,
      "height": 150,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#d0bfff",
      "fillStyle": "solid",
      "strokeWidth": 1,
      "strokeStyle": "solid",
      "roughness": 0,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "b3Q",
      "roundness": null,
      "seed": 43702227,
      "version": 328,
      "versionNonce": 1773573085,
      "isDeleted": false,
      "boundElements": [],
      "updated": 1734363604596,
      "link": null,
      "locked": false,
      "text": "2. Query\nmaxMagnitude to\ncalculate whether\nwithdrawable shares\nwere slashed while in\nqueue",
      "fontSize": 20,
      "fontFamily": 1,
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "07rBBHWqecyJpfZJMK1_M",
      "originalText": "2. Query maxMagnitude to calculate whether withdrawable shares were slashed while in queue",
      "autoResize": true,
      "lineHeight": 1.25
    },
    {
      "type": "rectangle",
      "version": 1868,
      "versionNonce": 848772413,
      "index": "b3R",
      "isDeleted": false,
      "id": "cyLRrqf8uVqIpnO4DRqqy",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1396.717409232908,
      "y": -982.4560167908803,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "#b2f2bb",
      "width": 266.00000000000017,
      "height": 114.99999999999997,
      "seed": 2131196029,
      "groupIds": [],
      "frameId": null,
      "roundness": {
        "type": 3
      },
      "boundElements": [
        {
          "type": "text",
          "id": "HTWzY9PIToNQf8PIFO6bi"
        }
      ],
      "updated": 1734363922343,
      "link": null,
      "locked": false
    },
    {
      "type": "text",
      "version": 1766,
      "versionNonce": 1981310643,
      "index": "b3S",
      "isDeleted": false,
      "id": "HTWzY9PIToNQf8PIFO6bi",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "solid",
      "roughness": 1,
      "opacity": 100,
      "angle": 0,
      "x": 1459.311159232908,
      "y": -934.5560167908803,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "width": 140.8125,
      "height": 19.2,
      "seed": 1876779229,
      "groupIds": [],
      "frameId": null,
      "roundness": null,
      "boundElements": [],
      "updated": 1734363922344,
      "link": null,
      "locked": false,
      "fontSize": 16,
      "fontFamily": 3,
      "text": "EIP-4788 Oracle",
      "textAlign": "center",
      "verticalAlign": "middle",
      "containerId": "cyLRrqf8uVqIpnO4DRqqy",
      "originalText": "EIP-4788 Oracle",
      "autoResize": true,
      "lineHeight": 1.2
    }
  ],
  "appState": {
    "gridSize": 20,
    "gridStep": 5,
    "gridModeEnabled": false,
    "viewBackgroundColor": "#ffffff"
  },
  "files": {}
}
````

## File: docs/permissions/PermissionController.md
````markdown
# PermissionController

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`PermissionController.sol`](../../src/contracts/permissions/PermissionController.sol) | Singleton | Transparent proxy |

The `PermissionController` handles user permissions for protocol contracts which explicitly integrate it. Note that "users" in the context of the `PermissionController` refers to **AVSs** and **operators**; it does *not* refer to **stakers**.

The `PermissionController` is integrated into other core contracts, enabling (for specific methods) AVSs and operators to designate _other accounts_ ("appointees") that can call these methods on their behalf. The core contracts using the `PermissionController` as a dependency are the:
* `DelegationManager`
* `AllocationManager`
* `RewardsCoordinator`

The `PermissionController` defines three different roles:
* [Accounts](#accounts)
* [Admins](#admins)
* [Appointees](#appointees)

---

## Accounts

**Accounts** refer to the Ethereum address through which one interacts with the protocol _if no appointees are set_. From the core contracts' perspective, accounts are the "state holder," i.e. the address referenced in storage when a contract method interacts with state. For example, in the `DelegationManager`, the `operator` address that holds shares in the `operatorShares` mapping is an "account." In the `AllocationManager`, an AVS's "account" is the address under which operator sets are created.

The `PermissionController` allows an account to designate **admins** and/or **appointees** to take certain actions on its behalf. Note that setting up admins/appointees is _optional_, and carries with it a significant responsibility to **ensure the designated actors are intentionally being granted authority**.

Both admins AND appointees can be granted authority to act on an account's behalf. Admins are granted full reign over any `PermissionController`-enabled functions, while appointees must be granted authority to call specific functions on specific contracts. The list of methods that are `PermissionController`-enabled follow.

For operators:
* `AllocationManager.modifyAllocations`
* `AllocationManager.registerForOperatorSets`
* `AllocationManager.deregisterFromOperatorSets`
* `AllocationManager.setAllocationDelay`
* `DelegationManager.modifyOperatorDetails`
* `DelegationManager.updateOperatorMetadataURI`
* `DelegationManager.undelegate`
* `RewardsCoordinator.setClaimerFor`
* `RewardsCoordinator.setClaimerFor`
* `RewardsCoordinator.setOperatorAVSSplit`
* `RewardsCoordinator.setOperatorPISplit`

For AVSs:
* `AllocationManager.slashOperator`
* `AllocationManager.deregisterFromOperatorSets`
* `AllocationManager.setAVSRegistrar`
* `AllocationManager.updateAVSMetadataURI`
* `AllocationManager.createOperatorSets`
* `AllocationManager.addStrategiesToOperatorSet`
* `AllocationManager.removeStrategiesFromOperatorSet`
* `RewardsCoordinator.createOperatorDirectedAVSRewardsSubmission`
* `RewardsCoordinator.setClaimerFor`

### Account Permissions

Account permissions are stored within a struct defined as follows:

```solidity
struct AccountPermissions {
    /// @notice The pending admins of the account
    EnumerableSet.AddressSet pendingAdmins;
    /// @notice The admins of the account
    EnumerableSet.AddressSet admins;
    /// @notice Mapping from an appointee to the list of encoded target & selectors
    mapping(address appointee => EnumerableSet.Bytes32Set) appointeePermissions;
    /// @notice Mapping from encoded target & selector to the list of appointees
    mapping(bytes32 targetSelector => EnumerableSet.AddressSet) permissionAppointees;
}
```

These structs are then stored within a mapping defined as follows, allowing for fetching account permissions for a given account with ease:

```solidity
mapping(address account => AccountPermissions) internal _permissions;
```

By default, no other address can perform an action on behalf of a given account. However, accounts can add admins and/or appointees to give other addresses the ability to act on their behalf.

## Admins

Admins are able to take ANY action on behalf of an original account -- including adding or removing admins. This enables operations like key rotation for operators, or creating a backup admin which is stored on a cold key.

**Note:** by default, an account is its own admin. However, once an admin is added, this is no longer the case; only the admins listed in `_permissions.admins` are admins. If an account wants to both add admins AND continue acting as its own admin, _it must be added to the admins list_.

### Adding an Admin

The relevant functions for admin addition are:

* [`addPendingAdmin`](#addpendingadmin)
* [`removePendingAdmin`](#removependingadmin)
* [`acceptAdmin`](#acceptadmin)

#### `addPendingAdmin`

```solidity
/**
 * @notice Sets a pending admin of an account
 * @param account to set pending admin for
 * @param admin to set
 * @dev Multiple admins can be set for an account
 */
function addPendingAdmin(address account, address admin) external onlyAdmin(account);
```

When adding a new admin, an account or admin must first call `addPendingAdmin()`. Then, the pending admin must call `acceptAdmin()` to complete the process. An account cannot force an admin role upon another account.

Pending admins do not have any particular authority, but are granted the full authority of an admin once they call `acceptAdmin()`.

*Effects*:
* An address is added to the `pendingAdmins` set for the account
* A `PendingAdminAdded` event is emitted specifying the account for which a pending admin was added

*Requirements*:
* The proposed admin MUST NOT already be an admin for the `account`
* The proposed admin MUST NOT be a pending admin for the `account`
* Caller MUST be an admin for the `account`, or the `account` itself if no admin is set

#### `removePendingAdmin`

```solidity
/**
 * @notice Removes a pending admin of an account
 * @param account to remove pending admin for
 * @param admin to remove
 * @dev Only the admin of the account can remove a pending admin
 */
function removePendingAdmin(address account, address admin) external onlyAdmin(account);
```

An account or admin can call `removePendingAdmin()` to prevent a pending admin from accepting their role. However, this will only work if the pending admin has not already called `acceptAdmin()`. If this occurs, an admin can call `removeAdmin` to remove the unwanted admin.

*Effects*:
* An address is removed from the `pendingAdmins` set for the account
* A `PendingAdminRemoved` event is emitted specifying the account for which a pending admin was removed

*Requirements*:
* The proposed admin MUST be a pending admin for the account
* Caller MUST be an admin for the account, or the account's address itself if no admin is set

#### `acceptAdmin`

```solidity
/**
 * @notice Accepts the admin role of an account
 * @param account to accept admin for
 * @dev Only a pending admin for the account can become an admin
 */
function acceptAdmin(address account) external;
```

Called by a pending admin to claim the admin role for an account. The caller must have been previously added as a pending admin.

Note that once an account has successfully added an admin (i.e. the pending admin has called `acceptAdmin()`), **the account's address itself no longer has its default admin privileges**. This behavior benefits accounts seeking to perform a *key rotation*, as adding an admin allows them to remove permissions from their original, potentially compromised, key. If an account wants to retain admin privileges for its own address, it is recommended to first add itself as an admin, then add any other admins as desired.

*Effects*:
* The caller is removed from the `pendingAdmins` set for the account
* The caller is added to the `admins` set for the account
* A `AdminSet` event is emitted specifying the account for which an admin was added

*Requirements*:
* Caller MUST be a pending admin for the account

### Removing an Admin

#### `removeAdmin`

```solidity
/**
 * @notice Remove an admin of an account
 * @param account to remove admin for
 * @param admin to remove
 * @dev Only the admin of the account can remove an admin
 * @dev Reverts when an admin is removed such that no admins are remaining
 */
function removeAdmin(address account, address admin) external onlyAdmin(account);
```

An admin of an account can call `removeAdmin()` to remove any other admins of the same account. However, one admin must always remain for any given account. In other words, once an account has added an admin, it must always have at least one admin in perpetuity.

*Effects*:
* The specified admin is removed from the `admins` set for the account
* An `AdminRemoved` event is emitted specifying the accuont for which an admin was removed

*Requirements*:
* `admins.length()` MUST be greater than 1, such that removing the admin does not remove all admins for the account
* The address to remove MUST be an admin for the account
* Caller MUST be an admin for the account, or the account's address itself if no admin is set

## Appointees

Appointees are able to act as another account *for a specific function for a specific contract*, granting accounts granular access control.

Specifically, an account (or its admins) can grant an appointee access to a specific `selector` (i.e [function](https://solidity-by-example.org/function-selector/)) on a given `target` (i.e. contract). The `target` and `selector` are combined in the form of the `targetSelector` and serve to uniquely identify a permissioned function on a specific contract.

Appointees can be granted access to multiple functions/contracts. Each new `targetSelector` permission granted requires setting the appointee from scratch, and revoking the appointee's permission requires revoking each individual `targetSelector` permission, as described below.

### Adding an Appointee

#### `setAppointee`

```solidity
/**
 * @notice Set an appointee for a given account
 * @param account to set appointee for
 * @param appointee to set
 * @param target to set appointee for
 * @param selector to set appointee for
 * @dev Only the admin of the account can set an appointee
 */
function setAppointee(
    address account,
    address appointee,
    address target,
    bytes4 selector
) external onlyAdmin(account);
```

An account (or its admins) can call `setAppointee()` to give another address the ability to call a specific function on a given contract. That address is then only able to call that specific function on that specific contract on behalf of `account`.

Note that unlike the process to become an admin, there is no requirement for the `appointee` to accept the appointment.

*Effects*:
* The `targetSelector` is added to the specified `appointee` set within the  `appointeePermissions` mapping
* The `appointee` is added to the specified `targetSelector` set within the  `permissionAppointees` mapping
* The `AppointeeSet` event is emitted, specifying the account, appointee, target contract, and function selector

*Requirements*:
* Caller MUST be an admin for the account, or the account's address itself if no admin is set
* The proposed appointee MUST NOT already have permissions for the given `targetSelector`

### Removing an Appointee

#### `removeAppointee`

```solidity
/**
 * Removes an appointee for a given account
 * @param account to remove appointee for
 * @param appointee to remove
 * @param target to remove appointee for
 * @param selector to remove appointee for
 * @dev Only the admin of the account can remove an appointee
 */
function removeAppointee(
    address account,
    address appointee,
    address target,
    bytes4 selector
) external onlyAdmin(account);
```

An account (or its admins) can call `removeAppointee()` to remove an `appointee's` permissions for a given contract/function pair. Note that there does not exist any way currently to atomically remove all permissions for a given appointee, or all appointees for a given function selector - each permission must be revoked individually.

Also note that permissions to specific functions/contracts cannot be revoked for _admins_. Admins always have full access, unless another admin removes them from the admin list.

*Effects*:
* The `targetSelector` is removed from the specified `appointee` set within the  `appointeePermissions` mapping
* The `appointee` is removed from the specified `targetSelector` set within the  `permissionAppointees` mapping
* The `AppointeeRemoved` event is emitted, specifying the account, appointee, target contract, and function selector

*Requirements*:
* Caller MUST be an admin for the account, or the account's address itself if no admin is set
* The proposed appointee MUST already have permissions for the given `targetSelector`
````

## File: docs/README.md
````markdown
[middleware-repo]: https://github.com/Layr-Labs/eigenlayer-middleware/
[elip-002]: https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md

## EigenLayer Docs - v1.3.0 Release

This repo contains the EigenLayer core contracts, which enable restaking of liquid staking tokens (LSTs), beacon chain ETH, and permissionlessly deployed ERC20 Strategies to secure new services called AVSs (actively validated services). For more info on AVSs, check out the EigenLayer middleware contracts [here][middleware-repo].

This document provides an overview of system components, contracts, and user roles and is up-to-date with the latest [ELIP-002][elip-002]. Further documentation on the major system contracts can be found in [/core](./core/).

#### Contents

* [System Components](#system-components)
    * [`EigenPodManager`](#eigenpodmanager)
    * [`StrategyManager`](#strategymanager)
    * [`DelegationManager`](#delegationmanager)
    * [`RewardsCoordinator`](#rewardscoordinator)
    * [`AVSDirectory`](#avsdirectory)
    * [`AllocationManager`](#allocationmanager)
    * [`PermissionController`](#permissioncontroller)
* [Roles and Actors](#roles-and-actors)
* [Common User Flows](#common-user-flows)
    * [Depositing Into EigenLayer](#depositing-into-eigenlayer)
    * [Delegating to an Operator](#delegating-to-an-operator)
    * [Undelegating or Queueing a Withdrawal](#undelegating-or-queueing-a-withdrawal)
    * [Completing a Withdrawal as Shares](#completing-a-withdrawal-as-shares)
    * [Completing a Withdrawal as Tokens](#completing-a-withdrawal-as-tokens)
    * [Withdrawal Processing: Validator Exits](#withdrawal-processing-validator-exits)
    * [Withdrawal Processing: Partial Beacon Chain Withdrawals](#withdrawal-processing-partial-beacon-chain-withdrawals)

### System Components

#### EigenPodManager

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`EigenPodManager.sol`](../src/contracts/pods/EigenPodManager.sol) | Singleton | Transparent proxy |
| [`EigenPod.sol`](../src/contracts/pods/EigenPod.sol) | Instanced, deployed per-user | Beacon proxy |

These contracts work together to enable native ETH restaking:
* Users deploy `EigenPods` via the `EigenPodManager`, which contain beacon chain state proof logic used to verify a validator's withdrawal credentials and current balances. An `EigenPod's` main role is to serve as the fee recipient and/or withdrawal credentials for one or more of a user's validators.
* The `EigenPodManager` handles `EigenPod` creation and accounting+interactions between users with restaked native ETH and the `DelegationManager`.

See full documentation in:
* [`/core/EigenPodManager.md`](./core/EigenPodManager.md)
* [`/core/EigenPod.md`](./core/EigenPod.md)

#### StrategyManager

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`StrategyManager.sol`](../src/contracts/core/StrategyManager.sol) | Singleton | Transparent proxy |
| [`StrategyFactory.sol`](../../src/contracts/core/StrategyFactory.sol) | Singleton | Transparent proxy |
| [`StrategyBaseTVLLimits.sol`](../../src/contracts/strategies/StrategyBaseTVLLimits.sol) | Instanced, one per supported token | - Strategies deployed outside the `StrategyFactory` use transparent proxies <br /> - Anything deployed via the `StrategyFactory` uses a Beacon proxy |

These contracts work together to enable restaking for ERC20 tokens supported by EigenLayer:
* The `StrategyManager` acts as the entry and exit point for any supported tokens in EigenLayer. It handles deposits into LST-specific strategies, and manages accounting+interactions between users with restaked LSTs and the `DelegationManager`.
* `StrategyFactory` allows anyone to deploy strategies to support deposits/withdrawals for new ERC20 tokens
* `StrategyBaseTVLLimits` is deployed as multiple separate instances, one for each supported token. When a user deposits into a strategy through the `StrategyManager`, this contract receives the tokens and awards the user with a proportional quantity of deposit shares in the strategy. When a user withdraws, the strategy contract sends the LSTs back to the user.

See full documentation in [`/core/StrategyManager.md`](./core/StrategyManager.md).

#### DelegationManager

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`DelegationManager.sol`](../src/contracts/core/DelegationManager.sol) | Singleton | Transparent proxy |

The `DelegationManager` sits between the `EigenPodManager` and `StrategyManager` to manage delegation and undelegation of stakers to operators. Its primary features are to allow users to become operators, to keep track of delegated shares to operators across different strategies, and to manage withdrawals on behalf of stakers via the `EigenPodManager` and `StrategyManager`.

The `DelegationManager` is tightly coupled with the `AllocationManager`. The `DelegationManager` ingests information about slashing as part of managing share accounting for stakers whose operators have been slashed. It also receives directives to slash/burn operator shares when an AVS slashes an operator.

See:
* full documentation in [`/core/DelegationManager.md`](./core/DelegationManager.md)
* share accounting documentation in [`/core/accounting/SharesAccounting.md`](./core/accounting/SharesAccounting.md)

#### RewardsCoordinator

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`RewardsCoordinator.sol`](../src/contracts/core/RewardsCoordinator.sol) | Singleton | Transparent proxy |

The `RewardsCoordinator` is the main entry point of submission and claiming of ERC20 rewards in EigenLayer. It carries out three basic functions:
* AVSs (via the AVS's contracts) submit "rewards submissions" to their registered operators and stakers over a specific time period 
* *Off-chain*, the rewards updater will use each RewardsSubmission time period to apply reward amounts to historical staker/operator stake weights. This is consolidated into a merkle root that is posted *on-chain* to the `RewardsCoordinator`, allowing stakers/operators to claim their allocated rewards.
* Stakers/operators can claim rewards posted by the rewards updater.

See full documentation in [`/core/RewardsCoordinator.md`](./core/RewardsCoordinator.md).

#### AVSDirectory

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`AVSDirectory.sol`](../src/contracts/core/AVSDirectory.sol) | Singleton | Transparent proxy |

##### Note: This contract is left unchanged for backwards compatibility. Operator<>AVS Registrations are to be replaced entirely with the `AllocationManager` and this contract will be deprecated in a future release.

Previously, the `AVSDirectory` handled interactions between AVSs and the EigenLayer core contracts. Once registered as an operator in EigenLayer core (via the `DelegationManager`), operators could register with one or more AVSs (via the AVS's contracts) to begin providing services to them offchain. As a part of registering with an AVS, the AVS would record this registration in the core contracts by calling into the `AVSDirectory`. As of the slashing release, this process is now managed by the [`AllocationManager`](#allocationmanager).

See full documentation in [`/core/AVSDirectory.md`](./core/AVSDirectory.md).

For more information on AVS contracts, see the [middleware repo][middleware-repo].

#### AllocationManager

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`AllocationManager.sol`](../src/contracts/core/AllocationManager.sol) | Singleton | Transparent proxy |

The `AllocationManager` is replaces the AVSDirectory with the introduction of _operator sets_ and slashing. It handles several use cases:
* AVSs can create operator sets and can define the EigenLayer Strategies within them
* Operators can register to or deregister from an AVS's operator sets
* Operators can make slashable security commitments to an operator set by allocating a proportion of their total delegated stake for a Strategy to be slashable. Ex. As an operator, I can allocate 50% of my delegated stETH to be slashable by a specific operator set
* AVSs can slash an operator who has allocated to and is registered for one of the AVS's operator sets

See full documentation in [`/core/AllocationManager.md`](./core/AllocationManager.md).

#### PermissionController

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`PermissionController.sol`](../src/contracts/permissions/PermissionController.sol) | Singleton | Transparent proxy |

The `PermissionController` allows AVSs and operators to delegate the ability to call certain core contract functions to other addresses. This delegation ability is not available to stakers, and is not available in ALL core contract functions.

The following core contracts use the `PermissionController` in certain methods:
* `DelegationManager`
* `AllocationManager`
* `RewardsCoordinator`

See full documentation in [`/permissions/PermissionController.md`](./permissions/PermissionController.md).

---

#### Roles and Actors

To see an example of the user flows described in this section, check out our integration tests: [/src/test/integration](../src/test/integration/).

##### Staker

A staker is any party who has assets deposited (or "restaked") into EigenLayer. Currently, these assets can be:
* Native beacon chain ETH (via the EigenPodManager)
* Arbitrary ERC20s (via the StrategyManager)

Stakers can restake any combination of these: a staker may hold ALL of these assets, or only one of them.

*Flows:*
* Stakers **deposit** assets into EigenLayer via either the StrategyManager (for ERC20s) or the EigenPodManager (for beacon chain ETH)
* Stakers **withdraw** assets via the DelegationManager, *no matter what assets they're withdrawing*
* Stakers **delegate** to an operator via the DelegationManager

##### Operator

An operator is a user who helps run the software built on top of EigenLayer (AVSs). operators register in EigenLayer and allow stakers to delegate to them, then opt in to provide various services built on top of EigenLayer. operators may themselves be stakers; these are not mutually exclusive.

*Flows:*
* Users can **register** as an operator via the DelegationManager
* Operators can **deposit** and **withdraw** assets just like stakers can
* Operators can opt in to providing services for an AVS using that AVS's middleware contracts. See the [EigenLayer middleware][middleware-repo] repo for more details.

---

#### Common User Flows

##### Depositing Into EigenLayer

Depositing into EigenLayer varies depending on whether the staker is depositing Native ETH or LSTs:

![.](./images/Staker%20Flow%20Diagrams/Depositing.png)

##### Delegating to an Operator

![.](./images/Staker%20Flow%20Diagrams/Delegating.png)

##### Undelegating or Queueing a Withdrawal

Undelegating from an operator automatically queues a withdrawal that needs to go through the `DelegationManager's` withdrawal delay. Stakers that want to withdraw can choose to `undelegate`, or can simply call `queueWithdrawals` directly.

![.](./images/Staker%20Flow%20Diagrams/Queue%20Withdrawal.png)

##### Completing a Withdrawal as Shares

This flow is mostly useful if a staker wants to change which operator they are delegated to. The staker first needs to undelegate (see above). At this point, they can delegate to a different operator. However, the new operator will only be awarded shares once the staker completes their queued withdrawal "as shares":

![.](./images/Staker%20Flow%20Diagrams/Complete%20Withdrawal%20as%20Shares.png)

##### Completing a Withdrawal as Tokens

Completing a queued withdrawal as tokens is roughly the same for both native ETH and LSTs. 

However, note that *before* a withdrawal can be completed, native ETH stakers will need to perform additional steps, detailed in the diagrams below. 

![.](./images/Staker%20Flow%20Diagrams/Complete%20Withdrawal%20as%20Tokens.png)

##### `EigenPods`: Processing Validator Exits

If a staker wants to fully withdraw from the beacon chain, they need to perform these additional steps before their withdrawal is completable:

![.](./images/Staker%20Flow%20Diagrams/Validator%20Exits.png)

##### `EigenPods`: Processing Validator Yield

As the staker's `EigenPod` accumulates consensus layer or execution layer yield, the `EigenPod's` balance will increase. The staker can Checkpoint their validator to claim this yield as shares, which can either remain staked in EigenLayer or be withdrawn via the `DelegationManager` withdrawal queue:

![.](./images/Staker%20Flow%20Diagrams/Validator%20Yield.png)
````
