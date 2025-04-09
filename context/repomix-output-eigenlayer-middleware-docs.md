Sourced from: https://github.com/Layr-Labs/eigenlayer-middleware


This file is a merged representation of a subset of the codebase, containing specifically included files, combined into a single document by Repomix.
The content has been processed where security check has been disabled.

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
- Files are sorted by Git change count (files with more changes are at the bottom)

## Additional Info

# Directory Structure
```
docs/
  registries/
    BLSApkRegistry.md
    IndexRegistry.md
    SocketRegistry.md
    StakeRegistry.md
  slashing/
    InstantSlasher.md
    SlasherBase.md
    VetoableSlasher.md
  storage-report/
    AVSRegistrar.md
    RegistryCoordinatorStorage.md
    ServiceManagerBaseStorage.md
  BLSSignatureChecker.md
  EjectionManager.md
  OperatorStateRetriever.md
  quick-start.md
  README.md
  RegistryCoordinator.md
  ServiceManagerBase.md
  SlashingRegistryCoordinator.md

```

# Files

## File: docs/registries/BLSApkRegistry.md
````markdown
## BLSApkRegistry

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`BLSApkRegistry.sol`](../../src/BLSApkRegistry.sol) | Singleton | Transparent proxy |

The `BLSApkRegistry` tracks the current aggregate BLS pubkey for all Operators registered to each quorum, and keeps a historical record of each quorum's aggregate BLS pubkey hash. This contract makes heavy use of the `BN254` library to perform various operations on the BN254 elliptic curve (see [`BN254.sol`](../../src/libraries/BN254.sol)).

Each time an Operator registers for a quorum, its BLS pubkey is added to that quorum's `currentApk`. Each time an Operator deregisters from a quorum, its BLS pubkey is subtracted from that quorum's `currentApk`. This contract maintains a history of the hash of each quorum's apk over time, which is used by the `BLSSignatureChecker` to fetch the "total signing key" for a quorum at a specific block number.

#### High-level Concepts

This document organizes methods according to the following themes (click each to be taken to the relevant section):
* [Registering and Deregistering](#registering-and-deregistering)
* [System Configuration](#system-configuration)

---    

### Registering and Deregistering

These methods are ONLY called through the `RegistryCoordinator` - when an Operator registers for or deregisters from one or more quorums:
* [`registerBLSPublicKey`](#registerblspublickey)
* [`registerOperator`](#registeroperator)
* [`deregisterOperator`](#deregisteroperator)

#### `registerBLSPublicKey`

```solidity
function registerBLSPublicKey(
    address operator,
    PubkeyRegistrationParams calldata params,
    BN254.G1Point calldata pubkeyRegistrationMessageHash
) 
    external 
    onlyRegistryCoordinator 
    returns (bytes32 operatorId)

struct PubkeyRegistrationParams {
    BN254.G1Point pubkeyRegistrationSignature;
    BN254.G1Point pubkeyG1;
    BN254.G2Point pubkeyG2;
}
```

This method is ONLY callable by the `RegistryCoordinator`. It is called when an Operator registers for the AVS for the first time.

This method validates a BLS signature over the `pubkeyRegistrationMessageHash`, then permanently assigns the pubkey to the Operator. The hash of `params.pubkeyG1` becomes the Operator's unique `operatorId`, which identifies the Operator throughout the registry contracts.

*Entry Points*:
* `RegistryCoordinator.registerOperator`
* `RegistryCoordinator.registerOperatorWithChurn`

*Effects*:
* Registers the Operator's BLS pubkey for the first time, updating the following mappings:
    * `operatorToPubkey[operator]`
    * `operatorToPubkeyHash[operator]`
    * `operatorToPubKeyG2[operator]`
    * `pubkeyHashToOperator[pubkeyHash]`

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `params.pubkeyG1` MUST NOT hash to the `ZERO_PK_HASH`
* `operator` MUST NOT have already registered a pubkey:
    * `operatorToPubkeyHash[operator]` MUST be zero
    * `pubkeyHashToOperator[pubkeyHash]` MUST be zero
* `params.pubkeyRegistrationSignature` MUST be a valid signature over `pubkeyRegistrationMessageHash`

#### `registerOperator`

```solidity
function registerOperator(
    address operator,
    bytes memory quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

`registerOperator` fetches the Operator's registered BLS pubkey (see `registerBLSPublicKey` above). Then, for each quorum in `quorumNumbers`, the Operator's pubkey is added to that quorum's `currentApk`. The `apkHistory` for the `quorumNumber` is also updated to reflect this change.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator registers for one or more quorums. This method *assumes* that `operator` is not already registered for any of `quorumNumbers`, and that there are no duplicates in `quorumNumbers`. These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperator`
* `RegistryCoordinator.registerOperatorWithChurn`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * Add the Operator's pubkey to the quorum's apk in `currentApk[quorum]`
    * Updates the quorum's `apkHistory`, pushing a new `ApkUpdate` for the current block number and setting its `apkHash` to the new hash of `currentApk[quorum]`.
        * *Note:* If the most recent entry in `apkHistory[quorum]` was made during the current block, this method updates the most recent entry rather than pushing a new one.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `operator` MUST already have a registered BLS pubkey (see `registerBLSPublicKey` above) 
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

#### `deregisterOperator`

```solidity
function deregisterOperator(
    address operator,
    bytes memory quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

`deregisterOperator` fetches the Operator's registered BLS pubkey (see `registerBLSPublicKey` above). For each quorum in `quorumNumbers`, `deregisterOperator` performs the same steps as `registerOperator` above - except that the Operator's pubkey is negated. Whereas `registerOperator` "adds" a pubkey to each quorum's apk, `deregisterOperator` "subtracts" a pubkey from each quorum's apk.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator deregisters from one or more quorums. This method *assumes* that `operator` is registered for all quorums in `quorumNumbers`, and that there are no duplicates in `quorumNumbers`. These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperatorWithChurn`
* `RegistryCoordinator.deregisterOperator`
* `RegistryCoordinator.ejectOperator`
* `RegistryCoordinator.updateOperators`
* `RegistryCoordinator.updateOperatorsForQuorum`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * Negate the Operator's pubkey, then subtract it from the quorum's apk in `currentApk[quorum]`
    * Updates the quorum's `apkHistory`, pushing a new `ApkUpdate` for the current block number and setting its `apkHash` to the new hash of `currentApk[quorum]`.
        * *Note:* If the most recent entry in `apkHistory[quorum]` was made during the current block, this method updates the most recent entry rather than pushing a new one.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `operator` MUST already have a registered BLS pubkey (see `registerBLSPublicKey` above)
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

#### `verifyAndRegisterG2PubkeyForOperator`

```solidity
function verifyAndRegisterG2PubkeyForOperator(
    address operator,
    BN254.G2Point calldata pubkeyG2
)
    external
    onlyRegistryCoordinatorOwner
```
`verifyAndRegisterG2PubkeyForOperator` verifies and registers a G2 public key for an operator that already has a G1 key. This method is used to retrieve all information for the `checkSignatures` entry point from a view function, avoiding the need to index this information offchain. The method ensures that the BLS key pair is derived from the same secret key and stores the G2 key for the operator.

This method is only callable by the `RegistryCoordinatorOwner`, which is the account that has the `owner` role inside the `RegistryCoordinator`.

*Effects*
* Stores the corresponding G2 public key of an operator's BLS keypair, based on their G1 public key, updating `operatorToPubkeyG2[operator]`.

*Requirements*
* Caller MUST be the `RegistryCoordinatorOwner`
* Operator must not have a G2 public key set
* The G2 pubic key must form a valid BN254 pairing with the stored G1 key, in effect verifying that the keypair is derived from the same secret key
---

### System Configuration

#### `initializeQuorum`

```solidity
function initializeQuorum(
    uint8 quorumNumber
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

This method is ONLY callable by the `RegistryCoordinator`. It is called when the `RegistryCoordinator` Owner creates a new quorum.

`initializeQuorum` initializes a new quorum by pushing an initial `ApkUpdate` to `apkHistory[quorumNumber]`. Other methods can validate that a quorum exists by checking whether `apkHistory[quorumNumber]` has a nonzero length.

*Entry Points*:
* `RegistryCoordinator.createQuorum`

*Effects*:
* Pushes an `ApkUpdate` to `apkHistory[quorumNumber]`. The update has a zeroed out `apkHash`, and its `updateBlockNumber` is set to the current block.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `apkHistory[quorumNumber].length` MUST be zero

---
````

## File: docs/registries/IndexRegistry.md
````markdown
## IndexRegistry

| File | Type | Proxy? |
| -------- | -------- | -------- |
| [`IndexRegistry.sol`](../../src/IndexRegistry.sol) | Singleton | Transparent proxy |

The `IndexRegistry` provides an `operatorIndex` for every registered Operator in every quorum. For example, if a quorum has `n` Operators, every Operator registered for that quorum will have an `operatorIndex` in the range `[0:n-1]`. The role of this contract is to provide an AVS with a common, on-chain ordering of Operators within a quorum.

*In EigenDA*, the Operator ordering properties of the `IndexRegistry` will eventually be used in proofs of custody, though this feature is not implemented yet.

#### Important State Variables

```solidity
/// @notice maps quorumNumber => operator id => current operatorIndex
/// NOTE: This mapping is NOT updated when an operator is deregistered,
/// so it's possible that an index retrieved from this mapping is inaccurate.
/// If you're querying for an operator that might be deregistered, ALWAYS 
/// check this index against the latest `_operatorIndexHistory` entry
mapping(uint8 => mapping(bytes32 => uint32)) public currentOperatorIndex;

/// @notice maps quorumNumber => operatorIndex => historical operator ids at that index
mapping(uint8 => mapping(uint32 => OperatorUpdate[])) internal _operatorIndexHistory;

/// @notice maps quorumNumber => historical number of unique registered operators
mapping(uint8 => QuorumUpdate[]) internal _operatorCountHistory;
 
struct OperatorUpdate {
    uint32 fromBlockNumber;
    bytes32 operatorId;
}

struct QuorumUpdate {
    uint32 fromBlockNumber;
    uint32 numOperators;
}
```

Operators are assigned a unique `operatorIndex` in each quorum they're registered for. If a quorum has `n` registered Operators, every Operator in that quorum will have an `operatorIndex` in the range `[0:n-1]`. To accomplish this, the `IndexRegistry` uses the three mappings listed above:
* `currentOperatorIndex` is a straightforward mapping of an Operator's current `operatorIndex` in a specific quorum. It is updated when an Operator registers for a quorum.
* `_operatorIndexHistory` keeps track of the `operatorIds` assigned to an `operatorIndex` at various points in time. This is used by offchain code to determine what `operatorId` belonged to an `operatorIndex` at a specific block.
* `_operatorCountHistory` keeps track of the number of Operators registered to each quorum over time. Note that a quorum's Operator count is also its "max `operatorIndex` + 1". Paired with `_operatorIndexHistory`, this allows offchain code to query the entire Operator set registered for a quorum at a given block number. For an example of this in the code, see `IndexRegistry.getOperatorListAtBlockNumber`.

*Note*: `currentOperatorIndex` is ONLY updated when an Operator is *assigned* to an `operatorIndex`. When an Operator deregisters and is removed, we don't update `currentOperatorIndex` because their `operatorIndex` is not "0" - that's held by another Operator. Their `operatorIndex` is also not the `operatorIndex` they currently have. There's not really a "right answer" for this - see https://github.com/Layr-Labs/eigenlayer-middleware/issues/126 for more details.

#### High-level Concepts

This document organizes methods according to the following themes (click each to be taken to the relevant section):
* [Registering and Deregistering](#registering-and-deregistering)
* [System Configuration](#system-configuration)

---    

### Registering and Deregistering

These methods are ONLY called through the `RegistryCoordinator` - when an Operator registers for or deregisters from one or more quorums:
* [`registerOperator`](#registeroperator)
* [`deregisterOperator`](#deregisteroperator)

#### `registerOperator`

```solidity
function registerOperator(
    bytes32 operatorId, 
    bytes calldata quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator 
    returns(uint32[] memory)
```

When an Operator registers for a quorum, the following things happen:
1. The current Operator count for the quorum is increased. 
    * This updates `_operatorCountHistory[quorum]`. The quorum's new "max `operatorIndex`" is equal to the quorum Operator count - 1.
    * Additionally, if the `_operatorIndexHistory` for the quorum indicates that this is the first time the quorum has reached a given Operator count, an initial `OperatorUpdate` is pushed to `_operatorIndexHistory` for the new operator count. This is to maintain an invariant: that existing quorum indices have nonzero history.
2. The quorum's max index (Operator count - 1) is assigned to the registering Operator as their current `operatorIndex`.
    * This updates `currentOperatorIndex[quorum][operatorId]`
    * This also updates `_operatorIndexHistory[quorum][prevOperatorCount]`, recording the `operatorId` as the latest holder of the `operatorIndex` in question.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator registers for one or more quorums. This method *assumes* that the `operatorId` is not already registered for any of `quorumNumbers`, and that there are no duplicates in `quorumNumbers`. These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperator`
* `RegistryCoordinator.registerOperatorWithChurn`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * Updates `_operatorCountHistory[quorum]`, increasing the quorum's `numOperators` by 1. 
        * Note that if the most recent update for the quorum is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * Updates `_operatorIndexHistory[quorum][newOperatorCount - 1]`, recording the `operatorId` as the latest holder of the new max `operatorIndex`. 
        * Note that if the most recent update for the quorum's index is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * Updates `currentOperatorIndex[quorum][operatorId]`, assigning the `operatorId` to the new max `operatorIndex`.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

#### `deregisterOperator`

```solidity
function deregisterOperator(
    bytes32 operatorId, 
    bytes calldata quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

When an Operator deregisters from a quorum, the following things happen:
1. The current Operator count for the quorum is decreased, updating `_operatorCountHistory[quorum]`. The new "max `operatorIndex`" is equal to the new Operator count (minus 1).
2. The Operator currently assigned to the now-invalid `operatorIndex` is "popped".
    * This updates `_operatorIndexHistory[quorum][newOperatorCount]`, recording that the Operator assigned to this `operatorIndex` is `OPERATOR_DOES_NOT_EXIST_ID`
3. If the deregistering Operator and the popped Operator are not the same, the popped Operator is assigned a new `operatorIndex`: the deregistering Operator's previous `operatorIndex`.
    * This updates `_operatorIndexHistory[quorum][removedOperatorIndex]`, recording that the popped Operator is assigned to this `operatorIndex`.
    * This also updates `currentOperatorIndex[quorum][removedOperator]`, assigning the popped Operator to the old Operator's `operatorIndex`.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator deregisters from one or more quorums. This method *assumes* that the `operatorId` is currently registered for each quorum in `quorumNumbers`, and that there are no duplicates in `quorumNumbers`. These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperatorWithChurn`
* `RegistryCoordinator.deregisterOperator`
* `RegistryCoordinator.ejectOperator`
* `RegistryCoordinator.updateOperators`
* `RegistryCoordinator.updateOperatorsForQuorum`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * Updates `_operatorCountHistory[quorum]`, decreasing the quorum's `numOperators` by 1. 
        * Note that if the most recent update for the quorum is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * Updates `_operatorIndexHistory[quorum][newOperatorCount]`, "popping" the Operator that currently holds this `operatorIndex`, and marking it as assigned to `OPERATOR_DOES_NOT_EXIST_ID`. 
        * Note that if the most recent update for the quorum's `operatorIndex` is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * If `operatorId` is NOT the popped Operator, the popped Operator is assigned to `operatorId's` current `operatorIndex`. (Updates `_operatorIndexHistory` and `currentOperatorIndex`)

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

---

### System Configuration

#### `initializeQuorum`

```solidity
function initializeQuorum(
    uint8 quorumNumber
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

This method is ONLY callable by the `RegistryCoordinator`. It is called when the `RegistryCoordinator` Owner creates a new quorum.

`initializeQuorum` initializes a new quorum by pushing an initial `QuorumUpdate` to `_operatorCountHistory[quorumNumber]`, setting the initial `numOperators` for the quorum to 0. 

Other methods can validate that a quorum exists by checking whether `_operatorCountHistory[quorumNumber]` has a nonzero length.

*Entry Points*:
* `RegistryCoordinator.createQuorum`

*Effects*:
* Pushes a `QuorumUpdate` to `_operatorCountHistory[quorumNumber]`. The update's `updateBlockNumber` is set to the current block, and `numOperators` is set to 0.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `_operatorCountHistory[quorumNumber].length` MUST be zero

---
````

## File: docs/registries/SocketRegistry.md
````markdown
## SocketRegistry

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`SocketRegistry.sol`](../src/SocketRegistry.sol) | Singleton | Transparent proxy |

The `SocketRegistry` is a simple registry contract that keeps track of operator sockets (arbitrary strings). This socket could represent network connection information such as IP addresses, ports, or other connectivity details.

#### High-level Concepts

This registry maintains a mapping between operator IDs (represented as bytes32 values) and their corresponding socket information. The contract is designed to work in conjunction with the `SlashingRegistryCoordinator`, which is the only contract authorized to update socket information for operators.

This document organizes methods according to the following themes:
* [Socket Management](#socket-management)

---    

### Socket Management

These methods allow for managing operator socket information:
* [`setOperatorSocket`](#setoperatorsocket)

#### `setOperatorSocket`

```solidity
function setOperatorSocket(
    bytes32 _operatorId,
    string memory _socket
)   
    external 
    onlySlashingRegistryCoordinator
```

Sets a socket string with an operator ID (hash of the G1 BLS public key) in the registry. This function is called by the `SlashingRegistryCoordinator`.

*Entry Points:*
Called by the `SlashingRegistryCoordinator` when an operator is registered or needs to update their socket information

*Effects:*
* Sets operatorIdToSocket[_operatorId] to the provided `_socket` string

*Requirements:*
* Caller MUST be the `SlashingRegistryCoordinator`
````

## File: docs/registries/StakeRegistry.md
````markdown
[core-docs-dev]: https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/docs

## StakeRegistry

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`StakeRegistry.sol`](../src/StakeRegistry.sol) | Singleton | Transparent proxy |

The `StakeRegistry` interfaces with the EigenLayer core contracts to determine the individual and collective stake weight of each Operator registered for each quorum. These weights are used to determine an Operator's relative weight for each of an AVS's quorums. And in the `RegistryCoordinator` specifically, they play an important role in *churn*: determining whether an Operator is eligible to replace another Operator in a quorum.

#### Calculating Stake Weight

Stake weight is primarily a function of the number of shares an Operator has been delegated within the EigenLayer core contracts, along with a per-quorum configuration maintained by the `RegistryCoordinator` Owner (see [System Configuration](#system-configuration) below). This configuration determines, for a given quorum, which Strategies "count" towards an Operator's total stake weight, as well as "how much" each Strategy counts for:

```solidity
/// @notice maps quorumNumber => list of strategies considered (and each strategy's multiplier)
mapping(uint8 => StrategyParams[]) public strategyParams;

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

For a given quorum, an Operator's stake weight is determined by iterating over the quorum's list of `StrategyParams` and querying `DelegationManager.operatorShares(operator, strategy)`. The result is multiplied by the corresponding `multiplier` (and divided by the `WEIGHTING_DIVISOR`) to calculate the Operator's weight for that strategy. Then, this result is added to a growing sum of stake weights -- and after the quorum's `StrategyParams` have all been considered, the Operator's total stake weight is calculated.

Note that the `RegistryCoordinator` Owner also configures a "minimum stake" for each quorum, which an Operator must meet in order to register for (or remain registered for) a quorum.

For more information on the `DelegationManager`, strategies, and shares, see the [EigenLayer core docs][core-docs-dev].

#### High-level Concepts

This document organizes methods according to the following themes (click each to be taken to the relevant section):
* [Registering and Deregistering](#registering-and-deregistering)
* [Updating Registered Operators](#updating-registered-operators)
* [System Configuration](#system-configuration)

---    

### Registering and Deregistering

These methods are ONLY called through the `RegistryCoordinator` - when an Operator registers for or deregisters from one or more quorums:
* [`registerOperator`](#registeroperator)
* [`deregisterOperator`](#deregisteroperator)

#### `registerOperator`

```solidity
function registerOperator(
    address operator,
    bytes32 operatorId,
    bytes calldata quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator 
    returns (uint96[] memory, uint96[] memory)
```

When an Operator registers for a quorum, the `StakeRegistry` first calculates the Operator's current weighted stake. If the Operator meets the quorum's configured minimum stake, the Operator's `operatorStakeHistory` is updated to reflect the Operator's current stake.

Additionally, the Operator's stake is added to the `_totalStakeHistory` for that quorum.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator registers for one or more quorums. This method *assumes* that:
* `operatorId` belongs to the `operator`
* `operatorId` is not already registered for any of `quorumNumbers`
* There are no duplicates in `quorumNumbers`

These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperator`
* `RegistryCoordinator.registerOperatorWithChurn`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * The Operator's total stake weight is calculated, and the result is recorded in `operatorStakeHistory[operatorId][quorum]`.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * The Operator's total stake weight is added to the quorum's total stake weight in `_totalStakeHistory[quorum]`.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)
* For each `quorum` in `quorumNumbers`:
    * The calculated total stake weight for the Operator MUST NOT be less than that quorum's minimum stake

#### `deregisterOperator`

```solidity
function deregisterOperator(
    bytes32 operatorId,
    bytes calldata quorumNumbers
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

When an Operator deregisters from a quorum, the `StakeRegistry` sets their stake to 0 and subtracts their stake from the quorum's total stake, updating `operatorStakeHistory` and `_totalStakeHistory`, respectively.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator deregisters from one or more quorums. This method *assumes* that:
* `operatorId` is currently registered for each quorum in `quorumNumbers`
* There are no duplicates in `quorumNumbers`

These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.registerOperatorWithChurn`
* `RegistryCoordinator.deregisterOperator`
* `RegistryCoordinator.ejectOperator`
* `RegistryCoordinator.updateOperators`
* `RegistryCoordinator.updateOperatorsForQuorum`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * The Operator's stake weight in `operatorStakeHistory[operatorId][quorum]` is set to 0.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * The Operator's stake weight is removed from the quorum's total stake weight in `_totalStakeHistory[quorum]`.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

---

### Updating Registered Operators

#### `updateOperatorStake`

```solidity
function updateOperatorStake(
    address operator, 
    bytes32 operatorId, 
    bytes calldata quorumNumbers
) 
    external 
    onlyRegistryCoordinator 
    returns (uint192)
```

AVSs will require up-to-date views on an Operator's stake. When an Operator's shares change in the EigenLayer core contracts (due to additional delegation, undelegation, withdrawals, etc), this change is not automatically pushed to middleware contracts. This is because middleware contracts are unique to each AVS, and core contract share updates would become prohibitively expensive if they needed to update each AVS every time an Operator's shares changed.

Rather than *pushing* updates, `RegistryCoordinator.updateOperators` and `updateOperatorsForQuorum` can be called by anyone to *pull* updates from the core contracts. Those `RegistryCoordinator` methods act as entry points for this method, which performs the same stake weight calculation as `registerOperator`, updating the Operator's `operatorStakeHistory` and the quorum's `_totalStakeHistory`.

*Note*: there is one major difference between `updateOperatorStake` and `registerOperator` - if an Operator does NOT meet the minimum stake for a quorum, their stake weight is set to 0 and removed from the quorum's total stake weight, mimicing the behavior of `deregisterOperator`. For each quorum where this occurs, that quorum's number is added to a bitmap, `uint192 quorumsToRemove`, which is returned to the `RegistryCoordinator`. The `RegistryCoordinator` uses this returned bitmap to completely deregister Operators, maintaining an invariant that if an Operator's stake weight for a quorum is 0, they are NOT registered for that quorum.

This method is ONLY callable by the `RegistryCoordinator`, and is called when an Operator registers for one or more quorums. This method *assumes* that:
* `operatorId` belongs to the `operator`
* `operatorId` is currently registered for each quorum in `quorumNumbers`
* There are no duplicates in `quorumNumbers`

These properties are enforced by the `RegistryCoordinator`.

*Entry Points*:
* `RegistryCoordinator.updateOperators`
* `RegistryCoordinator.updateOperatorsForQuorum`

*Effects*:
* For each `quorum` in `quorumNumbers`:
    * The Operator's total stake weight is calculated, and the result is recorded in `operatorStakeHistory[operatorId][quorum]`. If the Operator does NOT meet the quorum's configured minimum stake, their stake weight is set to 0 instead.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.
    * The Operator's stake weight delta is applied to the quorum's total stake weight in `_totalStakeHistory[quorum]`.
        * Note that if the most recent update is from the current block number, the entry is updated. Otherwise, a new entry is pushed.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* Each quorum in `quorumNumbers` MUST be initialized (see `initializeQuorum` below)

---

### System Configuration

Two methods are used by the `RegistryCoordinator` to initialize new quorums in the `StakeRegistry`:
* [`initializeDelegatedStakeQuorum`](#initializedelegatedstakequorum)
* [`initializeSlashableStakeQuorum`](#initializeslashablestakequorum)


These methods are used by the `RegistryCoordinator's` Owner to configure initialized quorums in the `StakeRegistry`. They are not expected to be called very often, and will require updating Operator stakes via `RegistryCoordinator.updateOperatorsForQuorum` to maintain up-to-date views on Operator stake weights. Methods follow:
* [`setMinimumStakeForQuorum`](#setminimumstakeforquorum)
* [`addStrategies`](#addstrategies)
* [`setSlashableLookAhead`](#setslashablelookahead)
* [`removeStrategies`](#removestrategies)
* [`modifyStrategyParams`](#modifystrategyparams)

#### `initializeDelegatedStakeQuorum`

```solidity
function initializeDelegatedStakeQuorum(
    uint8 quorumNumber,
    uint96 minimumStake,
    StrategyParams[] memory _strategyParams
) 
    public 
    virtual 
    onlyRegistryCoordinator

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

This method is ONLY callable by the `RegistryCoordinator`, and is called when the `RegistryCoordinator` Owner creates a new delegated stake quorum.

`initializeDelegatedStakeQuorum` initializes a new delegated stake quorum by pushing an initial `StakeUpdate` to `_totalStakeHistory[quorumNumber]`, with an initial stake of 0. Other methods can validate that a quorum exists by checking whether `_totalStakeHistory[quorumNumber]` has a nonzero length.

Additionally, this method configures a `minimumStake` for the quorum, as well as the `StrategyParams` it considers when calculating stake weight.

*Entry Points*:
* `RegistryCoordinator.createDelegatedStakeQuorum`

*Effects*:
* See `addStrategies` below
* See `setMinimumStakeForQuorum` below
* Pushes a `StakeUpdate` to `_totalStakeHistory[quorumNumber]`. The update's `updateBlockNumber` is set to the current block, and `stake` is set to 0.

*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `quorumNumber` MUST NOT belong to an existing, initialized quorum
* See `addStrategies` below
* See `setMinimumStakeForQuorum` below

#### `initializeSlashableStakeQuorum`

```solidity
function initializeSlashableStakeQuorum(
    uint8 quorumNumber,
    uint96 minimumStake,
    uint32 lookAheadPeriod,
    StrategyParams[] memory _strategyParams
)
    public
    virtual
    onlyRegistryCoordinator

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

This method is ONLY callable by the `RegistryCoordinator`, and is called when the `RegistryCoordinator` Owner creates a new slashable stake quorum.

`initializeSlashableStakeQuorum` initializes a new quorum by pushing an initial `StakeUpdate` to `_totalStakeHistory[quorumNumber]`, with an initial stake of 0. Other methods can validate that a quorum exists by checking whether `_totalStakeHistory[quorumNumber]` has a nonzero length.

This method configures a `minimumStake` for the quorum, defines the `StrategyParams` used when calculating stake weight and sets the `lookAheadPeriod` which defines how many blocks ahead to when considering the minimum amount of slahable stake. Note that `lookAheadPeriod` should be less than the `DEALLOCATION_DELAY` (14 days within the `AllocationManager`) blocks in the future, to avoid calculating stake values that may not reflect the operators true allocated stake.

*Entry Points*:
* `RegistryCoordinator.createSlashableStakeQuorum`

*Effects*:
* See `addStrategies` below
* See `setMinimumStakeForQuorum` below
* See `setSlashableLookAhead` below
* Pushes a `StakeUpdate` to `_totalStakeHistory[quorumNumber]`. The update's `updateBlockNumber` is set to the current block, and `stake` is set to 0.


*Requirements*:
* Caller MUST be the `RegistryCoordinator`
* `quorumNumber` MUST NOT belong to an existing, initialized quorum
* See `addStrategies` below
* See `setMinimumStakeForQuorum` below

#### `setMinimumStakeForQuorum`

```solidity
function setMinimumStakeForQuorum(
    uint8 quorumNumber, 
    uint96 minimumStake
) 
    public 
    virtual 
    onlyCoordinatorOwner 
    quorumExists(quorumNumber)
```

Allows the `RegistryCoordinator` Owner to configure the `minimumStake` for an existing quorum. This value is used to determine whether an Operator has sufficient stake to register for (or stay registered for) a quorum.

There is no lower or upper bound on a quorum's minimum stake.

*Effects*:
* Set `minimumStakeForQuorum[quorum]` to `minimumStake`

*Requirements*:
* Caller MUST be `RegistryCoordinator.owner()`
* `quorumNumber` MUST belong to an existing, initialized quorum

#### `addStrategies`

```solidity
function addStrategies(
    uint8 quorumNumber, 
    StrategyParams[] memory _strategyParams
) 
    public 
    virtual 
    onlyCoordinatorOwner 
    quorumExists(quorumNumber)

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

Allows the `RegistryCoordinator` Owner to add `StrategyParams` to a quorum, which effect how Operators' stake weights are calculated.

For each `StrategyParams` added, this method checks that the incoming `strategy` has not already been added to the quorum. This is done via a relatively expensive loop over storage, but this function isn't expected to be called very often.

*Effects*:
* Each added `_strategyParams` is pushed to the quorum's stored `strategyParams[quorumNumber]`

*Requirements*:
* Caller MUST be `RegistryCoordinator.owner()`
* `quorumNumber` MUST belong to an existing, initialized quorum
* `_strategyParams` MUST NOT be empty
* The quorum's current `StrategyParams` count plus the new `_strategyParams` MUST NOT exceed `MAX_WEIGHING_FUNCTION_LENGTH`
* `_strategyParams` MUST NOT contain duplicates, and MUST NOT contain strategies that are already being considered by the quorum
* For each `_strategyParams` being added, the `multiplier` MUST NOT be 0

#### `removeStrategies`

```solidity
function removeStrategies(
    uint8 quorumNumber,
    uint256[] memory indicesToRemove
) 
    public 
    virtual 
    onlyCoordinatorOwner 
    quorumExists(quorumNumber)

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

Allows the `RegistryCoordinator` Owner to remove `StrategyParams` from a quorum, which effect how Operators' stake weights are calculated. Removals are processed by removing specific indices passed in by the caller.

For each `StrategyParams` removed, this method replaces `strategyParams[quorumNumber][indicesToRemove[i]]` with the last item in `strategyParams[quorumNumber]`, then pops the last element of `strategyParams[quorumNumber]`.

*Effects*:
* Removes the specified `StrategyParams` according to their index in the quorum's `strategyParams` list.

*Requirements*:
* Caller MUST be `RegistryCoordinator.owner()`
* `quorumNumber` MUST belong to an existing, initialized quorum
* `indicesToRemove` MUST NOT be empty

#### `modifyStrategyParams`

```solidity
function modifyStrategyParams(
    uint8 quorumNumber,
    uint256[] calldata strategyIndices,
    uint96[] calldata newMultipliers
) 
    public 
    virtual 
    onlyCoordinatorOwner 
    quorumExists(quorumNumber)

struct StrategyParams {
    IStrategy strategy;
    uint96 multiplier;
}
```

Allows the `RegistryCoordinator` Owner to modify the multipliers specified in a quorum's configured `StrategyParams`.

*Effects*:
* The quorum's `StrategyParams` at the specified `strategyIndices` are given a new multiplier

*Requirements*:
* Caller MUST be `RegistryCoordinator.owner()`
* `quorumNumber` MUST belong to an existing, initialized quorum
* `strategyIndices` MUST NOT be empty
* `strategyIndices` and `newMultipliers` MUST have equal lengths


#### `setSlashableLookAhead`

```solidity
function setSlashableStakeLookahead(
    uint8 quorumNumber,
    uint32 _lookAheadBlocks
)
    external
    onlyCoordinatorOwner
    quorumExists(quorumNumber)
```

Allows the `RegistryCoordinator` Owner to modify the slashable lookahead for a slashable stake quorum.

*Effects*
* The quorum's `SlashablableStakeLookAhead` is updated to `_lookAheadBlocks`

*Requirements*
* Caller MUST be `RegistryCoordinator.owner()`
* `quorumNumber` MUST belong to an existing initialized quorum
* `quorumNumber` MUST map to a slashable stake quorum

---
````

## File: docs/slashing/InstantSlasher.md
````markdown
## InstantSlasher

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`InstantSlasher.sol`](../src/InstantSlasher.sol) | Implementation | No |

The `InstantSlasher` is a concrete implementation of the `SlasherBase` contract that provides immediate execution of slashing requests without any delay or veto period. This contract should be used with caution, as slashing is a critical operation within an AVS. This implementation is reccomended if your AVS is mature, robust and the slashing conditions are well understood (i.e. those which do not arise from subjective or non-malicious reasons like a software bug)

*As of current implementation*:
* This contract executes slashing requests immediately when initiated by the authorized slasher
* No waiting period or veto mechanism is implemented
* Each slashing request is assigned a unique ID

---    

### Core Functionality

#### `fulfillSlashingRequest`
```solidity
function fulfillSlashingRequest(
    IAllocationManager.SlashingParams calldata _slashingParams
) 
    external 
    virtual 
    override(IInstantSlasher) 
    onlySlasher
```
Immediately executes a slashing request against the specified operator.

*Entry Points*:
* External calls from the authorized slasher

*Effects*:
* Assigns a unique ID to the slashing request
* Calls the internal `_fulfillSlashingRequest` function to execute the slashing action
* Increments the `nextRequestId` counter for future requests

*Requirements*:
* Caller MUST be the authorized slasher (enforced by `onlySlasher` modifier)
* Slashing parameters must be valid
````

## File: docs/slashing/SlasherBase.md
````markdown
## SlasherBase

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`SlasherBase.sol`](../src/base/SlasherBase.sol) | Abstract | No |

The `SlasherBase` is an abstract contract that provides core slashing functionality intended for AVSs to inherit. It serves as the foundation for implementing slashing mechanisms that interact with EigenLayer's `AllocationManager`. There are two implementations of this contract which are the [`VetoableSlasher`](./VetoableSlasher.md) and the [`InstantSlasher`](./InstantSlasher.md).

*As of current implementation*:
* This contract provides the base functionality for slashing operators in EigenLayer based on certain conditions
* Concrete implementations will determine when and how slashing is performed

---    

### Core Functionality

#### `_fulfillSlashingRequest`
```solidity
function _fulfillSlashingRequest(
    uint256 _requestId,
    IAllocationManager.SlashingParams memory _params
) 
    internal 
    virtual
```
Internal function that executes a slashing request by calling the `AllocationManager.slashOperator` method. The implementations of this contract will call this internal method.

*Effects*:
* Calls the allocation manager to slash the specified operator
* Emits an `OperatorSlashed` event with details about the slashing action

*Requirements*:
* The allocation manager must be properly set up
* The slashing parameters must be valid

#### `_checkSlasher`
```solidity
function _checkSlasher(
    address account
) 
    internal 
    view 
    virtual
```
Internal function that verifies if an account is the authorized slasher.

*Effects*:
* Reverts with an `OnlySlasher` error if the provided account is not the authorized slasher

*Requirements*:
* The account must match the stored slasher address

### Modifiers

#### `onlySlasher`
```solidity
modifier onlySlasher()
```
Ensures that only the authorized slasher can call certain functions. This will commonly be set as the address of the AVS `ServiceManager` which would expose a permissioned or permissionless external function to call the slashing contract. Keeping the contracts decoupled allows for easier upgrade paths.

*Effects*:
* Calls `_checkSlasher` to verify that the caller is the authorized slasher
* Allows the function execution to proceed if the check passes
````

## File: docs/slashing/VetoableSlasher.md
````markdown
## VetoableSlasher

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`VetoableSlasher.sol`](../src/VetoableSlasher.sol) | Implementation | No |

The `VetoableSlasher` is a concrete implementation of the [`SlasherBase`](./SlasherBase.md) contract that adds a veto mechanism to the slashing process. This implementation introduces a waiting period during which a designated veto committee can cancel slashing requests before they are executed. This slasher implementation is recommended for AVSs to use as their systems matures, to account for slashing faults that arise from subjective conditions or non-malicious reasons such as a software bug. As the AVSs matures and the slashing conditions become well defined, the instant slasher may be a suitable contract. It is reccomended to have your veto comittee to be comprised of a set of diverse subject matter experts. 

*As of current implementation*:
* Slashing requests are queued and can only be fulfilled after a configurable veto window has passed
* A designated veto committee can cancel slashing requests during the veto window
* Slashing requests have a status that tracks their lifecycle: Requested, Cancelled, or Completed

---    

### Core Functionality

#### `queueSlashingRequest`
```solidity
function queueSlashingRequest(
    IAllocationManager.SlashingParams calldata params
) 
    external 
    virtual 
    override 
    onlySlasher
```
Creates and queues a new slashing request that will be executable after the veto window has passed.

*Entry Points*:
* External calls from the authorized slasher

*Effects*:
* Assigns a unique ID to the slashing request
* Creates a new slashing request with the provided parameters
* Sets the request status to `Requested`
* Stores the current block number as the request block
* Emits a `SlashingRequested` event

*Requirements*:
* Caller MUST be the authorized slasher (enforced by `onlySlasher` modifier)
* Slashing parameters must be valid

#### `cancelSlashingRequest`
```solidity
function cancelSlashingRequest(
    uint256 requestId
) 
    external 
    virtual 
    override 
    onlyVetoCommittee
```
Allows the veto committee to cancel a pending slashing request within the veto window.

*Entry Points*:
* External calls from the veto committee

*Effects*:
* Changes the request status from `Requested` to `Cancelled`
* Emits a `SlashingRequestCancelled` event

*Requirements*:
* Caller MUST be the veto committee (enforced by `onlyVetoCommittee` modifier)
* The request MUST be in the `Requested` status
* The current block number MUST be less than the request block plus the veto window

#### `fulfillSlashingRequest`
```solidity
function fulfillSlashingRequest(
    uint256 requestId
) 
    external 
    virtual 
    override 
    onlySlasher
```
Executes a slashing request after the veto window has passed, if it has not been cancelled.

*Entry Points*:
* External calls from the authorized slasher

*Effects*:
* Changes the request status from `Requested` to `Completed`
* Calls the internal `_fulfillSlashingRequest` function to execute the slashing action

*Requirements*:
* Caller MUST be the authorized slasher (enforced by `onlySlasher` modifier)
* The request MUST be in the `Requested` status
* The veto window MUST have passed (current block number >= request block + veto window)

### Modifiers

#### `onlyVetoCommittee`
```solidity
modifier onlyVetoCommittee()
```
Ensures that only the veto committee can call certain functions.

*Effects*:
* Calls `_checkVetoCommittee` to verify that the caller is the veto committee
* Allows the function execution to proceed if the check passes
````

## File: docs/storage-report/AVSRegistrar.md
````markdown
╭------+------+------+--------+-------+----------╮
| Name | Type | Slot | Offset | Bytes | Contract |
+================================================+
╰------+------+------+--------+-------+----------╯
````

## File: docs/storage-report/RegistryCoordinatorStorage.md
````markdown
╭-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------╮
| Name                    | Type                                                                      | Slot | Offset | Bytes | Contract                                                      |
+=============================================================================================================================================================================================+
| quorumCount             | uint8                                                                     | 0    | 0      | 1     | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| _quorumParams           | mapping(uint8 => struct ISlashingRegistryCoordinatorTypes.OperatorSetParam)       | 1    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| _operatorBitmapHistory  | mapping(bytes32 => struct ISlashingRegistryCoordinatorTypes.QuorumBitmapUpdate[]) | 2    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| _operatorInfo           | mapping(address => struct ISlashingRegistryCoordinatorTypes.OperatorInfo)         | 3    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| isChurnApproverSaltUsed | mapping(bytes32 => bool)                                                  | 4    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| quorumUpdateBlockNumber | mapping(uint8 => uint256)                                                 | 5    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| registries              | address[]                                                                 | 6    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| churnApprover           | address                                                                   | 7    | 0      | 20    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| ejector                 | address                                                                   | 8    | 0      | 20    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| lastEjectionTimestamp   | mapping(address => uint256)                                               | 9    | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| ejectionCooldown        | uint256                                                                   | 10   | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| isOperatorSetAVS        | bool                                                                      | 11   | 0      | 1     | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| isM2Quorum              | mapping(uint8 => bool)                                                    | 12   | 0      | 32    | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
|-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------|
| __GAP                   | uint256[37]                                                               | 13   | 0      | 1184  | src/RegistryCoordinatorStorage.sol:RegistryCoordinatorStorage |
╰-------------------------+---------------------------------------------------------------------------+------+--------+-------+---------------------------------------------------------------╯
````

## File: docs/storage-report/ServiceManagerBaseStorage.md
````markdown
╭------------------+-------------+------+--------+-------+-------------------------------------------------------------╮
| Name             | Type        | Slot | Offset | Bytes | Contract                                                    |
+======================================================================================================================+
| _initialized     | uint8       | 0    | 0      | 1     | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| _initializing    | bool        | 0    | 1      | 1     | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| __gap            | uint256[50] | 1    | 0      | 1600  | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| _owner           | address     | 51   | 0      | 20    | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| __gap            | uint256[49] | 52   | 0      | 1568  | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| rewardsInitiator | address     | 101  | 0      | 20    | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
|------------------+-------------+------+--------+-------+-------------------------------------------------------------|
| __GAP            | uint256[49] | 102  | 0      | 1568  | src/ServiceManagerBaseStorage.sol:ServiceManagerBaseStorage |
╰------------------+-------------+------+--------+-------+-------------------------------------------------------------╯
````

## File: docs/BLSSignatureChecker.md
````markdown
[core-docs-dev]: https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/docs
[core-dmgr-docs]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/DelegationManager.md

[eigenda-service-manager]: https://github.com/Layr-Labs/eigenda/blob/dev-contracts/contracts/src/core/EigenDAServiceManager.sol

## BLSSignatureChecker

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`BLSSignatureChecker.sol`](../src/BLSSignatureChecker.sol) | Singleton | Transparent proxy |
| [`OperatorStateRetriever.sol`](../src/OperatorStateRetriever.sol) | Singleton | None |

`BLSSignatureChecker` and `OperatorStateRetriever` perform (respectively) the onchain and offchain portions of BLS signature validation for the aggregate of a quorum's registered Operators.

The `OperatorStateRetriever` has various view methods intended to be called by offchain infrastructure in order to prepare a call to `BLSSignatureChecker.checkSignatures`. These methods traverse the state histories kept by the various registry contracts (see [./RegistryCoordinator.md](./RegistryCoordinator.md)) to query states at specific block numbers.

These historical states are then used within `BLSSignatureChecker` to validate a BLS signature formed from an aggregated subset of the Operators registered for one or more quorums at some specific block number.

#### High-level Concepts

This document organizes methods according to the following themes (click each to be taken to the relevant section):
* [Onchain](#onchain)
* [Offchain](#offchain)
* [System Configuration](#system-configuration)

---

### Onchain

#### `BLSSignatureChecker.checkSignatures`

```solidity
function checkSignatures(
    bytes32 msgHash, 
    bytes calldata quorumNumbers,
    uint32 referenceBlockNumber, 
    NonSignerStakesAndSignature memory params
)
    public 
    view
    returns (QuorumStakeTotals memory, bytes32)

struct NonSignerStakesAndSignature {
    uint32[] nonSignerQuorumBitmapIndices;
    BN254.G1Point[] nonSignerPubkeys;
    BN254.G1Point[] quorumApks;
    BN254.G2Point apkG2;
    BN254.G1Point sigma;
    uint32[] quorumApkIndices;
    uint32[] totalStakeIndices;
    uint32[][] nonSignerStakeIndices;
}

struct QuorumStakeTotals {
    uint96[] signedStakeForQuorum;
    uint96[] totalStakeForQuorum;
}
```

The goal of this method is to allow an AVS to validate a BLS signature formed from the aggregate pubkey ("apk") of Operators registered in one or more quorums at some `referenceBlockNumber`.

Some notes on method parameters:
* `msgHash` is the hash being signed by the apk. Note that the caller is responsible for ensuring `msgHash` is a hash! If someone can provide arbitrary input, it may be possible to tamper with signature verification.
* `referenceBlockNumber` is the reason each registry contract keeps historical states: so that lookups can be performed on each registry's info at a particular block. This is important because Operators may sign some data on behalf of an AVS, then deregister from one or more of the AVS's quorums. Historical states allow signature validation to be performed against a "fixed point" in AVS/quorum history.
* `quorumNumbers` is used to perform signature validation across one *or more* quorums. Also, Operators may be registered for more than one quorum - and for each quorum an Operator is registered for, that Operator's pubkey is included in that quorum's apk within the `BLSApkRegistry`. This means that, when calculating an apk across multiple `quorumNumbers`, Operators registered for more than one of these quorums will have their pubkey included more than once in the total apk.
* `params` contains both a signature from all signing Operators, as well as several fields that identify registered, non-signing Operators. While non-signing Operators haven't contributed to the signature, but need to be accounted for because, as Operators registered for one or more signing quorums, their public keys are included in that quorum's apk. Essentially, in order to validate the signature, nonsigners' public keys need to be subtracted out from the total apk to derive the apk that actually signed the message.

This method performs the following steps. Note that each step involves lookups of historical state from `referenceBlockNumber`, but the writing in this section will use the present tense because adding "at the `referenceBlockNumber`" everywhere gets confusing. Steps follow:
1. Calculate the *total nonsigner apk*, an aggregate pubkey composed of all nonsigner pubkeys. For each nonsigner:
    * Query the `RegistryCoordinator` to get the nonsigner's registered quorums.
    * Multiply the nonsigner's pubkey by the number of quorums in `quorumNumbers` the nonsigner is registered for.
    * Add the result to the *total nonsigner apk*.
2. Calculate the negative of the *total nonsigner apk*.
3. For each quorum:
    * Query the `BLSApkRegistry` to get the *quorum apk*: the aggregate pubkey of all Operators registered for that quorum.
    * Add the *quorum apk* to the *total nonsigner apk*. This effectively subtracts out any pubkeys belonging to nonsigning Operators in the quorum, leaving only pubkeys of signing Operators. We'll call the result the *total signing apk*.
    * Query the `StakeRegistry` to get the total stake for the quorum.
    * For each nonsigner, if the nonsigner is registered for the quorum, query the `StakeRegistry` for their stake and subtract it from the total. This leaves only stake belonging to signing Operators.
4. Use the `msgHash`, the *total signing apk*, `params.apkG2`, and `params.sigma` to validate the BLS signature.
5. Return the total stake and signing stakes for each quorum, along with a hash identifying the `referenceBlockNumber` and non-signers 

*Entry Points* (EigenDA):
* Called by [`EigenDAServiceManager.confirmBatch`][eigenda-service-manager]

*Requirements*:
* Input validation:
    * Quorum-related fields MUST have equal lengths: `quorumNumbers`, `params.quorumApks`, `params.quorumApkIndices`, `params.totalStakeIndices`, `params.nonSignerStakeIndices`
    * Nonsigner-related fields MUST have equal lengths: `params.nonSignerPubkeys`, `params.nonSignerQuorumBitmapIndices`
    * `referenceBlockNumber` MUST be less than `block.number`
    * `quorumNumbers` MUST be an ordered list of valid, initialized quorums
    * `params.nonSignerPubkeys` MUST ONLY contain unique pubkeys, in ascending order of their pubkey hash
* For each quorum:
    * If stale stakes are forbidden (see [`BLSSignatureChecker.setStaleStakesForbidden`](#blssignaturecheckersetstalestakesforbidden)), check the last `quorumUpdateBlockNumber` is within `DelegationManager.minWithdrawalDelayBlocks` of `referenceBlockNumber`. This references a value in the EigenLayer core contracts - see [EigenLayer core docs][core-docs-dev] for more info.
    * Validate that each `params.quorumApks` corresponds to the quorum's apk at the `referenceBlockNumber`
* For each historical state lookup, the `referenceBlockNumber` and provided index MUST point to a valid historical entry: 
    * `referenceBlockNumber` MUST come after the entry's `updateBlockNumber`
    * The entry's `nextUpdateBlockNumber` MUST EITHER be 0, OR greater than `referenceBlockNumber`

---

### Offchain

These methods perform very gas-heavy lookups through various registry states, and are called by offchain infrastructure to construct calldata for a call to `BLSSignatureChecker.checkSignatures`:
* [`OperatorStateRetriever.getOperatorState (operatorId)`](#operatorstateretrievergetoperatorstate-operatorid)
* [`OperatorStateRetriever.getOperatorState (quorumNumbers)`](#operatorstateretrievergetoperatorstate-quorumnumbers)
* [`OperatorStateRetriever.getCheckSignaturesIndices`](#operatorstateretrievergetchecksignaturesindices)

#### `OperatorStateRetriever.getOperatorState (operatorId)`

```solidity
function getOperatorState(
    IRegistryCoordinator registryCoordinator, 
    bytes32 operatorId, 
    uint32 blockNumber
) 
    external 
    view 
    returns (uint256, Operator[][] memory)

struct Operator {
    bytes32 operatorId;
    uint96 stake;
}
```

Traverses history in the `RegistryCoordinator`, `IndexRegistry`, and `StakeRegistry` to retrieve information on an Operator (given by `operatorId`) and the quorums they are registered for at a specific `blockNumber`. Returns:
* `uint256`: a bitmap of the quorums the Operator was registered for at `blockNumber`
* `Operator[][]`: For each of the quorums mentioned above, this is a list of the Operators registered for that quorum at `blockNumber`, containing each Operator's `operatorId` and `stake`.

#### `OperatorStateRetriever.getOperatorState (quorumNumbers)`

```solidity
function getOperatorState(
    IRegistryCoordinator registryCoordinator, 
    bytes memory quorumNumbers, 
    uint32 blockNumber
) 
    public 
    view 
    returns(Operator[][] memory)
```

Traverses history in the `RegistryCoordinator`, `IndexRegistry`, and `StakeRegistry` to retrieve information on the Operator set registered for each quorum in `quorumNumbers` at `blockNumber`. Returns:
* `Operator[][]`: For each quorum in `quorumNumbers`, this is a list of the Operators registered for that quorum at `blockNumber`, containing each Operator's `operatorId` and `stake`.

#### `OperatorStateRetriever.getCheckSignaturesIndices`

```solidity
function getCheckSignaturesIndices(
    IRegistryCoordinator registryCoordinator,
    uint32 referenceBlockNumber, 
    bytes calldata quorumNumbers, 
    bytes32[] calldata nonSignerOperatorIds
)
    external 
    view 
    returns (CheckSignaturesIndices memory)

struct CheckSignaturesIndices {
    uint32[] nonSignerQuorumBitmapIndices;
    uint32[] quorumApkIndices;
    uint32[] totalStakeIndices;  
    uint32[][] nonSignerStakeIndices; // nonSignerStakeIndices[quorumNumberIndex][nonSignerIndex]
}
```

Traverses histories in the `RegistryCoordinator`, `IndexRegistry`, `StakeRegistry`, and `BLSApkRegistry` to retrieve information on one or more quorums' Operator sets and nonsigning Operators at a given `referenceBlockNumber`.

The return values are all "indices," because of the linear historical state each registry keeps. Offchain code calls this method to compute indices into historical state, which later is leveraged for cheap lookups in `BLSSignatureChecker.checkSignatures` (rather than traversing over the history during an onchain operation).

For each quorum, this returns:
* `uint32[] nonSignerQuorumBitmapIndices`: The indices in `RegistryCoordinator._operatorBitmapHistory` where each nonsigner's registered quorum bitmap can be found at `referenceBlockNumber`. Length is equal to the number of nonsigners included in `nonSignerOperatorIds`
* `uint32[] quorumApkIndices`: The indices in `BLSApkRegistry.apkHistory` where the quorum's apk can be found at `referenceBlockNumber`. Length is equal to the number of quorums in `quorumNumbers`.
* `uint32[] totalStakeIndices`: The indices in `StakeRegistry._totalStakeHistory` where each quorum's total stake can be found at `referenceBlockNumber`. Length is equal to the number of quorums in `quorumNumbers`.
* `uint32[][] nonSignerStakeIndices`: For each quorum, a list of the indices of each nonsigner's `StakeRegistry.operatorStakeHistory` entry at `referenceBlockNumber`. Length is equal to the number of quorums in `quorumNumbers`, and each sub-list is equal in length to the number of nonsigners in `nonSignerOperatorIds` registered for that quorum at `referenceBlockNumber`

---

### System Configuration

#### `BLSSignatureChecker.setStaleStakesForbidden`

```solidity
function setStaleStakesForbidden(
    bool value
) 
    external 
    onlyCoordinatorOwner
```

This method allows the `RegistryCoordinator` Owner to update `staleStakesForbidden` in the `BLSSignatureChecker`. If stale stakes are forbidden, `BLSSignatureChecker.checkSignatures` will perform an additional check when querying each quorum's apk, Operator stakes, and total stakes.

This additional check requires that each quorum was updated within a certain block window of the `referenceBlockNumber` passed into `BLSSignatureChecker.checkSignatures`.

*Effects*:
* Sets `staleStakesForbidden` to `value`

*Requirements*:
* Caller MUST be the `RegistryCoordinator` Owner
````

## File: docs/EjectionManager.md
````markdown
## EjectionManager

| File | Type | Proxy? |
| -------- | -------- | -------- |
| [`EjectionManager.sol`](../src/EjectionManager.sol) | Singleton | Transparent proxy |

The `EjectionManager` facilitates automated ejection of operators from the `SlashingRegistryCoordinator` under a configurable rate limit. It allows authorized ejectors to remove operators from quorums.

#### High-level Concepts

This document or:
* [Ejection Rate Limiting](#ejection-rate-limiting)
* [Ejector Management](#ejector-management)
* [Operator Ejection](#operator-ejection)

#### Roles

* Owner: a permissioned role that can configure quorum ejection parameters, manage ejectors, and eject operators without rate limiting
* Ejector: a permissioned role that can eject operators under the configured rate limits

---    

### Ejection Rate Limiting

The ejection rate limit system prevents too many operators from being ejected in a short time period, which could potentially destabilize the system. Rate limits are configured per quorum.

* [`setQuorumEjectionParams`](#setquorumejectparams)
* [`amountEjectableForQuorum`](#amountejectableforquorum)

#### `setQuorumEjectionParams`

```solidity
function setQuorumEjectionParams(
    uint8 quorumNumber,
    QuorumEjectionParams memory _quorumEjectionParams
) external onlyOwner
```

Allows the Owner to set the rate limit parameters for a specific quorum.

*Effects*:
* Updates the quorum's ejection parameters, including the rate limit window and the percentage of stake that can be ejected within that window

*Requirements*:
* Caller MUST be the Owner
* `quorumNumber` MUST be less than `MAX_QUORUM_COUNT`

#### `amountEjectableForQuorum`

```solidity
function amountEjectableForQuorum(
    uint8 quorumNumber
) public view returns (uint256)
```

Calculates the amount of stake that can currently be ejected from a quorum, based on the quorum's ejection parameters and recent ejection history.

*Return Value*:
* The amount of stake that can be ejected at the current `block.timestamp`

*Calculation Logic*:
1. Determines the total ejectable stake as a percentage of the quorum's total stake, using `quorumEjectionParams[quorumNumber].ejectableStakePercent`
2. Calculates the stake already ejected during the current rate limit window
3. Returns the difference between the total ejectable stake and the stake already ejected, or 0 if more stake has been ejected than the limit allows

---

### Ejector Management

These methods are used to manage which addresses have the ability to eject operators under the rate limits:

* [`setEjector`](#setejector)

#### `setEjector`

```solidity
function setEjector(address ejector, bool status) external onlyOwner
```

Allows the Owner to add or remove an address from the list of authorized ejectors.

*Effects*:
* Sets the address' ejector status to the provided value
* Emits an `EjectorUpdated` event

*Requirements*:
* Caller MUST be the Owner

---

### Operator Ejection

These methods allow ejection of operators from quorums:

* [`ejectOperators`](#ejectoperators)

#### `ejectOperators`

```solidity
function ejectOperators(
    bytes32[][] memory operatorIds
) external
```

Ejects operators from quorums, respecting the rate limits if called by an ejector (not the owner).

The method processes operators for each quorum sequentially. For each quorum, it attempts to eject operators in the order provided, stopping if the rate limit is reached. The owner can bypass rate limits.

*Effects*:
* For each quorum, ejects as many operators as possible prioritizing operators at lower indexes
* If called by an ejector (not the owner), records the stake ejected to keep track of rate limits
* Emits an `OperatorEjected` event for each ejected operator
* Emits a `QuorumEjection` event for each quorum with the number of ejected operators and whether the rate limit was hit

*Requirements*:
* Caller MUST be either an ejector or the owner

*Implementation Details*:
* If called by the owner, rate limits are not enforced, allowing emergency ejections
* If called by an ejector, operators are ejected until the rate limit is hit

---

### Initialization

The contract is initialized with the following parameters:

```solidity
function initialize(
    address _owner,
    address[] memory _ejectors,
    QuorumEjectionParams[] memory _quorumEjectionParams
) external initializer
```

*Effects*:
* Sets the contract owner
* Configures the initial set of ejectors
* Sets up the ejection parameters for quorums

*Requirements*:
* Can only be called once due to the `initializer` modifier

---
````

## File: docs/OperatorStateRetriever.md
````markdown
# OperatorStateRetriever 

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`OperatorStateRetriever.sol`](../src/OperatorStateRetriever.sol) | Singleton | None |

The `OperatorStateRetriever` contract provides view methods that compose view calls from the registry contracts to surface enriched state information. These methods are intended to be called offchain to prepare calldata for BLS signature validation through the `BLSSignatureChecker.checkSignatures` method.

The contract traverses historical state records in the registry contracts ([`IndexRegistry`](./registries/IndexRegistry.md), [`StakeRegistry`](./registries/StakeRegistry.md), and [`BLSApkRegistry`](./registries/BLSApkRegistry.md)) to retrieve information about operators and quorums at specific block numbers. This historical data is essential for validating signatures against a fixed point in time, as operators may register for or deregister from quorums after signing data.

#### High-level Concepts

This document organizes methods according to the following themes:
* [Offchain Methods](#offchain-methods)
* [Utility Functions](#utility-functions)

---

### Offchain Methods

These methods traverse various registry histories to retrieve information needed for BLS signature validation:
* [`getOperatorState (operatorId)`](#getoperatorstate-operatorid)
* [`getOperatorState (quorumNumbers)`](#getoperatorstate-quorumnumbers)
* [`getCheckSignaturesIndices`](#getchecksignaturesindices)

#### `getOperatorState (operatorId)`

```solidity
function getOperatorState(
    ISlashingRegistryCoordinator registryCoordinator,
    bytes32 operatorId,
    uint32 blockNumber
) external view returns (uint256, Operator[][] memory)

struct Operator {
    address operator;
    bytes32 operatorId;
    uint96 stake;
}
```

This method is designed for AVS operators to retrieve their state and responsibilities when a new task is created by the AVS coordinator. It returns information about an Operator and the quorums they were registered for at a specific block number.

The method performs the following operations:
1. Retrieves the quorum bitmap for the Operator at the specified block number
2. Converts the bitmap to an array of quorum numbers
3. For each quorum the Operator was registered for, retrieves the complete list of registered Operators in that quorum

This eliminates the need for operators to run indexers to fetch on-chain data.

*Returns*:
* `uint256`: A bitmap representation of the quorums the Operator was registered for at the given block number
* `Operator[][]`: For each quorum the Operator was registered for, an ordered list of all Operators in that quorum, including their addresses, IDs, and stakes

#### `getOperatorState (quorumNumbers)`

```solidity
function getOperatorState(
    ISlashingRegistryCoordinator registryCoordinator,
    bytes memory quorumNumbers,
    uint32 blockNumber
) public view returns (Operator[][] memory)
```

This method returns comprehensive information about all Operators registered for specified quorums at a given block number. It traverses multiple registry contracts to compile complete operator data.

The method performs the following operations for each quorum:
1. Retrieves the list of operator IDs from the `IndexRegistry`
2. Fetches each operator's address from the `BLSApkRegistry`
3. Obtains stake amounts from the `StakeRegistry`
4. Compiles this information into an ordered list of Operators for each quorum

*Returns*:
* `Operator[][]`: For each quorum in `quorumNumbers`, an ordered list of all Operators registered for that quorum at the specified block number, including their addresses, IDs, and stakes

#### `getCheckSignaturesIndices`

```solidity
function getCheckSignaturesIndices(
    ISlashingRegistryCoordinator registryCoordinator,
    uint32 referenceBlockNumber,
    bytes calldata quorumNumbers,
    bytes32[] calldata nonSignerOperatorIds
) external view returns (CheckSignaturesIndices memory)

struct CheckSignaturesIndices {
    uint32[] nonSignerQuorumBitmapIndices;
    uint32[] quorumApkIndices;
    uint32[] totalStakeIndices;
    uint32[][] nonSignerStakeIndices; // nonSignerStakeIndices[quorumNumberIndex][nonSignerIndex]
}
```

This method is critical for BLS signature validation, as it retrieves indices into historical state that can be used for efficient lookups in `BLSSignatureChecker.checkSignatures`. The non-signer operator IDs are required here as signature verification is done against negation of the BLS aggregate public key. That is, negate the aggregate key then add the weight of each signer. 

The method generates the following indices:
1. Indices of quorum bitmap updates for each non-signing operator
2. Indices of total stake updates for each quorum
3. For each quorum, indices of stake updates for non-signing operators registered for that quorum
4. Indices of aggregate public key (APK) updates for each quorum

By pre-computing these indices offchain, the `BLSSignatureChecker.checkSignatures` method can perform cheap lookups rather than traversing over historical state during an expensive onchain operation.

*Returns*:
* `CheckSignaturesIndices`: A struct containing all indices needed for signature validation:
* `nonSignerQuorumBitmapIndices`: For each non-signer, the index in `RegistryCoordinator._operatorBitmapHistory` where their quorum bitmap can be found
* `quorumApkIndices`: For each quorum, the index in `BLSApkRegistry.apkHistory` where the quorum's APK can be found
* `totalStakeIndices`: For each quorum, the index in `StakeRegistry._totalStakeHistory` where the quorum's total stake can be found
* `nonSignerStakeIndices`: For each quorum, indices in `StakeRegistry.operatorStakeHistory` for each non-signer registered for that quorum

*Requirements*:
* Non-signer operator IDs must be registered (have non-zero quorum bitmaps)

---

### Utility Functions

These methods provide additional utilities for retrieving historical state and operator information:
* [`getQuorumBitmapsAtBlockNumber`](#getquorumbitmapsatblocknumber)
* [`getBatchOperatorId`](#getbatchoperatorid)
* [`getBatchOperatorFromId`](#getbatchoperatorfromid)

#### `getQuorumBitmapsAtBlockNumber`

```solidity
function getQuorumBitmapsAtBlockNumber(
    ISlashingRegistryCoordinator registryCoordinator,
    bytes32[] memory operatorIds,
    uint32 blockNumber
) external view returns (uint256[] memory)
```

This method retrieves quorum bitmaps for multiple operators at a specific block number, providing an efficient way to determine which quorums each operator was registered for at that point in time.

*Returns*:
* `uint256[]`: An array of quorum bitmaps, one for each operator ID provided, representing the quorums each operator was registered for at the given block number

#### `getBatchOperatorId`

```solidity
function getBatchOperatorId(
    ISlashingRegistryCoordinator registryCoordinator,
    address[] memory operators
) external view returns (bytes32[] memory operatorIds)
```

This utility function converts multiple operator addresses to their corresponding operator IDs in a single call, improving gas efficiency for batch operations.

*Returns*:
* `bytes32[]`: An array of operator IDs corresponding to the provided addresses
* If an operator is not registered, its ID will be 0

#### `getBatchOperatorFromId`

```solidity
function getBatchOperatorFromId(
    ISlashingRegistryCoordinator registryCoordinator,
    bytes32[] memory operatorIds
) external view returns (address[] memory operators)
```

This utility function converts multiple operator IDs to their corresponding operator addresses in a single call, improving gas efficiency for batch operations.

*Returns*:
* `address[]`: An array of operator addresses corresponding to the provided IDs
* If an operator ID is not registered, its address will be 0x0

---
````

## File: docs/quick-start.md
````markdown
[middleware-guide-link]: #quick-start-guide-to-build-avs-contracts
[operator-set-guide-link]: https://www.blog.eigenlayer.xyz/introducing-the-eigenlayer-security-model
[uam-link]: https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-003.md
[avs-sync-link]: https://github.com/Layr-Labs/avs-sync
[uam-elip-link]: https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-003.md
# Purpose
This document aims to describe and summarize how autonomous verifiable services (AVSs) building on EigenLayer interact with the core EigenLayer protocol. Currently, this doc explains how AVS developers can use the APIs for:
- enabling operators to opt-in to the AVS,
- enabling operators to opt-out (withdraw stake) from the AVS,
- enabling operators to continuously update their commitments to middlewares,
- enabling AVSs to create operator sets,
- enabling AVSs to submit rewards for operators and stakers, and
- enabling AVSs to slash operators for malicious behaviour.

# Introduction
In designing EigenLayer, the EigenLabs team aspired to make minimal assumptions about the structure of AVSs built on top of it. This repo contains code that can be extended, used directly, or consulted as a reference in building an AVS on top of EigenLayer. All the middleware contracts are AVS-specific and need to be deployed separately by AVS teams which interface with the EigenLayer core contracts, deployed by EigenLabs.

## Important Terminology
- **Tasks** - A task in EigenLayer is the smallest unit of work that operators commit to perform when serving an AVS. These tasks may be associated with one or more operator sets within the AVS.
- **Strategies** - A strategy in EigenLayer is a contract that holds staker deposits, i.e. it controls one or more asset(s) that can be restaked. EigenLayer's strategy design is flexible and open, meaning anyone can permissionlessly deploy a strategy using the `StrategyManager` in the core contracts.
- **Operator Sets** - An operator set in EigenLayer is a way to distinguish different classes of operators within an AVS. Operators may be separated on the basis of the tasks they perform, supported strategies, rewards they recieve or, the slashing conditions operators are subject to. There are two types of operator sets that an AVS may create, total delegated stake and unique stake. See [this][operator-set-guide-link] blog post for further information on the different types of operator sets.

# Key Design Considerations
1. *Decomposition into "Tasks"*: <br/>
    EigenLayer assumes that an AVS manages tasks that are executed over time by a registered operator. Each task is associated with the time period during which the AVS's operators' stakes are placed "at stake", i.e. potentially subject to slashing. Examples of tasks could be:
    - Hosting and serving a “DataStore” in the context of EigenDA
    - Posting a state root of another blockchain for a bridge service

2. *Stake is "At Stake" on Tasks for a Finite Duration*: <br/>
    It is assumed that every task (eventually) resolves. Each operator places their stake in EigenLayer “at stake” on the tasks that they perform. In order to “release” the stake (e.g. so the operator can withdraw their funds), these tasks need to eventually resolve. It is RECOMMENDED, but not required that a predefined duration is specified in the AVS contract for each task. As a guideline, the EigenLabs team believes that the duration of a task should be aligned with the longest reasonable duration that would be acceptable for an operator to keep funds “at stake”. An AVS builder should recognize that extending the duration of a task may impose significant negative externalities on the stakers of EigenLayer, and may disincentivize operators from opting-in to serving their application (so that they can attract more delegated stake). At the time of writing, the `DEALLOCATION_DELAY` which is the time it takes for stake to be deallocated from an AVS is 14 days.

3. *Services Slash Only Objectively Attributable Behavior*: <br/>
    EigenLayer is built to support slashing as a result of an on-chain checkable, objectively attributable action. An AVS SHOULD slash in EigenLayer only for such provable and attributable behavior. It is expected that operators will be very hesitant to opt-in to services that slash for other types of behavior, and other services may even choose to exclude operators who have opted-in to serving one or more AVSs with such “subjective slashing conditions”, as these slashing conditions present a significant challenge for risk modeling, and may be perceived as more dangerous in general. Some examples of on-chain checkable, objectively attributable behavior: 
    - double-signing a block in Ethereum, but NOT inactivity leak; 
    - proofs-of-custody in EigenDA, but NOT a node ceasing to serve data; 
    - a node in a light-node-bridge AVS signing an invalid block from another chain.

    Intersubjective slashing will be supported in the future through the Eigen token.

4. *Contract Interactions for Services and EigenLayer*: <br/>
    It is assumed that services have a two contracts that coordinate the service’s communications sent to EigenLayer. The first contract – referred to as the `ServiceManager` – informs EigenLayer when an operator should be slashed, evicted or jailed, when rewards are submitted, setting AVS metadata and, managing UAM (refer to [here][uam-link] for further information on UAM). The second contract is the `SlashingRegistryCoordinator` which calls into EigenLayer to create operator sets and, force ejection of operators. AVSs can choose deviate from this pattern, but this will result in a worse developer and operator experience and is not recommended.

## Integration with EigenLayer Contracts:
In this section, we will explain various API interfaces that EigenLayer provides which are essential for AVSs to integrate with EigenLayer. 

### *Operator Registration into EigenLayer Protocol*
Prior to operators registering into an AVS they must register into the EigenLayer protocol and likely accumulate some stake (AVSs may chose to not set a minimum stake). To do this operators must interact with the `DelegationManager` calling `registerAsOperator(..)` supplying the address which acts as the delegation approver (the address which can approve stakers to delegate to an operator. If set to the zero address, any staker can delegate to an operator), an allocation delay (the time it takes for their stake to become active), and a metadata URI. Once registered, stakers can then delegate their stake to the operator by calling `delegateTo(..)` on the `DelegationManager`.

### *Operator Registration into AVS*
An operator opts into an AVS by allocating stake to an AVS then register for the operator set. The flow is as follows:
1. The operator calls `modifyAllocations(..)`, supplying the operator set of the AVS, the strategies and the magnitudes to allocate. After the transaction is successful, the allocation delay is triggered, which is the time it takes for the stake to become active or slashable. Operators configure this value and can be set to 0.
2. After the allocation delay has elapsed, the operator can then register for the operator set they allocated to, by calling `registerForOperatorSets(..)`, supplying the address of the `SlashingRegistryCoordinator` which implements the `IAVSRegistrar` interface, operator set IDs and, any extra data needed for registration.

The following figure illustrates the above flow:

<p align="center">
  <img src="./images/operator_registration.png" alt="operator registration" width="500">
</p>

### *Operator Deregistration from AVS*
Operators deregister through the `AllocationManager` by calling `deregisterFromOperatorSets(..)` supplying the operator set IDs to deregister from and the AVS' `SlashingRegistryCoordinatorAddress`. Note that the stake will be slashable until the `DEALLOCATION_DELAY` passes, which is set within the protocol to be 14 days. After 14 days, the operator's status will be updated to `DEREGISTERED`

<p align="center">
  <img src="./images/operator_deregistration.png" alt="operator deregistration" width="400" height="800">
</p>

### *Stake Updates*
EigenLayer has a lazy stake state model, meaning when operators allocate or deallocate, stakers deposit or withdraw and when an operator is slashed, the updates are not automatically propagated into the `StakeRegistry` middleware contract. These updates must be pushed manually via calling `updateOperatorsForQuorum(..)` and passing in the operator addresses to update and the operator set IDs. These state updates are essential for the function of the AVS, for example, the amount of allocated stake must be known before distributing rewards to operators. EigenLabs provides a service to abstract this process called [AVS-Sync][avs-sync-link]. This proccess can be expensive and is important that AVSs consider this in their rewards schedule.

### *User Access Management*
User Access Management (UAM) is a protocol-level feature in EigenLayer for enhanced key management and modular smart contract design. UAM is intended to simplify the AVS architecture by splitting out responsibilities of the `ServiceManager`. The following table details the recommended UAM access patterns for AVS contracts and accounts. The `ServiceManager` should expose an interface with appropriate access control to call into the `PermissionsController` contract to manage UAM. Refer to the UAM [ELIP][uam-elip-link] for further details.
| Contract / Account                              | Responsibilities                                                                                        |
|-------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| `InstantSlasher` / `VetoableSlasher`            | Handles slashing by calling the `AllocationManager`                                                     |
| `SlashingRegistryCoordinator`                   | Handles operator set creation and forced operator deregistration by calling into the `AllocationManager`|
| AVS-controlled account (e.g., EOA or multisig)  | Sets AVS registrar and AVS metadata URI on the `AllocationManager`                                      |
| `StakeRegistry`                                 | Adds and removes strategies for operator sets on the `AllocationManager`                                |

### *Slashing*
EigenLayer middleware provides two smart contracts for slashing, the `InstantSlasher` and the `VetoableSlasher` where the former enacts slashing requests without any delay or veto period and the former has a delay and veto period. It is recommended that AVSs use the `VetoableSlasher` with a security council comprised of a diverse set of SMEs. These are good training wheels as your AVS matures, where faults may occur from non-malicious sources like a software bug. Queuing slashing requests is configurable (e.g. permissionlessly). It is recommended to expose an external or public function on the `ServiceManager` that will call to the slashing contract.

## Quick Start Guide to Build AVS Contracts:
The EigenLayer team has built this repo as a set of reusable and extensible contracts for use in AVSs built on top of EigenLayer, which comprises code that can be extended, used directly, or consulted as a reference in building AVSs on EigenLayer. There are several basic contracts that all AVS-specific contracts can be built on:
- The *SlashingRegistryCoordinator* handles the creation of operator sets and forced deregistration of operators. It also has callbacks that the `AllocationManager` calls to register and deregister operators.
- The *StakeRegistry* tracks an operator’s “weight” in a given operator set, across all strategies for operator sets, stake updates and history and management of which strategies are in each operator set.
- The *IndexRegistry* maintains an ordered list of operators within an operator set, surfacing operator updates, operator set updates and historical updates.
- The *BLSApkRegistry* tracks the aggregate BLS key for operator sets with registration/deregistration and allocation/deallocation updates. This registry also provides mappings between EVM addresses and BLS identities.
- The *SocketRegistry* surfaces optional operator metadata that is set upon operator registration, e.g. operators could set their HTTP URL within the socket data.
- The *OperatorStateRetriever* surfaces enriched operator state by composing calls to the registry contracts detailed above. This includes retrieving an operator's state across all sets (e.g. their relative stake in the set), the list of all operators for a set and their stake and the retrieval of signature indices needed for the `checkSignatures` functions on the BLS and ECDSA signature verification contracts.

Furthermore, it’s expected that many AVSs will require a quorum of registered operators to sign on commitments. There are two contracts to support this functionality; the `BLSSignatureChecker` and the `ECDSAStakeRegistry` (currently unaudited) supporting the two different signature schemes.
````

## File: docs/README.md
````markdown
<!-- 
Reference Links:
 -->
[core-contracts-repo]: https://github.com/Layr-Labs/eigenlayer-contracts
[core-docs-dev]: https://github.com/Layr-Labs/eigenlayer-contracts/tree/dev/docs
[eigenda-repo]: https://github.com/Layr-Labs/eigenda/
[bitmaputils-lib]: ../src/libraries/BitmapUtils.sol

[core-registerToAVS]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/AVSDirectory.md#registeroperatortoavs
[core-deregisterFromAVS]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/AVSDirectory.md#deregisteroperatorfromavs

## EigenLayer Middleware Docs

EigenLayer AVSs ("autonomous verifiable services") are protocols that make use of EigenLayer's restaking primitives. AVSs are validated by EigenLayer Operators, who are backed by restaked assets via the [EigenLayer core contracts][core-contracts-repo]. Each AVS will deploy or modify instances of the contracts in this repo to hook into the EigenLayer core contracts and ensure their service has an up-to-date view of its currently-registered Operators. Refer to the [quick start](./quick-start.md) guide for how to integrate with the core contracts.

The middleware contracts at a high level interface with the core contracts to provide:
* Operator set or quorum creation and management (e.g. updating the supported strategies or the minimum stake requirements)
* Operator set or quorum registration and deregistration
* Reward submissions
* Punitive actions such as slashing or jailing/eviction
* AVS identity and metadata management


### Contents

* [Important Concepts](#important-concepts)
    * [Quorums](#quorums)
    * [Strategies and Stake](#strategies-and-stake)
    * [Operator Sets and Churn](#operator-sets-and-churn)
    * [State Histories](#state-histories)
    * [Hooking Into EigenLayer Core](#hooking-into-eigenlayer-core)
* [System Components](#system-components)
    * [Service Manager](#service-manager)
    * [Registries](#registries)
    * [BLSSignatureChecker](#blssignaturechecker)

---

### Important Concepts

##### Quorums

A quorum is a grouping and configuration of specific kinds of stake that an AVS considers (either delegated or slashable) when interacting with Operators. When Operators register for an AVS, they select one or more quorums within the AVS to register for. Depending on its configuration, each quorum evaluates a specific subset of the Operator's restaked tokens and uses this to determine a specific weight for the Operator for that quorum. This weight is ultimately used to determine when an AVS has reached consensus.

The purpose of having a quorum is that an AVS can customize the makeup of its security offering by choosing which kinds of stake/security model it would like to utilize.

As an example, an AVS might want to support primarily native ETH stakers. It would do so by configuring a quorum to only weigh Operators that control shares belonging to the native eth strategy (defined in the core contracts).

The Owner initializes quorums in the `RegistryCoordinator` or `SlashingRegistryCoordinator` (for AVSs building post slashing release), and may configure them further in both either of the registry coordinators and `StakeRegistry` contracts. When quorums are initialized, they are assigned a unique, sequential quorum number.

*Note:* For the most part, quorum numbers are passed between contracts as `bytes` arrays containing an ordered list of quorum numbers. However, when storing lists of quorums in state (and for other operations), these `bytes` arrays are converted to bitmaps using the [`BitmapUtils` library][bitmaputils-lib].

##### Strategies and Stake

Each quorum has an associated list of `StrategyParams`, which the Owner can configure via the `StakeRegistry`.

`StrategyParams` define pairs of strategies and multipliers for the quorum:
* Strategies refer to the `DelegationManager` in the EigenLayer core contracts, which tracks shares delegated to each Operator for each supported strategy. Basically, a strategy is a wrapper around an underlying token - either an LST or Native ETH.
* Multipliers determine the relative weight given to shares belonging to the corresponding strategy.

When the `StakeRegistry` updates its view of an Operator's stake for a given quorum, it queries the `DelegationManager` to get the Operator's shares in each of the quorum's strategies and applies the multiplier to the returned share count.

For more information on the `DelegationManager`, see the [EigenLayer core docs][core-docs-dev].

##### Operator Sets and Churn

Quorums define a maximum Operator count as well as parameters that determine when a new Operator can replace an existing Operator when this max count is reached. The process of replacing an existing Operator when the max count is reached is called "churn," and requires a signature from the Churn Approver.

These definitions are contained in a quorum's `OperatorSetParam`, which the Owner can configure via the `RegistryCoordinator` or `SlashingRegistryCoordinator`. A quorum's `OperatorSetParam` defines both a max Operator count, as well as stake thresholds that the incoming and existing Operators need to meet to qualify for churn.

*Additional context*:

Currently for EigenDA, the max Operator count is 200. This maximum exists because EigenDA requires that completed "jobs" validate a signature by the aggregate BLS pubkey of the Operator set over some job parameters. Although an aggregate BLS pubkey's signature should have a fixed cost no matter the number of Operators, it may be the case that not all Operators sign off on a job.

When this happens, EigenDA needs to provide a list of the pubkeys of the non-signers to subtract them out from the quorum's aggregate pubkey ("Apk"). The limit of 200 Operators keeps the gas costs reasonable in a worst case scenario. See `BLSSignatureChecker.checkSignatures` for this part of the implementation.

In order to prevent the Operator set from getting calcified, the churn mechanism was introduced to allow Operators to be replaced in some cases. Future work is being done to increase the max Operator count and refine the churn mechanism.

##### State Histories

Many of the contracts in this repo keep histories of states over time. Generally, you'll see structs that look like this:

```solidity
struct ValueUpdate {
    Value value;
    uint32 updateBlockNumber;     // when the value started being valid
    uint32 nextUpdateBlockNumber; // when the value stopped being valid (or 0, if the value is still valid)
}
```

These histories are used by offchain code to query state at particular blocks, and are ultimately used to validate specific actions on-chain. See the [`BLSSignatureChecker` section](#blssignaturechecker) below.

##### Hooking Into EigenLayer Core

AVSs interface with the EigenLayer protocol through two main contracts, the `AVSDirectory` and the `AllocationManager`. Interactions with the `AVSDirectory` is done through the `ServiceManagerBase` when operators register and deregister from the AVS. Interactions with the `AllocationManager` are done through either the `RegistryCoordinator` or `SlashingRegistryCoordinator` to manage quorum creation and forced ejection of operators as well as slashing contracts (`InstantSlasher` or `VetoableSlasher`) to slash operators.

### System Components

#### Service Manager

| Code | Type | Proxy |
| -------- | -------- | -------- |
| [`ServiceManagerBase.sol`](../src/ServiceManagerBase.sol) | Singleton | Transparent proxy |

The Service Manager contract serves as the AVS's address relative to EigenLayer core contracts. See full documentation in [`ServiceManagerBase.md`](./ServiceManagerBase.md).

#### Registries

| Code | Type | Proxy |
| -------- | -------- | -------- |
| [`RegistryCoordinator.sol`](../src/RegistryCoordinator.sol) | Singleton | Transparent proxy |
| [`BLSApkRegistry.sol`](../src/BLSApkRegistry.sol) | Singleton | Transparent proxy |
| [`StakeRegistry.sol`](../src/StakeRegistry.sol) | Singleton | Transparent proxy |
| [`IndexRegistry.sol`](../src/IndexRegistry.sol) | Singleton | Transparent proxy |

The `RegistryCoordinator` and `SlashingRegistryCoordiator` keeps track of which quorums exist and have been initialized. It is also the primary entry point for Operators as they register for and deregister from an AVS's quorums.

When Operators register or deregister, the registry coordinator updates that Operator's currently-registered quorums, and pushes the registration/deregistration to each of the three registries it controls:
* `BLSApkRegistry`: tracks the aggregate BLS pubkey hash for the Operators registered to each quorum. Also maintains a history of these aggregate pubkey hashes.
* `StakeRegistry`: interfaces with the EigenLayer core contracts to determine the weight of Operators according to their stake and each quorum's configuration. Also maintains a history of these weights.
* `IndexRegistry`: assigns indices to Operators within each quorum, and tracks historical indices and Operators per quorum. Used primarily by offchain infrastructure to fetch ordered lists of Operators in quorums.

Both the registry coordinator and each of the registries maintain historical state for the specific information they track. This historical state tracking can be used to query state at a particular block, which is primarily used in offchain infrastructure.

See full documentation for the registry coordinators in [`RegistryCoordinator.md`](./RegistryCoordinator.md)/[`SlashingRegistryCoordinator`](./SlashingRegistryCoordinator.md), and for each registry in [`registries/`](./registries/).

#### BLSSignatureChecker

| Code | Type | Proxy |
| -------- | -------- | -------- |
| [`BLSSignatureChecker.sol`](../src/BLSSignatureChecker.sol) | Singleton | Transparent proxy |
| [`OperatorStateRetriever.sol`](../src/OperatorStateRetriever.sol) | Singleton | Transparent proxy |

The BLSSignatureChecker verifies signatures made by the aggregate pubkeys ("Apk") of Operators in one or more quorums. The primary function, `checkSignatures`, is called by an AVS when confirming that a given message hash is signed by Operators belonging to one or more quorums.

The `OperatorStateRetriever` is used by offchain code to query the `RegistryCoordinator` (and its registries) for information that will ultimately be passed into `BLSSignatureChecker.checkSignatures`.

See full documentation for both of these contracts in [`BLSSignatureChecker.md`](./BLSSignatureChecker.md).
````

## File: docs/RegistryCoordinator.md
````markdown
[core-dmgr-docs]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/DelegationManager.md
[core-dmgr-register]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/DelegationManager.md#registeroperatortoavs
[core-dmgr-deregister]: https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/DelegationManager.md#deregisteroperatorfromavs

## RegistryCoordinator

| File | Type | Proxy? |
| -------- | -------- | -------- |
| [`RegistryCoordinator.sol`](../src/RegistryCoordinator.sol) | Singleton | Transparent proxy |

The `RegistryCoordinator` is a contract that exisiting AVSs using M2 quorums who wish to enable operator sets should upgrade to. New AVSs should deploy the `SlashingRegistryCoordinator` to use operator sets. The `RegistryCoordinator` inherits the `SlashingRegistryCoordinator` to expose the operator set functionality.

The `RegistryCoordinator` has four primary functions:
1. It is the primary entry and exit point for operators as they register for and deregister from quorums, and manages registration and deregistration in the `BLSApkRegistry`, `StakeRegistry`, and `IndexRegistry`. It also hooks into the EigenLayer core contracts, updating the core `AVSDirectory` for M2 quorums and `AllocationManager` in the case of ejection and operator set quorums
2. It allows anyone to update the current stake of any registered operator
3. It allows the Owner to initialize and configure new quorums
4. Disabling M2 quorum registration to upgrade to operator sets

Refer to the [`SlashingRegistryCoordinator`](./SlashingRegistryCoordinator) for operator set functionality.

#### Migration
Existing AVSs upgrading to this version `RegistryCoordinator` will be in a state where both M2 quorum registration and operator set registration will be enabled. AVSs must be aware of this. Operator sets will be enabled on upon the first call to either `createDelegatedStakeQuorum` or `createSlashableStakeQuorum` which are inherited from the `SlashingRegistryCoordinator`. The suggested flow for this migration is as follows:
1. Upgrade `RegistryCoordinator`
2. Create delegated or slashable stake quorums via `createDelegatedStakeQuorum` or `createSlashableStakeQuorum`
3. Allow time for operators to register for the new quorums using the `AllocationManager`
4. After adequate stake has been migrated (e.g. for the sake of securing new tasks of operator sets), disable M2 registration by calling `disableM2QuorumRegistration`, note that operators can still deregister from the legacy quorums

#### High-level Concepts

This document organizes methods according to the following themes (click each to be taken to the relevant section):
* [Registering and Deregistering](#registering-and-deregistering)
* [Updating Registered Operators](#updating-registered-operators)
* [System Configuration](#system-configuration)

#### Roles

* Owner: a permissioned role that can create and configure quorums as well as manage other roles
* Ejector: a permissioned role that can forcibly eject an operator from a quorum via `RegistryCoordinator.ejectOperator`
* Churn Approver: a permissioned role that signs off on operator churn in `RegistryCoordinator.registerOperatorWithChurn`

---    

### Registering and Deregistering

These methods allow operators to register for/deregister from one or more quorums, and are the primary entry points to the middleware contracts as a whole:
* [`registerOperator`](#registeroperator)
* [`registerOperatorWithChurn`](#registeroperatorwithchurn)
* [`deregisterOperator`](#deregisteroperator)
* [`ejectOperator`](#ejectoperator)

#### `registerOperator`

```solidity
function registerOperator(
    bytes calldata quorumNumbers,
    string calldata socket,
    IBLSApkRegistryTypes.PubkeyRegistrationParams calldata params,
    SignatureWithSaltAndExpiry memory operatorSignature
) 
    external 
    onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR)
```

Registers the caller as an Operator for one or more quorums, as long as registration doesn't place any quorum's operator count above the configured cap. This method updates the Operator's current and historical bitmap of registered quorums, and forwards a call to each of the registry contracts:
* `BLSApkRegistry.registerOperator`
* `StakeRegistry.registerOperator`
* `IndexRegistry.registerOperator`

If the Operator has never registered for any of this AVS's quorums before, they need to register a BLS public key to participate in AVS signing events. In this case, this method will automatically pass `params` to the `BLSApkRegistry` to perform public key registration. The registered pubkey hash becomes the Operator's unique operator id, used to identify them in many places in the middleware contracts.

If the Operator was not currently registered for any quorums, this method will register the Operator to the AVS in the EigenLayer core contracts via the `ServiceManagerBase`.

*Effects*:
* If the Operator has never registered for the AVS before:
    * Registers their BLS pubkey in the `BLSApkRegistry` (see [`BLSApkRegistry.registerBLSPublicKey`](./registries/BLSApkRegistry.md#registerblspublickey))
* If the Operator was not currently registered for any quorums: 
    * Updates their status to `REGISTERED`
    * Registers them in the core contracts (see [`ServiceManagerBase.registerOperatorToAVS`](./ServiceManagerBase.md#registeroperatortoavs))
* Adds the new quorums to the Operator's current registered quorums, and updates the Operator's bitmap history
* See [`BLSApkRegistry.registerOperator`](./registries/BLSApkRegistry.md#registeroperator)
* See [`StakeRegistry.registerOperator`](./registries/StakeRegistry.md#registeroperator)
* See [`IndexRegistry.registerOperator`](./registries/IndexRegistry.md#registeroperator)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_REGISTER_OPERATOR`
* Caller MUST have a valid operator ID in the `BLSApkRegistry`
* `quorumNumbers` MUST be an ordered array of quorum numbers, with no entry exceeding the current `quorumCount`
* `quorumNumbers` MUST contain at least one valid quorum
* `quorumNumbers` MUST NOT contain any quorums the Operator is already registered for
* If the Operator was not currently registered for any quorums:
    * See [`ServiceManagerBase.registerOperatorToAVS`](./ServiceManagerBase.md#registeroperatortoavs)
* See [`BLSApkRegistry.registerOperator`](./registries/BLSApkRegistry.md#registeroperator)
* See [`StakeRegistry.registerOperator`](./registries/StakeRegistry.md#registeroperator)
* See [`IndexRegistry.registerOperator`](./registries/IndexRegistry.md#registeroperator)
* For each quorum being registered for, the Operator's addition MUST NOT put the total number of operators registered for the quorum above the quorum's configured `maxOperatorCount`

#### `registerOperatorWithChurn`

```solidity
function registerOperatorWithChurn(
    bytes calldata quorumNumbers, 
    string calldata socket,
    IBLSApkRegistryTypes.PubkeyRegistrationParams calldata params,
    OperatorKickParam[] calldata operatorKickParams,
    SignatureWithSaltAndExpiry memory churnApproverSignature,
    SignatureWithSaltAndExpiry memory operatorSignature
) 
    external 
    onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR)
```

This method performs similar steps to `registerOperator` above, except that for each quorum where the new Operator total exceeds the `maxOperatorCount`, the `operatorKickParams` are used to deregister a current Operator to make room for the new one.

This operation requires a valid signature from the Churn Approver. Additionally, the incoming and outgoing Operators must meet these requirements:
* The new Operator's stake must be greater than the old Operator's stake by a factor given by the quorum's configured `kickBIPsOfOperatorStake`
* The old Operator's stake must be lower than the total stake for the quorum by a factor given by the quorum's configured `kickBIPsOfTotalStake`

*Effects*:
* The new Operator is registered for one or more quorums (see `registerOperator` above)
* For any quorum where registration causes the operator count to exceed `maxOperatorCount`, the Operator selected to be replaced is deregistered (see `deregisterOperator` below)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_REGISTER_OPERATOR`
* The `churnApproverSignature` MUST be a valid, unexpired signature from the Churn Approver over the Operator's id and the `operatorKickParams`
* The old and new Operators MUST meet the stake requirements described above
* See `registerOperator` above
* See `deregisterOperator` below

#### `deregisterOperator`

```solidity
function deregisterOperator(
    bytes calldata quorumNumbers
) 
    external 
    onlyWhenNotPaused(PAUSED_DEREGISTER_OPERATOR)
```

Allows an Operator to deregister themselves from one or more quorums.

*Effects*:
* If the Operator is no longer registered for any quorums:
    * Updates their status to `DEREGISTERED`
    * Deregisters them in the core contracts (see [`ServiceManagerBase.deregisterOperatorFromAVS`](./ServiceManagerBase.md#deregisteroperatorfromavs))
* Removes the new quorums from the Operator's current registered quorums, and updates the Operator's bitmap history
* See [`BLSApkRegistry.deregisterOperator`](./registries/BLSApkRegistry.md#deregisteroperator)
* See [`StakeRegistry.deregisterOperator`](./registries/StakeRegistry.md#deregisteroperator)
* See [`IndexRegistry.deregisterOperator`](./registries/IndexRegistry.md#deregisteroperator)

*Requirements*:
* Pause status MUST NOT be set: `PAUSED_DEREGISTER_OPERATOR`
* The Operator MUST currently be in the `REGISTERED` status (i.e. registered for at least one quorum)
* `quorumNumbers` MUST be an ordered array of quorum numbers, with no entry exceeding the current `quorumCount`
* `quorumNumbers` MUST contain at least one valid quorum
* `quorumNumbers` MUST ONLY contain bits that are also set in the Operator's current registered quorum bitmap
* See [`ServiceManagerBase.deregisterOperatorFromAVS`](./ServiceManagerBase.md#deregisteroperatorfromavs)
* See [`BLSApkRegistry.deregisterOperator`](./registries/BLSApkRegistry.md#deregisteroperator)
* See [`StakeRegistry.deregisterOperator`](./registries/StakeRegistry.md#deregisteroperator)
* See [`IndexRegistry.deregisterOperator`](./registries/IndexRegistry.md#deregisteroperator)

### System Configuration

These methods are used by the Owner to configure the `RegistryCoordinator`:

#### `disableM2QuorumRegistration`

```solidity
function disableM2QuorumRegistration() external onlyOwner
```

Allows the Owner to disable M2 quorum registration. This disables the M2 legacy quorum registration. Operators can still deregister once M2 registration is disabled. Note that this is a one way function, meaning once M2 is disabled it cannot be re-enabled.

*Effects*
* Disables M2 quourum registration

*Requirements*
* Caller MUST be the Owner
* M2 registration must be enabled
````

## File: docs/ServiceManagerBase.md
````markdown
## ServiceManagerBase

| File | Type | Proxy |
| -------- | -------- | -------- |
| [`ServiceManagerBase.sol`](../src/ServiceManagerBase.sol) | Singleton | Transparent proxy |

Libraries and Mixins:

| File | Notes |
| -------- | -------- |
| [`BitmapUtils.sol`](../src/libraries/BitmapUtils.sol) | bitmap manipulation |
| [`LibMergeSort.sol`](../src/libraries/LibMergeSort.sol) | sorting utilities |

## Prior Reading

* [ELIP-002: Slashing via Unique Stake and Operator Sets](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)
* [ELIP-003: User Access Management (UAM)](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-003.md)

## Overview

The `ServiceManagerBase` contract is an abstract contract that serves as a minimal implementation for a `ServiceManager` contract that AVSs will deploy. This document will view this contract through the lens of an implementation of the `ServiceManagerBase`. AVSs are encouraged to extend this contract to meet their own functionality, such as implementing allowlisting for operator sets.

The `ServiceManager` is the AVS's identity within EigenLayer and is responsible for:

* Manages callbacks from the `SlashingRegsitryCoordinator` for operator registration for the AVS and operator sets. Calls will be forwarded to the `AVSDirectory`
* Handling rewards submissions to EigenLayer's `RewardsCoordinator`
* Managing access permissions via the `PermissionController`

## Concepts

* [User Access Management](#user-access-management)
* [Operator Registration](#operator-registration)
* [Rewards Management](#rewards-management)
* [Operator Sets](#operator-sets)

## User Access Management

The `ServiceManagerBase` implements User Access Management (UAM) as defined in [ELIP-003](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-003.md), allowing fine-grained control over which addresses can perform various actions on behalf of the AVS. UAM functions are primarily used by the contract owner to delegate permissions. For further information on the suggested UAM patterns, refer to the AVS [quick start](./quick-start.md) guide.

**Methods:**
* [`addPendingAdmin`](#addpendingadmin)
* [`removePendingAdmin`](#removependingadmin)
* [`removeAdmin`](#removeadmin)
* [`setAppointee`](#setappointee)
* [`removeAppointee`](#removeappointee)

#### `addPendingAdmin`

```solidity
function addPendingAdmin(
    address admin
) external onlyOwner
```

This function allows the contract owner to add a pending admin for the AVS. The new admin must accept adminhood via the `PermissionController` contract to become active.

*Effects:*
* Calls `addPendingAdmin` on the `PermissionController` contract to set `admin` as a pending admin for this AVS

*Requirements:*
* Caller MUST be the owner of the contract

#### `removePendingAdmin`

```solidity
function removePendingAdmin(
    address pendingAdmin
) external onlyOwner
```

This function allows the contract owner to remove an address from the list of pending admins.

*Effects:*
* Calls `removePendingAdmin` on the `PermissionController` contract to remove `pendingAdmin` from the list of pending admins

*Requirements:*
* Caller MUST be the owner of the contract

#### `removeAdmin`

```solidity
function removeAdmin(
    address admin
) external onlyOwner
```

This function allows the contract owner to remove an admin from the AVS.

*Effects:*
* Calls `removeAdmin` on the `PermissionController` contract to remove `admin` from the list of admins

*Requirements:*
* Caller MUST be the owner of the contract
* There MUST be at least one admin remaining after removal

#### `setAppointee`

```solidity
function setAppointee(
    address appointee,
    address target,`
    bytes4 selector
) external onlyOwner
```

This function allows the contract owner to delegate specific function permissions to an appointee.

*Effects:*
* Calls `setAppointee` on the `PermissionController` contract to grant `appointee` permission to call the function identified by `target` and `selector`

*Requirements:*
* Caller MUST be the owner of the contract

#### `removeAppointee`

```solidity
function removeAppointee(
    address appointee,
    address target,
    bytes4 selector
) external onlyOwner
```

This function allows the contract owner to revoke delegated permissions from an appointee.

*Effects:*
* Calls `removeAppointee` on the `PermissionController` contract to revoke `appointee`'s permission to call the function identified by `target` and `selector`

*Requirements:*
* Caller MUST be the owner of the contract

## Operator Registration

The `ServiceManagerBase` propagates state updates to the `AVSDirectory` (for backward compatibility).

**Methods:**
* [`registerOperatorToAVS`](#registeroperatortoavs)
* [`deregisterOperatorFromAVS`](#deregisteroperatorfromavs)
* [`deregisterOperatorFromOperatorSets`](#deregisteroperatorfromoperatorsets)

#### `registerOperatorToAVS`

```solidity
function registerOperatorToAVS(
    address operator,
    ISignatureUtils.SignatureWithSaltAndExpiry memory operatorSignature
)
    public
    virtual 
    onlyRegistryCoordinator
```

This function is called by the `SlashingRegistryCoordinator` when an operator registers for the AVS. It forwards the call to the EigenLayer core `AVSDirectory`.

*Effects:*
* Forwards the call to `AVSDirectory.registerOperatorToAVS` with the operator's address and signature

*Requirements:*
* Caller MUST be the `SlashingRegistryCoordinator`

#### `deregisterOperatorFromAVS`

```solidity
function deregisterOperatorFromAVS(
    address operator
) 
    public 
    virtual 
    onlyRegistryCoordinator
```

This function is called by the `SlashingRegistryCoordinator` when an operator deregisters from the AVS. It forwards the call to the EigenLayer core `AVSDirectory` contract to maintain backward compatibility.

*Effects:*
* Forwards the call to `AVSDirectory.deregisterOperatorFromAVS` with the operator's address

*Requirements:*
* Caller MUST be the `SlashingRegistryCoordinator`

#### `deregisterOperatorFromOperatorSets`

```solidity
function deregisterOperatorFromOperatorSets(
    address operator,
    uint32[] memory operatorSetIds
)
    public
    virtual
    onlyRegistryCoordinator
```

This function is called by the `SlashingRegistryCoordinator` to deregister an operator from specific operator.

*Effects:*
* Creates a `DeregisterParams` struct with the operator's address, the AVS address, and the operator set IDs
* Calls `AllocationManager.deregisterFromOperatorSets` with the constructed parameters

*Requirements:*
* Caller MUST be the `SlashingRegistryCoordinator`

## Rewards Management

The `ServiceManagerBase` allows the AVS to submit rewards to EigenLayer's `RewardsCoordinator` contract.

**Methods:**
* [`createAVSRewardsSubmission`](#createavsrewardssubmission)
* [`createOperatorDirectedAVSRewardsSubmission`](#createoperatordirectedavsrewardssubmission)
* [`setClaimerFor`](#setclaimerfor)
* [`setRewardsInitiator`](#setrewardsinitiator)

#### `createAVSRewardsSubmission`

```solidity
function createAVSRewardsSubmission(
    IRewardsCoordinator.RewardsSubmission[] calldata rewardsSubmissions
)
    public
    virtual
    onlyRewardsInitiator
```

This function allows the rewards initiator to create rewards submissions for the AVS. This submission will send rewards to all eligible operators according to stake weight.

*Effects:*
* For each `RewardsSubmission`:
  * Transfers tokens from caller to the ServiceManager
  * Approves the `RewardsCoordinator` to spend these tokens
* Calls `RewardsCoordinator.createAVSRewardsSubmission` with the provided submissions

*Requirements:*
* Caller MUST be the designated rewards initiator
* Token transfers and approvals MUST succeed

#### `createOperatorDirectedAVSRewardsSubmission`

```solidity
function createOperatorDirectedAVSRewardsSubmission(
    IRewardsCoordinator.OperatorDirectedRewardsSubmission[] calldata
        operatorDirectedRewardsSubmissions
) 
    public
    virtual
    onlyRewardsInitiator
```

This function allows the rewards initiator to create operator-directed rewards submissions, which provide more control over how rewards are distributed to specific operators.

*Effects:*
* For each `OperatorDirectedRewardsSubmission`:
  * Calculates the total token amount across all operator rewards
  * Transfers tokens from caller to the ServiceManager
  * Approves the `RewardsCoordinator` to spend these tokens
* Calls `RewardsCoordinator.createOperatorDirectedAVSRewardsSubmission` with the provided submissions

*Requirements:*
* Caller MUST be the designated rewards initiator
* Token transfers and approvals MUST succeed

#### `setClaimerFor`

```solidity
function setClaimerFor(
    address claimer
) 
    public
    virtual
    onlyOwner
```

This function allows the owner to set an address that can claim rewards on behalf of the AVS.

*Effects:*
* Calls `RewardsCoordinator.setClaimerFor` to set the claimer address

*Requirements:*
* Caller MUST be the owner

#### `setRewardsInitiator`

```solidity
function setRewardsInitiator(
    address newRewardsInitiator
)
    external
    onlyOwner
```

This function allows the owner to update the address that is permitted to submit rewards submissions on behalf of the AVS.

*Effects:*
* Updates the `rewardsInitiator` storage variable
* Emits a `RewardsInitiatorUpdated` event

*Requirements:*
* Caller MUST be the owner

## Metadata Management

**Methods:**
* [`updateAVSMetadataURI`](#updateavsmetadatauri)

#### `updateAVSMetadataURI`

```solidity
function updateAVSMetadataURI(
    string memory _metadataURI
)
    public
    virtual
    onlyOwner
```

This function allows the owner to update the metadata URI associated with the AVS.

*Effects:*
* Calls `AVSDirectory.updateAVSMetadataURI` with the provided URI

*Requirements:*
* Caller MUST be the owner

## View Functions

**Methods:**
* [`getRestakeableStrategies`](#getrestakeablestrategies)
* [`getOperatorRestakedStrategies`](#getoperatorrestakedstrategies)
* [`avsDirectory`](#avsdirectory)

#### `getRestakeableStrategies`

```solidity
function getRestakeableStrategies()
    external
    view
    virtual
    returns (address[] memory)
```

This function returns a list of strategy addresses that the AVS supports for restaking. This is intended to be called off-chain by the rewards calculation system.

*Returns:*
* An array of strategy addresses that the AVS supports for restaking across all quorums

#### `getOperatorRestakedStrategies`

```solidity
function getOperatorRestakedStrategies(
    address operator
)
    external
    view
    virtual
    returns (address[] memory)
```

This function returns a list of strategy addresses that a specific operator has potentially restaked with the AVS. This is intended to be called off-chain by the rewards calculation system.

*Returns:*
* An array of strategy addresses that the operator has potentially restaked with the AVS across all quorums they are registered for

#### `avsDirectory`

```solidity
function avsDirectory()
    external
    view
    override
    returns (address)
```

This function returns the address of the EigenLayer AVSDirectory contract.

*Returns:*
* The address of the EigenLayer AVSDirectory contract
````

## File: docs/SlashingRegistryCoordinator.md
````markdown
## SlashingRegistryCoordinator

| File | Notes |
| -------- | -------- |
| [`SlashingRegistryCoordinator.sol`](../../src/contracts/SlashingRegistryCoordinator.sol) |  |
| [`SlashingRegistryCoordinatorStorage.sol`](../../src/contracts/SlashingRegistryCoordinatorStorage.sol) | state variables |
| [`ISlashingRegistryCoordinator.sol`](../../src/contracts/interfaces/ISlashingRegistryCoordinator.sol) | interface |

Libraries and Mixins:

| File | Notes |
| -------- | -------- |
| [`BitmapUtils.sol`](../../src/contracts/libraries/BitmapUtils.sol) | bitmap manipulation |
| [`BN254.sol`](../../src/contracts/libraries/BN254.sol) | elliptic curve operations |
| [`SignatureCheckerLib.sol`](../../src/contracts/libraries/SignatureCheckerLib.sol) | signature verification |
| [`QuorumBitmapHistoryLib.sol`](../../src/contracts/libraries/QuorumBitmapHistoryLib.sol) | managing quorum registrations |
| [`Pausable.sol`](../../src/contracts/permissions/Pausable.sol) | pausable functionality |

## Prior Reading

* [EigenLayer Slashing ELIP-002](https://github.com/eigenfoundation/ELIPs/blob/main/ELIPs/ELIP-002.md)

## Overview

The `SlashingRegistryCoordinator` manages the registry contracts and integrates with the `AllocationManager` to manage quorum creation, registration and deregistration. The contract's responsibilities include:

* [Quorum Management](#quorum-management)
* [Operator Registration](#operator-registration)
* [Stake Management](#stake-management)
* [Operator Churn](#operator-churn)
* [AVS Integration](#avs-integration)



## Parameterization

* `MAX_QUORUM_COUNT`: The maximum number of quorums that can be created (hardcoded to `192`).
* `BIPS_DENOMINATOR`: Used for calculating percentages in basis points (hardcoded to `10000`).
* `ejectionCooldown`: The cooldown period an operator must wait after being ejected before they can re-register.
  * Default: Can be set by the contract owner.

---

## Quorum Management

Quorums are logical groupings of operators that share a common purpose within an AVS. Each quorum tracks operator registrations, stakes, and has its own configuration for operator management. The `SlashingRegistryCoordinator` supports two types of quorums:

1. **Total Delegated Stake Quorums**: Track the total delegated stake for operators
2. **Slashable Stake Quorums**: Track the slashable stake for operators, which is used for slashing

Each quorum is identified by a unique `quorumNumber` and has its own set of parameters defined in the `OperatorSetParam` struct:

```solidity
struct OperatorSetParam {
    uint32 maxOperatorCount;
    uint16 kickBIPsOfOperatorStake;
    uint16 kickBIPsOfTotalStake;
}
```

**Methods:**
* [`createTotalDelegatedStakeQuorum`](#createtotaldelegatedstakequorum)
* [`createSlashableStakeQuorum`](#createslashablestakequorum)
* [`setOperatorSetParams`](#setoperatorsetparams)

#### `createTotalDelegatedStakeQuorum`

```solidity
function createTotalDelegatedStakeQuorum(
    OperatorSetParam memory operatorSetParams,
    uint96 minimumStake,
    IStakeRegistryTypes.StrategyParams[] memory strategyParams
) 
    external
```

This function creates a new quorum that tracks the total delegated stake for operators. The quorum is initialized with the provided parameters and integrated with the underlying registry contracts.

*Effects:*
* Increments the `quorumCount` by 1
* Sets the operator set parameters for the new quorum
* Creates an operator set in the `AllocationManager`
* Initializes the quorum in all registry contracts:
  * `StakeRegistry`: Sets minimum stake and strategy parameters
  * `IndexRegistry`: Prepares the quorum for tracking operator indices
  * `BLSApkRegistry`: Prepares the quorum for tracking BLS public keys
* Emits an `OperatorSetParamsUpdated` event for the new quorum

*Requirements:*
* Caller MUST be the contract owner
* The quorum count MUST NOT exceed `MAX_QUORUM_COUNT` (192)

#### `createSlashableStakeQuorum`

```solidity
function createSlashableStakeQuorum(
    OperatorSetParam memory operatorSetParams,
    uint96 minimumStake,
    IStakeRegistryTypes.StrategyParams[] memory strategyParams,
    uint32 lookAheadPeriod
) 
    external
```

This function creates a new quorum that specifically tracks slashable stake for operators. This type of quorum provides slashing enforcement through the `AllocationManager`.

*Effects:*
* Same as `createTotalDelegatedStakeQuorum`, but initializes the quorum with slashable stake type
* Additionally configures the `lookAheadPeriod` for slashable stake calculation

*Requirements:*
* Same as `createTotalDelegatedStakeQuorum`

#### `setOperatorSetParams`

```solidity
function setOperatorSetParams(
    uint8 quorumNumber,
    OperatorSetParam memory operatorSetParams
) 
    external
```

This function updates the parameters for an existing quorum, allowing the owner to modify the maximum operator count and churn thresholds.

*Effects:*
* Updates the operator set parameters for the specified quorum
* Emits an `OperatorSetParamsUpdated` event

*Requirements:*
* Caller MUST be the contract owner
* The specified quorum MUST exist

---

## Operator Registration

Operators need to register with the `SlashingRegistryCoordinator` to participate in quorums. Registration involves providing BLS public keys, sockets, and allocating stake to specific quorums. The contract tracks an operator's registration status and history of quorum memberships. The `AllocationManager` calls these functions when an operator registers for a quorum.

**Methods:**
* [`registerOperator`](#registeroperator)
* [`deregisterOperator`](#deregisteroperator)
* [`updateSocket`](#updatesocket)
* [`ejectOperator`](#ejectoperator)

#### `registerOperator`

```solidity
function registerOperator(
    address operator,
    address avs,
    uint32[] calldata operatorSetIds,
    bytes calldata data
) 
    external
    onlyAllocationManager
    onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR)
```

This function is called by the `AllocationManager` when an operator wants to register for one or more quorums. It supports two registration types: normal registration and registration with churn.

*Effects:*
* Registers operator's BLS public key if not already registered
* Updates operator's quorum bitmap to include the new quorums
* Updates operator's socket information
* Updates operator's registration status to `REGISTERED`
* Registers the operator with all registry contracts
* If registering with churn, may deregister another operator to make room
* Emits an `OperatorRegistered` event

*Requirements:*
* Caller MUST be the Allocation Manager
* Contract MUST NOT be paused for operator registration
* Provided AVS address MUST match the contract's configured AVS
* Operator MUST NOT be already registered for the specified quorums
* Operator MUST be past their ejection cooldown if they were previously ejected
* For normal registration, quorums MUST NOT exceed their maximum operator count
* For registration with churn, the churn approver's signature MUST be valid

#### `deregisterOperator`

```solidity
function deregisterOperator(
    address operator,
    address avs,
    uint32[] calldata operatorSetIds
) 
    external
    onlyAllocationManager
    onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR)
```

This function is called by the `AllocationManager` when an operator wants to deregister from one or more quorums.

*Effects:*
* Updates operator's quorum bitmap to remove the specified quorums
* If the operator is no longer registered for any quorums, updates their status to `DEREGISTERED`
* Deregisters the operator from all registry contracts
* Emits an `OperatorDeregistered` event if the operator's status changes to `DEREGISTERED`

*Requirements:*
* Caller MUST be the Allocation Manager
* Contract MUST NOT be paused for operator deregistration
* Provided AVS address MUST match the contract's configured AVS
* Operator MUST be currently registered
* Operator MUST be registered for the specified quorums

#### `updateSocket`

```solidity
function updateSocket(
    string memory socket
) 
    external
```

This function allows a registered operator to update their socket information.

*Effects:*
* Updates the operator's socket in the SocketRegistry
* Emits an `OperatorSocketUpdate` event

*Requirements:*
* Caller MUST be a registered operator

#### `ejectOperator`

```solidity
function ejectOperator(
    address operator,
    bytes memory quorumNumbers
) 
    external
    onlyEjector
```

This function allows the designated ejector to forcibly remove an operator from specified quorums.

*Effects:*
* Sets the operator's `lastEjectionTimestamp` to the current timestamp
* Deregisters the operator from the specified quorums
* Forces deregistration from the AllocationManager
* The operator will be unable to re-register until the `ejectionCooldown` period passes

*Requirements:*
* Caller MUST be the designated ejector address
* Operator MUST be registered for the specified quorums

---

## Stake Management

The `SlashingRegistryCoordinator` manages operator stakes through the `StakeRegistry`. It enforces minimum stake requirements and provides mechanisms to update stake values and deregister operators who fall below the threshold.

**Methods:**
* [`updateOperatorsForQuorum`](#updateoperatorsforquorum)

#### `updateOperatorsForQuorum`

```solidity
function updateOperatorsForQuorum(
    address[][] memory operatorsPerQuorum,
    bytes calldata quorumNumbers
) 
    external
```

This function updates the stakes of all operators in specified quorums at once. This is more efficient than calling `updateOperators` for multiple operators individually.

*Effects:*
* Updates stake values for all operators in the specified quorums
* Updates each quorum's `quorumUpdateBlockNumber` to the current block number
* May deregister operators from quorums if their stake falls below the minimum
* Emits a `QuorumBlockNumberUpdated` event for each updated quorum

*Requirements:*
* Contract MUST NOT be paused for operator updates
* The number of operator lists MUST match the number of quorums
* Each operator list MUST contain the exact set of registered operators for the corresponding quorum
* Each operator list MUST be sorted in ascending order by operator address
* Operators MUST be registered for their respective quorums

---

## Operator Churn

Operator churn is the process of replacing an existing operator with a new one when a quorum has reached its maximum capacity. The `SlashingRegistryCoordinator` provides mechanisms for churn based on stake thresholds and authorized approvals.

**Concepts:**
* [Churn Thresholds](#churn-thresholds)
* [Churn Approval](#churn-approval)

**Methods:**
* [`setChurnApprover`](#setchurnapprover)

#### Churn Thresholds

Operator churn is governed by two threshold parameters defined in the `OperatorSetParam` struct:

1. `kickBIPsOfOperatorStake`: The minimum percentage (in basis points) by which a new operator's stake must exceed an existing operator's stake to qualify for churn.
2. `kickBIPsOfTotalStake`: The minimum percentage (in basis points) of total quorum stake that an operator must maintain to avoid being churned out.

These thresholds ensure that operators can only be replaced by meaningfully higher-staked operators, and that operators with significant stake relative to the quorum total are protected from churn.

The contract implements two helper functions to calculate these thresholds:

```solidity
function _individualKickThreshold(
    uint96 operatorStake,
    OperatorSetParam memory setParams
)   
    internal 
    pure 
```

```solidity
function _totalKickThreshold(
    uint96 totalStake,
    OperatorSetParam memory setParams
) 
    internal 
    pure 
```

#### Churn Approval

For security and coordination, operator churn requires approval from a designated churn approver. The churn approver must sign a message authorizing the replacement of specific operators.

The churn approval process uses EIP-712 typed signatures to ensure the integrity and non-reusability of churn approvals. Each approval includes:

1. The registering operator's address and ID
2. The parameters specifying which operators to kick
3. A unique salt to prevent replay attacks
4. An expiration timestamp

```solidity
function calculateOperatorChurnApprovalDigestHash(
    address registeringOperator,
    bytes32 registeringOperatorId,
    OperatorKickParam[] memory operatorKickParams,
    bytes32 salt,
    uint256 expiry
) 
    public
```

#### `setChurnApprover`

```solidity
function setChurnApprover(
    address _churnApprover
) 
    external
```

This function updates the address that is authorized to approve operator churn operations.

*Effects:*
* Updates the `churnApprover` address
* Emits a `ChurnApproverUpdated` event

*Requirements:*
* Caller MUST be the contract owner

---

## AVS Integration

The `SlashingRegistryCoordinator` integrates with `AllocationManager`, and is identified as the `AVSRegistrar` within the `AllocationManager`.

**Methods:**
* [`setAVS`](#setavs)
* [`supportsAVS`](#supportsavs)

#### `setAVS`

```solidity
function setAVS(
    address _avs
) 
    external
```

This function sets the AVS address for the AVS (this identitiy is used for UAM integration). Note: updating this will break existing operator sets, this value should only be set once. 
This value should be the address of the `ServiceManager` contract.

*Effects:*
* Sets the `avs` address

*Requirements:*
* Caller MUST be the contract owner

#### `supportsAVS`

```solidity
function supportsAVS(
    address _avs
)  
    public
```

This function checks whether a given AVS address is supported by this contract. It is used by the `AllocationManager` to verify that the contract is correctly configured for a specific AVS.

*Returns:*
* `true` if the provided address matches the configured AVS address
* `false` otherwise

---

## Configuration Functions

These functions allow the contract owner to configure various parameters and roles within the contract.

**Methods:**
* [`setEjector`](#setejector)
* [`setEjectionCooldown`](#setejectioncooldown)

#### `setEjector`

```solidity
function setEjector(
    address _ejector
) 
    external
```

This function updates the address that is authorized to forcibly eject operators from quorums.

*Effects:*
* Updates the `ejector` address
* Emits an `EjectorUpdated` event

*Requirements:*
* Caller MUST be the contract owner

#### `setEjectionCooldown`

```solidity
function setEjectionCooldown(
    uint256 _ejectionCooldown
) 
    external
```

This function updates the cooldown period that ejected operators must wait before they can re-register.

*Effects:*
* Updates the `ejectionCooldown` value

*Requirements:*
* Caller MUST be the contract owner
````
