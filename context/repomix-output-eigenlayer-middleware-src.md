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
- Only files matching these patterns are included:  src/**
- Files matching patterns in .gitignore are excluded
- Files matching default ignore patterns are excluded
- Security check has been disabled - content may contain sensitive information
- Files are sorted by Git change count (files with more changes are at the bottom)

## Additional Info

# Directory Structure
```
src/
  interfaces/
    IBLSApkRegistry.sol
    IBLSSignatureChecker.sol
    IECDSAStakeRegistry.sol
    IEjectionManager.sol
    IIndexRegistry.sol
    IInstantSlasher.sol
    IRegistryCoordinator.sol
    IServiceManager.sol
    IServiceManagerUI.sol
    ISlasher.sol
    ISlashingRegistryCoordinator.sol
    ISocketRegistry.sol
    IStakeRegistry.sol
    IVetoableSlasher.sol
  libraries/
    BitmapUtils.sol
    BN254.sol
    LibMergeSort.sol
    QuorumBitmapHistoryLib.sol
    SignatureCheckerLib.sol
  slashers/
    base/
      SlasherBase.sol
      SlasherStorage.sol
    InstantSlasher.sol
    VetoableSlasher.sol
  unaudited/
    examples/
      ECDSAStakeRegistryEqualWeight.sol
      ECDSAStakeRegistryPermissioned.sol
    ECDSAServiceManagerBase.sol
    ECDSAStakeRegistry.sol
    ECDSAStakeRegistryStorage.sol
    README.md
  BLSApkRegistry.sol
  BLSApkRegistryStorage.sol
  BLSSignatureChecker.sol
  BLSSignatureCheckerStorage.sol
  EjectionManager.sol
  EjectionManagerStorage.sol
  IndexRegistry.sol
  IndexRegistryStorage.sol
  OperatorStateRetriever.sol
  RegistryCoordinator.sol
  RegistryCoordinatorStorage.sol
  ServiceManagerBase.sol
  ServiceManagerBaseStorage.sol
  ServiceManagerRouter.sol
  SlashingRegistryCoordinator.sol
  SlashingRegistryCoordinatorStorage.sol
  SocketRegistry.sol
  SocketRegistryStorage.sol
  StakeRegistry.sol
  StakeRegistryStorage.sol
```

# Files

## File: src/interfaces/IBLSApkRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {BN254} from "../libraries/BN254.sol";

interface IBLSApkRegistryErrors {
    /// @notice Thrown when a non-RegistryCoordinator address calls a restricted function.
    error OnlyRegistryCoordinatorOwner();
    /// @notice Thrown when attempting to initialize a quorum that already exists.
    error QuorumAlreadyExists();
    /// @notice Thrown when a quorum does not exist.
    error QuorumDoesNotExist();
    /// @notice Thrown when a BLS pubkey provided is zero pubkey
    error ZeroPubKey();
    /// @notice Thrown when an operator has already registered a BLS pubkey.
    error OperatorAlreadyRegistered();
    /// @notice Thrown when the operator is not registered.
    error OperatorNotRegistered();
    /// @notice Thrown when a BLS pubkey has already been registered for an operator.
    error BLSPubkeyAlreadyRegistered();
    /// @notice Thrown when either the G1 signature is wrong, or G1 and G2 private key do not match.
    error InvalidBLSSignatureOrPrivateKey();
    /// @notice Thrown when the quorum apk update block number is too recent.
    error BlockNumberTooRecent();
    /// @notice Thrown when blocknumber and index provided is not the latest apk update.
    error BlockNumberNotLatest();
    /// @notice Thrown when the block number is before the first update.
    error BlockNumberBeforeFirstUpdate();
    /// @notice Thrown when a G2 pubkey has already been set for an operator
    error G2PubkeyAlreadySet();
}

interface IBLSApkRegistryTypes {
    /// @notice Tracks the history of aggregate public key updates for a quorum.
    /// @dev Each update contains a hash of the aggregate public key and block numbers for timing.
    /// @param apkHash First 24 bytes of keccak256(apk_x0, apk_x1, apk_y0, apk_y1) representing the aggregate public key.
    /// @param updateBlockNumber Block number when this update occurred (inclusive).
    /// @param nextUpdateBlockNumber Block number when the next update occurred (exclusive), or 0 if this is the latest update.
    struct ApkUpdate {
        bytes24 apkHash;
        uint32 updateBlockNumber;
        uint32 nextUpdateBlockNumber;
    }

    /// @notice Parameters required when registering a new BLS public key.
    /// @dev Contains the registration signature and both G1/G2 public key components.
    /// @param pubkeyRegistrationSignature Registration message signed by operator's private key to prove ownership.
    /// @param pubkeyG1 The operator's public key in G1 group format.
    /// @param pubkeyG2 The operator's public key in G2 group format, must correspond to the same private key as pubkeyG1.
    struct PubkeyRegistrationParams {
        BN254.G1Point pubkeyRegistrationSignature;
        BN254.G1Point pubkeyG1;
        BN254.G2Point pubkeyG2;
    }
}

interface IBLSApkRegistryEvents is IBLSApkRegistryTypes {
    /*
     * @notice Emitted when `operator` registers their BLS public key pair (`pubkeyG1` and `pubkeyG2`).
     * @param operator The address of the operator registering the keys.
     * @param pubkeyG1 The operator's G1 public key.
     * @param pubkeyG2 The operator's G2 public key.
     */
    event NewPubkeyRegistration(
        address indexed operator, BN254.G1Point pubkeyG1, BN254.G2Point pubkeyG2
    );

    /*
     * @notice Emitted when `operator`'s pubkey is registered for `quorumNumbers`.
     * @param operator The address of the operator being registered.
     * @param operatorId The unique identifier for this operator (pubkey hash).
     * @param quorumNumbers The quorum numbers the operator is being registered for.
     */
    event OperatorAddedToQuorums(address operator, bytes32 operatorId, bytes quorumNumbers);

    /*
     * @notice Emitted when `operator`'s pubkey is deregistered from `quorumNumbers`.
     * @param operator The address of the operator being deregistered.
     * @param operatorId The unique identifier for this operator (pubkey hash).
     * @param quorumNumbers The quorum numbers the operator is being deregistered from.
     */
    event OperatorRemovedFromQuorums(address operator, bytes32 operatorId, bytes quorumNumbers);

    /// @notice Emitted when a G2 public key is registered for an operator
    event NewG2PubkeyRegistration(address indexed operator, BN254.G2Point pubkeyG2);
}

interface IBLSApkRegistry is IBLSApkRegistryErrors, IBLSApkRegistryEvents {
    /* STORAGE */

    /*
     * @notice Returns the address of the registry coordinator contract.
     * @return The address of the registry coordinator.
     * @dev This value is immutable and set during contract construction.
     */
    function registryCoordinator() external view returns (address);

    /*
     * @notice Maps `operator` to their BLS public key hash (`operatorId`).
     * @param operator The address of the operator.
     * @return operatorId The hash of the operator's BLS public key.
     */
    function operatorToPubkeyHash(
        address operator
    ) external view returns (bytes32 operatorId);

    /*
     * @notice Maps `pubkeyHash` to their corresponding `operator` address.
     * @param pubkeyHash The hash of a BLS public key.
     * @return operator The address of the operator who registered this public key.
     */
    function pubkeyHashToOperator(
        bytes32 pubkeyHash
    ) external view returns (address operator);

    /*
     * @notice Maps `operator` to their BLS public key in G1.
     * @dev Returns a non-encoded BN254.G1Point.
     * @param operator The address of the operator.
     * @return The operator's BLS public key in G1.
     */
    function operatorToPubkey(
        address operator
    ) external view returns (uint256, uint256);

    /*
     * @notice Maps `operator` to their BLS public key in G2.
     * @param operator The address of the operator.
     * @return The operator's BLS public key in G2.
     */
    function getOperatorPubkeyG2(
        address operator
    ) external view returns (BN254.G2Point memory);

    /*
     * @notice Stores the history of aggregate public key updates for `quorumNumber` at `index`.
     * @dev Returns a non-encoded IBLSApkRegistryTypes.ApkUpdate.
     * @param quorumNumber The identifier of the quorum.
     * @param index The index in the history array.
     * @return The APK update entry at the specified index for the given quorum.
     * @dev Each entry contains the APK hash, update block number, and next update block number.
     */
    function apkHistory(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (bytes24, uint32, uint32);

    /*
     * @notice Maps `quorumNumber` to their current aggregate public key.
     * @dev Returns a non-encoded BN254.G1Point.
     * @param quorumNumber The identifier of the quorum.
     * @return The current APK as a G1 point.
     */
    function currentApk(
        uint8 quorumNumber
    ) external view returns (uint256, uint256);

    /* ACTIONS */

    /*
     * @notice Registers `operator`'s pubkey for `quorumNumbers`.
     * @param operator The address of the operator to register.
     * @param quorumNumbers The quorum numbers to register for, where each byte is an 8-bit integer.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions (assumed, not validated):
     *      1. `quorumNumbers` has no duplicates
     *      2. `quorumNumbers.length` != 0
     *      3. `quorumNumbers` is ordered ascending
     *      4. The operator is not already registered
     */
    function registerOperator(address operator, bytes calldata quorumNumbers) external;

    /*
     * @notice Deregisters `operator`'s pubkey from `quorumNumbers`.
     * @param operator The address of the operator to deregister.
     * @param quorumNumbers The quorum numbers to deregister from, where each byte is an 8-bit integer.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions (assumed, not validated):
     *      1. `quorumNumbers` has no duplicates
     *      2. `quorumNumbers.length` != 0
     *      3. `quorumNumbers` is ordered ascending
     *      4. The operator is not already deregistered
     *      5. `quorumNumbers` is a subset of the operator's registered quorums
     */
    function deregisterOperator(address operator, bytes calldata quorumNumbers) external;

    /*
     * @notice Initializes `quorumNumber` by pushing its first APK update.
     * @param quorumNumber The number of the new quorum.
     */
    function initializeQuorum(
        uint8 quorumNumber
    ) external;

    /*
     * @notice Registers `operator` as the owner of a BLS public key using `params` and `pubkeyRegistrationMessageHash`.
     * @param operator The operator for whom the key is being registered.
     * @param params Contains the G1 & G2 public keys and ownership proof signature.
     * @param pubkeyRegistrationMessageHash The hash that must be signed to prove key ownership.
     * @return operatorId The unique identifier (pubkey hash) for this operator.
     * @dev Called by the RegistryCoordinator.
     */
    function registerBLSPublicKey(
        address operator,
        IBLSApkRegistryTypes.PubkeyRegistrationParams calldata params,
        BN254.G1Point calldata pubkeyRegistrationMessageHash
    ) external returns (bytes32 operatorId);

    /* VIEW */

    /*
     * @notice Returns the pubkey and pubkey hash of `operator`.
     * @param operator The address of the operator.
     * @return The operator's G1 public key and its hash.
     * @dev Reverts if the operator has not registered a valid pubkey.
     */
    function getRegisteredPubkey(
        address operator
    ) external view returns (BN254.G1Point memory, bytes32);

    /*
     * @notice Returns the APK indices at `blockNumber` for `quorumNumbers`.
     * @param quorumNumbers The quorum numbers to get indices for.
     * @param blockNumber The block number to query at.
     * @return Array of indices corresponding to each quorum number.
     */
    function getApkIndicesAtBlockNumber(
        bytes calldata quorumNumbers,
        uint256 blockNumber
    ) external view returns (uint32[] memory);

    /*
     * @notice Returns the current aggregate public key for `quorumNumber`.
     * @param quorumNumber The quorum to query.
     * @return The current APK as a G1 point.
     */
    function getApk(
        uint8 quorumNumber
    ) external view returns (BN254.G1Point memory);

    /*
     * @notice Returns an APK update entry for `quorumNumber` at `index`.
     * @param quorumNumber The quorum to query.
     * @param index The index in the APK history.
     * @return The APK update entry.
     */
    function getApkUpdateAtIndex(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (IBLSApkRegistryTypes.ApkUpdate memory);

    /*
     * @notice Gets the 24-byte hash of `quorumNumber`'s APK at `blockNumber` and `index`.
     * @param quorumNumber The quorum to query.
     * @param blockNumber The block number to get the APK hash for.
     * @param index The index in the APK history.
     * @return The 24-byte APK hash.
     * @dev Called by checkSignatures in BLSSignatureChecker.sol.
     */
    function getApkHashAtBlockNumberAndIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        uint256 index
    ) external view returns (bytes24);

    /*
     * @notice Returns the number of APK updates for `quorumNumber`.
     * @param quorumNumber The quorum to query.
     * @return The length of the APK history.
     */
    function getApkHistoryLength(
        uint8 quorumNumber
    ) external view returns (uint32);

    /*
     * @notice Maps `operator` to their corresponding public key hash.
     * @param operator The address of the operator.
     * @return operatorId The hash of the operator's BLS public key.
     * @dev Returns bytes32(0) if the operator hasn't registered a key.
     */
    function getOperatorId(
        address operator
    ) external view returns (bytes32 operatorId);

    /*
     * @notice Maps `pubkeyHash` to their corresponding operator address.
     * @param pubkeyHash The hash of a BLS public key.
     * @return operator The address of the operator who registered this public key.
     * @dev Returns address(0) if the public key hash hasn't been registered.
     */
    function getOperatorFromPubkeyHash(
        bytes32 pubkeyHash
    ) external view returns (address operator);

    /**
     * @notice Gets an operator's ID if it exists, or registers a new BLS public key and returns the new ID
     * @param operator The address of the operator
     * @param params The parameters for registering a new BLS public key
     * @param pubkeyRegistrationMessageHash The hash of the message to sign for registration
     * @return operatorId The operator's ID (pubkey hash)
     */
    function getOrRegisterOperatorId(
        address operator,
        PubkeyRegistrationParams calldata params,
        BN254.G1Point calldata pubkeyRegistrationMessageHash
    ) external returns (bytes32 operatorId);
}
````

## File: src/interfaces/IBLSSignatureChecker.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {ISlashingRegistryCoordinator} from "./ISlashingRegistryCoordinator.sol";
import {IBLSApkRegistry} from "./IBLSApkRegistry.sol";
import {IStakeRegistry, IDelegationManager} from "./IStakeRegistry.sol";

import {BN254} from "../libraries/BN254.sol";

interface IBLSSignatureCheckerErrors {
    /// @notice Thrown when the caller is not the registry coordinator owner.
    error OnlyRegistryCoordinatorOwner();
    /// @notice Thrown when the quorum numbers input in is empty.
    error InputEmptyQuorumNumbers();
    /// @notice Thrown when two array parameters have mismatching lengths.
    error InputArrayLengthMismatch();
    /// @notice Thrown when the non-signer pubkey length does not match non-signer bitmap indices length.
    error InputNonSignerLengthMismatch();
    /// @notice Thrown when the reference block number is invalid.
    error InvalidReferenceBlocknumber();
    /// @notice Thrown when the non signer pubkeys are not sorted.
    error NonSignerPubkeysNotSorted();
    /// @notice Thrown when StakeRegistry updates have not been updated within withdrawalDelayBlocks window
    error StaleStakesForbidden();
    /// @notice Thrown when the quorum apk hash in storage does not match provided quorum apk.
    error InvalidQuorumApkHash();
    /// @notice Thrown when BLS pairing precompile call fails.
    error InvalidBLSPairingKey();
    /// @notice Thrown when BLS signature is invalid.
    error InvalidBLSSignature();
}

interface IBLSSignatureCheckerTypes {
    /// @notice Contains bitmap and pubkey hash information for non-signing operators.
    /// @param quorumBitmaps Array of bitmaps indicating which quorums each non-signer was registered for.
    /// @param pubkeyHashes Array of BLS public key hashes for each non-signer.
    struct NonSignerInfo {
        uint256[] quorumBitmaps;
        bytes32[] pubkeyHashes;
    }

    /// @notice Contains non-signer information and aggregated signature data for BLS verification.
    /// @param nonSignerQuorumBitmapIndices The indices of all non-signer quorum bitmaps.
    /// @param nonSignerPubkeys The G1 public keys of all non-signers.
    /// @param quorumApks The aggregate G1 public key of each quorum.
    /// @param apkG2 The aggregate G2 public key of all signers.
    /// @param sigma The aggregate G1 signature of all signers.
    /// @param quorumApkIndices The indices of each quorum's aggregate public key in the APK registry.
    /// @param totalStakeIndices The indices of each quorum's total stake in the stake registry.
    /// @param nonSignerStakeIndices The indices of each non-signer's stake within each quorum.
    /// @dev Used as input to checkSignatures() to verify BLS signatures.
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

    /// @notice Records the total stake amounts for operators in each quorum.
    /// @param signedStakeForQuorum Array of total stake amounts from operators who signed, per quorum.
    /// @param totalStakeForQuorum Array of total stake amounts from all operators, per quorum.
    /// @dev Used to track stake distribution and calculate quorum thresholds. Array indices correspond to quorum numbers.
    struct QuorumStakeTotals {
        uint96[] signedStakeForQuorum;
        uint96[] totalStakeForQuorum;
    }
}

interface IBLSSignatureCheckerEvents is IBLSSignatureCheckerTypes {
    /// @notice Emitted when `staleStakesForbiddenUpdate` is set.
    event StaleStakesForbiddenUpdate(bool value);
}

interface IBLSSignatureChecker is IBLSSignatureCheckerErrors, IBLSSignatureCheckerEvents {
    /* STATE */

    /*
     * @notice Returns the address of the registry coordinator contract.
     * @return The address of the registry coordinator.
     * @dev This value is immutable and set during contract construction.
     */
    function registryCoordinator() external view returns (ISlashingRegistryCoordinator);

    /*
     * @notice Returns the address of the stake registry contract.
     * @return The address of the stake registry.
     * @dev This value is immutable and set during contract construction.
     */
    function stakeRegistry() external view returns (IStakeRegistry);

    /*
     * @notice Returns the address of the BLS APK registry contract.
     * @return The address of the BLS APK registry.
     * @dev This value is immutable and set during contract construction.
     */
    function blsApkRegistry() external view returns (IBLSApkRegistry);

    /*
     * @notice Returns the address of the delegation manager contract.
     * @return The address of the delegation manager.
     * @dev This value is immutable and set during contract construction.
     */
    function delegation() external view returns (IDelegationManager);

    /*
     * @notice Returns whether stale stakes are forbidden in signature verification.
     * @return True if stale stakes are forbidden, false otherwise.
     */
    function staleStakesForbidden() external view returns (bool);

    /* ACTIONS */

    /*
     * @notice Sets `value` as the new staleStakesForbidden flag.
     * @param value True to forbid stale stakes, false to allow them.
     * @dev Access restricted to the registry coordinator owner.
     */
    function setStaleStakesForbidden(
        bool value
    ) external;

    /* VIEW */

    /*
     * @notice This function is called by disperser when it has aggregated all the signatures of the operators
     * that are part of the quorum for a particular taskNumber and is asserting them into onchain. The function
     * checks that the claim for aggregated signatures are valid.
     *
     * The thesis of this procedure entails:
     * 1. Getting the aggregated pubkey of all registered nodes at the time of pre-commit by the
     * disperser (represented by apk in the parameters)
     * 2. Subtracting the pubkeys of all non-signers (nonSignerPubkeys) and storing
     * the output in apk to get aggregated pubkey of all operators that are part of quorum
     * 3. Using this aggregated pubkey to verify the aggregated signature under BLS scheme
     *
     * @param msgHash The hash of the message that was signed. NOTE: Be careful to ensure msgHash is
     * collision-resistant! This method does not hash msgHash in any way, so if an attacker is able
     * to pass in an arbitrary value, they may be able to tamper with signature verification.
     * @param quorumNumbers The quorum numbers to verify signatures for, where each byte is an 8-bit integer.
     * @param referenceBlockNumber The block number at which the stake information is being verified
     * @param nonSignerStakesAndSignature Contains non-signer information and aggregated signature data.
     * @return quorumStakeTotals The struct containing the total and signed stake for each quorum
     * @return signatoryRecordHash The hash of the signatory record, which is used for fraud proofs
     * @dev Before signature verification, the function verifies operator stake information. This includes
     * ensuring that the provided referenceBlockNumber is valid and recent enough, and that the stake is
     * either the most recent update for the total stake (of the operator) or latest before the referenceBlockNumber.
     */
    function checkSignatures(
        bytes32 msgHash,
        bytes calldata quorumNumbers,
        uint32 referenceBlockNumber,
        NonSignerStakesAndSignature memory nonSignerStakesAndSignature
    ) external view returns (QuorumStakeTotals memory, bytes32);

    /*
     * @notice Attempts to verify signature `sigma` against message hash `msgHash` using aggregate public keys `apk` and `apkG2`.
     * @param msgHash The hash of the message that was signed.
     * @param apk The aggregate public key in G1.
     * @param apkG2 The aggregate public key in G2.
     * @param sigma The signature to verify.
     * @return pairingSuccessful True if the pairing check succeeded.
     * @return siganatureIsValid True if the signature is valid.
     */
    function trySignatureAndApkVerification(
        bytes32 msgHash,
        BN254.G1Point memory apk,
        BN254.G2Point memory apkG2,
        BN254.G1Point memory sigma
    ) external view returns (bool pairingSuccessful, bool siganatureIsValid);
}
````

## File: src/interfaces/IECDSAStakeRegistry.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {IERC1271Upgradeable} from
    "@openzeppelin-upgrades/contracts/interfaces/IERC1271Upgradeable.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";

// TODO: many of these errors do not have test coverage.

interface IECDSAStakeRegistryErrors {
    /// @notice Thrown when the lengths of the signers array and signatures array do not match.
    error LengthMismatch();
    /// @notice Thrown when encountering an invalid length for the signers or signatures array.
    error InvalidLength();
    /// @notice Thrown when encountering an invalid signature.
    error InvalidSignature();
    /// @notice Thrown when the threshold update is greater than BPS.
    error InvalidThreshold();
    /// @notice Thrown when missing operators in an update.
    error MustUpdateAllOperators();
    /// @notice Thrown when reference blocks must be for blocks that have already been confirmed.
    error InvalidReferenceBlock();
    /// @notice Thrown when operator weights were out of sync and the signed weight exceed the total.
    error InvalidSignedWeight();
    /// @notice Thrown when the total signed stake fails to meet the required threshold.
    error InsufficientSignedStake();
    /// @notice Thrown when an individual signer's weight fails to meet the required threshold.
    error InsufficientWeight();
    /// @notice Thrown when the quorum is invalid.
    error InvalidQuorum();
    /// @notice Thrown when the system finds a list of items unsorted.
    error NotSorted();
    /// @notice Thrown when registering an already registered operator.
    error OperatorAlreadyRegistered();
    /// @notice Thrown when de-registering or updating the stake for an unregisted operator.
    error OperatorNotRegistered();
}

interface IECDSAStakeRegistryTypes {
    /// @notice Parameters for a strategy and its weight multiplier.
    /// @param strategy The strategy contract reference.
    /// @param multiplier The multiplier applied to the strategy.
    struct StrategyParams {
        IStrategy strategy;
        uint96 multiplier;
    }

    /// @notice Configuration for a quorum's strategies.
    /// @param strategies An array of strategy parameters defining the quorum.
    struct Quorum {
        StrategyParams[] strategies;
    }
}

interface IECDSAStakeRegistryEvents is IECDSAStakeRegistryTypes {
    /*
     * @notice Emitted when the system registers an operator.
     * @param operator The address of the registered operator.
     * @param avs The address of the associated AVS.
     */
    event OperatorRegistered(address indexed operator, address indexed avs);

    /*
     * @notice Emitted when the system deregisters an operator.
     * @param operator The address of the deregistered operator.
     * @param avs The address of the associated AVS.
     */
    event OperatorDeregistered(address indexed operator, address indexed avs);

    /*
     * @notice Emitted when the system updates the quorum.
     * @param previous The previous quorum configuration.
     * @param current The new quorum configuration.
     */
    event QuorumUpdated(Quorum previous, Quorum current);

    /*
     * @notice Emitted when the weight to join the operator set updates.
     * @param previous The previous minimum weight.
     * @param current The new minimumWeight.
     */
    event MinimumWeightUpdated(uint256 previous, uint256 current);

    /*
     * @notice Emitted when the weight required to be an operator changes.
     * @param oldMinimumWeight The previous weight.
     * @param newMinimumWeight The updated weight.
     */
    event UpdateMinimumWeight(uint256 oldMinimumWeight, uint256 newMinimumWeight);

    /*
     * @notice Emitted when the system updates an operator's weight.
     * @param operator The address of the operator updated.
     * @param oldWeight The operator's weight before the update.
     * @param newWeight The operator's weight after the update.
     */
    event OperatorWeightUpdated(address indexed operator, uint256 oldWeight, uint256 newWeight);

    /*
     * @notice Emitted when the system updates the total weight.
     * @param oldTotalWeight The total weight before the update.
     * @param newTotalWeight The total weight after the update.
     */
    event TotalWeightUpdated(uint256 oldTotalWeight, uint256 newTotalWeight);

    /*
     * @notice Emits when setting a new threshold weight.
     */
    event ThresholdWeightUpdated(uint256 thresholdWeight);

    /*
     * @notice Emitted when an operator's signing key is updated.
     * @param operator The address of the operator whose signing key was updated.
     * @param updateBlock The block number at which the signing key was updated.
     * @param newSigningKey The operator's signing key after the update.
     * @param oldSigningKey The operator's signing key before the update.
     */
    event SigningKeyUpdate(
        address indexed operator,
        uint256 indexed updateBlock,
        address indexed newSigningKey,
        address oldSigningKey
    );
}

interface IECDSAStakeRegistry is
    IECDSAStakeRegistryErrors,
    IECDSAStakeRegistryEvents,
    IERC1271Upgradeable
{
    /* ACTIONS */

    /*
     * @notice Registers a new operator using a provided operators signature and signing key.
     * @param operatorSignature Contains the operator's signature, salt, and expiry.
     * @param signingKey The signing key to add to the operator's history.
     */
    function registerOperatorWithSignature(
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature,
        address signingKey
    ) external;

    /*
     * @notice Deregisters an existing operator.
     */
    function deregisterOperator() external;

    /*
     * @notice Updates the signing key for an operator.
     * @param newSigningKey The new signing key to set for the operator.
     * @dev Only callable by the operator themselves.
     */
    function updateOperatorSigningKey(
        address newSigningKey
    ) external;

    /*
     * @notice Updates the StakeRegistry's view of operators' stakes.
     * @param operators A list of operator addresses to update.
     * @dev Queries stakes from the Eigenlayer core DelegationManager contract.
     */
    function updateOperators(
        address[] memory operators
    ) external;

    /*
     * @notice Updates the quorum configuration and the set of operators.
     * @param quorum The new quorum configuration, including strategies and their new weights.
     * @param operators The list of operator addresses to update stakes for.
     */
    function updateQuorumConfig(
        IECDSAStakeRegistryTypes.Quorum memory quorum,
        address[] memory operators
    ) external;

    /*
     * @notice Updates the weight an operator must have to join the operator set.
     * @param newMinimumWeight The new weight an operator must have to join the operator set.
     * @param operators The list of operators to update after changing the minimum weight.
     */
    function updateMinimumWeight(uint256 newMinimumWeight, address[] memory operators) external;

    /*
     * @notice Sets a new cumulative threshold weight for message validation.
     * @param thresholdWeight The updated threshold weight required to validate a message.
     */
    function updateStakeThreshold(
        uint256 thresholdWeight
    ) external;

    /* VIEW */

    /*
     * @notice Retrieves the current stake quorum details.
     * @return The current quorum of strategies and weights.
     */
    function quorum() external view returns (IECDSAStakeRegistryTypes.Quorum memory);

    /*
     * @notice Retrieves the latest signing key for a given operator.
     * @param operator The address of the operator.
     * @return The latest signing key of the operator.
     */
    function getLatestOperatorSigningKey(
        address operator
    ) external view returns (address);

    /*
     * @notice Retrieves the signing key for an operator at a specific block.
     * @param operator The address of the operator.
     * @param blockNumber The block number to query at.
     * @return The signing key of the operator at the given block.
     */
    function getOperatorSigningKeyAtBlock(
        address operator,
        uint256 blockNumber
    ) external view returns (address);

    /*
     * @notice Retrieves the last recorded weight for a given operator.
     * @param operator The address of the operator.
     * @return The latest weight of the operator.
     */
    function getLastCheckpointOperatorWeight(
        address operator
    ) external view returns (uint256);

    /*
     * @notice Retrieves the last recorded total weight across all operators.
     * @return The latest total weight.
     */
    function getLastCheckpointTotalWeight() external view returns (uint256);

    /*
     * @notice Retrieves the last recorded threshold weight.
     * @return The latest threshold weight.
     */
    function getLastCheckpointThresholdWeight() external view returns (uint256);

    /*
     * @notice Returns whether an operator is currently registered.
     * @param operator The operator address to check.
     * @return Whether the operator is registered.
     */
    function operatorRegistered(
        address operator
    ) external view returns (bool);

    /*
     * @notice Returns the minimum weight required for operator participation.
     * @return The minimum weight threshold.
     */
    function minimumWeight() external view returns (uint256);

    /*
     * @notice Retrieves the operator's weight at a specific block number.
     * @param operator The address of the operator.
     * @param blockNumber The block number to query at.
     * @return The weight of the operator at the given block.
     */
    function getOperatorWeightAtBlock(
        address operator,
        uint32 blockNumber
    ) external view returns (uint256);

    /*
     * @notice Retrieves the operator's weight.
     * @param operator The address of the operator.
     * @return The current weight of the operator.
     */
    function getOperatorWeight(
        address operator
    ) external view returns (uint256);

    /*
     * @notice Updates operators for a specific quorum.
     * @param operatorsPerQuorum Array of operator addresses per quorum.
     * @param data Additional data (unused but kept for interface compatibility).
     */
    function updateOperatorsForQuorum(
        address[][] memory operatorsPerQuorum,
        bytes memory data
    ) external;

    /*
     * @notice Retrieves the total weight at a specific block number.
     * @param blockNumber The block number to query at.
     * @return The total weight at the given block.
     */
    function getLastCheckpointTotalWeightAtBlock(
        uint32 blockNumber
    ) external view returns (uint256);

    /*
     * @notice Retrieves the threshold weight at a specific block number.
     * @param blockNumber The block number to query at.
     * @return The threshold weight at the given block.
     */
    function getLastCheckpointThresholdWeightAtBlock(
        uint32 blockNumber
    ) external view returns (uint256);
}
````

## File: src/interfaces/IEjectionManager.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {ISlashingRegistryCoordinator} from "./ISlashingRegistryCoordinator.sol";
import {IStakeRegistry} from "./IStakeRegistry.sol";

interface IEjectionManagerErrors {
    /// @notice Thrown when the caller is not the owner or ejector.
    error OnlyOwnerOrEjector();
    /// @notice Thrown when quorum number exceeds MAX_QUORUM_COUNT.
    error MaxQuorumCount();
}

interface IEjectionManagerTypes {
    /// @notice Parameters for controlling ejection rate limits per quorum.
    /// @param rateLimitWindow Time window to track ejection rate (in seconds).
    /// @param ejectableStakePercent Maximum percentage of stake that can be ejected per window (in BIPS).
    struct QuorumEjectionParams {
        uint32 rateLimitWindow;
        uint16 ejectableStakePercent;
    }

    /// @notice Records a stake ejection event with timing and amount.
    /// @param timestamp Time when the ejection occurred.
    /// @param stakeEjected Amount of stake that was ejected.
    struct StakeEjection {
        uint256 timestamp;
        uint256 stakeEjected;
    }
}

interface IEjectionManagerEvents is IEjectionManagerTypes {
    /*
     * @notice Emitted when the ejector address is set.
     * @param ejector The address being configured as ejector.
     * @param status The new status for the ejector address.
     */
    event EjectorUpdated(address ejector, bool status);

    /*
     * @notice Emitted when the rate limit parameters for a quorum are set.
     * @param quorumNumber The quorum number being configured.
     * @param rateLimitWindow The new time window for rate limiting.
     * @param ejectableStakePercent The new percentage of stake that can be ejected.
     */
    event QuorumEjectionParamsSet(
        uint8 quorumNumber, uint32 rateLimitWindow, uint16 ejectableStakePercent
    );

    /*
     * @notice Emitted when an operator is ejected.
     * @param operatorId The unique identifier of the ejected operator.
     * @param quorumNumber The quorum number the operator was ejected from.
     */
    event OperatorEjected(bytes32 operatorId, uint8 quorumNumber);

    /*
     * @notice Emitted when operators are ejected for a quorum.
     * @param ejectedOperators Number of operators that were ejected.
     * @param ratelimitHit Whether the ejection rate limit was reached.
     */
    event QuorumEjection(uint32 ejectedOperators, bool ratelimitHit);
}

interface IEjectionManager is IEjectionManagerErrors, IEjectionManagerEvents {
    /* STATE */

    /*
     * @notice Returns the address of the slashing registry coordinator contract.
     * @return The address of the slashing registry coordinator.
     * @dev This value is immutable and set during contract construction.
     */
    function slashingRegistryCoordinator() external view returns (ISlashingRegistryCoordinator);

    /*
     * @notice Returns the address of the stake registry contract.
     * @return The address of the stake registry.
     * @dev This value is immutable and set during contract construction.
     */
    function stakeRegistry() external view returns (IStakeRegistry);

    /*
     * @notice Returns whether `ejector` is authorized to eject operators under a rate limit.
     * @param ejector The address to check.
     * @return Whether the address is authorized to eject operators.
     */
    function isEjector(
        address ejector
    ) external view returns (bool);

    /*
     * @notice Returns the stake ejected for a quorum `quorumNumber` at array offset `index`.
     * @param quorumNumber The quorum number to query.
     * @param index The index in the ejection history.
     * @return timestamp The timestamp of the ejection.
     * @return stakeEjected The amount of stake ejected.
     */
    function stakeEjectedForQuorum(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (uint256 timestamp, uint256 stakeEjected);

    /*
     * @notice Returns the rate limit parameters for quorum `quorumNumber`.
     * @param quorumNumber The quorum number to query.
     * @return rateLimitWindow The time window to track ejection rate (in seconds).
     * @return ejectableStakePercent The maximum percentage of stake that can be ejected per window (in BIPS).
     */
    function quorumEjectionParams(
        uint8 quorumNumber
    ) external view returns (uint32 rateLimitWindow, uint16 ejectableStakePercent);

    /* ACTIONS */

    /*
     * @notice Ejects operators specified in `operatorIds` from the AVS's RegistryCoordinator under a rate limit.
     * @param operatorIds The ids of the operators to eject for each quorum.
     * @dev This function will eject as many operators as possible prioritizing operators at the lower index.
     * @dev The owner can eject operators without recording of stake ejection.
     */
    function ejectOperators(
        bytes32[][] memory operatorIds
    ) external;

    /*
     * @notice Sets the rate limit parameters for quorum `quorumNumber` to `quorumEjectionParams`.
     * @param quorumNumber The quorum number to set the rate limit parameters for.
     * @param quorumEjectionParams The quorum rate limit parameters to set.
     */
    function setQuorumEjectionParams(
        uint8 quorumNumber,
        QuorumEjectionParams memory quorumEjectionParams
    ) external;

    /*
     * @notice Sets whether address `ejector` is permissioned to eject operators under a rate limit to `status`.
     * @param ejector The address to permission.
     * @param status The status to set for the given address.
     */
    function setEjector(address ejector, bool status) external;

    /* VIEW */

    /*
     * @notice Returns the amount of stake that can be ejected for quorum `quorumNumber` at the current block.timestamp.
     * @param quorumNumber The quorum number to view ejectable stake for.
     * @return The amount of stake that can be ejected.
     */
    function amountEjectableForQuorum(
        uint8 quorumNumber
    ) external view returns (uint256);
}
````

## File: src/interfaces/IIndexRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

interface IIndexRegistryErrors {
    /// @notice Thrown when a function is called by an address that is not the RegistryCoordinator.
    error OnlyRegistryCoordinator();
    /// @notice Thrown when attempting to query a quorum that has no history.
    error QuorumDoesNotExist();
    /// @notice Thrown when attempting to look up an operator that does not exist at the specified block number.
    error OperatorIdDoesNotExist();
}

interface IIndexRegistryTypes {
    /// @notice Represents an update to an operator's status at a specific index.
    /// @param fromBlockNumber The block number from which this update takes effect.
    /// @param operatorId The unique identifier of the operator.
    struct OperatorUpdate {
        uint32 fromBlockNumber;
        bytes32 operatorId;
    }

    /// @notice Represents an update to the total number of operators in a quorum.
    /// @param fromBlockNumber The block number from which this update takes effect.
    /// @param numOperators The total number of operators after the update.
    struct QuorumUpdate {
        uint32 fromBlockNumber;
        uint32 numOperators;
    }
}

interface IIndexRegistryEvents is IIndexRegistryTypes {
    /*
     * @notice Emitted when an operator's index in a quorum is updated.
     * @param operatorId The unique identifier of the operator.
     * @param quorumNumber The identifier of the quorum.
     * @param newOperatorIndex The new index assigned to the operator.
     */
    event QuorumIndexUpdate(
        bytes32 indexed operatorId, uint8 quorumNumber, uint32 newOperatorIndex
    );
}

interface IIndexRegistry is IIndexRegistryErrors, IIndexRegistryEvents {
    /*
     * @notice Returns the special identifier used to indicate a non-existent operator.
     * @return The bytes32 constant OPERATOR_DOES_NOT_EXIST_ID.
     */
    function OPERATOR_DOES_NOT_EXIST_ID() external pure returns (bytes32);

    /*
     * @notice Returns the address of the RegistryCoordinator contract.
     * @return The address of the RegistryCoordinator.
     */
    function registryCoordinator() external view returns (address);

    /*
     * @notice Returns the current index of an operator with ID `operatorId` in quorum `quorumNumber`.
     * @dev This mapping is NOT updated when an operator is deregistered,
     * so it's possible that an index retrieved from this mapping is inaccurate.
     * If you're querying for an operator that might be deregistered, ALWAYS
     * check this index against the latest `_operatorIndexHistory` entry.
     * @param quorumNumber The identifier of the quorum.
     * @param operatorId The unique identifier of the operator.
     * @return The current index of the operator.
     */
    function currentOperatorIndex(
        uint8 quorumNumber,
        bytes32 operatorId
    ) external view returns (uint32);

    // ACTIONS

    /*
     * @notice Registers the operator with the specified `operatorId` for the quorums specified by `quorumNumbers`.
     * @param operatorId The unique identifier of the operator.
     * @param quorumNumbers The quorum numbers to register for.
     * @return An array containing a list of the number of operators (including the registering operator)
     *         in each of the quorums the operator is registered for.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions:
     *         1) `quorumNumbers` has no duplicates
     *         2) `quorumNumbers.length` != 0
     *         3) `quorumNumbers` is ordered in ascending order
     *         4) the operator is not already registered
     */
    function registerOperator(
        bytes32 operatorId,
        bytes calldata quorumNumbers
    ) external returns (uint32[] memory);

    /*
     * @notice Deregisters the operator with the specified `operatorId` for the quorums specified by `quorumNumbers`.
     * @param operatorId The unique identifier of the operator.
     * @param quorumNumbers The quorum numbers to deregister from.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions:
     *         1) `quorumNumbers` has no duplicates
     *         2) `quorumNumbers.length` != 0
     *         3) `quorumNumbers` is ordered in ascending order
     *         4) the operator is not already deregistered
     *         5) `quorumNumbers` is a subset of the quorumNumbers that the operator is registered for
     */
    function deregisterOperator(bytes32 operatorId, bytes calldata quorumNumbers) external;

    /*
     * @notice Initializes a new quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum to initialize.
     */
    function initializeQuorum(
        uint8 quorumNumber
    ) external;

    // VIEW

    /*
     * @notice Returns the operator update at index `arrayIndex` for operator at index `operatorIndex` in quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @param operatorIndex The index of the operator.
     * @param arrayIndex The index in the update history.
     * @return The operator update entry.
     */
    function getOperatorUpdateAtIndex(
        uint8 quorumNumber,
        uint32 operatorIndex,
        uint32 arrayIndex
    ) external view returns (OperatorUpdate memory);

    /*
     * @notice Returns the quorum update at index `quorumIndex` for quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @param quorumIndex The index in the quorum's update history.
     * @return The quorum update entry.
     */
    function getQuorumUpdateAtIndex(
        uint8 quorumNumber,
        uint32 quorumIndex
    ) external view returns (QuorumUpdate memory);

    /*
     * @notice Returns the latest quorum update for quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @return The most recent quorum update.
     */
    function getLatestQuorumUpdate(
        uint8 quorumNumber
    ) external view returns (QuorumUpdate memory);

    /*
     * @notice Returns the latest operator update for operator at index `operatorIndex` in quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @param operatorIndex The index of the operator.
     * @return The most recent operator update.
     */
    function getLatestOperatorUpdate(
        uint8 quorumNumber,
        uint32 operatorIndex
    ) external view returns (OperatorUpdate memory);

    /*
     * @notice Returns the list of operators in quorum `quorumNumber` at block `blockNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @param blockNumber The block number to query.
     * @return An array of operator IDs.
     */
    function getOperatorListAtBlockNumber(
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (bytes32[] memory);

    /*
     * @notice Returns the total number of operators in quorum `quorumNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @return The total number of operators.
     */
    function totalOperatorsForQuorum(
        uint8 quorumNumber
    ) external view returns (uint32);

    /*
     * @notice Returns the total number of operators in quorum `quorumNumber` at block `blockNumber`.
     * @param quorumNumber The identifier of the quorum.
     * @param blockNumber The block number to query.
     * @return The total number of operators at the specified block.
     */
    function totalOperatorsForQuorumAtBlockNumber(
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint32);
}
````

## File: src/interfaces/IInstantSlasher.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISlasher} from "./ISlasher.sol";

/// @title IInstantSlasher
/// @notice A slashing contract that immediately executes slashing requests without any delay or veto period
/// @dev Extends base interfaces to provide access controlled slashing functionality
interface IInstantSlasher is ISlasher {
    /// @notice Immediately executes a slashing request
    /// @param _slashingParams Parameters defining the slashing request including operator and amount
    /// @dev Can only be called by the authorized slasher
    function fulfillSlashingRequest(
        IAllocationManager.SlashingParams memory _slashingParams
    ) external;
}
````

## File: src/interfaces/IRegistryCoordinator.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {
    ISlashingRegistryCoordinator,
    ISlashingRegistryCoordinatorErrors,
    ISlashingRegistryCoordinatorEvents,
    ISlashingRegistryCoordinatorTypes
} from "./ISlashingRegistryCoordinator.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IBLSApkRegistry, IBLSApkRegistryTypes} from "./IBLSApkRegistry.sol";
import {IServiceManager} from "./IServiceManager.sol";
import {IStakeRegistry} from "./IStakeRegistry.sol";
import {IIndexRegistry} from "./IIndexRegistry.sol";
import {ISocketRegistry} from "./ISocketRegistry.sol";
import {IPauserRegistry} from "eigenlayer-contracts/src/contracts/interfaces/IPauserRegistry.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";

interface IRegistryCoordinatorErrors is ISlashingRegistryCoordinatorErrors {
    /// @notice Thrown when operator sets mode is already enabled.
    error OperatorSetsAlreadyEnabled();
    /// @notice Thrown when a quorum is an operator set quorum.
    error OperatorSetQuorum();
    /// @notice Thrown when M2 quorums are already disabled.
    error M2QuorumRegistrationIsDisabled();
    /// @notice Thrown when operator set operations are attempted while not enabled.
    error OperatorSetsNotEnabled();
    /// @notice Thrown when only M2 quorums are allowed.
    error OnlyM2QuorumsAllowed();
}

interface IRegistryCoordinatorTypes is ISlashingRegistryCoordinatorTypes {
    /**
     * @notice Parameters for initializing SlashingRegistryCoordinator
     * @param stakeRegistry The StakeRegistry contract that keeps track of operators' stakes
     * @param blsApkRegistry The BLSApkRegistry contract that keeps track of operators' BLS public keys
     * @param indexRegistry The IndexRegistry contract that keeps track of ordered operator lists
     * @param socketRegistry The SocketRegistry contract that keeps track of operators' sockets
     * @param allocationManager The AllocationManager contract for operator set management
     * @param pauserRegistry The PauserRegistry contract for pausing functionality
     */
    struct SlashingRegistryParams {
        IStakeRegistry stakeRegistry;
        IBLSApkRegistry blsApkRegistry;
        IIndexRegistry indexRegistry;
        ISocketRegistry socketRegistry;
        IAllocationManager allocationManager;
        IPauserRegistry pauserRegistry;
    }

    /**
     * @notice Parameters for initializing RegistryCoordinator
     * @param serviceManager The ServiceManager contract for this AVS
     * @param slashingParams Parameters for initializing SlashingRegistryCoordinator
     */
    struct RegistryCoordinatorParams {
        IServiceManager serviceManager;
        SlashingRegistryParams slashingParams;
    }
}

interface IRegistryCoordinatorEvents is
    ISlashingRegistryCoordinatorEvents,
    IRegistryCoordinatorTypes
{
    /**
     * @notice Emitted when operator sets mode is enabled.
     * @dev Emitted in enableOperatorSets().
     */
    event OperatorSetsEnabled();

    /**
     * @notice Emitted when M2 quorum registration is disabled.
     * @dev Emitted in disableM2QuorumRegistration().
     */
    event M2QuorumRegistrationDisabled();
}

interface IRegistryCoordinator is
    IRegistryCoordinatorErrors,
    IRegistryCoordinatorEvents,
    ISlashingRegistryCoordinator
{
    /**
     * @notice Reference to the ServiceManager contract.
     * @return The ServiceManager contract interface.
     * @dev This is only relevant for Pre-Slashing AVSs
     */
    function serviceManager() external view returns (IServiceManager);

    /// ACTIONS

    /**
     * @notice Registers an operator for service in specified quorums. If any quorum exceeds its maximum
     * operator capacity after the operator is registered, this method will fail.
     * @param quorumNumbers is an ordered byte array containing the quorum numbers being registered for AVSDirectory.
     * @param socket is the socket of the operator (typically an IP address).
     * @param params contains the G1 & G2 public keys of the operator, and a signature proving their ownership.
     * @param operatorSignature is the signature of the operator used by the AVS to register the operator in the delegation manager.
     * @dev `params` is ignored if the caller has previously registered a public key.
     * @dev `operatorSignature` is ignored if the operator's status is already REGISTERED.
     * @dev This function registers operators to the AVSDirectory using the M2-registration pathway.
     */
    function registerOperator(
        bytes memory quorumNumbers,
        string memory socket,
        IBLSApkRegistryTypes.PubkeyRegistrationParams memory params,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) external;

    /**
     * @notice Registers an operator while replacing existing operators in full quorums. If any quorum reaches its maximum operator
     * capacity, `operatorKickParams` is used to replace an old operator with the new one.
     * @param quorumNumbers is an ordered byte array containing the quorum numbers being registered for AVSDirectory.
     * @param socket is the socket of the operator (typically an IP address).
     * @param params contains the G1 & G2 public keys of the operator, and a signature proving their ownership.
     * @param operatorKickParams used to determine which operator is removed to maintain quorum capacity as the
     * operator registers for quorums.
     * @param churnApproverSignature is the signature of the churnApprover over the `operatorKickParams`.
     * @param operatorSignature is the signature of the operator used by the AVS to register the operator in the delegation manager.
     * @dev `params` is ignored if the caller has previously registered a public key.
     * @dev `operatorSignature` is ignored if the operator's status is already REGISTERED.
     * @dev This function registers operators to the AVSDirectory using the M2-registration pathway.
     */
    function registerOperatorWithChurn(
        bytes calldata quorumNumbers,
        string memory socket,
        IBLSApkRegistryTypes.PubkeyRegistrationParams memory params,
        OperatorKickParam[] memory operatorKickParams,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory churnApproverSignature,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) external;

    /**
     * @notice Deregisters the caller from one or more quorums. The operator will be removed from all registry contracts
     * and their quorum bitmap will be updated accordingly. If the operator is deregistered from all quorums, their status
     * will be updated to DEREGISTERED.
     * @param quorumNumbers is an ordered byte array containing the quorum numbers being deregistered from.
     * @dev Will revert if operator is not currently registered for any of the specified quorums.
     * @dev This function deregisters operators from the AVSDirectory using the M2-registration pathway.
     */
    function deregisterOperator(
        bytes memory quorumNumbers
    ) external;

    function operatorSetsEnabled() external view returns (bool);

    /**
     * @notice Checks if a quorum is an M2 quorum.
     * @param quorumNumber The quorum identifier.
     * @return True if the quorum is M2, false otherwise.
     */
    function isM2Quorum(
        uint8 quorumNumber
    ) external view returns (bool);

    /**
     * @notice Returns whether M2 quorum registration is disabled.
     * @return True if M2 quorum registration is disabled, false otherwise.
     */
    function isM2QuorumRegistrationDisabled() external view returns (bool);

    /**
     * @notice Disables M2 quorum registration for the AVS. Once disabled, this cannot be enabled.
     * @dev When disabled, all registrations to M2 quorums will revert. Deregistrations are still possible.
     */
    function disableM2QuorumRegistration() external;
}
````

## File: src/interfaces/IServiceManager.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity >=0.5.0;

import {IRewardsCoordinator} from
    "eigenlayer-contracts/src/contracts/interfaces/IRewardsCoordinator.sol";
import {IServiceManagerUI} from "./IServiceManagerUI.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IAllocationManagerTypes} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IAVSRegistrar} from "eigenlayer-contracts/src/contracts/interfaces/IAVSRegistrar.sol";

interface IServiceManagerErrors {
    /// @notice Thrown when a function is called by an address that is not the RegistryCoordinator.
    error OnlyRegistryCoordinator();
    /// @notice Thrown when a function is called by an address that is not the RewardsInitiator.
    error OnlyRewardsInitiator();
    /// @notice Thrown when a function is called by an address that is not the StakeRegistry.
    error OnlyStakeRegistry();
    /// @notice Thrown when a slashing proposal delay has not been met yet.
    error DelayPeriodNotPassed();
}

interface IServiceManagerEvents {
    /**
     * @notice Emitted when the rewards initiator address is updated.
     * @param prevRewardsInitiator The previous rewards initiator address.
     * @param newRewardsInitiator The new rewards initiator address.
     */
    event RewardsInitiatorUpdated(address prevRewardsInitiator, address newRewardsInitiator);
}

interface IServiceManager is IServiceManagerUI, IServiceManagerErrors, IServiceManagerEvents {
    /**
     * @notice Creates a new rewards submission to the EigenLayer RewardsCoordinator contract.
     * @dev Only callable by the permissioned rewardsInitiator address.
     * @dev The duration of the `rewardsSubmission` cannot exceed `MAX_REWARDS_DURATION`.
     * @dev The tokens are sent to the `RewardsCoordinator` contract.
     * @dev Strategies must be in ascending order of addresses to check for duplicates.
     * @dev This function will revert if the `rewardsSubmission` is malformed,
     *      e.g. if the `strategies` and `weights` arrays are of non-equal lengths.
     * @param rewardsSubmissions The rewards submissions to be split amongst the set of stakers
     *        delegated to operators who are registered to this `avs`.
     */
    function createAVSRewardsSubmission(
        IRewardsCoordinator.RewardsSubmission[] calldata rewardsSubmissions
    ) external;

    /**
     * @notice PERMISSIONCONTROLLER FUNCTIONS
     */

    /**
     * @notice Calls `addPendingAdmin` on the `PermissionController` contract.
     * @dev Only callable by the owner of the contract.
     * @param admin The address of the admin to add.
     */
    function addPendingAdmin(
        address admin
    ) external;

    /**
     * @notice Calls `removePendingAdmin` on the `PermissionController` contract.
     * @dev Only callable by the owner of the contract.
     * @param pendingAdmin The address of the pending admin to remove.
     */
    function removePendingAdmin(
        address pendingAdmin
    ) external;

    /**
     * @notice Calls `removeAdmin` on the `PermissionController` contract.
     * @dev Only callable by the owner of the contract.
     * @param admin The address of the admin to remove.
     */
    function removeAdmin(
        address admin
    ) external;

    /**
     * @notice Calls `setAppointee` on the `PermissionController` contract.
     * @dev Only callable by the owner of the contract.
     * @param appointee The address of the appointee to set.
     * @param target The address of the target to set the appointee for.
     * @param selector The function selector to set the appointee for.
     */
    function setAppointee(address appointee, address target, bytes4 selector) external;

    /**
     * @notice Calls `removeAppointee` on the `PermissionController` contract.
     * @dev Only callable by the owner of the contract.
     * @param appointee The address of the appointee to remove.
     * @param target The address of the target to remove the appointee for.
     * @param selector The function selector to remove the appointee for.
     */
    function removeAppointee(address appointee, address target, bytes4 selector) external;

    /**
     * @notice Deregisters an operator from specified operator sets
     * @param operator The address of the operator to deregister
     * @param operatorSetIds The IDs of the operator sets to deregister from
     * @dev Only callable by the RegistryCoordinator
     */
    function deregisterOperatorFromOperatorSets(
        address operator,
        uint32[] memory operatorSetIds
    ) external;
}
````

## File: src/interfaces/IServiceManagerUI.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity >=0.5.0;

import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";

/**
 * @title Minimal interface for a ServiceManager-type contract that AVS ServiceManager contracts must implement
 * for eigenlabs to be able to index their data on the AVS marketplace frontend.
 * @author Layr Labs, Inc.
 */
interface IServiceManagerUI {
    /**
     * @notice Updates the metadata URI for the AVS,
     * @param metadataURI is the metadata URI for the AVS.
     * @dev Metadata should follow the format outlined by this example.
     *     {
     *         "name": "EigenLabs AVS 1",
     *         "website": "https://www.eigenlayer.xyz/",
     *         "description": "This is my 1st AVS",
     *         "logo": "https://holesky-operator-metadata.s3.amazonaws.com/eigenlayer.png",
     *         "twitter": "https://twitter.com/eigenlayer"
     *     }
     */
    function updateAVSMetadataURI(
        string memory metadataURI
    ) external;

    /**
     * @notice Forwards a call to EigenLayer's AVSDirectory contract to confirm operator registration with the AVS.
     * @param operator The address of the operator to register.
     * @param operatorSignature The signature, salt, and expiry of the operator's signature.
     */
    function registerOperatorToAVS(
        address operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) external;

    /**
     * @notice Forwards a call to EigenLayer's AVSDirectory contract to confirm operator deregistration from the AVS.
     * @param operator The address of the operator to deregister.
     */
    function deregisterOperatorFromAVS(
        address operator
    ) external;

    /**
     * @notice Returns the list of strategies that the operator has potentially restaked on the AVS.
     * @param operator The address of the operator to get restaked strategies for.
     * @dev This function is intended to be called off-chain.
     * @dev No guarantee is made on whether the operator has shares for a strategy in a quorum or uniqueness
     *      of each element in the returned array. The off-chain service should do that validation separately.
     */
    function getOperatorRestakedStrategies(
        address operator
    ) external view returns (address[] memory);

    /**
     * @notice Returns the list of strategies that the AVS supports for restaking.
     * @dev This function is intended to be called off-chain.
     * @dev No guarantee is made on uniqueness of each element in the returned array.
     *      The off-chain service should do that validation separately.
     */
    function getRestakeableStrategies() external view returns (address[] memory);

    /**
     * @notice Returns the EigenLayer AVSDirectory contract.
     */
    function avsDirectory() external view returns (address);
}
````

## File: src/interfaces/ISlasher.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";

interface ISlasherErrors {
    /// @notice Thrown when a caller without slasher privileges attempts a restricted operation
    error OnlySlasher();
}

interface ISlasherTypes {
    /// @notice Structure containing details about a slashing request
    struct SlashingRequest {
        IAllocationManager.SlashingParams params;
        uint256 requestTimestamp;
    }
}

interface ISlasherEvents is ISlasherTypes {
    /// @notice Emitted when an operator is successfully slashed
    event OperatorSlashed(
        uint256 indexed slashingRequestId,
        address indexed operator,
        uint32 indexed operatorSetId,
        uint256[] wadsToSlash,
        string description
    );
}

/// @title ISlasher
/// @notice Base interface containing shared functionality for all slasher implementations
interface ISlasher is ISlasherErrors, ISlasherEvents {
    /// @notice Returns the address authorized to create and fulfill slashing requests
    function slasher() external view returns (address);

    /// @notice Returns the next slashing request ID
    function nextRequestId() external view returns (uint256);
}
````

## File: src/interfaces/ISlashingRegistryCoordinator.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IBLSApkRegistry} from "./IBLSApkRegistry.sol";
import {IStakeRegistry} from "./IStakeRegistry.sol";
import {IIndexRegistry} from "./IIndexRegistry.sol";
import {BN254} from "../libraries/BN254.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IBLSApkRegistry} from "./IBLSApkRegistry.sol";
import {IStakeRegistry, IStakeRegistryTypes} from "./IStakeRegistry.sol";
import {IIndexRegistry} from "./IIndexRegistry.sol";
import {ISocketRegistry} from "./ISocketRegistry.sol";
import {BN254} from "../libraries/BN254.sol";
import {IAVSRegistrar} from "eigenlayer-contracts/src/contracts/interfaces/IAVSRegistrar.sol";

interface ISlashingRegistryCoordinatorErrors {
    /// @notice Thrown when array lengths in input parameters don't match.
    error InputLengthMismatch();
    /// @notice Thrown when an invalid registration type is provided.
    error InvalidRegistrationType();
    /// @notice Thrown when non-allocation manager calls restricted function.
    error OnlyAllocationManager();
    /// @notice Thrown when non-ejector calls restricted function.
    error OnlyEjector();
    /// @notice Thrown when operating on a non-existent quorum.
    error QuorumDoesNotExist();
    /// @notice Thrown when registering/deregistering with empty bitmap.
    error BitmapEmpty();
    /// @notice Thrown when registering for already registered quorums.
    error AlreadyRegisteredForQuorums();
    /// @notice Thrown when registering before ejection cooldown expires.
    error CannotReregisterYet();
    /// @notice Thrown when unregistered operator attempts restricted operation.
    error NotRegistered();
    /// @notice Thrown when operator attempts self-churn.
    error CannotChurnSelf();
    /// @notice Thrown when operator count doesn't match quorum requirements.
    error QuorumOperatorCountMismatch();
    /// @notice Thrown when operator has insufficient stake for churn.
    error InsufficientStakeForChurn();
    /// @notice Thrown when attempting to kick operator above stake threshold.
    error CannotKickOperatorAboveThreshold();
    /// @notice Thrown when updating to zero bitmap.
    error BitmapCannotBeZero();
    /// @notice Thrown when deregistering from unregistered quorum.
    error NotRegisteredForQuorum();
    /// @notice Thrown when churn approver salt is already used.
    error ChurnApproverSaltUsed();
    /// @notice Thrown when operators or quorums list is not sorted ascending.
    error NotSorted();
    /// @notice Thrown when maximum quorum count is reached.
    error MaxQuorumsReached();
    /// @notice Thrown when the provided AVS address does not match the expected one.
    error InvalidAVS();
    /// @notice Thrown when attempting to kick an operator that is not registered.
    error OperatorNotRegistered();
    /// @notice Thrown when lookAheadPeriod is greater than or equal to DEALLOCATION_DELAY.
    error LookAheadPeriodTooLong();
    /// @notice Thrown when the number of operators in a quorum would exceed the maximum allowed.
    error MaxOperatorCountReached();
}

interface ISlashingRegistryCoordinatorTypes {
    /// @notice Core data structure for tracking operator information.
    /// @dev Links an operator's unique identifier with their current registration status.
    /// @param operatorId Unique identifier for the operator, typically derived from their BLS public key.
    /// @param status Current registration state of the operator in the system.
    struct OperatorInfo {
        bytes32 operatorId;
        OperatorStatus status;
    }

    /// @notice Records historical changes to an operator's quorum registrations.
    /// @dev Used for querying an operator's quorum memberships at specific block numbers.
    /// @param updateBlockNumber Block number when this update occurred (inclusive).
    /// @param nextUpdateBlockNumber Block number when the next update occurred (exclusive), or 0 if this is the latest update.
    /// @param quorumBitmap Bitmap where each bit represents registration in a specific quorum (1 = registered, 0 = not registered).
    struct QuorumBitmapUpdate {
        uint32 updateBlockNumber;
        uint32 nextUpdateBlockNumber;
        uint192 quorumBitmap;
    }

    /// @notice Configuration parameters for operator management within a quorum.
    /// @dev All BIPs (Basis Points) values are in relation to BIPS_DENOMINATOR (10000).
    /// @param maxOperatorCount Maximum number of operators allowed in the quorum.
    /// @param kickBIPsOfOperatorStake Required stake ratio (in BIPs) between new and existing operator for churn.
    ///        Example: 10500 means new operator needs 105% of existing operator's stake.
    /// @param kickBIPsOfTotalStake Minimum stake ratio (in BIPs) of total quorum stake an operator must maintain.
    ///        Example: 100 means operator needs 1% of total quorum stake to avoid being churned.
    struct OperatorSetParam {
        uint32 maxOperatorCount;
        uint16 kickBIPsOfOperatorStake;
        uint16 kickBIPsOfTotalStake;
    }

    /// @notice Parameters for removing an operator during churn.
    /// @dev Used in registerOperatorWithChurn to specify which operator to replace.
    /// @param quorumNumber The quorum from which to remove the operator.
    /// @param operator Address of the operator to be removed.
    struct OperatorKickParam {
        uint8 quorumNumber;
        address operator;
    }

    /// @notice Represents the registration state of an operator.
    /// @dev Used to track an operator's lifecycle in the system.
    /// @custom:enum NEVER_REGISTERED The operator has never registered with the system.
    /// @custom:enum REGISTERED The operator is currently registered and active.
    /// @custom:enum DEREGISTERED The operator was previously registered but has since deregistered.
    enum OperatorStatus {
        NEVER_REGISTERED,
        REGISTERED,
        DEREGISTERED
    }

    /**
     * @notice Enum representing the type of operator registration.
     * @custom:enum NORMAL Represents a normal operator registration.
     * @custom:enum CHURN Represents an operator registration during a churn event.
     */
    enum RegistrationType {
        NORMAL,
        CHURN
    }

    /**
     * @notice Data structure for storing the results of a registerOperator call.
     * @dev Contains arrays storing per-quorum information about operator counts and stakes.
     * @param numOperatorsPerQuorum For each quorum the operator registered for, stores the number of operators registered.
     * @param operatorStakes For each quorum the operator registered for, stores the stake of the operator in the quorum.
     * @param totalStakes For each quorum the operator registered for, stores the total stake of the quorum.
     */
    struct RegisterResults {
        uint32[] numOperatorsPerQuorum;
        uint96[] operatorStakes;
        uint96[] totalStakes;
    }
}

interface ISlashingRegistryCoordinatorEvents is ISlashingRegistryCoordinatorTypes {
    /**
     * @notice Emitted when an operator registers for service in one or more quorums.
     * @dev Emitted in _registerOperator() and _registerOperatorToOperatorSet().
     * @param operator The address of the registered operator.
     * @param operatorId The unique identifier of the operator (BLS public key hash).
     */
    event OperatorRegistered(address indexed operator, bytes32 indexed operatorId);

    /**
     * @notice Emitted when an operator deregisters from service in one or more quorums.
     * @dev Emitted in _deregisterOperator().
     * @param operator The address of the deregistered operator.
     * @param operatorId The unique identifier of the operator (BLS public key hash).
     */
    event OperatorDeregistered(address indexed operator, bytes32 indexed operatorId);

    /**
     * @notice Emitted when a new quorum is created.
     * @param quorumNumber The identifier of the quorum being created.
     * @param operatorSetParams The operator set parameters for the quorum.
     * @param minimumStake The minimum stake required for operators in this quorum.
     * @param strategyParams The strategy parameters for stake calculation.
     * @param stakeType The type of stake being tracked (TOTAL_DELEGATED or TOTAL_SLASHABLE).
     * @param lookAheadPeriod The number of blocks to look ahead when calculating slashable stake (only used for TOTAL_SLASHABLE).
     */
    event QuorumCreated(
        uint8 indexed quorumNumber,
        OperatorSetParam operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] strategyParams,
        IStakeRegistryTypes.StakeType stakeType,
        uint32 lookAheadPeriod
    );

    /**
     * @notice Emitted when a quorum's operator set parameters are updated.
     * @dev Emitted in _setOperatorSetParams().
     * @param quorumNumber The identifier of the quorum being updated.
     * @param operatorSetParams The new operator set parameters for the quorum.
     */
    event OperatorSetParamsUpdated(uint8 indexed quorumNumber, OperatorSetParam operatorSetParams);

    /**
     * @notice Emitted when the churn approver address is updated.
     * @dev Emitted in _setChurnApprover().
     * @param prevChurnApprover The previous churn approver address.
     * @param newChurnApprover The new churn approver address.
     */
    event ChurnApproverUpdated(address prevChurnApprover, address newChurnApprover);

    /**
     * @notice Emitted when the AVS address is updated.
     * @param prevAVS The previous AVS address.
     * @param newAVS The new AVS address.
     */
    event AVSUpdated(address prevAVS, address newAVS);

    /**
     * @notice Emitted when the ejector address is updated.
     * @dev Emitted in _setEjector().
     * @param prevEjector The previous ejector address.
     * @param newEjector The new ejector address.
     */
    event EjectorUpdated(address prevEjector, address newEjector);

    /**
     * @notice Emitted when all operators in a quorum are updated simultaneously.
     * @dev Emitted in updateOperatorsForQuorum().
     * @param quorumNumber The identifier of the quorum being updated.
     * @param blocknumber The block number at which the quorum update occurred.
     */
    event QuorumBlockNumberUpdated(uint8 indexed quorumNumber, uint256 blocknumber);

    /**
     * @notice Emitted when an operator's socket is updated.
     * @dev Emitted in updateSocket().
     * @param operatorId The unique identifier of the operator (BLS public key hash).
     * @param socket The new socket address for the operator (typically an IP address).
     */
    event OperatorSocketUpdate(bytes32 indexed operatorId, string socket);

    /**
     * @notice Emitted when the ejection cooldown period is updated.
     * @dev Emitted in setEjectionCooldown().
     * @param prevEjectionCooldown The previous cooldown duration in seconds.
     * @param newEjectionCooldown The new cooldown duration in seconds.
     */
    event EjectionCooldownUpdated(uint256 prevEjectionCooldown, uint256 newEjectionCooldown);
}

interface ISlashingRegistryCoordinator is
    IAVSRegistrar,
    ISlashingRegistryCoordinatorErrors,
    ISlashingRegistryCoordinatorEvents
{
    /// IMMUTABLES & CONSTANTS

    /**
     * @notice EIP-712 typehash for operator churn approval signatures.
     * @return The typehash constant.
     */
    function OPERATOR_CHURN_APPROVAL_TYPEHASH() external view returns (bytes32);

    /**
     * @notice EIP-712 typehash for pubkey registration signatures.
     * @return The typehash constant.
     */
    function PUBKEY_REGISTRATION_TYPEHASH() external view returns (bytes32);

    /**
     * @notice Reference to the BLSApkRegistry contract.
     * @return The BLSApkRegistry contract interface.
     */
    function blsApkRegistry() external view returns (IBLSApkRegistry);

    /**
     * @notice Reference to the StakeRegistry contract.
     * @return The StakeRegistry contract interface.
     */
    function stakeRegistry() external view returns (IStakeRegistry);

    /**
     * @notice Reference to the IndexRegistry contract.
     * @return The IndexRegistry contract interface.
     */
    function indexRegistry() external view returns (IIndexRegistry);

    /**
     * @notice Reference to the AllocationManager contract.
     * @return The AllocationManager contract interface.
     * @dev This is only relevant for Slashing AVSs
     */
    function allocationManager() external view returns (IAllocationManager);

    /**
     * @notice Reference to the SocketRegistry contract.
     * @return The SocketRegistry contract interface.
     */
    function socketRegistry() external view returns (ISocketRegistry);

    /// STORAGE

    /**
     * @notice The total number of quorums that have been created.
     * @return The count of quorums.
     */
    function quorumCount() external view returns (uint8);

    /**
     * @notice Checks if a churn approver salt has been used.
     * @param salt The salt to check.
     * @return True if the salt has been used, false otherwise.
     */
    function isChurnApproverSaltUsed(
        bytes32 salt
    ) external view returns (bool);

    /**
     * @notice Gets the last block number when all operators in a quorum were updated.
     * @param quorumNumber The quorum identifier.
     * @return The block number of the last update.
     */
    function quorumUpdateBlockNumber(
        uint8 quorumNumber
    ) external view returns (uint256);

    /**
     * @notice The address authorized to approve operator churn operations.
     * @return The churn approver address.
     */
    function churnApprover() external view returns (address);

    /**
     * @notice The address authorized to forcibly eject operators.
     * @return The ejector address.
     */
    function ejector() external view returns (address);

    /**
     * @notice Gets the timestamp of an operator's last ejection.
     * @param operator The operator address.
     * @return The timestamp of the last ejection.
     */
    function lastEjectionTimestamp(
        address operator
    ) external view returns (uint256);

    /**
     * @notice The cooldown period after ejection before an operator can re-register.
     * @return The cooldown duration in seconds.
     */
    function ejectionCooldown() external view returns (uint256);

    /// ACTIONS

    /**
     * @notice Updates stake weights for specified operators. If any operator is found to be below
     * the minimum stake for their registered quorums, they are deregistered from those quorums.
     * @param operators The operators whose stakes should be updated.
     * @dev Stakes are queried from the Eigenlayer core DelegationManager contract.
     * @dev WILL BE DEPRECATED IN FAVOR OF updateOperatorsForQuorum
     */
    function updateOperators(
        address[] memory operators
    ) external;

    /**
     * @notice For each quorum in `quorumNumbers`, updates the StakeRegistry's view of ALL its registered operators' stakes.
     * Each quorum's `quorumUpdateBlockNumber` is also updated, which tracks the most recent block number when ALL registered
     * operators were updated.
     * @dev stakes are queried from the Eigenlayer core DelegationManager contract
     * @param operatorsPerQuorum for each quorum in `quorumNumbers`, this has a corresponding list of operators to update.
     * @dev Each list of operator addresses MUST be sorted in ascending order
     * @dev Each list of operator addresses MUST represent the entire list of registered operators for the corresponding quorum
     * @param quorumNumbers is an ordered byte array containing the quorum numbers being updated
     * @dev invariant: Each list of `operatorsPerQuorum` MUST be a sorted version of `IndexRegistry.getOperatorListAtBlockNumber`
     * for the corresponding quorum.
     * @dev note on race condition: if an operator registers/deregisters for any quorum in `quorumNumbers` after a txn to
     * this method is broadcast (but before it is executed), the method will fail
     */
    function updateOperatorsForQuorum(
        address[][] memory operatorsPerQuorum,
        bytes calldata quorumNumbers
    ) external;

    /**
     * @notice Updates the socket of the msg.sender given they are a registered operator.
     * @param socket The new socket address for the operator (typically an IP address).
     * @dev Will revert if msg.sender is not a registered operator.
     */
    function updateSocket(
        string memory socket
    ) external;

    /**
     * @notice Forcibly removes an operator from specified quorums and sets their ejection timestamp.
     * @param operator The operator address to eject.
     * @param quorumNumbers The quorum numbers to eject the operator from.
     * @dev Can only be called by the ejector address.
     * @dev The operator cannot re-register until ejectionCooldown period has passed.
     */
    function ejectOperator(address operator, bytes memory quorumNumbers) external;

    /**
     * @notice Creates a new quorum that tracks total delegated stake for operators.
     * @param operatorSetParams Configures the quorum's max operator count and churn parameters.
     * @param minimumStake Sets the minimum stake required for an operator to register or remain registered.
     * @param strategyParams A list of strategies and multipliers used by the StakeRegistry to calculate
     * an operator's stake weight for the quorum.
     * @dev For m2 AVS this function has the same behavior as createQuorum before.
     * @dev For migrated AVS that enable operator sets this will create a quorum that measures total delegated stake for operator set.
     */
    function createTotalDelegatedStakeQuorum(
        OperatorSetParam memory operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] memory strategyParams
    ) external;

    /**
     * @notice Creates a new quorum that tracks slashable stake for operators.
     * @param operatorSetParams Configures the quorum's max operator count and churn parameters.
     * @param minimumStake Sets the minimum stake required for an operator to register or remain registered.
     * @param strategyParams A list of strategies and multipliers used by the StakeRegistry to calculate
     * an operator's stake weight for the quorum.
     * @param lookAheadPeriod The number of blocks to look ahead when calculating slashable stake.
     * @dev Can only be called when operator sets are enabled.
     */
    function createSlashableStakeQuorum(
        OperatorSetParam memory operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] memory strategyParams,
        uint32 lookAheadPeriod
    ) external;

    /**
     * @notice Updates the configuration parameters for an existing operator set quorum.
     * @param quorumNumber The identifier of the quorum to update.
     * @param operatorSetParams The new operator set parameters to apply.
     * @dev Can only be called by the contract owner.
     */
    function setOperatorSetParams(
        uint8 quorumNumber,
        OperatorSetParam memory operatorSetParams
    ) external;

    /**
     * @notice Updates the address authorized to approve operator churn operations.
     * @param _churnApprover The new churn approver address.
     * @dev Can only be called by the contract owner.
     * @dev The churn approver is responsible for signing off on operator replacements in full quorums.
     */
    function setChurnApprover(
        address _churnApprover
    ) external;

    /**
     * @notice Updates the address authorized to forcibly eject operators.
     * @param _ejector The new ejector address.
     * @dev Can only be called by the contract owner.
     * @dev The ejector can force-remove operators from quorums regardless of their stake.
     */
    function setEjector(
        address _ejector
    ) external;

    /**
     * @notice Updates the duration operators must wait after ejection before re-registering.
     * @param _ejectionCooldown The new cooldown duration in seconds.
     * @dev Can only be called by the contract owner.
     */
    function setEjectionCooldown(
        uint256 _ejectionCooldown
    ) external;

    /**
     * @notice Updates the avs address for this AVS (used for UAM integration in EigenLayer)
     * @param _avs The new avs address
     * @dev Can only be called by the contract owner
     * @dev NOTE: Updating this value will break existing OperatorSets and UAM integration. This value should only be set once.
     */
    function setAVS(
        address _avs
    ) external;

    /// VIEW

    /**
     * @notice Returns the hash of the message that operators must sign with their BLS key to register
     * @param operator The operator's Ethereum address
     */
    function calculatePubkeyRegistrationMessageHash(
        address operator
    ) external view returns (bytes32);

    /**
     * @notice Returns the operator set parameters for a given quorum.
     * @param quorumNumber The identifier of the quorum to query.
     * @return The OperatorSetParam struct containing max operator count and churn thresholds.
     */
    function getOperatorSetParams(
        uint8 quorumNumber
    ) external view returns (OperatorSetParam memory);

    /**
     * @notice Returns the complete operator information for a given address.
     * @param operator The operator address to query.
     * @return An OperatorInfo struct containing the operator's ID and registration status.
     */
    function getOperator(
        address operator
    ) external view returns (OperatorInfo memory);

    /**
     * @notice Returns the unique identifier for a given operator address.
     * @param operator The operator address to query.
     * @return The operator's ID (derived from their BLS public key hash).
     */
    function getOperatorId(
        address operator
    ) external view returns (bytes32);

    /**
     * @notice Returns the operator address associated with a given operator ID.
     * @param operatorId The unique identifier to look up.
     * @return The operator's address.
     * @dev Returns address(0) if the ID is not registered.
     */
    function getOperatorFromId(
        bytes32 operatorId
    ) external view returns (address);

    /**
     * @notice Returns the current registration status for a given operator.
     * @param operator The operator address to query.
     * @return The operator's status (NEVER_REGISTERED, REGISTERED, or DEREGISTERED).
     */
    function getOperatorStatus(
        address operator
    ) external view returns (OperatorStatus);

    /**
     * @notice Returns the indices needed to look up quorum bitmaps for operators at a specific block.
     * @param blockNumber The historical block number to query.
     * @param operatorIds Array of operator IDs to get indices for.
     * @return Array of indices corresponding to each operator ID.
     * @dev Reverts if any operator had not yet registered at the specified block.
     * @dev This function is designed to find proper inputs for getQuorumBitmapAtBlockNumberByIndex.
     */
    function getQuorumBitmapIndicesAtBlockNumber(
        uint32 blockNumber,
        bytes32[] memory operatorIds
    ) external view returns (uint32[] memory);

    /**
     * @notice Returns the quorum bitmap for an operator at a specific historical block.
     * @param operatorId The operator's unique identifier.
     * @param blockNumber The historical block number to query.
     * @param index The index in the operator's bitmap history (from getQuorumBitmapIndicesAtBlockNumber).
     * @return The quorum bitmap showing which quorums the operator was registered for.
     * @dev Reverts if the index is incorrect for the specified block number.
     */
    function getQuorumBitmapAtBlockNumberByIndex(
        bytes32 operatorId,
        uint32 blockNumber,
        uint256 index
    ) external view returns (uint192);

    /**
     * @notice Returns a specific update from an operator's quorum bitmap history.
     * @param operatorId The operator's unique identifier.
     * @param index The index in the bitmap history to query.
     * @return The QuorumBitmapUpdate struct at that index.
     */
    function getQuorumBitmapUpdateByIndex(
        bytes32 operatorId,
        uint256 index
    ) external view returns (QuorumBitmapUpdate memory);

    /**
     * @notice Returns the current quorum bitmap for an operator.
     * @param operatorId The operator's unique identifier.
     * @return A bitmap where each bit represents registration in a specific quorum.
     * @dev Returns 0 if the operator is not registered for any quorums.
     */
    function getCurrentQuorumBitmap(
        bytes32 operatorId
    ) external view returns (uint192);

    /**
     * @notice Returns the number of updates in an operator's bitmap history.
     * @param operatorId The operator's unique identifier.
     * @return The length of the bitmap history array.
     */
    function getQuorumBitmapHistoryLength(
        bytes32 operatorId
    ) external view returns (uint256);

    /**
     * @notice Calculates the digest hash that must be signed by the churn approver.
     * @param registeringOperator The address of the operator attempting to register.
     * @param registeringOperatorId The unique ID of the registering operator.
     * @param operatorKickParams Parameters specifying which operators to replace in full quorums.
     * @param salt Random value to ensure signature uniqueness.
     * @param expiry Timestamp after which the signature becomes invalid.
     * @return The EIP-712 typed data hash to be signed.
     */
    function calculateOperatorChurnApprovalDigestHash(
        address registeringOperator,
        bytes32 registeringOperatorId,
        OperatorKickParam[] memory operatorKickParams,
        bytes32 salt,
        uint256 expiry
    ) external view returns (bytes32);

    /**
     * @notice Returns the message hash that an operator must sign to register their BLS public key.
     * @param operator The address of the operator registering their key.
     * @return A point on the G1 curve representing the message hash.
     */
    function pubkeyRegistrationMessageHash(
        address operator
    ) external view returns (BN254.G1Point memory);

    /**
     * @notice Returns the avs address for this AVS (used for UAM integration in EigenLayer)
     * @dev NOTE: Updating this value will break existing OperatorSets and UAM integration. This value should only be set once.
     * @return The avs address
     */
    function avs() external view returns (address);
}
````

## File: src/interfaces/ISocketRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.0;

interface ISocketRegistryErrors {
    /// @notice Thrown when the caller is not the SlashingRegistryCoordinator
    error OnlySlashingRegistryCoordinator();
}

interface ISocketRegistry is ISocketRegistryErrors {
    /**
     * @notice Sets the socket for an operator.
     * @param _operatorId The id of the operator to set the socket for.
     * @param _socket The socket (any arbitrary string as deemed useful by an AVS) to set.
     * @dev Only callable by the SlashingRegistryCoordinator.
     */
    function setOperatorSocket(bytes32 _operatorId, string memory _socket) external;

    /**
     * @notice Gets the stored socket for an operator.
     * @param _operatorId The id of the operator to query.
     * @return The stored socket associated with the operator.
     */
    function getOperatorSocket(
        bytes32 _operatorId
    ) external view returns (string memory);
}
````

## File: src/interfaces/IStakeRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";

/// @notice Interface containing all error definitions for the StakeRegistry contract.
interface IStakeRegistryErrors {
    /// @dev Thrown when the caller is not the registry coordinator
    error OnlySlashingRegistryCoordinator();
    /// @dev Thrown when the caller is not the owner of the registry coordinator
    error OnlySlashingRegistryCoordinatorOwner();
    /// @dev Thrown when the stake is below the minimum required for a quorum
    error BelowMinimumStakeRequirement();
    /// @notice Thrown when attempting to create a quorum that already exists.
    error QuorumAlreadyExists();
    /// @notice Thrown when attempting to interact with a quorum that does not exist.
    error QuorumDoesNotExist();
    /// @notice Thrown when two array parameters have mismatching lengths.
    error InputArrayLengthMismatch();
    /// @notice Thrown when an input array has zero length.
    error InputArrayLengthZero();
    /// @notice Thrown when a duplicate strategy is provided in an input array.
    error InputDuplicateStrategy();
    /// @notice Thrown when a multiplier input is zero.
    error InputMultiplierZero();
    /// @notice Thrown when the provided block number is invalid for the stake update.
    error InvalidBlockNumber();
    /// @notice Thrown when attempting to access stake history that doesn't exist for a quorum.
    error EmptyStakeHistory();
    /// @notice Thrown when the quorum is not slashable and the caller attempts to set the look ahead period.
    error QuorumNotSlashable();
}

interface IStakeRegistryTypes {
    /// @notice Defines the type of stake being tracked.
    /// @param TOTAL_DELEGATED Represents the total delegated stake.
    /// @param TOTAL_SLASHABLE Represents the total slashable stake.
    enum StakeType {
        TOTAL_DELEGATED,
        TOTAL_SLASHABLE
    }

    /// @notice Stores stake information for an operator or total stakes at a specific block.
    /// @param updateBlockNumber The block number at which the stake amounts were updated.
    /// @param nextUpdateBlockNumber The block number at which the next update occurred (0 if no next update).
    /// @param stake The stake weight for the quorum.
    struct StakeUpdate {
        uint32 updateBlockNumber;
        uint32 nextUpdateBlockNumber;
        uint96 stake;
    }

    /// @notice Parameters for weighing a particular strategy's stake.
    /// @param strategy The strategy contract address.
    /// @param multiplier The weight multiplier applied to the strategy's stake.
    struct StrategyParams {
        IStrategy strategy;
        uint96 multiplier;
    }
}

interface IStakeRegistryEvents is IStakeRegistryTypes {
    /**
     * @notice Emitted when an operator's stake is updated.
     * @param operatorId The unique identifier of the operator (indexed).
     * @param quorumNumber The quorum number for which the stake was updated.
     * @param stake The new stake amount.
     */
    event OperatorStakeUpdate(bytes32 indexed operatorId, uint8 quorumNumber, uint96 stake);

    /**
     * @notice Emitted when the look ahead period for checking operator shares is updated.
     * @param oldLookAheadBlocks The previous look ahead period.
     * @param newLookAheadBlocks The new look ahead period.
     */
    event LookAheadPeriodChanged(uint32 oldLookAheadBlocks, uint32 newLookAheadBlocks);

    /**
     * @notice Emitted when the stake type is updated.
     * @param newStakeType The new stake type being set.
     */
    event StakeTypeSet(StakeType newStakeType);

    /**
     * @notice Emitted when the minimum stake for a quorum is updated.
     * @param quorumNumber The quorum number being updated (indexed).
     * @param minimumStake The new minimum stake requirement.
     */
    event MinimumStakeForQuorumUpdated(uint8 indexed quorumNumber, uint96 minimumStake);

    /**
     * @notice Emitted when a new quorum is created.
     * @param quorumNumber The number of the newly created quorum (indexed).
     */
    event QuorumCreated(uint8 indexed quorumNumber);

    /**
     * @notice Emitted when a strategy is added to a quorum.
     * @param quorumNumber The quorum number the strategy was added to (indexed).
     * @param strategy The strategy contract that was added.
     */
    event StrategyAddedToQuorum(uint8 indexed quorumNumber, IStrategy strategy);

    /**
     * @notice Emitted when a strategy is removed from a quorum.
     * @param quorumNumber The quorum number the strategy was removed from (indexed).
     * @param strategy The strategy contract that was removed.
     */
    event StrategyRemovedFromQuorum(uint8 indexed quorumNumber, IStrategy strategy);

    /**
     * @notice Emitted when a strategy's multiplier is updated.
     * @param quorumNumber The quorum number for the strategy update (indexed).
     * @param strategy The strategy contract being updated.
     * @param multiplier The new multiplier value.
     */
    event StrategyMultiplierUpdated(
        uint8 indexed quorumNumber, IStrategy strategy, uint256 multiplier
    );
}

interface IStakeRegistry is IStakeRegistryErrors, IStakeRegistryEvents {
    /// STATE

    /**
     * @notice Returns the EigenLayer delegation manager contract.
     */
    function delegation() external view returns (IDelegationManager);

    /// ACTIONS

    /**
     * @notice Registers the `operator` with `operatorId` for the specified `quorumNumbers`.
     * @param operator The address of the operator to register.
     * @param operatorId The id of the operator to register.
     * @param quorumNumbers The quorum numbers the operator is registering for, where each byte is an 8 bit integer quorumNumber.
     * @return operatorStakes The operator's current stake for each quorum.
     * @return totalStakes The total stake for each quorum.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions (these are assumed, not validated in this contract):
     *     1) `quorumNumbers` has no duplicates.
     *     2) `quorumNumbers.length` != 0.
     *     3) `quorumNumbers` is ordered in ascending order.
     *     4) The operator is not already registered.
     */
    function registerOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers
    ) external returns (uint96[] memory operatorStakes, uint96[] memory totalStakes);

    /**
     * @notice Deregisters the operator with `operatorId` for the specified `quorumNumbers`.
     * @param operatorId The id of the operator to deregister.
     * @param quorumNumbers The quorum numbers the operator is deregistering from, where each byte is an 8 bit integer quorumNumber.
     * @dev Access restricted to the RegistryCoordinator.
     * @dev Preconditions (these are assumed, not validated in this contract):
     *     1) `quorumNumbers` has no duplicates.
     *     2) `quorumNumbers.length` != 0.
     *     3) `quorumNumbers` is ordered in ascending order.
     *     4) The operator is not already deregistered.
     *     5) `quorumNumbers` is a subset of the quorumNumbers that the operator is registered for.
     */
    function deregisterOperator(bytes32 operatorId, bytes memory quorumNumbers) external;

    /**
     * @notice Called by the registry coordinator to update the stake of a list of operators for a specific quorum.
     * @param operators The addresses of the operators to update.
     * @param operatorIds The ids of the operators to update.
     * @param quorumNumber The quorum number to update the stake for.
     * @return A list of bools, true if the corresponding operator should be deregistered since they no longer meet the minimum stake requirement.
     */
    function updateOperatorsStake(
        address[] memory operators,
        bytes32[] memory operatorIds,
        uint8 quorumNumber
    ) external returns (bool[] memory);

    /**
     * @notice Initialize a new quorum created by the registry coordinator by setting strategies, weights, and minimum stake.
     * @param quorumNumber The number of the quorum to initialize.
     * @param minimumStake The minimum stake required for the quorum.
     * @param strategyParams The initial strategy parameters for the quorum.
     */
    function initializeDelegatedStakeQuorum(
        uint8 quorumNumber,
        uint96 minimumStake,
        StrategyParams[] memory strategyParams
    ) external;

    /**
     * @notice Initialize a new quorum and push its first history update.
     * @param quorumNumber The number of the quorum to initialize.
     * @param minimumStake The minimum stake required for the quorum.
     * @param lookAheadPeriod The look ahead period for checking operator shares.
     * @param strategyParams The initial strategy parameters for the quorum.
     */
    function initializeSlashableStakeQuorum(
        uint8 quorumNumber,
        uint96 minimumStake,
        uint32 lookAheadPeriod,
        StrategyParams[] memory strategyParams
    ) external;

    /**
     * @notice Sets the minimum stake requirement for a quorum `quorumNumber`.
     * @param quorumNumber The quorum number to set the minimum stake for.
     * @param minimumStake The new minimum stake requirement.
     */
    function setMinimumStakeForQuorum(uint8 quorumNumber, uint96 minimumStake) external;

    /**
     * @notice Sets the look ahead time to `lookAheadBlocks` for checking operator shares for a specific quorum.
     * @param quorumNumber The quorum number to set the look ahead period for.
     * @param lookAheadBlocks The number of blocks to look ahead when checking shares.
     */
    function setSlashableStakeLookahead(uint8 quorumNumber, uint32 lookAheadBlocks) external;

    /**
     * @notice Adds new strategies and their associated multipliers to the specified quorum.
     * @dev Checks to make sure that the *same* strategy cannot be added multiple times (checks against both against existing and new strategies).
     * @dev This function has no check to make sure that the strategies for a single quorum have the same underlying asset. This is a concious choice,
     * since a middleware may want, e.g., a stablecoin quorum that accepts USDC, USDT, DAI, etc. as underlying assets and trades them as "equivalent".
     * @param quorumNumber The quorum number to add strategies to.
     * @param strategyParams The strategy parameters to add.
     */
    function addStrategies(uint8 quorumNumber, StrategyParams[] memory strategyParams) external;

    /**
     * @notice Removes strategies and their associated weights from the specified quorum.
     * @param quorumNumber The quorum number to remove strategies from.
     * @param indicesToRemove The indices of strategies to remove.
     * @dev Higher indices should be *first* in the list of `indicesToRemove`, since otherwise
     *     the removal of lower index entries will cause a shift in the indices of the other strategiesToRemove.
     */
    function removeStrategies(uint8 quorumNumber, uint256[] calldata indicesToRemove) external;

    /**
     * @notice Modifies the weights of strategies that are already in the mapping strategyParams.
     * @param quorumNumber The quorum number to change the strategy for.
     * @param strategyIndices The indices of the strategies to change.
     * @param newMultipliers The new multipliers for the strategies.
     */
    function modifyStrategyParams(
        uint8 quorumNumber,
        uint256[] calldata strategyIndices,
        uint96[] calldata newMultipliers
    ) external;

    /// VIEW

    /**
     * @notice Returns the minimum stake requirement for a quorum `quorumNumber`.
     * @dev In order to register for a quorum i, an operator must have at least `minimumStakeForQuorum[i]`.
     * @param quorumNumber The quorum number to query.
     * @return The minimum stake requirement.
     */
    function minimumStakeForQuorum(
        uint8 quorumNumber
    ) external view returns (uint96);

    /**
     * @notice Returns the length of the dynamic array stored in `strategyParams[quorumNumber]`.
     * @param quorumNumber The quorum number to query.
     * @return The number of strategies for the quorum.
     */
    function strategyParamsLength(
        uint8 quorumNumber
    ) external view returns (uint256);

    /**
     * @notice Returns the strategy and weight multiplier for the `index`'th strategy in the quorum.
     * @param quorumNumber The quorum number to query.
     * @param index The index of the strategy to query.
     * @return The strategy parameters.
     */
    function strategyParamsByIndex(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (StrategyParams memory);

    /**
     * @notice Returns the length of the stake history for an operator in a quorum.
     * @param operatorId The id of the operator to query.
     * @param quorumNumber The quorum number to query.
     * @return The length of the stake history array.
     */
    function getStakeHistoryLength(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (uint256);

    /**
     * @notice Computes the total weight of the operator in the specified quorum.
     * @param quorumNumber The quorum number to query.
     * @param operator The operator address to query.
     * @return The total weight of the operator.
     * @dev Reverts if `quorumNumber` is greater than or equal to `quorumCount`.
     */
    function weightOfOperatorForQuorum(
        uint8 quorumNumber,
        address operator
    ) external view returns (uint96);

    /**
     * @notice Returns the entire stake history array for an operator in a quorum.
     * @param operatorId The id of the operator of interest.
     * @param quorumNumber The quorum number to get the stake for.
     * @return The array of stake updates.
     */
    function getStakeHistory(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (StakeUpdate[] memory);

    /**
     * @notice Returns the length of the total stake history for a quorum.
     * @param quorumNumber The quorum number to query.
     * @return The length of the total stake history array.
     */
    function getTotalStakeHistoryLength(
        uint8 quorumNumber
    ) external view returns (uint256);

    /**
     * @notice Returns the stake update at the specified index in the total stake history.
     * @param quorumNumber The quorum number to query.
     * @param index The index to query.
     * @return The stake update at the specified index.
     */
    function getTotalStakeUpdateAtIndex(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (StakeUpdate memory);

    /**
     * @notice Returns the index of the operator's stake update at the specified block number.
     * @param operatorId The id of the operator to query.
     * @param quorumNumber The quorum number to query.
     * @param blockNumber The block number to query.
     * @return The index of the stake update.
     */
    function getStakeUpdateIndexAtBlockNumber(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint32);

    /**
     * @notice Returns the indices of total stakes for the provided quorums at the given block number.
     * @param blockNumber The block number to query.
     * @param quorumNumbers The quorum numbers to query.
     * @return The array of stake update indices.
     */
    function getTotalStakeIndicesAtBlockNumber(
        uint32 blockNumber,
        bytes calldata quorumNumbers
    ) external view returns (uint32[] memory);

    /**
     * @notice Returns the stake update at the specified index for an operator in a quorum.
     * @param quorumNumber The quorum number to query.
     * @param operatorId The id of the operator to query.
     * @param index The index to query.
     * @return The stake update at the specified index.
     * @dev Function will revert if `index` is out-of-bounds.
     */
    function getStakeUpdateAtIndex(
        uint8 quorumNumber,
        bytes32 operatorId,
        uint256 index
    ) external view returns (StakeUpdate memory);

    /**
     * @notice Returns the most recent stake update for an operator in a quorum.
     * @param operatorId The id of the operator to query.
     * @param quorumNumber The quorum number to query.
     * @return The most recent stake update.
     * @dev Returns a StakeUpdate struct with all entries equal to 0 if the operator has no stake history.
     */
    function getLatestStakeUpdate(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (StakeUpdate memory);

    /**
     * @notice Returns the stake at the specified block number and index for an operator in a quorum.
     * @param quorumNumber The quorum number to query.
     * @param blockNumber The block number to query.
     * @param operatorId The id of the operator to query.
     * @param index The index to query.
     * @return The stake amount.
     * @dev Function will revert if `index` is out-of-bounds.
     * @dev Used by the BLSSignatureChecker to get past stakes of signing operators.
     */
    function getStakeAtBlockNumberAndIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        bytes32 operatorId,
        uint256 index
    ) external view returns (uint96);

    /**
     * @notice Returns the total stake at the specified block number and index for a quorum.
     * @param quorumNumber The quorum number to query.
     * @param blockNumber The block number to query.
     * @param index The index to query.
     * @return The total stake amount.
     * @dev Function will revert if `index` is out-of-bounds.
     * @dev Used by the BLSSignatureChecker to get past stakes of signing operators.
     */
    function getTotalStakeAtBlockNumberFromIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        uint256 index
    ) external view returns (uint96);

    /**
     * @notice Returns the current stake for an operator in a quorum.
     * @param operatorId The id of the operator to query.
     * @param quorumNumber The quorum number to query.
     * @return The current stake amount.
     * @dev Returns 0 if the operator has no stake history.
     */
    function getCurrentStake(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (uint96);

    /**
     * @notice Returns the stake of an operator at a specific block number.
     * @param operatorId The id of the operator to query.
     * @param quorumNumber The quorum number to query.
     * @param blockNumber The block number to query.
     * @return The stake amount at the specified block.
     */
    function getStakeAtBlockNumber(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint96);

    /**
     * @notice Returns the current total stake for a quorum.
     * @param quorumNumber The quorum number to query.
     * @return The current total stake amount.
     * @dev Will revert if `_totalStakeHistory[quorumNumber]` is empty.
     */
    function getCurrentTotalStake(
        uint8 quorumNumber
    ) external view returns (uint96);
}
````

## File: src/interfaces/IVetoableSlasher.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISlasher} from "./ISlasher.sol";

interface IVetoableSlasherErrors {
    /// @notice Thrown when a caller without veto committee privileges attempts a restricted operation
    error OnlyVetoCommittee();
    /// @notice Thrown when attempting to veto a slashing request after the veto period has expired
    error VetoPeriodPassed();
    /// @notice Thrown when attempting to execute a slashing request before the veto period has ended
    error VetoPeriodNotPassed();
    /// @notice Thrown when attempting to interact with a slashing request that has been cancelled
    error SlashingRequestIsCancelled();
    /// @notice Thrown when attempting to modify a slashing request that does not exist
    error SlashingRequestNotRequested();
}

interface IVetoableSlasherTypes {
    /// @notice Represents the status of a slashing request
    enum SlashingStatus {
        Requested,
        Cancelled,
        Completed
    }

    /// @notice Structure containing details about a vetoable slashing request
    struct VetoableSlashingRequest {
        IAllocationManager.SlashingParams params;
        uint256 requestBlock;
        SlashingStatus status;
    }
}

interface IVetoableSlasherEvents {
    /// @notice Emitted when a new slashing request is created
    event SlashingRequested(
        uint256 indexed requestId,
        address indexed operator,
        uint32 operatorSetId,
        uint256[] wadsToSlash,
        string description
    );

    /// @notice Emitted when a slashing request is cancelled by the veto committee
    event SlashingRequestCancelled(uint256 indexed requestId);
}

/// @title IVetoableSlasher
/// @notice A slashing contract that implements a veto mechanism allowing a designated committee to cancel slashing requests
/// @dev Extends base interfaces and adds a veto period during which slashing requests can be cancelled
interface IVetoableSlasher is
    ISlasher,
    IVetoableSlasherErrors,
    IVetoableSlasherTypes,
    IVetoableSlasherEvents
{
    /// @notice Duration of the veto period during which the veto committee can cancel slashing requests
    function vetoWindowBlocks() external view returns (uint32);

    /// @notice Address of the committee that has veto power over slashing requests
    function vetoCommittee() external view returns (address);

    /// @notice Queues a new slashing request
    /// @param params Parameters defining the slashing request including operator and amount
    /// @dev Can only be called by the authorized slasher
    function queueSlashingRequest(
        IAllocationManager.SlashingParams calldata params
    ) external;

    /// @notice Cancels a pending slashing request
    /// @param requestId The ID of the slashing request to cancel
    /// @dev Can only be called by the veto committee during the veto period
    function cancelSlashingRequest(
        uint256 requestId
    ) external;

    /// @notice Executes a slashing request after the veto period has passed
    /// @param requestId The ID of the slashing request to fulfill
    /// @dev Can only be called by the authorized slasher after the veto period
    function fulfillSlashingRequest(
        uint256 requestId
    ) external;
}
````

## File: src/libraries/BitmapUtils.sol
````
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.27;

/**
 * @title Library for Bitmap utilities such as converting between an array of bytes and a bitmap and finding the number of 1s in a bitmap.
 * @author Layr Labs, Inc.
 */
library BitmapUtils {
    /// @dev Thrown when the input byte array is too long to be converted to a bitmap
    error BytesArrayLengthTooLong();
    /// @dev Thrown when the input byte array is not strictly ordered
    error BytesArrayNotOrdered();
    /// @dev Thrown when the bitmap value is too large
    error BitmapValueTooLarge();

    /**
     * @notice Byte arrays are meant to contain unique bytes.
     * If the array length exceeds 256, then it's impossible for all entries to be unique.
     * This constant captures the max allowed array length (inclusive, i.e. 256 is allowed).
     */
    uint256 internal constant MAX_BYTE_ARRAY_LENGTH = 256;

    /**
     * @notice Converts an ordered array of bytes into a bitmap.
     * @param orderedBytesArray The array of bytes to convert/compress into a bitmap. Must be in strictly ascending order.
     * @return The resulting bitmap.
     * @dev Each byte in the input is processed as indicating a single bit to flip in the bitmap.
     * @dev This function will eventually revert in the event that the `orderedBytesArray` is not properly ordered (in ascending order).
     * @dev This function will also revert if the `orderedBytesArray` input contains any duplicate entries (i.e. duplicate bytes).
     */
    function orderedBytesArrayToBitmap(
        bytes memory orderedBytesArray
    ) internal pure returns (uint256) {
        // sanity-check on input. a too-long input would fail later on due to having duplicate entry(s)
        require(orderedBytesArray.length <= MAX_BYTE_ARRAY_LENGTH, BytesArrayLengthTooLong());

        // return empty bitmap early if length of array is 0
        if (orderedBytesArray.length == 0) {
            return uint256(0);
        }

        // initialize the empty bitmap, to be built inside the loop
        uint256 bitmap;
        // initialize an empty uint256 to be used as a bitmask inside the loop
        uint256 bitMask;

        // perform the 0-th loop iteration with the ordering check *omitted* (since it is unnecessary / will always pass)
        // construct a single-bit mask from the numerical value of the 0th byte of the array, and immediately add it to the bitmap
        bitmap = uint256(1 << uint8(orderedBytesArray[0]));

        // loop through each byte in the array to construct the bitmap
        for (uint256 i = 1; i < orderedBytesArray.length; ++i) {
            // construct a single-bit mask from the numerical value of the next byte of the array
            bitMask = uint256(1 << uint8(orderedBytesArray[i]));
            // check strictly ascending array ordering by comparing the mask to the bitmap so far (revert if mask isn't greater than bitmap)
            require(bitMask > bitmap, BytesArrayNotOrdered());
            // add the entry to the bitmap
            bitmap = (bitmap | bitMask);
        }
        return bitmap;
    }

    /**
     * @notice Converts an ordered byte array to a bitmap, validating that all bits are less than `bitUpperBound`
     * @param orderedBytesArray The array to convert to a bitmap; must be in strictly ascending order
     * @param bitUpperBound The exclusive largest bit. Each bit must be strictly less than this value.
     * @dev Reverts if bitmap contains a bit greater than or equal to `bitUpperBound`
     */
    function orderedBytesArrayToBitmap(
        bytes memory orderedBytesArray,
        uint8 bitUpperBound
    ) internal pure returns (uint256) {
        uint256 bitmap = orderedBytesArrayToBitmap(orderedBytesArray);

        require((1 << bitUpperBound) > bitmap, BitmapValueTooLarge());

        return bitmap;
    }

    /**
     * @notice Utility function for checking if a bytes array is strictly ordered, in ascending order.
     * @param bytesArray the bytes array of interest
     * @return Returns 'true' if the array is ordered in strictly ascending order, and 'false' otherwise.
     * @dev This function returns 'true' for the edge case of the `bytesArray` having zero length.
     * It also returns 'false' early for arrays with length in excess of MAX_BYTE_ARRAY_LENGTH (i.e. so long that they cannot be strictly ordered)
     */
    function isArrayStrictlyAscendingOrdered(
        bytes calldata bytesArray
    ) internal pure returns (bool) {
        // Return early if the array is too long, or has a length of 0
        if (bytesArray.length > MAX_BYTE_ARRAY_LENGTH) {
            return false;
        }

        if (bytesArray.length == 0) {
            return true;
        }

        // Perform the 0-th loop iteration by pulling the 0th byte out of the array
        bytes1 singleByte = bytesArray[0];

        // For each byte, validate that each entry is *strictly greater than* the previous
        // If it isn't, return false as the array is not ordered
        for (uint256 i = 1; i < bytesArray.length; ++i) {
            if (uint256(uint8(bytesArray[i])) <= uint256(uint8(singleByte))) {
                return false;
            }

            // Pull the next byte out of the array
            singleByte = bytesArray[i];
        }

        return true;
    }

    /**
     * @notice Converts a bitmap into an array of bytes.
     * @param bitmap The bitmap to decompress/convert to an array of bytes.
     * @return bytesArray The resulting bitmap array of bytes.
     * @dev Each byte in the input is processed as indicating a single bit to flip in the bitmap
     */
    function bitmapToBytesArray(
        uint256 bitmap
    ) internal pure returns (bytes memory /*bytesArray*/ ) {
        // initialize an empty uint256 to be used as a bitmask inside the loop
        uint256 bitMask;
        // allocate only the needed amount of memory
        bytes memory bytesArray = new bytes(countNumOnes(bitmap));
        // track the array index to assign to
        uint256 arrayIndex = 0;
        /**
         * loop through each index in the bitmap to construct the array,
         * but short-circuit the loop if we reach the number of ones and thus are done
         * assigning to memory
         */
        for (uint256 i = 0; (arrayIndex < bytesArray.length) && (i < 256); ++i) {
            // construct a single-bit mask for the i-th bit
            bitMask = uint256(1 << i);
            // check if the i-th bit is flipped in the bitmap
            if (bitmap & bitMask != 0) {
                // if the i-th bit is flipped, then add a byte encoding the value 'i' to the `bytesArray`
                bytesArray[arrayIndex] = bytes1(uint8(i));
                // increment the bytesArray slot since we've assigned one more byte of memory
                unchecked {
                    ++arrayIndex;
                }
            }
        }
        return bytesArray;
    }

    /// @return count number of ones in binary representation of `n`
    function countNumOnes(
        uint256 n
    ) internal pure returns (uint16) {
        uint16 count = 0;
        while (n > 0) {
            n &= (n - 1); // Clear the least significant bit (turn off the rightmost set bit).
            count++; // Increment the count for each cleared bit (each one encountered).
        }
        return count; // Return the total count of ones in the binary representation of n.
    }

    /// @notice Returns `true` if `bit` is in `bitmap`. Returns `false` otherwise.
    function isSet(uint256 bitmap, uint8 bit) internal pure returns (bool) {
        return 1 == ((bitmap >> bit) & 1);
    }

    /**
     * @notice Returns a copy of `bitmap` with `bit` set.
     * @dev IMPORTANT: we're dealing with stack values here, so this doesn't modify
     * the original bitmap. Using this correctly requires an assignment statement:
     * `bitmap = bitmap.setBit(bit);`
     */
    function setBit(uint256 bitmap, uint8 bit) internal pure returns (uint256) {
        return bitmap | (1 << bit);
    }

    /**
     * @notice Returns true if `bitmap` has no set bits
     */
    function isEmpty(
        uint256 bitmap
    ) internal pure returns (bool) {
        return bitmap == 0;
    }

    /**
     * @notice Returns true if `a` and `b` have no common set bits
     */
    function noBitsInCommon(uint256 a, uint256 b) internal pure returns (bool) {
        return a & b == 0;
    }

    /**
     * @notice Returns true if `a` is a subset of `b`: ALL of the bits in `a` are also in `b`
     */
    function isSubsetOf(uint256 a, uint256 b) internal pure returns (bool) {
        return a & b == a;
    }

    /**
     * @notice Returns a new bitmap that contains all bits set in either `a` or `b`
     * @dev Result is the union of `a` and `b`
     */
    function plus(uint256 a, uint256 b) internal pure returns (uint256) {
        return a | b;
    }

    /**
     * @notice Returns a new bitmap that clears all set bits of `b` from `a`
     * @dev Negates `b` and returns the intersection of the result with `a`
     */
    function minus(uint256 a, uint256 b) internal pure returns (uint256) {
        return a & ~b;
    }

    /**
     * @notice Returns a new bitmap that contains only bits set in both `a` and `b`
     * @dev Result is the intersection of `a` and `b`
     */
    function and(uint256 a, uint256 b) internal pure returns (uint256) {
        return a & b;
    }
}
````

## File: src/libraries/BN254.sol
````
// SPDX-License-Identifier: MIT
// several functions are taken or adapted from https://github.com/HarryR/solcrypto/blob/master/contracts/altbn128.sol (MIT license):
// Copyright 2017 Christian Reitwiessner
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to
// deal in the Software without restriction, including without limitation the
// rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
// sell copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
// The above copyright notice and this permission notice shall be included in
// all copies or substantial portions of the Software.
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
// FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
// IN THE SOFTWARE.

// The remainder of the code in this library is written by LayrLabs Inc. and is also under an MIT license

pragma solidity ^0.8.27;

/**
 * @title Library for operations on the BN254 elliptic curve.
 * @author Layr Labs, Inc.
 * @notice Terms of Service: https://docs.eigenlayer.xyz/overview/terms-of-service
 * @notice Contains BN254 parameters, common operations (addition, scalar mul, pairing), and BLS signature functionality.
 */
library BN254 {
    // modulus for the underlying field F_p of the elliptic curve
    uint256 internal constant FP_MODULUS =
        21888242871839275222246405745257275088696311157297823662689037894645226208583;
    // modulus for the underlying field F_r of the elliptic curve
    uint256 internal constant FR_MODULUS =
        21888242871839275222246405745257275088548364400416034343698204186575808495617;

    struct G1Point {
        uint256 X;
        uint256 Y;
    }

    // Encoding of field elements is: X[1] * i + X[0]
    struct G2Point {
        uint256[2] X;
        uint256[2] Y;
    }

    /// @dev Thrown when the sum of two points of G1 fails
    error ECAddFailed();
    /// @dev Thrown when the scalar multiplication of a point of G1 fails
    error ECMulFailed();
    /// @dev Thrown when the scalar is too large.
    error ScalarTooLarge();
    /// @dev Thrown when the pairing check fails
    error ECPairingFailed();
    /// @dev Thrown when the exponentiation mod fails
    error ExpModFailed();

    function generatorG1() internal pure returns (G1Point memory) {
        return G1Point(1, 2);
    }

    // generator of group G2
    /// @dev Generator point in F_q2 is of the form: (x0 + ix1, y0 + iy1).
    uint256 internal constant G2x1 =
        11559732032986387107991004021392285783925812861821192530917403151452391805634;
    uint256 internal constant G2x0 =
        10857046999023057135944570762232829481370756359578518086990519993285655852781;
    uint256 internal constant G2y1 =
        4082367875863433681332203403145435568316851327593401208105741076214120093531;
    uint256 internal constant G2y0 =
        8495653923123431417604973247489272438418190587263600148770280649306958101930;

    /// @notice returns the G2 generator
    /// @dev mind the ordering of the 1s and 0s!
    ///      this is because of the (unknown to us) convention used in the bn254 pairing precompile contract
    ///      "Elements a * i + b of F_p^2 are encoded as two elements of F_p, (a, b)."
    ///      https://github.com/ethereum/EIPs/blob/master/EIPS/eip-197.md#encoding
    function generatorG2() internal pure returns (G2Point memory) {
        return G2Point([G2x1, G2x0], [G2y1, G2y0]);
    }

    // negation of the generator of group G2
    /// @dev Generator point in F_q2 is of the form: (x0 + ix1, y0 + iy1).
    uint256 internal constant nG2x1 =
        11559732032986387107991004021392285783925812861821192530917403151452391805634;
    uint256 internal constant nG2x0 =
        10857046999023057135944570762232829481370756359578518086990519993285655852781;
    uint256 internal constant nG2y1 =
        17805874995975841540914202342111839520379459829704422454583296818431106115052;
    uint256 internal constant nG2y0 =
        13392588948715843804641432497768002650278120570034223513918757245338268106653;

    function negGeneratorG2() internal pure returns (G2Point memory) {
        return G2Point([nG2x1, nG2x0], [nG2y1, nG2y0]);
    }

    bytes32 internal constant powersOfTauMerkleRoot =
        0x22c998e49752bbb1918ba87d6d59dd0e83620a311ba91dd4b2cc84990b31b56f;

    /**
     * @param p Some point in G1.
     * @return The negation of `p`, i.e. p.plus(p.negate()) should be zero.
     */
    function negate(
        G1Point memory p
    ) internal pure returns (G1Point memory) {
        // The prime q in the base field F_q for G1
        if (p.X == 0 && p.Y == 0) {
            return G1Point(0, 0);
        } else {
            return G1Point(p.X, FP_MODULUS - (p.Y % FP_MODULUS));
        }
    }

    /**
     * @return r the sum of two points of G1
     */
    function plus(G1Point memory p1, G1Point memory p2) internal view returns (G1Point memory r) {
        uint256[4] memory input;
        input[0] = p1.X;
        input[1] = p1.Y;
        input[2] = p2.X;
        input[3] = p2.Y;
        bool success;

        // solium-disable-next-line security/no-inline-assembly
        assembly {
            success := staticcall(sub(gas(), 2000), 6, input, 0x80, r, 0x40)
            // Use "invalid" to make gas estimation work
            switch success
            case 0 { invalid() }
        }

        require(success, ECAddFailed());
    }

    /**
     * @notice an optimized ecMul implementation that takes O(log_2(s)) ecAdds
     * @param p the point to multiply
     * @param s the scalar to multiply by
     * @dev this function is only safe to use if the scalar is 9 bits or less
     */
    function scalar_mul_tiny(
        BN254.G1Point memory p,
        uint16 s
    ) internal view returns (BN254.G1Point memory) {
        require(s < 2 ** 9, ScalarTooLarge());

        // if s is 1 return p
        if (s == 1) {
            return p;
        }

        // the accumulated product to return
        BN254.G1Point memory acc = BN254.G1Point(0, 0);
        // the 2^n*p to add to the accumulated product in each iteration
        BN254.G1Point memory p2n = p;
        // value of most significant bit
        uint16 m = 1;
        // index of most significant bit
        uint8 i = 0;

        //loop until we reach the most significant bit
        while (s >= m) {
            unchecked {
                // if the  current bit is 1, add the 2^n*p to the accumulated product
                if ((s >> i) & 1 == 1) {
                    acc = plus(acc, p2n);
                }
                // double the 2^n*p for the next iteration
                p2n = plus(p2n, p2n);

                // increment the index and double the value of the most significant bit
                m <<= 1;
                ++i;
            }
        }

        // return the accumulated product
        return acc;
    }

    /**
     * @return r the product of a point on G1 and a scalar, i.e.
     *         p == p.scalar_mul(1) and p.plus(p) == p.scalar_mul(2) for all
     *         points p.
     */
    function scalar_mul(G1Point memory p, uint256 s) internal view returns (G1Point memory r) {
        uint256[3] memory input;
        input[0] = p.X;
        input[1] = p.Y;
        input[2] = s;
        bool success;
        // solium-disable-next-line security/no-inline-assembly
        assembly {
            success := staticcall(sub(gas(), 2000), 7, input, 0x60, r, 0x40)
            // Use "invalid" to make gas estimation work
            switch success
            case 0 { invalid() }
        }
        require(success, ECMulFailed());
    }

    /**
     *  @return The result of computing the pairing check
     *         e(p1[0], p2[0]) *  .... * e(p1[n], p2[n]) == 1
     *         For example,
     *         pairing([P1(), P1().negate()], [P2(), P2()]) should return true.
     */
    function pairing(
        G1Point memory a1,
        G2Point memory a2,
        G1Point memory b1,
        G2Point memory b2
    ) internal view returns (bool) {
        G1Point[2] memory p1 = [a1, b1];
        G2Point[2] memory p2 = [a2, b2];

        uint256[12] memory input;

        for (uint256 i = 0; i < 2; i++) {
            uint256 j = i * 6;
            input[j + 0] = p1[i].X;
            input[j + 1] = p1[i].Y;
            input[j + 2] = p2[i].X[0];
            input[j + 3] = p2[i].X[1];
            input[j + 4] = p2[i].Y[0];
            input[j + 5] = p2[i].Y[1];
        }

        uint256[1] memory out;
        bool success;

        // solium-disable-next-line security/no-inline-assembly
        assembly {
            success := staticcall(sub(gas(), 2000), 8, input, mul(12, 0x20), out, 0x20)
            // Use "invalid" to make gas estimation work
            switch success
            case 0 { invalid() }
        }

        require(success, ECPairingFailed());

        return out[0] != 0;
    }

    /**
     * @notice This function is functionally the same as pairing(), however it specifies a gas limit
     *         the user can set, as a precompile may use the entire gas budget if it reverts.
     */
    function safePairing(
        G1Point memory a1,
        G2Point memory a2,
        G1Point memory b1,
        G2Point memory b2,
        uint256 pairingGas
    ) internal view returns (bool, bool) {
        G1Point[2] memory p1 = [a1, b1];
        G2Point[2] memory p2 = [a2, b2];

        uint256[12] memory input;

        for (uint256 i = 0; i < 2; i++) {
            uint256 j = i * 6;
            input[j + 0] = p1[i].X;
            input[j + 1] = p1[i].Y;
            input[j + 2] = p2[i].X[0];
            input[j + 3] = p2[i].X[1];
            input[j + 4] = p2[i].Y[0];
            input[j + 5] = p2[i].Y[1];
        }

        uint256[1] memory out;
        bool success;

        // solium-disable-next-line security/no-inline-assembly
        assembly {
            success := staticcall(pairingGas, 8, input, mul(12, 0x20), out, 0x20)
        }

        //Out is the output of the pairing precompile, either 0 or 1 based on whether the two pairings are equal.
        //Success is true if the precompile actually goes through (aka all inputs are valid)

        return (success, out[0] != 0);
    }

    /// @return hashedG1 the keccak256 hash of the G1 Point
    /// @dev used for BLS signatures
    function hashG1Point(
        BN254.G1Point memory pk
    ) internal pure returns (bytes32 hashedG1) {
        assembly {
            mstore(0, mload(pk))
            mstore(0x20, mload(add(0x20, pk)))
            hashedG1 := keccak256(0, 0x40)
        }
    }

    /// @return the keccak256 hash of the G2 Point
    /// @dev used for BLS signatures
    function hashG2Point(
        BN254.G2Point memory pk
    ) internal pure returns (bytes32) {
        return keccak256(abi.encodePacked(pk.X[0], pk.X[1], pk.Y[0], pk.Y[1]));
    }

    /**
     * @notice adapted from https://github.com/HarryR/solcrypto/blob/master/contracts/altbn128.sol
     */
    function hashToG1(
        bytes32 _x
    ) internal view returns (G1Point memory) {
        uint256 beta = 0;
        uint256 y = 0;

        uint256 x = uint256(_x) % FP_MODULUS;

        while (true) {
            (beta, y) = findYFromX(x);

            // y^2 == beta
            if (beta == mulmod(y, y, FP_MODULUS)) {
                return G1Point(x, y);
            }

            x = addmod(x, 1, FP_MODULUS);
        }
        return G1Point(0, 0);
    }

    /**
     * Given X, find Y
     *
     *   where y = sqrt(x^3 + b)
     *
     * Returns: (x^3 + b), y
     */
    function findYFromX(
        uint256 x
    ) internal view returns (uint256, uint256) {
        // beta = (x^3 + b) % p
        uint256 beta = addmod(mulmod(mulmod(x, x, FP_MODULUS), x, FP_MODULUS), 3, FP_MODULUS);

        // y^2 = x^3 + b
        // this acts like: y = sqrt(beta) = beta^((p+1) / 4)
        uint256 y = expMod(
            beta, 0xc19139cb84c680a6e14116da060561765e05aa45a1c72a34f082305b61f3f52, FP_MODULUS
        );

        return (beta, y);
    }

    function expMod(
        uint256 _base,
        uint256 _exponent,
        uint256 _modulus
    ) internal view returns (uint256 retval) {
        bool success;
        uint256[1] memory output;
        uint256[6] memory input;
        input[0] = 0x20; // baseLen = new(big.Int).SetBytes(getData(input, 0, 32))
        input[1] = 0x20; // expLen  = new(big.Int).SetBytes(getData(input, 32, 32))
        input[2] = 0x20; // modLen  = new(big.Int).SetBytes(getData(input, 64, 32))
        input[3] = _base;
        input[4] = _exponent;
        input[5] = _modulus;
        assembly {
            success := staticcall(sub(gas(), 2000), 5, input, 0xc0, output, 0x20)
            // Use "invalid" to make gas estimation work
            switch success
            case 0 { invalid() }
        }
        require(success, ExpModFailed());
        return output[0];
    }
}
````

## File: src/libraries/LibMergeSort.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

library LibMergeSort {
    function sort(
        address[] memory array
    ) internal pure returns (address[] memory) {
        if (array.length <= 1) {
            return array;
        }

        uint256 mid = array.length / 2;
        address[] memory left = new address[](mid);
        address[] memory right = new address[](array.length - mid);

        for (uint256 i = 0; i < mid; i++) {
            left[i] = array[i];
        }
        for (uint256 i = mid; i < array.length; i++) {
            right[i - mid] = array[i];
        }

        return mergeSortArrays(sort(left), sort(right));
    }

    function mergeSortArrays(
        address[] memory left,
        address[] memory right
    ) internal pure returns (address[] memory) {
        uint256 leftLength = left.length;
        uint256 rightLength = right.length;
        address[] memory merged = new address[](leftLength + rightLength);

        uint256 i = 0; // Index for left array
        uint256 j = 0; // Index for right array
        uint256 k = 0; // Index for merged array

        // Merge the two arrays into the merged array
        while (i < leftLength && j < rightLength) {
            if (left[i] < right[j]) {
                merged[k++] = left[i++];
            } else if (left[i] > right[j]) {
                merged[k++] = right[j++];
            } else {
                merged[k++] = left[i++];
                j++;
            }
        }

        // Copy remaining elements of left, if any
        while (i < leftLength) {
            merged[k++] = left[i++];
        }

        // Copy remaining elements of right, if any
        while (j < rightLength) {
            merged[k++] = right[j++];
        }

        // Resize the merged array to remove unused space
        assembly {
            mstore(merged, k)
        }

        return merged;
    }
}
````

## File: src/libraries/QuorumBitmapHistoryLib.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {
    ISlashingRegistryCoordinator,
    ISlashingRegistryCoordinatorTypes
} from "../interfaces/ISlashingRegistryCoordinator.sol";

/// @title QuorumBitmapHistoryLib
/// @notice This library operates on the _operatorBitmapHistory in the RegistryCoordinator
library QuorumBitmapHistoryLib {
    /// @dev Thrown when the quorum bitmap update is not found
    error BitmapUpdateNotFound();
    /// @dev Thrown when quorum bitmap update is after the block number
    error BitmapUpdateIsAfterBlockNumber();
    /// @dev Thrown when the next update block number is not 0 and strictly greater than blockNumber
    error NextBitmapUpdateIsBeforeBlockNumber();

    /// @notice Retrieves the index of the quorum bitmap update at or before the specified block number
    /// @param self The mapping of operator IDs to their quorum bitmap update history
    /// @param blockNumber The block number to search for
    /// @param operatorId The ID of the operator
    /// @return index The index of the quorum bitmap update
    function getQuorumBitmapIndexAtBlockNumber(
        mapping(bytes32 => ISlashingRegistryCoordinator.QuorumBitmapUpdate[]) storage self,
        uint32 blockNumber,
        bytes32 operatorId
    ) internal view returns (uint32 index) {
        uint256 length = self[operatorId].length;

        // Traverse the operator's bitmap history in reverse, returning the first index
        // corresponding to an update made before or at `blockNumber`
        for (uint256 i = 0; i < length; i++) {
            index = uint32(length - i - 1);

            if (self[operatorId][index].updateBlockNumber <= blockNumber) {
                return index;
            }
        }

        revert(
            "RegistryCoordinator.getQuorumBitmapIndexAtBlockNumber: no bitmap update found for operatorId"
        );
    }

    /// @notice Retrieves the current quorum bitmap for the given operator ID
    /// @param self The mapping of operator IDs to their quorum bitmap update history
    /// @param operatorId The ID of the operator
    /// @return The current quorum bitmap
    function currentOperatorBitmap(
        mapping(bytes32 => ISlashingRegistryCoordinator.QuorumBitmapUpdate[]) storage self,
        bytes32 operatorId
    ) external view returns (uint192) {
        uint256 historyLength = self[operatorId].length;
        if (historyLength == 0) {
            return 0;
        } else {
            return self[operatorId][historyLength - 1].quorumBitmap;
        }
    }

    /// @notice Retrieves the indices of the quorum bitmap updates for the given operator IDs at the specified block number
    /// @param self The mapping of operator IDs to their quorum bitmap update history
    /// @param blockNumber The block number to search for
    /// @param operatorIds The array of operator IDs
    /// @return An array of indices corresponding to the quorum bitmap updates
    function getQuorumBitmapIndicesAtBlockNumber(
        mapping(bytes32 => ISlashingRegistryCoordinator.QuorumBitmapUpdate[]) storage self,
        uint32 blockNumber,
        bytes32[] memory operatorIds
    ) external view returns (uint32[] memory) {
        uint32[] memory indices = new uint32[](operatorIds.length);
        for (uint256 i = 0; i < operatorIds.length; i++) {
            indices[i] = getQuorumBitmapIndexAtBlockNumber(self, blockNumber, operatorIds[i]);
        }
        return indices;
    }

    /// @notice Retrieves the quorum bitmap for the given operator ID at the specified block number and index
    /// @param self The mapping of operator IDs to their quorum bitmap update history
    /// @param operatorId The ID of the operator
    /// @param blockNumber The block number to validate against
    /// @param index The index of the quorum bitmap update
    /// @return The quorum bitmap at the specified index
    function getQuorumBitmapAtBlockNumberByIndex(
        mapping(bytes32 => ISlashingRegistryCoordinator.QuorumBitmapUpdate[]) storage self,
        bytes32 operatorId,
        uint32 blockNumber,
        uint256 index
    ) external view returns (uint192) {
        ISlashingRegistryCoordinator.QuorumBitmapUpdate memory quorumBitmapUpdate =
            self[operatorId][index];

        /**
         * Validate that the update is valid for the given blockNumber:
         * - blockNumber should be >= the update block number
         * - the next update block number should be either 0 or strictly greater than blockNumber
         */
        require(
            blockNumber >= quorumBitmapUpdate.updateBlockNumber, BitmapUpdateIsAfterBlockNumber()
        );
        require(
            quorumBitmapUpdate.nextUpdateBlockNumber == 0
                || blockNumber < quorumBitmapUpdate.nextUpdateBlockNumber,
            NextBitmapUpdateIsBeforeBlockNumber()
        );

        return quorumBitmapUpdate.quorumBitmap;
    }

    /// @notice Updates the quorum bitmap for the given operator ID
    /// @param self The mapping of operator IDs to their quorum bitmap update history
    /// @param operatorId The ID of the operator
    /// @param newBitmap The new quorum bitmap to set
    function updateOperatorBitmap(
        mapping(bytes32 => ISlashingRegistryCoordinator.QuorumBitmapUpdate[]) storage self,
        bytes32 operatorId,
        uint192 newBitmap
    ) external {
        uint256 historyLength = self[operatorId].length;

        if (historyLength == 0) {
            // No prior bitmap history - push our first entry
            self[operatorId].push(
                ISlashingRegistryCoordinatorTypes.QuorumBitmapUpdate({
                    updateBlockNumber: uint32(block.number),
                    nextUpdateBlockNumber: 0,
                    quorumBitmap: newBitmap
                })
            );
        } else {
            // We have prior history - fetch our last-recorded update
            ISlashingRegistryCoordinator.QuorumBitmapUpdate storage lastUpdate =
                self[operatorId][historyLength - 1];

            /**
             * If the last update was made in the current block, update the entry.
             * Otherwise, push a new entry and update the previous entry's "next" field
             */
            if (lastUpdate.updateBlockNumber == uint32(block.number)) {
                lastUpdate.quorumBitmap = newBitmap;
            } else {
                lastUpdate.nextUpdateBlockNumber = uint32(block.number);
                self[operatorId].push(
                    ISlashingRegistryCoordinatorTypes.QuorumBitmapUpdate({
                        updateBlockNumber: uint32(block.number),
                        nextUpdateBlockNumber: 0,
                        quorumBitmap: newBitmap
                    })
                );
            }
        }
    }
}
````

## File: src/libraries/SignatureCheckerLib.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import "@openzeppelin-upgrades/contracts/utils/cryptography/SignatureCheckerUpgradeable.sol";

/**
 * @title SignatureCheckerLib
 * @dev This library wraps the EIP1271SignatureUtils library to provide an external function for signature validation.
 * This approach helps in reducing the code size of the RegistryCoordinator contract by offloading the signature
 * validation logic to this external library.
 */
library SignatureCheckerLib {
    error InvalidSignature();

    /**
     * @notice Validates a signature using EIP-1271 standard.
     * @param signer The address of the signer.
     * @param digestHash The hash of the data that was signed.
     * @param signature The signature to be validated.
     */
    function isValidSignature(
        address signer,
        bytes32 digestHash,
        bytes memory signature
    ) external view {
        if (!SignatureCheckerUpgradeable.isValidSignatureNow(signer, digestHash, signature)) {
            revert InvalidSignature();
        }
    }
}
````

## File: src/slashers/base/SlasherBase.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {SlasherStorage, ISlashingRegistryCoordinator} from "./SlasherStorage.sol";
import {
    IAllocationManagerTypes,
    IAllocationManager
} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";

/// @title SlasherBase
/// @notice Base contract for implementing slashing functionality in EigenLayer middleware
/// @dev Provides core slashing functionality and interfaces with EigenLayer's AllocationManager
abstract contract SlasherBase is SlasherStorage {
    /// @notice Ensures only the authorized slasher can call certain functions
    modifier onlySlasher() {
        _checkSlasher(msg.sender);
        _;
    }

    /// @notice Constructs the base slasher contract
    /// @param _allocationManager The EigenLayer allocation manager contract
    /// @param _registryCoordinator The registry coordinator for this middleware
    /// @param _slasher The address of the slasher
    constructor(
        IAllocationManager _allocationManager,
        ISlashingRegistryCoordinator _registryCoordinator,
        address _slasher
    ) SlasherStorage(_allocationManager, _registryCoordinator, _slasher) {}

    /// @notice Internal function to execute a slashing request
    /// @param _requestId The ID of the slashing request to fulfill
    /// @param _params Parameters defining the slashing request including operator, strategies, and amounts
    /// @dev Calls AllocationManager.slashOperator to perform the actual slashing
    function _fulfillSlashingRequest(
        uint256 _requestId,
        IAllocationManager.SlashingParams memory _params
    ) internal virtual {
        allocationManager.slashOperator({avs: slashingRegistryCoordinator.avs(), params: _params});
        emit OperatorSlashed(
            _requestId,
            _params.operator,
            _params.operatorSetId,
            _params.wadsToSlash,
            _params.description
        );
    }

    /// @notice Internal function to verify if an account is the authorized slasher
    /// @param account The address to check
    /// @dev Reverts if the account is not the authorized slasher
    function _checkSlasher(
        address account
    ) internal view virtual {
        require(account == slasher, OnlySlasher());
    }
}
````

## File: src/slashers/base/SlasherStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISlashingRegistryCoordinator} from "../../interfaces/ISlashingRegistryCoordinator.sol";
import {ISlasher} from "../../interfaces/ISlasher.sol";

/// @title SlasherStorage
/// @notice Base storage contract for slashing functionality
/// @dev Provides storage variables and events for slashing operations
abstract contract SlasherStorage is ISlasher {
    /**
     *
     *                            CONSTANTS AND IMMUTABLES
     *
     */

    /// @notice the AllocationManager that tracks OperatorSets and Slashing in EigenLayer
    IAllocationManager public immutable allocationManager;
    /// @notice the SlashingRegistryCoordinator for this AVS
    ISlashingRegistryCoordinator public immutable slashingRegistryCoordinator;
    /// @notice the address of the slasher
    address public immutable slasher;

    uint256 public nextRequestId;

    constructor(
        IAllocationManager _allocationManager,
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        address _slasher
    ) {
        allocationManager = _allocationManager;
        slashingRegistryCoordinator = _slashingRegistryCoordinator;
        slasher = _slasher;
    }

    uint256[49] private __gap;
}
````

## File: src/slashers/InstantSlasher.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {SlasherBase} from "./base/SlasherBase.sol";
import {ISlashingRegistryCoordinator} from "../interfaces/ISlashingRegistryCoordinator.sol";
import {IInstantSlasher} from "../interfaces/IInstantSlasher.sol";

/// @title InstantSlasher
/// @notice A slashing contract that immediately executes slashing requests without any delay or veto period
/// @dev Extends SlasherBase to provide access controlled slashing functionality
contract InstantSlasher is IInstantSlasher, SlasherBase {
    constructor(
        IAllocationManager _allocationManager,
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        address _slasher
    ) SlasherBase(_allocationManager, _slashingRegistryCoordinator, _slasher) {}

    /// @inheritdoc IInstantSlasher
    function fulfillSlashingRequest(
        IAllocationManager.SlashingParams calldata _slashingParams
    ) external virtual override(IInstantSlasher) onlySlasher {
        uint256 requestId = nextRequestId++;
        _fulfillSlashingRequest(requestId, _slashingParams);

        address[] memory operators = new address[](1);
        operators[0] = _slashingParams.operator;
        slashingRegistryCoordinator.updateOperators(operators);
    }
}
````

## File: src/slashers/VetoableSlasher.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {SlasherBase} from "./base/SlasherBase.sol";
import {ISlashingRegistryCoordinator} from "../interfaces/ISlashingRegistryCoordinator.sol";
import {IVetoableSlasher, IVetoableSlasherTypes} from "../interfaces/IVetoableSlasher.sol";

/// @title VetoableSlasher
/// @notice A slashing contract that implements a veto mechanism allowing a designated committee to cancel slashing requests
/// @dev Extends SlasherBase and adds a veto period during which slashing requests can be cancelled
contract VetoableSlasher is IVetoableSlasher, SlasherBase {
    /// @inheritdoc IVetoableSlasher
    uint32 public immutable override vetoWindowBlocks;

    /// @inheritdoc IVetoableSlasher
    address public immutable override vetoCommittee;

    /// @notice Mapping of request IDs to their corresponding slashing request details
    mapping(uint256 => IVetoableSlasherTypes.VetoableSlashingRequest) public slashingRequests;

    /// @notice Modifier to restrict function access to only the veto committee
    modifier onlyVetoCommittee() {
        _checkVetoCommittee(msg.sender);
        _;
    }

    constructor(
        IAllocationManager _allocationManager,
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        address _slasher,
        address _vetoCommittee,
        uint32 _vetoWindowBlocks
    ) SlasherBase(_allocationManager, _slashingRegistryCoordinator, _slasher) {
        vetoWindowBlocks = _vetoWindowBlocks;
        vetoCommittee = _vetoCommittee;
    }

    /// @inheritdoc IVetoableSlasher
    function queueSlashingRequest(
        IAllocationManager.SlashingParams calldata params
    ) external virtual override onlySlasher {
        _queueSlashingRequest(params);
    }

    /// @inheritdoc IVetoableSlasher
    function cancelSlashingRequest(
        uint256 requestId
    ) external virtual override onlyVetoCommittee {
        _cancelSlashingRequest(requestId);
    }

    /// @inheritdoc IVetoableSlasher
    function fulfillSlashingRequest(
        uint256 requestId
    ) external virtual override onlySlasher {
        _fulfillSlashingRequestAndMarkAsCompleted(requestId);
    }

    /// @notice Internal function to create and store a new slashing request
    /// @param params Parameters defining the slashing request
    function _queueSlashingRequest(
        IAllocationManager.SlashingParams memory params
    ) internal virtual {
        uint256 requestId = nextRequestId++;
        slashingRequests[requestId] = IVetoableSlasherTypes.VetoableSlashingRequest({
            params: params,
            requestBlock: block.number,
            status: IVetoableSlasherTypes.SlashingStatus.Requested
        });

        emit SlashingRequested(
            requestId, params.operator, params.operatorSetId, params.wadsToSlash, params.description
        );
    }

    /// @notice Internal function to mark a slashing request as cancelled
    /// @param requestId The ID of the slashing request to cancel
    function _cancelSlashingRequest(
        uint256 requestId
    ) internal virtual {
        require(
            block.number < slashingRequests[requestId].requestBlock + vetoWindowBlocks,
            VetoPeriodPassed()
        );
        require(
            slashingRequests[requestId].status == IVetoableSlasherTypes.SlashingStatus.Requested,
            SlashingRequestNotRequested()
        );

        slashingRequests[requestId].status = IVetoableSlasherTypes.SlashingStatus.Cancelled;
        emit SlashingRequestCancelled(requestId);
    }

    /// @notice Internal function to fulfill a slashing request and mark it as completed
    /// @param requestId The ID of the slashing request to fulfill
    function _fulfillSlashingRequestAndMarkAsCompleted(
        uint256 requestId
    ) internal virtual {
        IVetoableSlasherTypes.VetoableSlashingRequest storage request = slashingRequests[requestId];
        require(block.number >= request.requestBlock + vetoWindowBlocks, VetoPeriodNotPassed());
        require(
            request.status == IVetoableSlasherTypes.SlashingStatus.Requested,
            SlashingRequestIsCancelled()
        );

        request.status = IVetoableSlasherTypes.SlashingStatus.Completed;

        _fulfillSlashingRequest(requestId, request.params);

        address[] memory operators = new address[](1);
        operators[0] = request.params.operator;
        slashingRegistryCoordinator.updateOperators(operators);
    }

    /// @notice Internal function to verify if an account is the veto committee
    /// @param account The address to check
    /// @dev Reverts if the account is not the veto committee
    function _checkVetoCommittee(
        address account
    ) internal view virtual {
        require(account == vetoCommittee, OnlyVetoCommittee());
    }
}
````

## File: src/unaudited/examples/ECDSAStakeRegistryEqualWeight.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {ECDSAStakeRegistryPermissioned} from "./ECDSAStakeRegistryPermissioned.sol";
import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {CheckpointsUpgradeable} from
    "@openzeppelin-upgrades/contracts/utils/CheckpointsUpgradeable.sol";

/// @title ECDSA Stake Registry with Equal Weight
/// @dev THIS CONTRACT IS NOT AUDITED
/// @notice A contract to manage operator stakes with equal weighting for operators
contract ECDSAStakeRegistryEqualWeight is ECDSAStakeRegistryPermissioned {
    using CheckpointsUpgradeable for CheckpointsUpgradeable.History;

    /// @notice Initializes the contract with a specified delegation manager.
    /// @dev Passes the delegation manager to the parent constructor.
    /// @param _delegationManager The address of the delegation manager contract.
    constructor(
        IDelegationManager _delegationManager
    ) ECDSAStakeRegistryPermissioned(_delegationManager) {
        // _disableInitializers();
    }

    /// @notice Updates the weight of an operator in the stake registry.
    /// @dev Overrides the _updateOperatorWeight function from the parent class to implement equal weighting.
    ///      Emits an OperatorWeightUpdated event upon successful update.
    /// @param _operator The address of the operator whose weight is being updated.
    function _updateOperatorWeight(
        address _operator
    ) internal override returns (int256) {
        uint256 oldWeight;
        uint256 newWeight;
        int256 delta;
        if (_operatorRegistered[_operator]) {
            (oldWeight,) = _operatorWeightHistory[_operator].push(1);
            delta = int256(1) - int256(oldWeight); // handles if they were already registered
        } else {
            (oldWeight,) = _operatorWeightHistory[_operator].push(0);
            delta = int256(0) - int256(oldWeight);
        }
        emit OperatorWeightUpdated(_operator, oldWeight, newWeight);
        return delta;
    }
}
````

## File: src/unaudited/examples/ECDSAStakeRegistryPermissioned.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {ECDSAStakeRegistry} from "../ECDSAStakeRegistry.sol";
import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";

/// @title ECDSA Stake Registry with an Operator Allowlist
/// @dev THIS CONTRACT IS NOT AUDITED
/// @notice This contract extends ECDSAStakeRegistry by adding functionality to allowlist and remove operators
contract ECDSAStakeRegistryPermissioned is ECDSAStakeRegistry {
    /// @notice A mapping to keep track of whether an operator can register with this AVS or not.
    mapping(address => bool) public allowlistedOperators;

    /// @dev Emits when an operator is added to the allowlist.
    event OperatorPermitted(address indexed operator);

    /// @dev Emits when an operator is removed from the allowlist.
    event OperatorRevoked(address indexed operator);

    /// @dev Emits when an operator is removed from the active operator set.
    event OperatorEjected(address indexed operator);

    /// @dev Custom error to signal that an operator is not allowlisted.
    error OperatorNotAllowlisted();

    /// @dev Custom error to signal that an operator is already allowlisted.
    error OperatorAlreadyAllowlisted();

    constructor(
        IDelegationManager _delegationManager
    ) ECDSAStakeRegistry(_delegationManager) {
        // _disableInitializers();
    }

    /// @notice Adds an operator to the allowlisted operator set
    /// @dev An allowlisted operator isn't a part of the operator set. They must subsequently register themselves
    /// @param _operator The address of the operator to be allowlisted
    function permitOperator(
        address _operator
    ) external onlyOwner {
        _permitOperator(_operator);
    }

    /// @notice Revokes an operator's permission and deregisters them
    /// @dev Emits the OperatorRevoked event if the operator was previously allowlisted.
    /// @param _operator The address of the operator to remove from the allowlist and deregistered.
    function revokeOperator(
        address _operator
    ) external onlyOwner {
        _revokeOperator(_operator);
    }

    /// @notice Directly deregisters an operator without removing from the allowlist
    /// @dev Does not emit an event because it does not modify the allowlist.
    /// @param _operator The address of the operator to deregister
    function ejectOperator(
        address _operator
    ) external onlyOwner {
        _ejectOperator(_operator);
    }

    /// @dev Deregisters and operator from the active operator set
    /// @param _operator The address of the operator to remove.
    function _ejectOperator(
        address _operator
    ) internal {
        _deregisterOperator(_operator);
        emit OperatorEjected(_operator);
    }

    /// @dev Adds an operator to the allowlisted operator set
    /// Doesn't register the operator into the operator set
    /// @param _operator The address of the operator to allowlist.
    function _permitOperator(
        address _operator
    ) internal {
        if (allowlistedOperators[_operator]) {
            revert OperatorAlreadyAllowlisted();
        }
        allowlistedOperators[_operator] = true;
        emit OperatorPermitted(_operator);
    }

    /// @dev Removes an operator from the allowlist.
    /// If the operator is registered, also deregisters the operator.
    /// @param _operator The address of the operator to be revoked.
    function _revokeOperator(
        address _operator
    ) internal {
        if (!allowlistedOperators[_operator]) {
            revert OperatorNotAllowlisted();
        }
        delete allowlistedOperators[_operator];
        emit OperatorRevoked(_operator);
        if (_operatorRegistered[_operator]) {
            _ejectOperator(_operator);
        }
    }

    /// @inheritdoc ECDSAStakeRegistry
    function _registerOperatorWithSig(
        address _operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory _operatorSignature,
        address _operatorSigningKey
    ) internal override {
        if (allowlistedOperators[_operator] != true) {
            revert OperatorNotAllowlisted();
        }
        super._registerOperatorWithSig(_operator, _operatorSignature, _operatorSigningKey);
    }
}
````

## File: src/unaudited/ECDSAServiceManagerBase.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {IServiceManager} from "../interfaces/IServiceManager.sol";
import {IServiceManagerUI} from "../interfaces/IServiceManagerUI.sol";
import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IStakeRegistry} from "../interfaces/IStakeRegistry.sol";
import {IRewardsCoordinator} from
    "eigenlayer-contracts/src/contracts/interfaces/IRewardsCoordinator.sol";
import {IECDSAStakeRegistryTypes} from "../interfaces/IECDSAStakeRegistry.sol";
import {ECDSAStakeRegistry} from "../unaudited/ECDSAStakeRegistry.sol";
import {IAVSRegistrar} from "eigenlayer-contracts/src/contracts/interfaces/IAVSRegistrar.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";

abstract contract ECDSAServiceManagerBase is IServiceManager, OwnableUpgradeable {
    using SafeERC20 for IERC20;

    /// @notice Address of the stake registry contract, which manages registration and stake recording.
    address public immutable stakeRegistry;

    /// @notice Address of the AVS directory contract, which manages AVS-related data for registered operators.
    address public immutable avsDirectory;

    /// @notice Address of the AllocationManager contract
    address public immutable allocationManager;

    /// @notice Address of the rewards coordinator contract, which handles rewards distributions.
    address internal immutable rewardsCoordinator;

    /// @notice Address of the delegation manager contract, which manages staker delegations to operators.
    address internal immutable delegationManager;

    /// @notice Address of the rewards initiator, which is allowed to create AVS rewards submissions.
    address public rewardsInitiator;

    /**
     * @dev Ensures that the function is only callable by the `stakeRegistry` contract.
     * This is used to restrict certain registration and deregistration functionality to the `stakeRegistry`
     */
    modifier onlyStakeRegistry() {
        require(msg.sender == stakeRegistry, OnlyStakeRegistry());
        _;
    }

    /**
     * @dev Ensures that the function is only callable by the `rewardsInitiator`.
     */
    modifier onlyRewardsInitiator() {
        _checkRewardsInitiator();
        _;
    }

    function _checkRewardsInitiator() internal view {
        require(msg.sender == rewardsInitiator, OnlyRewardsInitiator());
    }

    /**
     * @dev Constructor for ECDSAServiceManagerBase, initializing immutable contract addresses and disabling initializers.
     * @param _avsDirectory The address of the AVS directory contract, managing AVS-related data for registered operators.
     * @param _stakeRegistry The address of the stake registry contract, managing registration and stake recording.
     * @param _rewardsCoordinator The address of the rewards coordinator contract, handling rewards distributions.
     * @param _delegationManager The address of the delegation manager contract, managing staker delegations to operators.
     */
    constructor(
        address _avsDirectory,
        address _stakeRegistry,
        address _rewardsCoordinator,
        address _delegationManager,
        address _allocationManager
    ) {
        avsDirectory = _avsDirectory;
        stakeRegistry = _stakeRegistry;
        rewardsCoordinator = _rewardsCoordinator;
        delegationManager = _delegationManager;
        allocationManager = _allocationManager;
        _disableInitializers();
    }

    /**
     * @dev Initializes the base service manager by transferring ownership to the initial owner.
     * @param initialOwner The address to which the ownership of the contract will be transferred.
     * @param _rewardsInitiator The address which is allowed to create AVS rewards submissions.
     */
    function __ServiceManagerBase_init(
        address initialOwner,
        address _rewardsInitiator
    ) internal virtual onlyInitializing {
        _transferOwnership(initialOwner);
        _setRewardsInitiator(_rewardsInitiator);
    }

    /// @inheritdoc IServiceManagerUI
    function updateAVSMetadataURI(
        string memory _metadataURI
    ) external virtual onlyOwner {
        _updateAVSMetadataURI(_metadataURI);
    }

    /// @inheritdoc IServiceManager
    function createAVSRewardsSubmission(
        IRewardsCoordinator.RewardsSubmission[] calldata rewardsSubmissions
    ) external virtual onlyRewardsInitiator {
        _createAVSRewardsSubmission(rewardsSubmissions);
    }

    function createOperatorDirectedAVSRewardsSubmission(
        IRewardsCoordinator.OperatorDirectedRewardsSubmission[] calldata
            operatorDirectedRewardsSubmissions
    ) external virtual onlyRewardsInitiator {
        _createOperatorDirectedAVSRewardsSubmission(operatorDirectedRewardsSubmissions);
    }

    function setClaimerFor(
        address claimer
    ) external virtual onlyOwner {
        _setClaimerFor(claimer);
    }

    /// @inheritdoc IServiceManagerUI
    function registerOperatorToAVS(
        address operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) external virtual onlyStakeRegistry {
        _registerOperatorToAVS(operator, operatorSignature);
    }

    /// @inheritdoc IServiceManagerUI
    function deregisterOperatorFromAVS(
        address operator
    ) external virtual onlyStakeRegistry {
        _deregisterOperatorFromAVS(operator);
    }

    /// @inheritdoc IServiceManagerUI
    function getRestakeableStrategies() external view virtual returns (address[] memory) {
        return _getRestakeableStrategies();
    }

    /// @inheritdoc IServiceManagerUI
    function getOperatorRestakedStrategies(
        address _operator
    ) external view virtual returns (address[] memory) {
        return _getOperatorRestakedStrategies(_operator);
    }

    /**
     * @notice Forwards the call to update AVS metadata URI in the AVSDirectory contract.
     * @dev This internal function is a proxy to the `updateAVSMetadataURI` function of the AVSDirectory contract.
     * @param _metadataURI The new metadata URI to be set.
     */
    function _updateAVSMetadataURI(
        string memory _metadataURI
    ) internal virtual {
        IAVSDirectory(avsDirectory).updateAVSMetadataURI(_metadataURI);
    }

    /**
     * @notice Forwards the call to register an operator in the AVSDirectory contract.
     * @dev This internal function is a proxy to the `registerOperatorToAVS` function of the AVSDirectory contract.
     * @param operator The address of the operator to register.
     * @param operatorSignature The signature, salt, and expiry details of the operator's registration.
     */
    function _registerOperatorToAVS(
        address operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) internal virtual {
        IAVSDirectory(avsDirectory).registerOperatorToAVS(operator, operatorSignature);
    }

    /**
     * @notice Forwards the call to deregister an operator from the AVSDirectory contract.
     * @dev This internal function is a proxy to the `deregisterOperatorFromAVS` function of the AVSDirectory contract.
     * @param operator The address of the operator to deregister.
     */
    function _deregisterOperatorFromAVS(
        address operator
    ) internal virtual {
        IAVSDirectory(avsDirectory).deregisterOperatorFromAVS(operator);
    }

    /**
     * @notice Processes a batch of rewards submissions by transferring the specified amounts from the sender to this contract and then approving the RewardsCoordinator to use these amounts.
     * @dev This function handles the transfer and approval of tokens necessary for rewards submissions. It then delegates the actual rewards logic to the RewardsCoordinator contract.
     * @param rewardsSubmissions An array of `RewardsSubmission` structs, each representing rewards for a specific range.
     */
    function _createAVSRewardsSubmission(
        IRewardsCoordinator.RewardsSubmission[] calldata rewardsSubmissions
    ) internal virtual {
        for (uint256 i = 0; i < rewardsSubmissions.length; ++i) {
            rewardsSubmissions[i].token.safeTransferFrom(
                msg.sender, address(this), rewardsSubmissions[i].amount
            );
            rewardsSubmissions[i].token.safeIncreaseAllowance(
                rewardsCoordinator, rewardsSubmissions[i].amount
            );
        }

        IRewardsCoordinator(rewardsCoordinator).createAVSRewardsSubmission(rewardsSubmissions);
    }

    /**
     * @notice Creates a new operator-directed rewards submission, to be split amongst the operators and
     * set of stakers delegated to operators who are registered to this `avs`.
     * @param operatorDirectedRewardsSubmissions The operator-directed rewards submissions being created.
     */
    function _createOperatorDirectedAVSRewardsSubmission(
        IRewardsCoordinator.OperatorDirectedRewardsSubmission[] calldata
            operatorDirectedRewardsSubmissions
    ) internal virtual {
        for (uint256 i = 0; i < operatorDirectedRewardsSubmissions.length; ++i) {
            // Calculate total amount of token to transfer
            uint256 totalAmount = 0;
            for (
                uint256 j = 0; j < operatorDirectedRewardsSubmissions[i].operatorRewards.length; ++j
            ) {
                totalAmount += operatorDirectedRewardsSubmissions[i].operatorRewards[j].amount;
            }

            // Transfer token to ServiceManager and approve RewardsCoordinator to transfer again
            // in createOperatorDirectedAVSRewardsSubmission() call
            operatorDirectedRewardsSubmissions[i].token.safeTransferFrom(
                msg.sender, address(this), totalAmount
            );
            operatorDirectedRewardsSubmissions[i].token.safeIncreaseAllowance(
                rewardsCoordinator, totalAmount
            );
        }

        IRewardsCoordinator(rewardsCoordinator).createOperatorDirectedAVSRewardsSubmission(
            address(this), operatorDirectedRewardsSubmissions
        );
    }

    /**
     * @notice Forwards a call to Eigenlayer's RewardsCoordinator contract to set the address of the entity that can call `processClaim` on behalf of this contract.
     * @param claimer The address of the entity that can call `processClaim` on behalf of the earner.
     */
    function _setClaimerFor(
        address claimer
    ) internal virtual {
        IRewardsCoordinator(rewardsCoordinator).setClaimerFor(claimer);
    }

    /**
     * @notice Retrieves the addresses of all strategies that are part of the current quorum.
     * @dev Fetches the quorum configuration from the ECDSAStakeRegistry and extracts the strategy addresses.
     * @return strategies An array of addresses representing the strategies in the current quorum.
     */
    function _getRestakeableStrategies() internal view virtual returns (address[] memory) {
        IECDSAStakeRegistryTypes.Quorum memory quorum = ECDSAStakeRegistry(stakeRegistry).quorum();
        address[] memory strategies = new address[](quorum.strategies.length);
        for (uint256 i = 0; i < quorum.strategies.length; i++) {
            strategies[i] = address(quorum.strategies[i].strategy);
        }
        return strategies;
    }

    /**
     * @notice Sets the AVS registrar address in the AllocationManager
     * @param registrar The new AVS registrar address
     * @dev Only callable by the registry coordinator
     */
    function setAVSRegistrar(
        IAVSRegistrar registrar
    ) external onlyOwner {
        IAllocationManager(allocationManager).setAVSRegistrar(address(this), registrar);
    }

    /**
     * @notice Retrieves the addresses of strategies where the operator has restaked.
     * @dev This function fetches the quorum details from the ECDSAStakeRegistry, retrieves the operator's shares for each strategy,
     * and filters out strategies with non-zero shares indicating active restaking by the operator.
     * @param _operator The address of the operator whose restaked strategies are to be retrieved.
     * @return restakedStrategies An array of addresses of strategies where the operator has active restakes.
     */
    function _getOperatorRestakedStrategies(
        address _operator
    ) internal view virtual returns (address[] memory) {
        IECDSAStakeRegistryTypes.Quorum memory quorum = ECDSAStakeRegistry(stakeRegistry).quorum();
        uint256 count = quorum.strategies.length;
        IStrategy[] memory strategies = new IStrategy[](count);
        for (uint256 i; i < count; i++) {
            strategies[i] = quorum.strategies[i].strategy;
        }
        uint256[] memory shares =
            IDelegationManager(delegationManager).getOperatorShares(_operator, strategies);

        uint256 activeCount;
        for (uint256 i; i < count; i++) {
            if (shares[i] > 0) {
                activeCount++;
            }
        }

        // Resize the array to fit only the active strategies
        address[] memory restakedStrategies = new address[](activeCount);
        uint256 index;
        for (uint256 j = 0; j < count; j++) {
            if (shares[j] > 0) {
                restakedStrategies[index] = address(strategies[j]);
                index++;
            }
        }

        return restakedStrategies;
    }

    /**
     * @notice Sets the rewards initiator address.
     * @param newRewardsInitiator The new rewards initiator address.
     * @dev Only callable by the owner.
     */
    function setRewardsInitiator(
        address newRewardsInitiator
    ) external onlyOwner {
        _setRewardsInitiator(newRewardsInitiator);
    }

    function _setRewardsInitiator(
        address newRewardsInitiator
    ) internal {
        emit RewardsInitiatorUpdated(rewardsInitiator, newRewardsInitiator);
        rewardsInitiator = newRewardsInitiator;
    }

    // storage gap for upgradeability
    // slither-disable-next-line shadowing-state
    uint256[49] private __GAP;
}
````

## File: src/unaudited/ECDSAStakeRegistry.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {
    IECDSAStakeRegistry,
    ECDSAStakeRegistryStorage,
    IECDSAStakeRegistryTypes
} from "./ECDSAStakeRegistryStorage.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IServiceManager} from "../interfaces/IServiceManager.sol";

import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";
import {CheckpointsUpgradeable} from
    "@openzeppelin-upgrades/contracts/utils/CheckpointsUpgradeable.sol";
import {SignatureCheckerUpgradeable} from
    "@openzeppelin-upgrades/contracts/utils/cryptography/SignatureCheckerUpgradeable.sol";
import {IERC1271Upgradeable} from
    "@openzeppelin-upgrades/contracts/interfaces/IERC1271Upgradeable.sol";

/// @title ECDSA Stake Registry
/// @dev THIS CONTRACT IS NOT AUDITED
/// @notice Manages operator registration and quorum updates for an AVS using ECDSA signatures.
contract ECDSAStakeRegistry is
    IERC1271Upgradeable,
    OwnableUpgradeable,
    ECDSAStakeRegistryStorage
{
    using SignatureCheckerUpgradeable for address;
    using CheckpointsUpgradeable for CheckpointsUpgradeable.History;

    /// @dev Constructor to create ECDSAStakeRegistry.
    /// @param _delegationManager Address of the DelegationManager contract that this registry interacts with.
    constructor(
        IDelegationManager _delegationManager
    ) ECDSAStakeRegistryStorage(_delegationManager) {
        // _disableInitializers();
    }

    /// @notice Initializes the contract with the given parameters.
    /// @param _serviceManager The address of the service manager.
    /// @param thresholdWeight The threshold weight in basis points.
    /// @param quorum The quorum struct containing the details of the quorum thresholds.
    function initialize(
        address _serviceManager,
        uint256 thresholdWeight,
        IECDSAStakeRegistryTypes.Quorum memory quorum
    ) external initializer {
        __ECDSAStakeRegistry_init(_serviceManager, thresholdWeight, quorum);
    }

    /// @notice Initializes state for the StakeRegistry
    /// @param _serviceManagerAddr The AVS' ServiceManager contract's address
    function __ECDSAStakeRegistry_init(
        address _serviceManagerAddr,
        uint256 thresholdWeight,
        IECDSAStakeRegistryTypes.Quorum memory quorum
    ) internal onlyInitializing {
        _serviceManager = _serviceManagerAddr;
        _updateStakeThreshold(thresholdWeight);
        _updateQuorumConfig(quorum);
        __Ownable_init();
    }

    /// @inheritdoc IECDSAStakeRegistry
    function registerOperatorWithSignature(
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature,
        address signingKey
    ) external {
        _registerOperatorWithSig(msg.sender, operatorSignature, signingKey);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function deregisterOperator() external {
        _deregisterOperator(msg.sender);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateOperatorSigningKey(
        address newSigningKey
    ) external {
        if (!_operatorRegistered[msg.sender]) {
            revert OperatorNotRegistered();
        }
        _updateOperatorSigningKey(msg.sender, newSigningKey);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateOperators(
        address[] memory operators
    ) external {
        _updateOperators(operators);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateQuorumConfig(
        IECDSAStakeRegistryTypes.Quorum memory quorum,
        address[] memory operators
    ) external onlyOwner {
        _updateQuorumConfig(quorum);
        _updateOperators(operators);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateMinimumWeight(
        uint256 newMinimumWeight,
        address[] memory operators
    ) external onlyOwner {
        _updateMinimumWeight(newMinimumWeight);
        _updateOperators(operators);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateStakeThreshold(
        uint256 thresholdWeight
    ) external onlyOwner {
        _updateStakeThreshold(thresholdWeight);
    }

    function isValidSignature(
        bytes32 digest,
        bytes memory _signatureData
    ) external view returns (bytes4) {
        (address[] memory operators, bytes[] memory signatures, uint32 referenceBlock) =
            abi.decode(_signatureData, (address[], bytes[], uint32));
        _checkSignatures(digest, operators, signatures, referenceBlock);
        return IERC1271Upgradeable.isValidSignature.selector;
    }

    /// @inheritdoc IECDSAStakeRegistry
    function quorum() external view returns (IECDSAStakeRegistryTypes.Quorum memory) {
        return _quorum;
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLatestOperatorSigningKey(
        address operator
    ) external view returns (address) {
        return address(uint160(_operatorSigningKeyHistory[operator].latest()));
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getOperatorSigningKeyAtBlock(
        address operator,
        uint256 blockNumber
    ) external view returns (address) {
        return address(uint160(_operatorSigningKeyHistory[operator].getAtBlock(blockNumber)));
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLastCheckpointOperatorWeight(
        address operator
    ) external view returns (uint256) {
        return _operatorWeightHistory[operator].latest();
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLastCheckpointTotalWeight() external view returns (uint256) {
        return _totalWeightHistory.latest();
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLastCheckpointThresholdWeight() external view returns (uint256) {
        return _thresholdWeightHistory.latest();
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getOperatorWeightAtBlock(
        address operator,
        uint32 blockNumber
    ) external view returns (uint256) {
        return _operatorWeightHistory[operator].getAtBlock(blockNumber);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLastCheckpointTotalWeightAtBlock(
        uint32 blockNumber
    ) external view returns (uint256) {
        return _totalWeightHistory.getAtBlock(blockNumber);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getLastCheckpointThresholdWeightAtBlock(
        uint32 blockNumber
    ) external view returns (uint256) {
        return _thresholdWeightHistory.getAtBlock(blockNumber);
    }

    /// @inheritdoc IECDSAStakeRegistry
    function operatorRegistered(
        address operator
    ) external view returns (bool) {
        return _operatorRegistered[operator];
    }

    /// @inheritdoc IECDSAStakeRegistry
    function minimumWeight() external view returns (uint256) {
        return _minimumWeight;
    }

    /// @inheritdoc IECDSAStakeRegistry
    function getOperatorWeight(
        address operator
    ) public view returns (uint256) {
        StrategyParams[] memory strategyParams = _quorum.strategies;
        uint256 weight;
        IStrategy[] memory strategies = new IStrategy[](strategyParams.length);
        for (uint256 i; i < strategyParams.length; i++) {
            strategies[i] = strategyParams[i].strategy;
        }
        uint256[] memory shares = DELEGATION_MANAGER.getOperatorShares(operator, strategies);
        for (uint256 i; i < strategyParams.length; i++) {
            weight += shares[i] * strategyParams[i].multiplier;
        }
        weight = weight / BPS;

        if (weight >= _minimumWeight) {
            return weight;
        } else {
            return 0;
        }
    }

    /// @inheritdoc IECDSAStakeRegistry
    function updateOperatorsForQuorum(
        address[][] memory operatorsPerQuorum,
        bytes memory
    ) external {
        _updateAllOperators(operatorsPerQuorum[0]);
    }

    /// @dev Updates the list of operators if the provided list has the correct number of operators.
    /// Reverts if the provided list of operators does not match the expected total count of operators.
    /// @param operators The list of operator addresses to update.
    function _updateAllOperators(
        address[] memory operators
    ) internal {
        if (operators.length != _totalOperators) {
            revert MustUpdateAllOperators();
        }
        _updateOperators(operators);
    }

    /// @dev Updates the weights for a given list of operator addresses.
    /// When passing an operator that isn't registered, then 0 is added to their history
    /// @param operators An array of addresses for which to update the weights.
    function _updateOperators(
        address[] memory operators
    ) internal {
        int256 delta;
        for (uint256 i; i < operators.length; i++) {
            delta += _updateOperatorWeight(operators[i]);
        }
        _updateTotalWeight(delta);
    }

    /// @dev Updates the stake threshold weight and records the history.
    /// @param thresholdWeight The new threshold weight to set and record in the history.
    function _updateStakeThreshold(
        uint256 thresholdWeight
    ) internal {
        _thresholdWeightHistory.push(thresholdWeight);
        emit ThresholdWeightUpdated(thresholdWeight);
    }

    /// @dev Updates the weight an operator must have to join the operator set
    /// @param newMinimumWeight The new weight an operator must have to join the operator set
    function _updateMinimumWeight(
        uint256 newMinimumWeight
    ) internal {
        uint256 oldMinimumWeight = _minimumWeight;
        _minimumWeight = newMinimumWeight;
        emit MinimumWeightUpdated(oldMinimumWeight, newMinimumWeight);
    }

    /// @notice Updates the quorum configuration
    /// @dev Replaces the current quorum configuration with `newQuorum` if valid.
    /// Reverts with `InvalidQuorum` if the new quorum configuration is not valid.
    /// Emits `QuorumUpdated` event with the old and new quorum configurations.
    /// @param newQuorum The new quorum configuration to set.
    function _updateQuorumConfig(
        IECDSAStakeRegistryTypes.Quorum memory newQuorum
    ) internal {
        if (!_isValidQuorum(newQuorum)) {
            revert InvalidQuorum();
        }
        IECDSAStakeRegistryTypes.Quorum memory oldQuorum = _quorum;
        delete _quorum;
        for (uint256 i; i < newQuorum.strategies.length; i++) {
            _quorum.strategies.push(newQuorum.strategies[i]);
        }
        emit QuorumUpdated(oldQuorum, newQuorum);
    }

    /// @dev Internal function to deregister an operator
    /// @param operator The operator's address to deregister
    function _deregisterOperator(
        address operator
    ) internal {
        if (!_operatorRegistered[operator]) {
            revert OperatorNotRegistered();
        }
        _totalOperators--;
        delete _operatorRegistered[operator];
        int256 delta = _updateOperatorWeight(operator);
        _updateTotalWeight(delta);
        IServiceManager(_serviceManager).deregisterOperatorFromAVS(operator);
        emit OperatorDeregistered(operator, address(_serviceManager));
    }

    /// @dev registers an operator through a provided signature
    /// @param operatorSignature Contains the operator's signature, salt, and expiry
    /// @param signingKey The signing key to add to the operator's history
    function _registerOperatorWithSig(
        address operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature,
        address signingKey
    ) internal virtual {
        if (_operatorRegistered[operator]) {
            revert OperatorAlreadyRegistered();
        }
        _totalOperators++;
        _operatorRegistered[operator] = true;
        int256 delta = _updateOperatorWeight(operator);
        _updateTotalWeight(delta);
        _updateOperatorSigningKey(operator, signingKey);
        IServiceManager(_serviceManager).registerOperatorToAVS(operator, operatorSignature);
        emit OperatorRegistered(operator, _serviceManager);
    }

    /// @dev Internal function to update an operator's signing key
    /// @param operator The address of the operator to update the signing key for
    /// @param newSigningKey The new signing key to set for the operator
    function _updateOperatorSigningKey(address operator, address newSigningKey) internal {
        address oldSigningKey = address(uint160(_operatorSigningKeyHistory[operator].latest()));
        if (newSigningKey == oldSigningKey) {
            return;
        }
        _operatorSigningKeyHistory[operator].push(uint160(newSigningKey));
        emit SigningKeyUpdate(operator, block.number, newSigningKey, oldSigningKey);
    }

    /// @notice Updates the weight of an operator and returns the previous and current weights.
    /// @param operator The address of the operator to update the weight of.
    function _updateOperatorWeight(
        address operator
    ) internal virtual returns (int256) {
        int256 delta;
        uint256 newWeight;
        uint256 oldWeight = _operatorWeightHistory[operator].latest();
        if (!_operatorRegistered[operator]) {
            delta -= int256(oldWeight);
            if (delta == 0) {
                return delta;
            }
            _operatorWeightHistory[operator].push(0);
        } else {
            newWeight = getOperatorWeight(operator);
            delta = int256(newWeight) - int256(oldWeight);
            if (delta == 0) {
                return delta;
            }
            _operatorWeightHistory[operator].push(newWeight);
        }
        emit OperatorWeightUpdated(operator, oldWeight, newWeight);
        return delta;
    }

    /// @dev Internal function to update the total weight of the stake
    /// @param delta The change in stake applied last total weight
    /// @return oldTotalWeight The weight before the update
    /// @return newTotalWeight The updated weight after applying the delta
    function _updateTotalWeight(
        int256 delta
    ) internal returns (uint256 oldTotalWeight, uint256 newTotalWeight) {
        oldTotalWeight = _totalWeightHistory.latest();
        int256 newWeight = int256(oldTotalWeight) + delta;
        newTotalWeight = uint256(newWeight);
        _totalWeightHistory.push(newTotalWeight);
        emit TotalWeightUpdated(oldTotalWeight, newTotalWeight);
    }

    /**
     * @dev Verifies that a specified quorum configuration is valid. A valid quorum has:
     *      1. Weights that sum to exactly 10,000 basis points, ensuring proportional representation.
     *      2. Unique strategies without duplicates to maintain quorum integrity.
     * @param quorum The quorum configuration to be validated.
     * @return bool True if the quorum configuration is valid, otherwise false.
     */
    function _isValidQuorum(
        IECDSAStakeRegistryTypes.Quorum memory quorum
    ) internal pure returns (bool) {
        StrategyParams[] memory strategies = quorum.strategies;
        address lastStrategy;
        address currentStrategy;
        uint256 totalMultiplier;
        for (uint256 i; i < strategies.length; i++) {
            currentStrategy = address(strategies[i].strategy);
            if (lastStrategy >= currentStrategy) revert NotSorted();
            lastStrategy = currentStrategy;
            totalMultiplier += strategies[i].multiplier;
        }
        if (totalMultiplier != BPS) {
            return false;
        } else {
            return true;
        }
    }

    /**
     * @notice Common logic to verify a batch of ECDSA signatures against a hash, using either last stake weight or at a specific block.
     * @param digest The hash of the data the signers endorsed.
     * @param operators A collection of addresses that endorsed the data hash.
     * @param signatures A collection of signatures matching the signers.
     * @param referenceBlock The block number for evaluating stake weight; use max uint32 for latest weight.
     */
    function _checkSignatures(
        bytes32 digest,
        address[] memory operators,
        bytes[] memory signatures,
        uint32 referenceBlock
    ) internal view {
        uint256 signersLength = operators.length;
        address currentOperator;
        address lastOperator;
        address signer;
        uint256 signedWeight;

        _validateSignaturesLength(signersLength, signatures.length);
        for (uint256 i; i < signersLength; i++) {
            currentOperator = operators[i];
            signer = _getOperatorSigningKey(currentOperator, referenceBlock);

            _validateSortedSigners(lastOperator, currentOperator);
            _validateSignature(signer, digest, signatures[i]);

            lastOperator = currentOperator;
            uint256 operatorWeight = _getOperatorWeight(currentOperator, referenceBlock);
            signedWeight += operatorWeight;
        }

        _validateThresholdStake(signedWeight, referenceBlock);
    }

    /// @notice Validates that the number of signers equals the number of signatures, and neither is zero.
    /// @param signersLength The number of signers.
    /// @param signaturesLength The number of signatures.
    function _validateSignaturesLength(
        uint256 signersLength,
        uint256 signaturesLength
    ) internal pure {
        if (signersLength != signaturesLength) {
            revert LengthMismatch();
        }
        if (signersLength == 0) {
            revert InvalidLength();
        }
    }

    /// @notice Ensures that signers are sorted in ascending order by address.
    /// @param lastSigner The address of the last signer.
    /// @param currentSigner The address of the current signer.
    function _validateSortedSigners(address lastSigner, address currentSigner) internal pure {
        if (lastSigner >= currentSigner) {
            revert NotSorted();
        }
    }

    /// @notice Validates a given signature against the signer's address and data hash.
    /// @param signer The address of the signer to validate.
    /// @param digest The hash of the data that is signed.
    /// @param signature The signature to validate.
    function _validateSignature(
        address signer,
        bytes32 digest,
        bytes memory signature
    ) internal view {
        if (!signer.isValidSignatureNow(digest, signature)) {
            revert InvalidSignature();
        }
    }

    /// @notice Retrieves the operator weight for a signer, either at the last checkpoint or a specified block.
    /// @param operator The operator to query their signing key history for
    /// @param referenceBlock The block number to query the operator's weight at, or the maximum uint32 value for the last checkpoint.
    /// @return The weight of the operator.
    function _getOperatorSigningKey(
        address operator,
        uint32 referenceBlock
    ) internal view returns (address) {
        if (referenceBlock >= block.number) {
            revert InvalidReferenceBlock();
        }
        return address(uint160(_operatorSigningKeyHistory[operator].getAtBlock(referenceBlock)));
    }

    /// @notice Retrieves the operator weight for a signer, either at the last checkpoint or a specified block.
    /// @param signer The address of the signer whose weight is returned.
    /// @param referenceBlock The block number to query the operator's weight at, or the maximum uint32 value for the last checkpoint.
    /// @return The weight of the operator.
    function _getOperatorWeight(
        address signer,
        uint32 referenceBlock
    ) internal view returns (uint256) {
        if (referenceBlock >= block.number) {
            revert InvalidReferenceBlock();
        }
        return _operatorWeightHistory[signer].getAtBlock(referenceBlock);
    }

    /// @notice Retrieve the total stake weight at a specific block or the latest if not specified.
    /// @dev If the `referenceBlock` is the maximum value for uint32, the latest total weight is returned.
    /// @param referenceBlock The block number to retrieve the total stake weight from.
    /// @return The total stake weight at the given block or the latest if the given block is the max uint32 value.
    function _getTotalWeight(
        uint32 referenceBlock
    ) internal view returns (uint256) {
        if (referenceBlock >= block.number) {
            revert InvalidReferenceBlock();
        }
        return _totalWeightHistory.getAtBlock(referenceBlock);
    }

    /// @notice Retrieves the threshold stake for a given reference block.
    /// @param referenceBlock The block number to query the threshold stake for.
    /// If set to the maximum uint32 value, it retrieves the latest threshold stake.
    /// @return The threshold stake in basis points for the reference block.
    function _getThresholdStake(
        uint32 referenceBlock
    ) internal view returns (uint256) {
        if (referenceBlock >= block.number) {
            revert InvalidReferenceBlock();
        }
        return _thresholdWeightHistory.getAtBlock(referenceBlock);
    }

    /// @notice Validates that the cumulative stake of signed messages meets or exceeds the required threshold.
    /// @param signedWeight The cumulative weight of the signers that have signed the message.
    /// @param referenceBlock The block number to verify the stake threshold for
    function _validateThresholdStake(uint256 signedWeight, uint32 referenceBlock) internal view {
        uint256 totalWeight = _getTotalWeight(referenceBlock);
        if (signedWeight > totalWeight) {
            revert InvalidSignedWeight();
        }
        uint256 thresholdStake = _getThresholdStake(referenceBlock);
        if (thresholdStake > signedWeight) {
            revert InsufficientSignedStake();
        }
    }
}
````

## File: src/unaudited/ECDSAStakeRegistryStorage.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {CheckpointsUpgradeable} from
    "@openzeppelin-upgrades/contracts/utils/CheckpointsUpgradeable.sol";
import {
    IECDSAStakeRegistry, IECDSAStakeRegistryTypes
} from "../interfaces/IECDSAStakeRegistry.sol";

abstract contract ECDSAStakeRegistryStorage is IECDSAStakeRegistry {
    /// @notice Manages staking delegations through the DelegationManager interface
    IDelegationManager internal immutable DELEGATION_MANAGER;

    /// @dev The total amount of multipliers to weigh stakes
    uint256 internal constant BPS = 10000;

    /// @notice The size of the current operator set
    uint256 internal _totalOperators;

    /// @notice Stores the current quorum configuration
    IECDSAStakeRegistryTypes.Quorum internal _quorum;

    /// @notice Specifies the weight required to become an operator
    uint256 internal _minimumWeight;

    /// @notice Holds the address of the service manager
    address internal _serviceManager;

    /// @notice Defines the duration after which the stake's weight expires.
    uint256 internal _stakeExpiry;

    /// @notice Maps an operator to their signing key history using checkpoints
    mapping(address => CheckpointsUpgradeable.History) internal _operatorSigningKeyHistory;

    /// @notice Tracks the total stake history over time using checkpoints
    CheckpointsUpgradeable.History internal _totalWeightHistory;

    /// @notice Tracks the threshold bps history using checkpoints
    CheckpointsUpgradeable.History internal _thresholdWeightHistory;

    /// @notice Maps operator addresses to their respective stake histories using checkpoints
    mapping(address => CheckpointsUpgradeable.History) internal _operatorWeightHistory;

    /// @notice Maps an operator to their registration status
    mapping(address => bool) internal _operatorRegistered;

    /// @param _delegationManager Connects this registry with the DelegationManager
    constructor(
        IDelegationManager _delegationManager
    ) {
        DELEGATION_MANAGER = _delegationManager;
    }

    // slither-disable-next-line shadowing-state
    /// @dev Reserves storage slots for future upgrades
    // solhint-disable-next-line
    uint256[40] private __gap;
}
````

## File: src/unaudited/README.md
````markdown
> **Contracts in this directory aren't audited and should not be used in production without undergoing an independent security audit**

## Purpose

The code provided in this directory serves as examples for concepts that might be useful for developers exploring the ways to implement Stake Registries for their AVS
````

## File: src/BLSApkRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {BLSApkRegistryStorage, IBLSApkRegistry} from "./BLSApkRegistryStorage.sol";

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";

import {BN254} from "./libraries/BN254.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

contract BLSApkRegistry is BLSApkRegistryStorage {
    using BN254 for BN254.G1Point;

    /// @notice when applied to a function, only allows the RegistryCoordinator to call it
    modifier onlyRegistryCoordinator() {
        _checkRegistryCoordinator();
        _;
    }

    /// @notice when applied to a function, only allows the RegistryCoordinator owner to call it
    modifier onlyRegistryCoordinatorOwner() {
        _checkRegistryCoordinatorOwner();
        _;
    }

    /// @notice Sets the (immutable) `registryCoordinator` address
    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator
    ) BLSApkRegistryStorage(_slashingRegistryCoordinator) {}

    /**
     *
     *                   EXTERNAL FUNCTIONS - REGISTRY COORDINATOR
     *
     */

    /// @inheritdoc IBLSApkRegistry
    function registerOperator(
        address operator,
        bytes memory quorumNumbers
    ) public virtual onlyRegistryCoordinator {
        // Get the operator's pubkey. Reverts if they have not registered a key
        (BN254.G1Point memory pubkey,) = getRegisteredPubkey(operator);

        // Update each quorum's aggregate pubkey
        _processQuorumApkUpdate(quorumNumbers, pubkey);

        // Return pubkeyHash, which will become the operator's unique id
        emit OperatorAddedToQuorums(operator, getOperatorId(operator), quorumNumbers);
    }

    /// @inheritdoc IBLSApkRegistry
    function deregisterOperator(
        address operator,
        bytes memory quorumNumbers
    ) public virtual onlyRegistryCoordinator {
        // Get the operator's pubkey. Reverts if they have not registered a key
        (BN254.G1Point memory pubkey,) = getRegisteredPubkey(operator);

        // Update each quorum's aggregate pubkey
        _processQuorumApkUpdate(quorumNumbers, pubkey.negate());
        emit OperatorRemovedFromQuorums(operator, getOperatorId(operator), quorumNumbers);
    }

    /// @inheritdoc IBLSApkRegistry
    function initializeQuorum(
        uint8 quorumNumber
    ) public virtual onlyRegistryCoordinator {
        require(apkHistory[quorumNumber].length == 0, QuorumAlreadyExists());

        apkHistory[quorumNumber].push(
            ApkUpdate({
                apkHash: bytes24(0),
                updateBlockNumber: uint32(block.number),
                nextUpdateBlockNumber: 0
            })
        );
    }

    /// @inheritdoc IBLSApkRegistry
    function registerBLSPublicKey(
        address operator,
        PubkeyRegistrationParams calldata params,
        BN254.G1Point calldata pubkeyRegistrationMessageHash
    ) public onlyRegistryCoordinator returns (bytes32 operatorId) {
        bytes32 pubkeyHash = BN254.hashG1Point(params.pubkeyG1);
        require(pubkeyHash != ZERO_PK_HASH, ZeroPubKey());
        require(getOperatorId(operator) == bytes32(0), OperatorAlreadyRegistered());
        require(pubkeyHashToOperator[pubkeyHash] == address(0), BLSPubkeyAlreadyRegistered());

        // gamma = h(sigma, P, P', H(m))
        uint256 gamma = uint256(
            keccak256(
                abi.encodePacked(
                    params.pubkeyRegistrationSignature.X,
                    params.pubkeyRegistrationSignature.Y,
                    params.pubkeyG1.X,
                    params.pubkeyG1.Y,
                    params.pubkeyG2.X,
                    params.pubkeyG2.Y,
                    pubkeyRegistrationMessageHash.X,
                    pubkeyRegistrationMessageHash.Y
                )
            )
        ) % BN254.FR_MODULUS;

        // e(sigma + P * gamma, [-1]_2) = e(H(m) + [1]_1 * gamma, P')
        require(
            BN254.pairing(
                params.pubkeyRegistrationSignature.plus(params.pubkeyG1.scalar_mul(gamma)),
                BN254.negGeneratorG2(),
                pubkeyRegistrationMessageHash.plus(BN254.generatorG1().scalar_mul(gamma)),
                params.pubkeyG2
            ),
            InvalidBLSSignatureOrPrivateKey()
        );

        operatorToPubkey[operator] = params.pubkeyG1;
        operatorToPubkeyG2[operator] = params.pubkeyG2;
        operatorToPubkeyHash[operator] = pubkeyHash;
        pubkeyHashToOperator[pubkeyHash] = operator;

        emit NewPubkeyRegistration(operator, params.pubkeyG1, params.pubkeyG2);
        return pubkeyHash;
    }

    /// @inheritdoc IBLSApkRegistry
    function getOrRegisterOperatorId(
        address operator,
        PubkeyRegistrationParams calldata params,
        BN254.G1Point calldata pubkeyRegistrationMessageHash
    ) external onlyRegistryCoordinator returns (bytes32 operatorId) {
        operatorId = getOperatorId(operator);
        if (operatorId == 0) {
            operatorId = registerBLSPublicKey(operator, params, pubkeyRegistrationMessageHash);
        }
        return operatorId;
    }

    /// @notice Verifies and registers a G2 public key for an operator that already has a G1 key
    /// @dev This is meant to be used as a one-time way to add G2 public keys for operators that have G1 keys but no G2 key on chain
    /// @param operator The address of the operator to register the G2 key for
    /// @param pubkeyG2 The G2 public key to register
    function verifyAndRegisterG2PubkeyForOperator(
        address operator,
        BN254.G2Point calldata pubkeyG2
    ) external onlyRegistryCoordinatorOwner {
        // Get the operator's G1 pubkey. Reverts if they have not registered a key
        (BN254.G1Point memory pubkeyG1,) = getRegisteredPubkey(operator);

        _checkG2PubkeyNotSet(operator);

        require(
            BN254.pairing(pubkeyG1, BN254.negGeneratorG2(), BN254.generatorG1(), pubkeyG2),
            InvalidBLSSignatureOrPrivateKey()
        );

        operatorToPubkeyG2[operator] = pubkeyG2;

        emit NewG2PubkeyRegistration(operator, pubkeyG2);
    }

    /**
     *
     *                         INTERNAL FUNCTIONS
     *
     */
    function _processQuorumApkUpdate(
        bytes memory quorumNumbers,
        BN254.G1Point memory point
    ) internal {
        BN254.G1Point memory newApk;

        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            // Validate quorum exists and get history length
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            uint256 historyLength = apkHistory[quorumNumber].length;
            require(historyLength != 0, QuorumDoesNotExist());

            // Update aggregate public key for this quorum
            newApk = currentApk[quorumNumber].plus(point);
            currentApk[quorumNumber] = newApk;
            bytes24 newApkHash = bytes24(BN254.hashG1Point(newApk));

            // Update apk history. If the last update was made in this block, update the entry
            // Otherwise, push a new historical entry and update the prev->next pointer
            ApkUpdate storage lastUpdate = apkHistory[quorumNumber][historyLength - 1];
            if (lastUpdate.updateBlockNumber == uint32(block.number)) {
                lastUpdate.apkHash = newApkHash;
            } else {
                lastUpdate.nextUpdateBlockNumber = uint32(block.number);
                apkHistory[quorumNumber].push(
                    ApkUpdate({
                        apkHash: newApkHash,
                        updateBlockNumber: uint32(block.number),
                        nextUpdateBlockNumber: 0
                    })
                );
            }
        }
    }

    /**
     *
     *                         VIEW FUNCTIONS
     *
     */

    /// @inheritdoc IBLSApkRegistry
    function getRegisteredPubkey(
        address operator
    ) public view returns (BN254.G1Point memory, bytes32) {
        BN254.G1Point memory pubkey = operatorToPubkey[operator];
        bytes32 pubkeyHash = getOperatorId(operator);

        require(pubkeyHash != bytes32(0), OperatorNotRegistered());

        return (pubkey, pubkeyHash);
    }

    /// @inheritdoc IBLSApkRegistry
    function getApkIndicesAtBlockNumber(
        bytes calldata quorumNumbers,
        uint256 blockNumber
    ) external view returns (uint32[] memory) {
        uint32[] memory indices = new uint32[](quorumNumbers.length);

        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);

            uint256 quorumApkUpdatesLength = apkHistory[quorumNumber].length;
            if (
                quorumApkUpdatesLength == 0
                    || blockNumber < apkHistory[quorumNumber][0].updateBlockNumber
            ) {
                revert BlockNumberBeforeFirstUpdate();
            }

            // Loop backward through apkHistory until we find an entry that precedes `blockNumber`
            for (uint256 j = quorumApkUpdatesLength; j > 0; j--) {
                if (apkHistory[quorumNumber][j - 1].updateBlockNumber <= blockNumber) {
                    indices[i] = uint32(j - 1);
                    break;
                }
            }
        }
        return indices;
    }

    /// @inheritdoc IBLSApkRegistry
    function getApk(
        uint8 quorumNumber
    ) external view returns (BN254.G1Point memory) {
        return currentApk[quorumNumber];
    }

    /// @inheritdoc IBLSApkRegistry
    function getApkUpdateAtIndex(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (ApkUpdate memory) {
        return apkHistory[quorumNumber][index];
    }

    /// @inheritdoc IBLSApkRegistry
    function getApkHashAtBlockNumberAndIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        uint256 index
    ) external view returns (bytes24) {
        ApkUpdate memory quorumApkUpdate = apkHistory[quorumNumber][index];

        /**
         * Validate that the update is valid for the given blockNumber:
         * - blockNumber should be >= the update block number
         * - the next update block number should be either 0 or strictly greater than blockNumber
         */
        require(blockNumber >= quorumApkUpdate.updateBlockNumber, BlockNumberTooRecent());
        require(
            quorumApkUpdate.nextUpdateBlockNumber == 0
                || blockNumber < quorumApkUpdate.nextUpdateBlockNumber,
            BlockNumberNotLatest()
        );

        return quorumApkUpdate.apkHash;
    }

    /// @inheritdoc IBLSApkRegistry
    function getApkHistoryLength(
        uint8 quorumNumber
    ) external view returns (uint32) {
        return uint32(apkHistory[quorumNumber].length);
    }

    /// @inheritdoc IBLSApkRegistry
    function getOperatorFromPubkeyHash(
        bytes32 pubkeyHash
    ) public view returns (address) {
        return pubkeyHashToOperator[pubkeyHash];
    }

    /// @inheritdoc IBLSApkRegistry
    function getOperatorId(
        address operator
    ) public view returns (bytes32) {
        return operatorToPubkeyHash[operator];
    }

    /// @inheritdoc IBLSApkRegistry
    function getOperatorPubkeyG2(
        address operator
    ) public view override returns (BN254.G2Point memory) {
        return operatorToPubkeyG2[operator];
    }

    function _checkRegistryCoordinator() internal view {
        require(msg.sender == address(registryCoordinator), OnlyRegistryCoordinatorOwner());
    }

    function _checkRegistryCoordinatorOwner() internal view {
        require(
            msg.sender == Ownable(address(registryCoordinator)).owner(),
            OnlyRegistryCoordinatorOwner()
        );
    }

    /// @notice Checks if a G2 pubkey is already set for an operator
    function _checkG2PubkeyNotSet(
        address operator
    ) internal view {
        BN254.G2Point memory existingG2Pubkey = getOperatorPubkeyG2(operator);
        require(
            existingG2Pubkey.X[0] == 0 && existingG2Pubkey.X[1] == 0 && existingG2Pubkey.Y[0] == 0
                && existingG2Pubkey.Y[1] == 0,
            G2PubkeyAlreadySet()
        );
    }
}
````

## File: src/BLSApkRegistryStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IBLSApkRegistry, IBLSApkRegistryTypes} from "./interfaces/IBLSApkRegistry.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";

import {Initializable} from "@openzeppelin-upgrades/contracts/proxy/utils/Initializable.sol";

import {BN254} from "./libraries/BN254.sol";

abstract contract BLSApkRegistryStorage is Initializable, IBLSApkRegistry {
    /// @dev Returns the hash of the zero pubkey aka BN254.G1Point(0,0)
    bytes32 internal constant ZERO_PK_HASH =
        hex"ad3228b676f7d3cd4284a5443f17f1962b36e491b30a40b2405849e597ba5fb5";

    /// @inheritdoc IBLSApkRegistry
    address public immutable registryCoordinator;

    /// INDIVIDUAL PUBLIC KEY STORAGE

    /// @inheritdoc IBLSApkRegistry
    mapping(address operator => bytes32 operatorId) public operatorToPubkeyHash;
    /// @inheritdoc IBLSApkRegistry
    mapping(bytes32 pubkeyHash => address operator) public pubkeyHashToOperator;
    /// @inheritdoc IBLSApkRegistry
    mapping(address operator => BN254.G1Point pubkeyG1) public operatorToPubkey;

    /// @inheritdoc IBLSApkRegistry
    mapping(uint8 quorumNumber => IBLSApkRegistryTypes.ApkUpdate[]) public apkHistory;
    /// @inheritdoc IBLSApkRegistry
    mapping(uint8 quorumNumber => BN254.G1Point) public currentApk;

    mapping(address operator => BN254.G2Point) internal operatorToPubkeyG2;

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator
    ) {
        registryCoordinator = address(_slashingRegistryCoordinator);
        // disable initializers so that the implementation contract cannot be initialized
        _disableInitializers();
    }

    uint256[44] private __GAP;
}
````

## File: src/BLSSignatureChecker.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";
import {BN254} from "./libraries/BN254.sol";

import "./BLSSignatureCheckerStorage.sol";

/**
 * @title Used for checking BLS aggregate signatures from the operators of a `BLSRegistry`.
 * @author Layr Labs, Inc.
 * @notice Terms of Service: https://docs.eigenlayer.xyz/overview/terms-of-service
 * @notice This is the contract for checking the validity of aggregate operator signatures.
 */
contract BLSSignatureChecker is BLSSignatureCheckerStorage {
    using BN254 for BN254.G1Point;

    /// MODIFIERS

    modifier onlyCoordinatorOwner() {
        require(
            msg.sender == Ownable(address(registryCoordinator)).owner(),
            OnlyRegistryCoordinatorOwner()
        );
        _;
    }

    /// CONSTRUCTION

    constructor(
        ISlashingRegistryCoordinator _registryCoordinator
    ) BLSSignatureCheckerStorage(_registryCoordinator) {}

    /// ACTIONS

    /// @inheritdoc IBLSSignatureChecker
    function setStaleStakesForbidden(
        bool value
    ) external onlyCoordinatorOwner {
        _setStaleStakesForbidden(value);
    }

    /// VIEW

    /// @inheritdoc IBLSSignatureChecker
    function checkSignatures(
        bytes32 msgHash,
        bytes calldata quorumNumbers,
        uint32 referenceBlockNumber,
        NonSignerStakesAndSignature memory params
    ) public view returns (QuorumStakeTotals memory, bytes32) {
        require(quorumNumbers.length != 0, InputEmptyQuorumNumbers());

        require(
            (quorumNumbers.length == params.quorumApks.length)
                && (quorumNumbers.length == params.quorumApkIndices.length)
                && (quorumNumbers.length == params.totalStakeIndices.length)
                && (quorumNumbers.length == params.nonSignerStakeIndices.length),
            InputArrayLengthMismatch()
        );

        require(
            params.nonSignerPubkeys.length == params.nonSignerQuorumBitmapIndices.length,
            InputNonSignerLengthMismatch()
        );

        require(referenceBlockNumber < uint32(block.number), InvalidReferenceBlocknumber());

        // This method needs to calculate the aggregate pubkey for all signing operators across
        // all signing quorums. To do that, we can query the aggregate pubkey for each quorum
        // and subtract out the pubkey for each nonsigning operator registered to that quorum.
        //
        // In practice, we do this in reverse - calculating an aggregate pubkey for all nonsigners,
        // negating that pubkey, then adding the aggregate pubkey for each quorum.
        BN254.G1Point memory apk = BN254.G1Point(0, 0);

        // For each quorum, we're also going to query the total stake for all registered operators
        // at the referenceBlockNumber, and derive the stake held by signers by subtracting out
        // stakes held by nonsigners.
        QuorumStakeTotals memory stakeTotals;
        stakeTotals.totalStakeForQuorum = new uint96[](quorumNumbers.length);
        stakeTotals.signedStakeForQuorum = new uint96[](quorumNumbers.length);

        NonSignerInfo memory nonSigners;
        nonSigners.quorumBitmaps = new uint256[](params.nonSignerPubkeys.length);
        nonSigners.pubkeyHashes = new bytes32[](params.nonSignerPubkeys.length);

        {
            // Get a bitmap of the quorums signing the message, and validate that
            // quorumNumbers contains only unique, valid quorum numbers
            uint256 signingQuorumBitmap = BitmapUtils.orderedBytesArrayToBitmap(
                quorumNumbers, registryCoordinator.quorumCount()
            );

            for (uint256 j = 0; j < params.nonSignerPubkeys.length; j++) {
                // The nonsigner's pubkey hash doubles as their operatorId
                // The check below validates that these operatorIds are sorted (and therefore
                // free of duplicates)
                nonSigners.pubkeyHashes[j] = params.nonSignerPubkeys[j].hashG1Point();
                if (j != 0) {
                    require(
                        uint256(nonSigners.pubkeyHashes[j])
                            > uint256(nonSigners.pubkeyHashes[j - 1]),
                        NonSignerPubkeysNotSorted()
                    );
                }

                // Get the quorums the nonsigner was registered for at referenceBlockNumber
                nonSigners.quorumBitmaps[j] = registryCoordinator
                    .getQuorumBitmapAtBlockNumberByIndex({
                    operatorId: nonSigners.pubkeyHashes[j],
                    blockNumber: referenceBlockNumber,
                    index: params.nonSignerQuorumBitmapIndices[j]
                });

                // Add the nonsigner's pubkey to the total apk, multiplied by the number
                // of quorums they have in common with the signing quorums, because their
                // public key will be a part of each signing quorum's aggregate pubkey
                apk = apk.plus(
                    params.nonSignerPubkeys[j].scalar_mul_tiny(
                        BitmapUtils.countNumOnes(nonSigners.quorumBitmaps[j] & signingQuorumBitmap)
                    )
                );
            }
        }

        // Negate the sum of the nonsigner aggregate pubkeys - from here, we'll add the
        // total aggregate pubkey from each quorum. Because the nonsigners' pubkeys are
        // in these quorums, this initial negation ensures they're cancelled out
        apk = apk.negate();

        /**
         * For each quorum (at referenceBlockNumber):
         * - add the apk for all registered operators
         * - query the total stake for each quorum
         * - subtract the stake for each nonsigner to calculate the stake belonging to signers
         */
        {
            bool _staleStakesForbidden = staleStakesForbidden;
            uint256 withdrawalDelayBlocks =
                _staleStakesForbidden ? delegation.minWithdrawalDelayBlocks() : 0;

            for (uint256 i = 0; i < quorumNumbers.length; i++) {
                // If we're disallowing stale stake updates, check that each quorum's last update block
                // is within withdrawalDelayBlocks
                if (_staleStakesForbidden) {
                    require(
                        registryCoordinator.quorumUpdateBlockNumber(uint8(quorumNumbers[i]))
                            + withdrawalDelayBlocks > referenceBlockNumber,
                        StaleStakesForbidden()
                    );
                }

                // Validate params.quorumApks is correct for this quorum at the referenceBlockNumber,
                // then add it to the total apk
                require(
                    bytes24(params.quorumApks[i].hashG1Point())
                        == blsApkRegistry.getApkHashAtBlockNumberAndIndex({
                            quorumNumber: uint8(quorumNumbers[i]),
                            blockNumber: referenceBlockNumber,
                            index: params.quorumApkIndices[i]
                        }),
                    InvalidQuorumApkHash()
                );
                apk = apk.plus(params.quorumApks[i]);

                // Get the total and starting signed stake for the quorum at referenceBlockNumber
                stakeTotals.totalStakeForQuorum[i] = stakeRegistry
                    .getTotalStakeAtBlockNumberFromIndex({
                    quorumNumber: uint8(quorumNumbers[i]),
                    blockNumber: referenceBlockNumber,
                    index: params.totalStakeIndices[i]
                });
                stakeTotals.signedStakeForQuorum[i] = stakeTotals.totalStakeForQuorum[i];

                // Keep track of the nonSigners index in the quorum
                uint256 nonSignerForQuorumIndex = 0;

                // loop through all nonSigners, checking that they are a part of the quorum via their quorumBitmap
                // if so, load their stake at referenceBlockNumber and subtract it from running stake signed
                for (uint256 j = 0; j < params.nonSignerPubkeys.length; j++) {
                    // if the nonSigner is a part of the quorum, subtract their stake from the running total
                    if (BitmapUtils.isSet(nonSigners.quorumBitmaps[j], uint8(quorumNumbers[i]))) {
                        stakeTotals.signedStakeForQuorum[i] -= stakeRegistry
                            .getStakeAtBlockNumberAndIndex({
                            quorumNumber: uint8(quorumNumbers[i]),
                            blockNumber: referenceBlockNumber,
                            operatorId: nonSigners.pubkeyHashes[j],
                            index: params.nonSignerStakeIndices[i][nonSignerForQuorumIndex]
                        });
                        unchecked {
                            ++nonSignerForQuorumIndex;
                        }
                    }
                }
            }
        }
        {
            // verify the signature
            (bool pairingSuccessful, bool signatureIsValid) =
                trySignatureAndApkVerification(msgHash, apk, params.apkG2, params.sigma);
            require(pairingSuccessful, InvalidBLSPairingKey());
            require(signatureIsValid, InvalidBLSSignature());
        }
        // set signatoryRecordHash variable used for fraudproofs
        bytes32 signatoryRecordHash =
            keccak256(abi.encodePacked(referenceBlockNumber, nonSigners.pubkeyHashes));

        // return the total stakes that signed for each quorum, and a hash of the information required to prove the exact signers and stake
        return (stakeTotals, signatoryRecordHash);
    }

    /// @inheritdoc IBLSSignatureChecker
    function trySignatureAndApkVerification(
        bytes32 msgHash,
        BN254.G1Point memory apk,
        BN254.G2Point memory apkG2,
        BN254.G1Point memory sigma
    ) public view returns (bool pairingSuccessful, bool siganatureIsValid) {
        // gamma = keccak256(abi.encodePacked(msgHash, apk, apkG2, sigma))
        uint256 gamma = uint256(
            keccak256(
                abi.encodePacked(
                    msgHash,
                    apk.X,
                    apk.Y,
                    apkG2.X[0],
                    apkG2.X[1],
                    apkG2.Y[0],
                    apkG2.Y[1],
                    sigma.X,
                    sigma.Y
                )
            )
        ) % BN254.FR_MODULUS;
        // verify the signature
        (pairingSuccessful, siganatureIsValid) = BN254.safePairing(
            sigma.plus(apk.scalar_mul(gamma)),
            BN254.negGeneratorG2(),
            BN254.hashToG1(msgHash).plus(BN254.generatorG1().scalar_mul(gamma)),
            apkG2,
            PAIRING_EQUALITY_CHECK_GAS
        );
    }

    function _setStaleStakesForbidden(
        bool value
    ) internal {
        staleStakesForbidden = value;
        emit StaleStakesForbiddenUpdate(value);
    }
}
````

## File: src/BLSSignatureCheckerStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IBLSSignatureChecker} from "./interfaces/IBLSSignatureChecker.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IBLSApkRegistry} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry, IDelegationManager} from "./interfaces/IStakeRegistry.sol";

abstract contract BLSSignatureCheckerStorage is IBLSSignatureChecker {
    /// @dev Returns the assumed gas cost of multiplying 2 pairings.
    uint256 internal constant PAIRING_EQUALITY_CHECK_GAS = 120000;

    /// @inheritdoc IBLSSignatureChecker
    ISlashingRegistryCoordinator public immutable registryCoordinator;
    /// @inheritdoc IBLSSignatureChecker
    IStakeRegistry public immutable stakeRegistry;
    /// @inheritdoc IBLSSignatureChecker
    IBLSApkRegistry public immutable blsApkRegistry;
    /// @inheritdoc IBLSSignatureChecker
    IDelegationManager public immutable delegation;

    /// STATE

    /// @inheritdoc IBLSSignatureChecker
    bool public staleStakesForbidden;

    constructor(
        ISlashingRegistryCoordinator _registryCoordinator
    ) {
        registryCoordinator = _registryCoordinator;
        stakeRegistry = _registryCoordinator.stakeRegistry();
        blsApkRegistry = _registryCoordinator.blsApkRegistry();
        delegation = stakeRegistry.delegation();
    }

    // slither-disable-next-line shadowing-state
    uint256[49] private __GAP;
}
````

## File: src/EjectionManager.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";
import {
    EjectionManagerStorage,
    IEjectionManager,
    ISlashingRegistryCoordinator,
    IStakeRegistry
} from "./EjectionManagerStorage.sol";

/**
 * @title Used for automated ejection of operators from the SlashingRegistryCoordinator under a ratelimit
 * @author Layr Labs, Inc.
 */
contract EjectionManager is OwnableUpgradeable, EjectionManagerStorage {
    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        IStakeRegistry _stakeRegistry
    ) EjectionManagerStorage(_slashingRegistryCoordinator, _stakeRegistry) {
        _disableInitializers();
    }

    function initialize(
        address _owner,
        address[] memory _ejectors,
        QuorumEjectionParams[] memory _quorumEjectionParams
    ) external initializer {
        _transferOwnership(_owner);
        for (uint8 i = 0; i < _ejectors.length; i++) {
            _setEjector(_ejectors[i], true);
        }
        for (uint8 i = 0; i < _quorumEjectionParams.length; i++) {
            _setQuorumEjectionParams(i, _quorumEjectionParams[i]);
        }
    }

    /// @inheritdoc IEjectionManager
    function ejectOperators(
        bytes32[][] memory operatorIds
    ) external {
        require(isEjector[msg.sender] || msg.sender == owner(), OnlyOwnerOrEjector());

        for (uint256 i = 0; i < operatorIds.length; ++i) {
            uint8 quorumNumber = uint8(i);

            uint256 amountEjectable = amountEjectableForQuorum(quorumNumber);
            uint256 stakeForEjection;
            uint32 ejectedOperators;

            bool ratelimitHit;
            if (amountEjectable > 0 || msg.sender == owner()) {
                for (uint8 j = 0; j < operatorIds[i].length; ++j) {
                    uint256 operatorStake =
                        stakeRegistry.getCurrentStake(operatorIds[i][j], quorumNumber);

                    //if caller is ejector enforce ratelimit
                    if (
                        isEjector[msg.sender]
                            && quorumEjectionParams[quorumNumber].rateLimitWindow > 0
                            && stakeForEjection + operatorStake > amountEjectable
                    ) {
                        ratelimitHit = true;

                        stakeForEjection += operatorStake;
                        ++ejectedOperators;

                        slashingRegistryCoordinator.ejectOperator(
                            slashingRegistryCoordinator.getOperatorFromId(operatorIds[i][j]),
                            abi.encodePacked(quorumNumber)
                        );

                        emit OperatorEjected(operatorIds[i][j], quorumNumber);

                        break;
                    }

                    stakeForEjection += operatorStake;
                    ++ejectedOperators;

                    slashingRegistryCoordinator.ejectOperator(
                        slashingRegistryCoordinator.getOperatorFromId(operatorIds[i][j]),
                        abi.encodePacked(quorumNumber)
                    );

                    emit OperatorEjected(operatorIds[i][j], quorumNumber);
                }
            }

            //record the stake ejected if ejector and ratelimit enforced
            if (isEjector[msg.sender] && stakeForEjection > 0) {
                stakeEjectedForQuorum[quorumNumber].push(
                    StakeEjection({timestamp: block.timestamp, stakeEjected: stakeForEjection})
                );
            }

            emit QuorumEjection(ejectedOperators, ratelimitHit);
        }
    }

    /// @inheritdoc IEjectionManager
    function setQuorumEjectionParams(
        uint8 quorumNumber,
        QuorumEjectionParams memory _quorumEjectionParams
    ) external onlyOwner {
        _setQuorumEjectionParams(quorumNumber, _quorumEjectionParams);
    }

    /// @inheritdoc IEjectionManager
    function setEjector(address ejector, bool status) external onlyOwner {
        _setEjector(ejector, status);
    }

    ///@dev internal function to set the quorum ejection params
    function _setQuorumEjectionParams(
        uint8 quorumNumber,
        QuorumEjectionParams memory _quorumEjectionParams
    ) internal {
        require(quorumNumber < MAX_QUORUM_COUNT, MaxQuorumCount());
        quorumEjectionParams[quorumNumber] = _quorumEjectionParams;
        emit QuorumEjectionParamsSet(
            quorumNumber,
            _quorumEjectionParams.rateLimitWindow,
            _quorumEjectionParams.ejectableStakePercent
        );
    }

    ///@dev internal function to set the ejector
    function _setEjector(address ejector, bool status) internal {
        isEjector[ejector] = status;
        emit EjectorUpdated(ejector, status);
    }

    /// @inheritdoc IEjectionManager
    function amountEjectableForQuorum(
        uint8 quorumNumber
    ) public view returns (uint256) {
        uint256 totalEjectable = uint256(quorumEjectionParams[quorumNumber].ejectableStakePercent)
            * uint256(stakeRegistry.getCurrentTotalStake(quorumNumber)) / uint256(BIPS_DENOMINATOR);

        if (stakeEjectedForQuorum[quorumNumber].length == 0) {
            return totalEjectable;
        }

        uint256 cutoffTime = block.timestamp - quorumEjectionParams[quorumNumber].rateLimitWindow;
        uint256 totalEjected = 0;
        uint256 i = stakeEjectedForQuorum[quorumNumber].length - 1;

        while (stakeEjectedForQuorum[quorumNumber][i].timestamp > cutoffTime) {
            totalEjected += stakeEjectedForQuorum[quorumNumber][i].stakeEjected;
            if (i == 0) {
                break;
            } else {
                --i;
            }
        }

        if (totalEjected >= totalEjectable) {
            return 0;
        }
        return totalEjectable - totalEjected;
    }
}
````

## File: src/EjectionManagerStorage.sol
````
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.27;

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";
import {IEjectionManager} from "./interfaces/IEjectionManager.sol";

abstract contract EjectionManagerStorage is IEjectionManager {
    /// @notice The basis point denominator for the ejectable stake percent
    uint16 internal constant BIPS_DENOMINATOR = 10000;
    /// @notice The max number of quorums
    uint8 internal constant MAX_QUORUM_COUNT = 192;

    /// @inheritdoc IEjectionManager
    ISlashingRegistryCoordinator public immutable slashingRegistryCoordinator;
    /// @inheritdoc IEjectionManager
    IStakeRegistry public immutable stakeRegistry;

    /// @inheritdoc IEjectionManager
    mapping(address => bool) public isEjector;
    /// @inheritdoc IEjectionManager
    mapping(uint8 => StakeEjection[]) public stakeEjectedForQuorum;
    /// @inheritdoc IEjectionManager
    mapping(uint8 => QuorumEjectionParams) public quorumEjectionParams;

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        IStakeRegistry _stakeRegistry
    ) {
        slashingRegistryCoordinator = _slashingRegistryCoordinator;
        stakeRegistry = _stakeRegistry;
    }

    /// @dev This was missing before the slashing release, if your contract
    /// was deployed pre-slashing, you should double check your storage layout.
    uint256[47] private __gap;
}
````

## File: src/IndexRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IIndexRegistry, IndexRegistryStorage} from "./IndexRegistryStorage.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";

/**
 * @title A `Registry` that keeps track of an ordered list of operators for each quorum
 * @author Layr Labs, Inc.
 */
contract IndexRegistry is IndexRegistryStorage {
    modifier onlyRegistryCoordinator() {
        _checkRegistryCoordinator();
        _;
    }

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator
    ) IndexRegistryStorage(_slashingRegistryCoordinator) {}

    /**
     *
     *                   EXTERNAL FUNCTIONS - REGISTRY COORDINATOR
     *
     */

    /// @inheritdoc IIndexRegistry
    function registerOperator(
        bytes32 operatorId,
        bytes calldata quorumNumbers
    ) public virtual onlyRegistryCoordinator returns (uint32[] memory) {
        uint32[] memory numOperatorsPerQuorum = new uint32[](quorumNumbers.length);

        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            // Validate quorum exists and get current operator count
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            uint256 historyLength = _operatorCountHistory[quorumNumber].length;
            require(historyLength != 0, QuorumDoesNotExist());

            /**
             * Increase the number of operators currently active for this quorum,
             * and assign the operator to the last operatorIndex available
             */
            uint32 newOperatorCount = _increaseOperatorCount(quorumNumber);
            _assignOperatorToIndex({
                operatorId: operatorId,
                quorumNumber: quorumNumber,
                operatorIndex: newOperatorCount - 1
            });

            // Record the current operator count for each quorum
            numOperatorsPerQuorum[i] = newOperatorCount;
        }

        return numOperatorsPerQuorum;
    }

    /// @inheritdoc IIndexRegistry
    function deregisterOperator(
        bytes32 operatorId,
        bytes calldata quorumNumbers
    ) public virtual onlyRegistryCoordinator {
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            // Validate quorum exists and get the operatorIndex of the operator being deregistered
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            uint256 historyLength = _operatorCountHistory[quorumNumber].length;
            require(historyLength != 0, QuorumDoesNotExist());
            uint32 operatorIndexToRemove = currentOperatorIndex[quorumNumber][operatorId];

            /**
             * "Pop" the operator from the registry:
             * 1. Decrease the operator count for the quorum
             * 2. Remove the last operator associated with the count
             * 3. Place the last operator in the deregistered operator's old position
             */
            uint32 newOperatorCount = _decreaseOperatorCount(quorumNumber);
            bytes32 lastOperatorId = _popLastOperator(quorumNumber, newOperatorCount);
            if (operatorId != lastOperatorId) {
                _assignOperatorToIndex({
                    operatorId: lastOperatorId,
                    quorumNumber: quorumNumber,
                    operatorIndex: operatorIndexToRemove
                });
            }
        }
    }

    /// @inheritdoc IIndexRegistry
    function initializeQuorum(
        uint8 quorumNumber
    ) public virtual onlyRegistryCoordinator {
        require(_operatorCountHistory[quorumNumber].length == 0, QuorumDoesNotExist());

        _operatorCountHistory[quorumNumber].push(
            QuorumUpdate({numOperators: 0, fromBlockNumber: uint32(block.number)})
        );
    }

    /**
     *
     *                             INTERNAL FUNCTIONS
     *
     */

    /// @notice Increases the historical operator count by 1 and returns the new count.
    function _increaseOperatorCount(
        uint8 quorumNumber
    ) internal returns (uint32) {
        QuorumUpdate storage lastUpdate = _latestQuorumUpdate(quorumNumber);
        uint32 newOperatorCount = lastUpdate.numOperators + 1;

        _updateOperatorCountHistory(quorumNumber, lastUpdate, newOperatorCount);

        // If this is the first time we're using this operatorIndex, push its first update
        // This maintains an invariant: existing indices have nonzero history
        if (_operatorIndexHistory[quorumNumber][newOperatorCount - 1].length == 0) {
            _operatorIndexHistory[quorumNumber][newOperatorCount - 1].push(
                OperatorUpdate({
                    operatorId: OPERATOR_DOES_NOT_EXIST_ID,
                    fromBlockNumber: uint32(block.number)
                })
            );
        }

        return newOperatorCount;
    }

    /// @notice Decreases the historical operator count by 1 and returns the new count.
    function _decreaseOperatorCount(
        uint8 quorumNumber
    ) internal returns (uint32) {
        QuorumUpdate storage lastUpdate = _latestQuorumUpdate(quorumNumber);
        uint32 newOperatorCount = lastUpdate.numOperators - 1;

        _updateOperatorCountHistory(quorumNumber, lastUpdate, newOperatorCount);

        return newOperatorCount;
    }

    /// @notice Updates `_operatorCountHistory` with a new operator count.
    /// @dev If the lastUpdate was made in this block, update the entry.
    /// Otherwise, push a new historical entry.
    function _updateOperatorCountHistory(
        uint8 quorumNumber,
        QuorumUpdate storage lastUpdate,
        uint32 newOperatorCount
    ) internal {
        if (lastUpdate.fromBlockNumber == uint32(block.number)) {
            lastUpdate.numOperators = newOperatorCount;
        } else {
            _operatorCountHistory[quorumNumber].push(
                QuorumUpdate({numOperators: newOperatorCount, fromBlockNumber: uint32(block.number)})
            );
        }
    }

    /// @notice For a given quorum and operatorIndex, pop and return the last operatorId in the history.
    /// @dev The last entry's operatorId is updated to OPERATOR_DOES_NOT_EXIST_ID.
    /// @return The removed operatorId.
    function _popLastOperator(
        uint8 quorumNumber,
        uint32 operatorIndex
    ) internal returns (bytes32) {
        OperatorUpdate storage lastUpdate = _latestOperatorIndexUpdate(quorumNumber, operatorIndex);
        bytes32 removedOperatorId = lastUpdate.operatorId;

        // Set the current operator id for this operatorIndex to 0
        _updateOperatorIndexHistory(
            quorumNumber, operatorIndex, lastUpdate, OPERATOR_DOES_NOT_EXIST_ID
        );

        return removedOperatorId;
    }

    /// @notice Assigns an operator to an index and updates the index history.
    /// @param operatorId operatorId of the operator to update.
    /// @param quorumNumber quorumNumber of the operator to update.
    /// @param operatorIndex the latest index of that operator in the list of operators registered for this quorum.
    function _assignOperatorToIndex(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 operatorIndex
    ) internal {
        OperatorUpdate storage lastUpdate = _latestOperatorIndexUpdate(quorumNumber, operatorIndex);

        _updateOperatorIndexHistory(quorumNumber, operatorIndex, lastUpdate, operatorId);

        // Assign the operator to their new current operatorIndex
        currentOperatorIndex[quorumNumber][operatorId] = operatorIndex;
        emit QuorumIndexUpdate(operatorId, quorumNumber, operatorIndex);
    }

    /// @notice Updates `_operatorIndexHistory` with a new operator id for the current block.
    /// @dev If the lastUpdate was made in this block, update the entry.
    /// Otherwise, push a new historical entry.
    function _updateOperatorIndexHistory(
        uint8 quorumNumber,
        uint32 operatorIndex,
        OperatorUpdate storage lastUpdate,
        bytes32 newOperatorId
    ) internal {
        if (lastUpdate.fromBlockNumber == uint32(block.number)) {
            lastUpdate.operatorId = newOperatorId;
        } else {
            _operatorIndexHistory[quorumNumber][operatorIndex].push(
                OperatorUpdate({operatorId: newOperatorId, fromBlockNumber: uint32(block.number)})
            );
        }
    }

    /// @notice Returns the most recent operator count update for a quorum.
    /// @dev Reverts if the quorum does not exist (history length == 0).
    function _latestQuorumUpdate(
        uint8 quorumNumber
    ) internal view returns (QuorumUpdate storage) {
        uint256 historyLength = _operatorCountHistory[quorumNumber].length;
        return _operatorCountHistory[quorumNumber][historyLength - 1];
    }

    /// @notice Returns the most recent operator id update for an index.
    /// @dev Reverts if the index has never been used (history length == 0).
    function _latestOperatorIndexUpdate(
        uint8 quorumNumber,
        uint32 operatorIndex
    ) internal view returns (OperatorUpdate storage) {
        uint256 historyLength = _operatorIndexHistory[quorumNumber][operatorIndex].length;
        return _operatorIndexHistory[quorumNumber][operatorIndex][historyLength - 1];
    }

    /// @notice Returns the total number of operators of the service for the given `quorumNumber` at the given `blockNumber`.
    /// @dev Reverts if the quorum does not exist, or if the blockNumber is from before the quorum existed.
    function _operatorCountAtBlockNumber(
        uint8 quorumNumber,
        uint32 blockNumber
    ) internal view returns (uint32) {
        uint256 historyLength = _operatorCountHistory[quorumNumber].length;

        // Loop backwards through _operatorCountHistory until we find an entry that preceeds `blockNumber`
        for (uint256 i = historyLength; i > 0; i--) {
            QuorumUpdate memory quorumUpdate = _operatorCountHistory[quorumNumber][i - 1];

            if (quorumUpdate.fromBlockNumber <= blockNumber) {
                return quorumUpdate.numOperators;
            }
        }

        revert(
            "IndexRegistry._operatorCountAtBlockNumber: quorum did not exist at given block number"
        );
    }

    /// @notice Returns the operatorId at the given `operatorIndex` at the given `blockNumber` for the given `quorumNumber`.
    /// @dev Requires that the operatorIndex was active at the given block number for quorum.
    function _operatorIdForIndexAtBlockNumber(
        uint8 quorumNumber,
        uint32 operatorIndex,
        uint32 blockNumber
    ) internal view returns (bytes32) {
        uint256 historyLength = _operatorIndexHistory[quorumNumber][operatorIndex].length;

        // Loop backward through _operatorIndexHistory until we find an entry that preceeds `blockNumber`
        for (uint256 i = historyLength; i > 0; i--) {
            OperatorUpdate memory operatorIndexUpdate =
                _operatorIndexHistory[quorumNumber][operatorIndex][i - 1];

            if (operatorIndexUpdate.fromBlockNumber <= blockNumber) {
                // Special case: this will be OPERATOR_DOES_NOT_EXIST_ID if this operatorIndex was not used at the block number
                return operatorIndexUpdate.operatorId;
            }
        }

        // we should only hit this if the operatorIndex was never used before blockNumber
        return OPERATOR_DOES_NOT_EXIST_ID;
    }

    /**
     *
     *                              VIEW FUNCTIONS
     *
     */

    /// @inheritdoc IIndexRegistry
    function getOperatorUpdateAtIndex(
        uint8 quorumNumber,
        uint32 operatorIndex,
        uint32 arrayIndex
    ) external view returns (OperatorUpdate memory) {
        return _operatorIndexHistory[quorumNumber][operatorIndex][arrayIndex];
    }

    /// @inheritdoc IIndexRegistry
    function getQuorumUpdateAtIndex(
        uint8 quorumNumber,
        uint32 quorumIndex
    ) external view returns (QuorumUpdate memory) {
        return _operatorCountHistory[quorumNumber][quorumIndex];
    }

    /// @inheritdoc IIndexRegistry
    function getLatestQuorumUpdate(
        uint8 quorumNumber
    ) external view returns (QuorumUpdate memory) {
        return _latestQuorumUpdate(quorumNumber);
    }

    /// @inheritdoc IIndexRegistry
    function getLatestOperatorUpdate(
        uint8 quorumNumber,
        uint32 operatorIndex
    ) external view returns (OperatorUpdate memory) {
        return _latestOperatorIndexUpdate(quorumNumber, operatorIndex);
    }

    /// @inheritdoc IIndexRegistry
    function getOperatorListAtBlockNumber(
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (bytes32[] memory) {
        uint32 operatorCount = _operatorCountAtBlockNumber(quorumNumber, blockNumber);
        bytes32[] memory operatorList = new bytes32[](operatorCount);
        for (uint256 i = 0; i < operatorCount; i++) {
            operatorList[i] = _operatorIdForIndexAtBlockNumber(quorumNumber, uint32(i), blockNumber);
            require(operatorList[i] != OPERATOR_DOES_NOT_EXIST_ID, OperatorIdDoesNotExist());
        }
        return operatorList;
    }

    /// @inheritdoc IIndexRegistry
    function totalOperatorsForQuorum(
        uint8 quorumNumber
    ) external view returns (uint32) {
        return _latestQuorumUpdate(quorumNumber).numOperators;
    }

    /// @inheritdoc IIndexRegistry
    function totalOperatorsForQuorumAtBlockNumber(
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint32) {
        return _operatorCountAtBlockNumber(quorumNumber, blockNumber);
    }

    function _checkRegistryCoordinator() internal view {
        require(msg.sender == address(registryCoordinator), OnlyRegistryCoordinator());
    }
}
````

## File: src/IndexRegistryStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {Initializable} from "@openzeppelin-upgrades/contracts/proxy/utils/Initializable.sol";

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";

/**
 * @title Storage variables for the `IndexRegistry` contract.
 * @author Layr Labs, Inc.
 * @notice This storage contract is separate from the logic to simplify the upgrade process.
 */
abstract contract IndexRegistryStorage is Initializable, IIndexRegistry {
    /// @notice The value that is returned when an operator does not exist at an index at a certain block
    bytes32 public constant OPERATOR_DOES_NOT_EXIST_ID = bytes32(0);

    /// @notice The RegistryCoordinator contract for this middleware
    address public immutable registryCoordinator;

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

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator
    ) {
        registryCoordinator = address(_slashingRegistryCoordinator);
        // disable initializers so that the implementation contract cannot be initialized
        _disableInitializers();
    }

    // storage gap for upgradeability
    uint256[47] private __GAP;
}
````

## File: src/OperatorStateRetriever.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IBLSApkRegistry} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";

/**
 * @title OperatorStateRetriever with view functions that allow to retrieve the state of an AVSs registry system.
 * @author Layr Labs Inc.
 */
contract OperatorStateRetriever {
    struct Operator {
        address operator;
        bytes32 operatorId;
        uint96 stake;
    }

    struct CheckSignaturesIndices {
        uint32[] nonSignerQuorumBitmapIndices;
        uint32[] quorumApkIndices;
        uint32[] totalStakeIndices;
        uint32[][] nonSignerStakeIndices; // nonSignerStakeIndices[quorumNumberIndex][nonSignerIndex]
    }

    error OperatorNotRegistered();

    /**
     * @notice This function is intended to to be called by AVS operators every time a new task is created (i.e.)
     * the AVS coordinator makes a request to AVS operators. Since all of the crucial information is kept onchain,
     * operators don't need to run indexers to fetch the data.
     * @param registryCoordinator is the registry coordinator to fetch the AVS registry information from
     * @param operatorId the id of the operator to fetch the quorums lists
     * @param blockNumber is the block number to get the operator state for
     * @return 1) the quorumBitmap of the operator at the given blockNumber
     *         2) 2d array of Operator structs. For each quorum the provided operator
     *            was a part of at `blockNumber`, an ordered list of operators.
     */
    function getOperatorState(
        ISlashingRegistryCoordinator registryCoordinator,
        bytes32 operatorId,
        uint32 blockNumber
    ) external view returns (uint256, Operator[][] memory) {
        bytes32[] memory operatorIds = new bytes32[](1);
        operatorIds[0] = operatorId;
        uint256 index =
            registryCoordinator.getQuorumBitmapIndicesAtBlockNumber(blockNumber, operatorIds)[0];

        uint256 quorumBitmap =
            registryCoordinator.getQuorumBitmapAtBlockNumberByIndex(operatorId, blockNumber, index);

        bytes memory quorumNumbers = BitmapUtils.bitmapToBytesArray(quorumBitmap);

        return (quorumBitmap, getOperatorState(registryCoordinator, quorumNumbers, blockNumber));
    }

    /**
     * @notice returns the ordered list of operators (id and stake) for each quorum. The AVS coordinator
     * may call this function directly to get the operator state for a given block number
     * @param registryCoordinator is the registry coordinator to fetch the AVS registry information from
     * @param quorumNumbers are the ids of the quorums to get the operator state for
     * @param blockNumber is the block number to get the operator state for
     * @return 2d array of Operators. For each quorum, an ordered list of Operators
     */
    function getOperatorState(
        ISlashingRegistryCoordinator registryCoordinator,
        bytes memory quorumNumbers,
        uint32 blockNumber
    ) public view returns (Operator[][] memory) {
        IStakeRegistry stakeRegistry = registryCoordinator.stakeRegistry();
        IIndexRegistry indexRegistry = registryCoordinator.indexRegistry();
        IBLSApkRegistry blsApkRegistry = registryCoordinator.blsApkRegistry();

        Operator[][] memory operators = new Operator[][](quorumNumbers.length);
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            bytes32[] memory operatorIds =
                indexRegistry.getOperatorListAtBlockNumber(quorumNumber, blockNumber);
            operators[i] = new Operator[](operatorIds.length);
            for (uint256 j = 0; j < operatorIds.length; j++) {
                operators[i][j] = Operator({
                    operator: blsApkRegistry.getOperatorFromPubkeyHash(operatorIds[j]),
                    operatorId: bytes32(operatorIds[j]),
                    stake: stakeRegistry.getStakeAtBlockNumber(
                        bytes32(operatorIds[j]), quorumNumber, blockNumber
                    )
                });
            }
        }

        return operators;
    }

    /**
     * @notice this is called by the AVS operator to get the relevant indices for the checkSignatures function
     * if they are not running an indexer
     * @param registryCoordinator is the registry coordinator to fetch the AVS registry information from
     * @param referenceBlockNumber is the block number to get the indices for
     * @param quorumNumbers are the ids of the quorums to get the operator state for
     * @param nonSignerOperatorIds are the ids of the nonsigning operators
     * @return 1) the indices of the quorumBitmaps for each of the operators in the @param nonSignerOperatorIds array at the given blocknumber
     *         2) the indices of the total stakes entries for the given quorums at the given blocknumber
     *         3) the indices of the stakes of each of the nonsigners in each of the quorums they were a
     *            part of (for each nonsigner, an array of length the number of quorums they were a part of
     *            that are also part of the provided quorumNumbers) at the given blocknumber
     *         4) the indices of the quorum apks for each of the provided quorums at the given blocknumber
     */
    function getCheckSignaturesIndices(
        ISlashingRegistryCoordinator registryCoordinator,
        uint32 referenceBlockNumber,
        bytes calldata quorumNumbers,
        bytes32[] calldata nonSignerOperatorIds
    ) external view returns (CheckSignaturesIndices memory) {
        IStakeRegistry stakeRegistry = registryCoordinator.stakeRegistry();
        CheckSignaturesIndices memory checkSignaturesIndices;

        // get the indices of the quorumBitmap updates for each of the operators in the nonSignerOperatorIds array
        checkSignaturesIndices.nonSignerQuorumBitmapIndices = registryCoordinator
            .getQuorumBitmapIndicesAtBlockNumber(referenceBlockNumber, nonSignerOperatorIds);

        // get the indices of the totalStake updates for each of the quorums in the quorumNumbers array
        checkSignaturesIndices.totalStakeIndices =
            stakeRegistry.getTotalStakeIndicesAtBlockNumber(referenceBlockNumber, quorumNumbers);

        checkSignaturesIndices.nonSignerStakeIndices = new uint32[][](quorumNumbers.length);
        for (
            uint8 quorumNumberIndex = 0;
            quorumNumberIndex < quorumNumbers.length;
            quorumNumberIndex++
        ) {
            uint256 numNonSignersForQuorum = 0;
            // this array's length will be at most the number of nonSignerOperatorIds, this will be trimmed after it is filled
            checkSignaturesIndices.nonSignerStakeIndices[quorumNumberIndex] =
                new uint32[](nonSignerOperatorIds.length);

            for (uint256 i = 0; i < nonSignerOperatorIds.length; i++) {
                // get the quorumBitmap for the operator at the given blocknumber and index
                uint192 nonSignerQuorumBitmap = registryCoordinator
                    .getQuorumBitmapAtBlockNumberByIndex(
                    nonSignerOperatorIds[i],
                    referenceBlockNumber,
                    checkSignaturesIndices.nonSignerQuorumBitmapIndices[i]
                );

                require(nonSignerQuorumBitmap != 0, OperatorNotRegistered());

                // if the operator was a part of the quorum and the quorum is a part of the provided quorumNumbers
                if ((nonSignerQuorumBitmap >> uint8(quorumNumbers[quorumNumberIndex])) & 1 == 1) {
                    // get the index of the stake update for the operator at the given blocknumber and quorum number
                    checkSignaturesIndices.nonSignerStakeIndices[quorumNumberIndex][numNonSignersForQuorum]
                    = stakeRegistry.getStakeUpdateIndexAtBlockNumber(
                        nonSignerOperatorIds[i],
                        uint8(quorumNumbers[quorumNumberIndex]),
                        referenceBlockNumber
                    );
                    numNonSignersForQuorum++;
                }
            }

            // resize the array to the number of nonSigners for this quorum
            uint32[] memory nonSignerStakeIndicesForQuorum = new uint32[](numNonSignersForQuorum);
            for (uint256 i = 0; i < numNonSignersForQuorum; i++) {
                nonSignerStakeIndicesForQuorum[i] =
                    checkSignaturesIndices.nonSignerStakeIndices[quorumNumberIndex][i];
            }
            checkSignaturesIndices.nonSignerStakeIndices[quorumNumberIndex] =
                nonSignerStakeIndicesForQuorum;
        }

        IBLSApkRegistry blsApkRegistry = registryCoordinator.blsApkRegistry();
        // get the indices of the quorum apks for each of the provided quorums at the given blocknumber
        checkSignaturesIndices.quorumApkIndices =
            blsApkRegistry.getApkIndicesAtBlockNumber(quorumNumbers, referenceBlockNumber);

        return checkSignaturesIndices;
    }

    /**
     * @notice this function returns the quorumBitmaps for each of the operators in the operatorIds array at the given blocknumber
     * @param registryCoordinator is the AVS registry coordinator to fetch the operator information from
     * @param operatorIds are the ids of the operators to get the quorumBitmaps for
     * @param blockNumber is the block number to get the quorumBitmaps for
     */
    function getQuorumBitmapsAtBlockNumber(
        ISlashingRegistryCoordinator registryCoordinator,
        bytes32[] memory operatorIds,
        uint32 blockNumber
    ) external view returns (uint256[] memory) {
        uint32[] memory quorumBitmapIndices =
            registryCoordinator.getQuorumBitmapIndicesAtBlockNumber(blockNumber, operatorIds);
        uint256[] memory quorumBitmaps = new uint256[](operatorIds.length);
        for (uint256 i = 0; i < operatorIds.length; i++) {
            quorumBitmaps[i] = registryCoordinator.getQuorumBitmapAtBlockNumberByIndex(
                operatorIds[i], blockNumber, quorumBitmapIndices[i]
            );
        }
        return quorumBitmaps;
    }

    /**
     * @notice This function returns the operatorIds for each of the operators in the operators array
     * @param registryCoordinator is the AVS registry coordinator to fetch the operator information from
     * @param operators is the array of operator address to get corresponding operatorIds for
     * @dev if an operator is not registered, the operatorId will be 0
     */
    function getBatchOperatorId(
        ISlashingRegistryCoordinator registryCoordinator,
        address[] memory operators
    ) external view returns (bytes32[] memory operatorIds) {
        operatorIds = new bytes32[](operators.length);
        for (uint256 i = 0; i < operators.length; ++i) {
            operatorIds[i] = registryCoordinator.getOperatorId(operators[i]);
        }
    }

    /**
     * @notice This function returns the operator addresses for each of the operators in the operatorIds array
     * @param registryCoordinator is the AVS registry coordinator to fetch the operator information from
     * @param operators is the array of operatorIds to get corresponding operator addresses for
     * @dev if an operator is not registered, the operator address will be 0
     */
    function getBatchOperatorFromId(
        ISlashingRegistryCoordinator registryCoordinator,
        bytes32[] memory operatorIds
    ) external view returns (address[] memory operators) {
        operators = new address[](operatorIds.length);
        for (uint256 i = 0; i < operatorIds.length; ++i) {
            operators[i] = registryCoordinator.getOperatorFromId(operatorIds[i]);
        }
    }
}
````

## File: src/RegistryCoordinator.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IPauserRegistry} from "eigenlayer-contracts/src/contracts/interfaces/IPauserRegistry.sol";
import {
    IAllocationManager,
    OperatorSet
} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISignatureUtilsMixin} from
    "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {ISemVerMixin} from "eigenlayer-contracts/src/contracts/interfaces/ISemVerMixin.sol";
import {SemVerMixin} from "eigenlayer-contracts/src/contracts/mixins/SemVerMixin.sol";
import {IBLSApkRegistry, IBLSApkRegistryTypes} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";
import {IServiceManager} from "./interfaces/IServiceManager.sol";
import {
    IRegistryCoordinator, IRegistryCoordinatorTypes
} from "./interfaces/IRegistryCoordinator.sol";
import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";
import {SlashingRegistryCoordinator} from "./SlashingRegistryCoordinator.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";
import {RegistryCoordinatorStorage} from "./RegistryCoordinatorStorage.sol";

/**
 * @title A `RegistryCoordinator` that has four registries:
 *      1) a `StakeRegistry` that keeps track of operators' stakes
 *      2) a `BLSApkRegistry` that keeps track of operators' BLS public keys and aggregate BLS public keys for each quorum
 *      3) an `IndexRegistry` that keeps track of an ordered list of operators for each quorum
 *      4) a `SocketRegistry` that keeps track of operators' sockets (arbitrary strings)
 *
 * @author Layr Labs, Inc.
 */
contract RegistryCoordinator is SlashingRegistryCoordinator, RegistryCoordinatorStorage {
    using BitmapUtils for *;

    constructor(
        IRegistryCoordinatorTypes.RegistryCoordinatorParams memory params
    )
        RegistryCoordinatorStorage(params.serviceManager)
        SlashingRegistryCoordinator(
            params.slashingParams.stakeRegistry,
            params.slashingParams.blsApkRegistry,
            params.slashingParams.indexRegistry,
            params.slashingParams.socketRegistry,
            params.slashingParams.allocationManager,
            params.slashingParams.pauserRegistry,
            "v0.0.1"
        )
    {}

    /**
     *
     *                         EXTERNAL FUNCTIONS
     *
     */

    /// @inheritdoc IRegistryCoordinator
    function registerOperator(
        bytes memory quorumNumbers,
        string memory socket,
        IBLSApkRegistryTypes.PubkeyRegistrationParams memory params,
        SignatureWithSaltAndExpiry memory operatorSignature
    ) external virtual onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR) {
        require(!isM2QuorumRegistrationDisabled, M2QuorumRegistrationIsDisabled());
        require(
            quorumNumbers.orderedBytesArrayToBitmap().isSubsetOf(m2QuorumBitmap()),
            OnlyM2QuorumsAllowed()
        );

        // Check if the operator has registered before
        bool operatorRegisteredBefore =
            _operatorInfo[msg.sender].status == OperatorStatus.REGISTERED;

        // register the operator with the registry coordinator
        _registerOperator({
            operator: msg.sender,
            operatorId: _getOrCreateOperatorId(msg.sender, params),
            quorumNumbers: quorumNumbers,
            socket: socket,
            checkMaxOperatorCount: true
        });

        // If the operator has never registered before, register them with the AVSDirectory
        if (!operatorRegisteredBefore) {
            serviceManager.registerOperatorToAVS(msg.sender, operatorSignature);
        }
    }

    /// @inheritdoc IRegistryCoordinator
    function registerOperatorWithChurn(
        bytes calldata quorumNumbers,
        string memory socket,
        IBLSApkRegistryTypes.PubkeyRegistrationParams memory params,
        OperatorKickParam[] memory operatorKickParams,
        SignatureWithSaltAndExpiry memory churnApproverSignature,
        SignatureWithSaltAndExpiry memory operatorSignature
    ) external virtual onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR) {
        require(!isM2QuorumRegistrationDisabled, M2QuorumRegistrationIsDisabled());
        require(
            quorumNumbers.orderedBytesArrayToBitmap().isSubsetOf(m2QuorumBitmap()),
            OnlyM2QuorumsAllowed()
        );

        // Check if the operator has registered before
        bool operatorRegisteredBefore =
            _operatorInfo[msg.sender].status == OperatorStatus.REGISTERED;

        // register the operator with the registry coordinator with churn
        _registerOperatorWithChurn({
            operator: msg.sender,
            operatorId: _getOrCreateOperatorId(msg.sender, params),
            quorumNumbers: quorumNumbers,
            socket: socket,
            operatorKickParams: operatorKickParams,
            churnApproverSignature: churnApproverSignature
        });

        // If the operator has never registered before, register them with the AVSDirectory
        if (!operatorRegisteredBefore) {
            serviceManager.registerOperatorToAVS(msg.sender, operatorSignature);
        }
    }

    /// @inheritdoc IRegistryCoordinator
    function deregisterOperator(
        bytes memory quorumNumbers
    ) external override onlyWhenNotPaused(PAUSED_DEREGISTER_OPERATOR) {
        // Check that the quorum numbers are M2 quorums
        require(
            quorumNumbers.orderedBytesArrayToBitmap().isSubsetOf(m2QuorumBitmap()),
            OnlyM2QuorumsAllowed()
        );

        _deregisterOperator({operator: msg.sender, quorumNumbers: quorumNumbers});
    }

    /// @inheritdoc IRegistryCoordinator
    function disableM2QuorumRegistration() external onlyOwner {
        require(!isM2QuorumRegistrationDisabled, M2QuorumRegistrationIsDisabled());

        isM2QuorumRegistrationDisabled = true;

        emit M2QuorumRegistrationDisabled();
    }

    /**
     *
     *                            INTERNAL FUNCTIONS
     *
     */

    /// @dev override the _kickOperator function to handle M2 quorum forced deregistration
    function _kickOperator(
        address operator,
        bytes memory quorumNumbers
    ) internal virtual override {
        OperatorInfo storage operatorInfo = _operatorInfo[operator];
        uint192 quorumsToRemove =
            uint192(BitmapUtils.orderedBytesArrayToBitmap(quorumNumbers, quorumCount));
        if (operatorInfo.status == OperatorStatus.REGISTERED && !quorumsToRemove.isEmpty()) {
            // Allocate memory once outside the loop
            bytes memory singleQuorumNumber = new bytes(1);
            // For each quorum number, check if it's an M2 quorum
            for (uint256 i = 0; i < quorumNumbers.length; i++) {
                singleQuorumNumber[0] = quorumNumbers[i];

                if (isM2Quorum(uint8(quorumNumbers[i]))) {
                    // For M2 quorums, use _deregisterOperator
                    _deregisterOperator({operator: operator, quorumNumbers: singleQuorumNumber});
                } else {
                    // For non-M2 quorums, use _forceDeregisterOperator
                    _forceDeregisterOperator(operator, singleQuorumNumber);
                }
            }
        }
    }

    /// @dev override the _forceDeregisterOperator function to handle M2 quorum deregistration
    function _forceDeregisterOperator(
        address operator,
        bytes memory quorumNumbers
    ) internal virtual override {
        // filter out M2 quorums from the quorum numbers
        uint256 operatorSetBitmap =
            quorumNumbers.orderedBytesArrayToBitmap().minus(m2QuorumBitmap());
        if (!operatorSetBitmap.isEmpty()) {
            // call the parent _forceDeregisterOperator function for operator sets quorums
            super._forceDeregisterOperator(operator, operatorSetBitmap.bitmapToBytesArray());
        }
    }

    /// @dev Hook to prevent any new quorums from being created if operator sets are not enabled
    function _beforeCreateQuorum(
        uint8
    ) internal virtual override {
        // If operator sets are not enabled, set the m2 quorum bitmap to the current m2 quorum bitmap
        // and enable operator sets
        if (!operatorSetsEnabled) {
            _enableOperatorSets();
        }
    }

    /// @dev Internal function to enable operator sets and set the M2 quorum bitmap
    function _enableOperatorSets() internal {
        require(!operatorSetsEnabled, OperatorSetsAlreadyEnabled());
        _m2QuorumBitmap = _getTotalQuorumBitmap();
        operatorSetsEnabled = true;
        emit OperatorSetsEnabled();
    }

    /// @dev Hook to allow for any post-deregister logic
    function _afterDeregisterOperator(
        address operator,
        bytes32,
        bytes memory,
        uint192 newBitmap
    ) internal virtual override {
        // Bitmap representing all quorums including M2 and OperatorSet quorums
        uint256 totalQuorumBitmap = _getTotalQuorumBitmap();
        // Bitmap representing only OperatorSet quorums. Equal to 0 if operatorSets not enabled
        uint256 operatorSetQuorumBitmap = totalQuorumBitmap.minus(m2QuorumBitmap());
        // Operators updated M2 quorum bitmap, clear all the bits of operatorSetQuorumBitmap which gives the
        // operator's M2 quorum bitmap.
        uint256 operatorM2QuorumBitmap = newBitmap.minus(operatorSetQuorumBitmap);
        // If the operator is no longer registered for any M2 quorums, update their status and deregister
        // them from the AVS via the EigenLayer core contracts
        if (operatorM2QuorumBitmap.isEmpty()) {
            serviceManager.deregisterOperatorFromAVS(operator);
        }
    }

    /**
     * @dev Helper function to update operator stakes and deregister operators with insufficient stake
     * This function handles two cases:
     * 1. Operators who no longer meet the minimum stake requirement for a quorum
     * 2. Operators who have been force-deregistered from the AllocationManager but not from this contract
     * (e.g. due to out of gas errors in the deregistration callback)
     * @param operators The list of operators to check and update
     * @param operatorIds The corresponding operator IDs
     * @param quorumNumber The quorum number to check stakes for
     */
    function _updateOperatorsStakes(
        address[] memory operators,
        bytes32[] memory operatorIds,
        uint8 quorumNumber
    ) internal virtual override {
        bytes memory singleQuorumNumber = new bytes(1);
        singleQuorumNumber[0] = bytes1(quorumNumber);
        bool[] memory doesNotMeetStakeThreshold =
            stakeRegistry.updateOperatorsStake(operators, operatorIds, quorumNumber);

        for (uint256 i = 0; i < operators.length; ++i) {
            if (doesNotMeetStakeThreshold[i]) {
                _kickOperator(operators[i], singleQuorumNumber);
            }
        }
    }

    /// @notice Return bitmap representing all quorums(Legacy M2 and OperatorSet) quorums
    function _getTotalQuorumBitmap() internal view returns (uint256) {
        // This creates a number where all bits up to quorumCount are set to 1
        // For example:
        // quorumCount = 3 -> 0111 (7 in decimal)
        // quorumCount = 5 -> 011111 (31 in decimal)
        // This is a safe operation since we limit MAX_QUORUM_COUNT to 192
        return (1 << quorumCount) - 1;
    }

    /**
     *
     *                            VIEW FUNCTIONS
     *
     */

    /// @dev Returns a bitmap with all bits set up to `quorumCount`. Used for bit-masking quorum numbers
    /// and differentiating between operator sets and M2 quorums
    function m2QuorumBitmap() public view returns (uint256) {
        // If operator sets are enabled, return the current m2 quorum bitmap
        if (operatorSetsEnabled) {
            return _m2QuorumBitmap;
        }

        return _getTotalQuorumBitmap();
    }

    /// @notice Returns true if the quorum number is an M2 quorum
    function isM2Quorum(
        uint8 quorumNumber
    ) public view returns (bool) {
        return m2QuorumBitmap().isSet(quorumNumber);
    }

    /**
     * @notice Returns the domain separator used for EIP-712 signatures
     * @return The domain separator
     */
    function domainSeparator() external view virtual override returns (bytes32) {
        return _domainSeparatorV4();
    }

    /**
     * @notice Returns the version of the contract
     * @return The version string
     */
    function version()
        public
        view
        virtual
        override(ISemVerMixin, SemVerMixin)
        returns (string memory)
    {
        return "v0.0.1";
    }
}
````

## File: src/RegistryCoordinatorStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IPauserRegistry} from "eigenlayer-contracts/src/contracts/interfaces/IPauserRegistry.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IBLSApkRegistry, IBLSApkRegistryTypes} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";
import {IServiceManager} from "./interfaces/IServiceManager.sol";
import {
    IRegistryCoordinator, IRegistryCoordinatorTypes
} from "./interfaces/IRegistryCoordinator.sol";
import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";

abstract contract RegistryCoordinatorStorage is IRegistryCoordinator {
    /**
     *
     *                            CONSTANTS AND IMMUTABLES
     *
     */

    /// @notice the ServiceManager for this AVS, which forwards calls onto EigenLayer's core contracts
    IServiceManager public immutable serviceManager;

    /**
     *
     *                                    STATE
     *
     */

    /// @notice Whether this AVS allows operator sets for creation/registration
    /// @dev If true, then operator sets may be created and operators may register to operator sets via the AllocationManager
    bool public operatorSetsEnabled;

    /// @notice Whether this AVS allows M2 quorums for registration
    /// @dev If true, operators may **not** register to M2 quorums. Deregistration is still allowed.
    bool public isM2QuorumRegistrationDisabled;

    /// @notice The bitmap containing all M2 quorums. This is only used for existing AVS middlewares that have M2 quorums
    /// and need to call `enableOperatorSets()` to enable operator sets mode.
    uint256 internal _m2QuorumBitmap;

    constructor(
        IServiceManager _serviceManager
    ) {
        serviceManager = _serviceManager;
    }

    uint256[48] private __GAP;
}
````

## File: src/ServiceManagerBase.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {Initializable} from "@openzeppelin-upgrades/contracts/proxy/utils/Initializable.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import {
    ISignatureUtilsMixin,
    ISignatureUtilsMixinTypes
} from "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IRewardsCoordinator} from
    "eigenlayer-contracts/src/contracts/interfaces/IRewardsCoordinator.sol";
import {
    IAllocationManager,
    IAllocationManagerTypes
} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IPermissionController} from
    "eigenlayer-contracts/src/contracts/interfaces/IPermissionController.sol";
import {IPauserRegistry} from "eigenlayer-contracts/src/contracts/interfaces/IPauserRegistry.sol";
import {Pausable} from "eigenlayer-contracts/src/contracts/permissions/Pausable.sol";
import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";

import {ServiceManagerBaseStorage} from "./ServiceManagerBaseStorage.sol";
import {IServiceManager} from "./interfaces/IServiceManager.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";
import {LibMergeSort} from "./libraries/LibMergeSort.sol";

/**
 * @title Minimal implementation of a ServiceManager-type contract.
 * This contract can be inherited from or simply used as a point-of-reference.
 * @author Layr Labs, Inc.
 */
abstract contract ServiceManagerBase is ServiceManagerBaseStorage {
    using SafeERC20 for IERC20;
    using BitmapUtils for *;

    /// @notice when applied to a function, only allows the RegistryCoordinator to call it
    modifier onlyRegistryCoordinator() {
        require(msg.sender == address(_registryCoordinator), OnlyRegistryCoordinator());
        _;
    }

    /// @notice only rewardsInitiator can call createAVSRewardsSubmission
    modifier onlyRewardsInitiator() {
        _checkRewardsInitiator();
        _;
    }

    /// @notice Sets the (immutable) `_registryCoordinator` address
    constructor(
        IAVSDirectory __avsDirectory,
        IRewardsCoordinator __rewardsCoordinator,
        ISlashingRegistryCoordinator __registryCoordinator,
        IStakeRegistry __stakeRegistry,
        IPermissionController __permissionController,
        IAllocationManager __allocationManager
    )
        ServiceManagerBaseStorage(
            __avsDirectory,
            __rewardsCoordinator,
            __registryCoordinator,
            __stakeRegistry,
            __permissionController,
            __allocationManager
        )
    {
        _disableInitializers();
    }

    function __ServiceManagerBase_init(
        address initialOwner,
        address _rewardsInitiator
    ) internal virtual onlyInitializing {
        _transferOwnership(initialOwner);
        _setRewardsInitiator(_rewardsInitiator);
    }

    /// @inheritdoc IServiceManager
    function addPendingAdmin(
        address admin
    ) external onlyOwner {
        _permissionController.addPendingAdmin({account: address(this), admin: admin});
    }

    /// @inheritdoc IServiceManager
    function removePendingAdmin(
        address pendingAdmin
    ) external onlyOwner {
        _permissionController.removePendingAdmin({account: address(this), admin: pendingAdmin});
    }

    /// @inheritdoc IServiceManager
    function removeAdmin(
        address admin
    ) external onlyOwner {
        _permissionController.removeAdmin({account: address(this), admin: admin});
    }

    /// @inheritdoc IServiceManager
    function setAppointee(address appointee, address target, bytes4 selector) external onlyOwner {
        _permissionController.setAppointee({
            account: address(this),
            appointee: appointee,
            target: target,
            selector: selector
        });
    }

    /// @inheritdoc IServiceManager
    function removeAppointee(
        address appointee,
        address target,
        bytes4 selector
    ) external onlyOwner {
        _permissionController.removeAppointee({
            account: address(this),
            appointee: appointee,
            target: target,
            selector: selector
        });
    }

    /**
     * @notice Updates the metadata URI for the AVS
     * @param _metadataURI is the metadata URI for the AVS
     * @dev only callable by the owner
     */
    function updateAVSMetadataURI(
        string memory _metadataURI
    ) public virtual onlyOwner {
        _avsDirectory.updateAVSMetadataURI(_metadataURI);
    }

    /**
     * @notice Creates a new rewards submission to the EigenLayer RewardsCoordinator contract, to be split amongst the
     * set of stakers delegated to operators who are registered to this `avs`
     * @param rewardsSubmissions The rewards submissions being created
     * @dev Only callable by the permissioned rewardsInitiator address
     * @dev The duration of the `rewardsSubmission` cannot exceed `MAX_REWARDS_DURATION`
     * @dev The tokens are sent to the `RewardsCoordinator` contract
     * @dev Strategies must be in ascending order of addresses to check for duplicates
     * @dev This function will revert if the `rewardsSubmission` is malformed,
     * e.g. if the `strategies` and `weights` arrays are of non-equal lengths
     * @dev This function may fail to execute with a large number of submissions due to gas limits. Use a
     * smaller array of submissions if necessary.
     */
    function createAVSRewardsSubmission(
        IRewardsCoordinator.RewardsSubmission[] calldata rewardsSubmissions
    ) public virtual onlyRewardsInitiator {
        for (uint256 i = 0; i < rewardsSubmissions.length; ++i) {
            // transfer token to ServiceManager and approve RewardsCoordinator to transfer again
            // in createAVSRewardsSubmission() call
            rewardsSubmissions[i].token.safeTransferFrom(
                msg.sender, address(this), rewardsSubmissions[i].amount
            );
            rewardsSubmissions[i].token.safeIncreaseAllowance(
                address(_rewardsCoordinator), rewardsSubmissions[i].amount
            );
        }

        _rewardsCoordinator.createAVSRewardsSubmission(rewardsSubmissions);
    }

    /**
     * @notice Creates a new operator-directed rewards submission, to be split amongst the operators and
     * set of stakers delegated to operators who are registered to this `avs`.
     * @param operatorDirectedRewardsSubmissions The operator-directed rewards submissions being created.
     * @dev Only callable by the permissioned rewardsInitiator address
     * @dev The duration of the `rewardsSubmission` cannot exceed `MAX_REWARDS_DURATION`
     * @dev The tokens are sent to the `RewardsCoordinator` contract
     * @dev This contract needs a token approval of sum of all `operatorRewards` in the `operatorDirectedRewardsSubmissions`, before calling this function.
     * @dev Strategies must be in ascending order of addresses to check for duplicates
     * @dev Operators must be in ascending order of addresses to check for duplicates.
     * @dev This function will revert if the `operatorDirectedRewardsSubmissions` is malformed.
     * @dev This function may fail to execute with a large number of submissions due to gas limits. Use a
     * smaller array of submissions if necessary.
     */
    function createOperatorDirectedAVSRewardsSubmission(
        IRewardsCoordinator.OperatorDirectedRewardsSubmission[] calldata
            operatorDirectedRewardsSubmissions
    ) public virtual onlyRewardsInitiator {
        for (uint256 i = 0; i < operatorDirectedRewardsSubmissions.length; ++i) {
            // Calculate total amount of token to transfer
            uint256 totalAmount = 0;
            for (
                uint256 j = 0; j < operatorDirectedRewardsSubmissions[i].operatorRewards.length; ++j
            ) {
                totalAmount += operatorDirectedRewardsSubmissions[i].operatorRewards[j].amount;
            }

            // Transfer token to ServiceManager and approve RewardsCoordinator to transfer again
            // in createOperatorDirectedAVSRewardsSubmission() call
            operatorDirectedRewardsSubmissions[i].token.safeTransferFrom(
                msg.sender, address(this), totalAmount
            );
            operatorDirectedRewardsSubmissions[i].token.safeIncreaseAllowance(
                address(_rewardsCoordinator), totalAmount
            );
        }

        _rewardsCoordinator.createOperatorDirectedAVSRewardsSubmission(
            address(this), operatorDirectedRewardsSubmissions
        );
    }

    /**
     * @notice Forwards a call to Eigenlayer's RewardsCoordinator contract to set the address of the entity that can call `processClaim` on behalf of this contract.
     * @param claimer The address of the entity that can call `processClaim` on behalf of the earner
     * @dev Only callable by the owner.
     */
    function setClaimerFor(
        address claimer
    ) public virtual onlyOwner {
        _rewardsCoordinator.setClaimerFor(claimer);
    }

    /**
     * @notice Forwards a call to EigenLayer's AVSDirectory contract to confirm operator registration with the AVS
     * @param operator The address of the operator to register.
     * @param operatorSignature The signature, salt, and expiry of the operator's signature.
     */
    function registerOperatorToAVS(
        address operator,
        ISignatureUtilsMixinTypes.SignatureWithSaltAndExpiry memory operatorSignature
    ) public virtual onlyRegistryCoordinator {
        _avsDirectory.registerOperatorToAVS(operator, operatorSignature);
    }

    /**
     * @notice Forwards a call to EigenLayer's AVSDirectory contract to confirm operator deregistration from the AVS
     * @param operator The address of the operator to deregister.
     */
    function deregisterOperatorFromAVS(
        address operator
    ) public virtual onlyRegistryCoordinator {
        _avsDirectory.deregisterOperatorFromAVS(operator);
    }

    function deregisterOperatorFromOperatorSets(
        address operator,
        uint32[] memory operatorSetIds
    ) public virtual onlyRegistryCoordinator {
        IAllocationManager.DeregisterParams memory params = IAllocationManagerTypes.DeregisterParams({
            operator: operator,
            avs: address(this),
            operatorSetIds: operatorSetIds
        });
        _allocationManager.deregisterFromOperatorSets(params);
    }

    /**
     * @notice Sets the rewards initiator address
     * @param newRewardsInitiator The new rewards initiator address
     * @dev only callable by the owner
     */
    function setRewardsInitiator(
        address newRewardsInitiator
    ) external onlyOwner {
        _setRewardsInitiator(newRewardsInitiator);
    }

    function _setRewardsInitiator(
        address newRewardsInitiator
    ) internal {
        emit RewardsInitiatorUpdated(rewardsInitiator, newRewardsInitiator);
        rewardsInitiator = newRewardsInitiator;
    }

    /**
     * @notice Returns the list of strategies that the AVS supports for restaking
     * @dev This function is intended to be called off-chain
     * @dev No guarantee is made on uniqueness of each element in the returned array.
     *      The off-chain service should do that validation separately
     */
    function getRestakeableStrategies() external view virtual returns (address[] memory) {
        uint256 quorumCount = _registryCoordinator.quorumCount();

        if (quorumCount == 0) {
            return new address[](0);
        }

        uint256 strategyCount;
        for (uint256 i = 0; i < quorumCount; i++) {
            strategyCount += _stakeRegistry.strategyParamsLength(uint8(i));
        }

        address[] memory restakedStrategies = new address[](strategyCount);
        uint256 index = 0;
        for (uint256 i = 0; i < _registryCoordinator.quorumCount(); i++) {
            uint256 strategyParamsLength = _stakeRegistry.strategyParamsLength(uint8(i));
            for (uint256 j = 0; j < strategyParamsLength; j++) {
                restakedStrategies[index] =
                    address(_stakeRegistry.strategyParamsByIndex(uint8(i), j).strategy);
                index++;
            }
        }
        return restakedStrategies;
    }

    /**
     * @notice Returns the list of strategies that the operator has potentially restaked on the AVS
     * @param operator The address of the operator to get restaked strategies for
     * @dev This function is intended to be called off-chain
     * @dev No guarantee is made on whether the operator has shares for a strategy in a quorum or uniqueness
     *      of each element in the returned array. The off-chain service should do that validation separately
     */
    function getOperatorRestakedStrategies(
        address operator
    ) external view virtual returns (address[] memory) {
        bytes32 operatorId = _registryCoordinator.getOperatorId(operator);
        uint192 operatorBitmap = _registryCoordinator.getCurrentQuorumBitmap(operatorId);

        if (operatorBitmap == 0 || _registryCoordinator.quorumCount() == 0) {
            return new address[](0);
        }

        // Get number of strategies for each quorum in operator bitmap
        bytes memory operatorRestakedQuorums = BitmapUtils.bitmapToBytesArray(operatorBitmap);
        uint256 strategyCount;
        for (uint256 i = 0; i < operatorRestakedQuorums.length; i++) {
            strategyCount += _stakeRegistry.strategyParamsLength(uint8(operatorRestakedQuorums[i]));
        }

        // Get strategies for each quorum in operator bitmap
        address[] memory restakedStrategies = new address[](strategyCount);
        uint256 index = 0;
        for (uint256 i = 0; i < operatorRestakedQuorums.length; i++) {
            uint8 quorum = uint8(operatorRestakedQuorums[i]);
            uint256 strategyParamsLength = _stakeRegistry.strategyParamsLength(quorum);
            for (uint256 j = 0; j < strategyParamsLength; j++) {
                restakedStrategies[index] =
                    address(_stakeRegistry.strategyParamsByIndex(quorum, j).strategy);
                index++;
            }
        }
        return restakedStrategies;
    }

    /// @notice Returns the EigenLayer AVSDirectory contract.
    function avsDirectory() external view override returns (address) {
        return address(_avsDirectory);
    }

    function _checkRewardsInitiator() internal view {
        require(msg.sender == rewardsInitiator, OnlyRewardsInitiator());
    }
}
````

## File: src/ServiceManagerBaseStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";

import {IServiceManager} from "./interfaces/IServiceManager.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";

import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IRewardsCoordinator} from
    "eigenlayer-contracts/src/contracts/interfaces/IRewardsCoordinator.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IPermissionController} from
    "eigenlayer-contracts/src/contracts/interfaces/IPermissionController.sol";

/**
 * @title Storage variables for the `ServiceManagerBase` contract.
 * @author Layr Labs, Inc.
 * @notice This storage contract is separate from the logic to simplify the upgrade process.
 */
abstract contract ServiceManagerBaseStorage is IServiceManager, OwnableUpgradeable {
    /**
     *
     *                            CONSTANTS AND IMMUTABLES
     *
     */
    IAVSDirectory internal immutable _avsDirectory;
    IAllocationManager internal immutable _allocationManager;
    IRewardsCoordinator internal immutable _rewardsCoordinator;
    ISlashingRegistryCoordinator internal immutable _registryCoordinator;
    IStakeRegistry internal immutable _stakeRegistry;
    IPermissionController internal immutable _permissionController;

    /**
     *
     *                            STATE VARIABLES
     *
     */

    /// @notice The address of the entity that can initiate rewards
    address public rewardsInitiator;

    /// @notice Sets the (immutable) `_avsDirectory`, `_rewardsCoordinator`, `_registryCoordinator`, `_stakeRegistry`, and `_allocationManager` addresses
    constructor(
        IAVSDirectory __avsDirectory,
        IRewardsCoordinator __rewardsCoordinator,
        ISlashingRegistryCoordinator __registryCoordinator,
        IStakeRegistry __stakeRegistry,
        IPermissionController __permissionController,
        IAllocationManager __allocationManager
    ) {
        _avsDirectory = __avsDirectory;
        _rewardsCoordinator = __rewardsCoordinator;
        _registryCoordinator = __registryCoordinator;
        _stakeRegistry = __stakeRegistry;
        _permissionController = __permissionController;
        _allocationManager = __allocationManager;
    }

    // storage gap for upgradeability
    uint256[49] private __GAP;
}
````

## File: src/ServiceManagerRouter.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IServiceManagerUI} from "./interfaces/IServiceManagerUI.sol";

/**
 * @title Contract that proxies calls to a ServiceManager contract.
 * This contract is designed to be used by off-chain services which need
 * errors to be handled gracefully.
 * @author Layr Labs, Inc.
 */
contract ServiceManagerRouter {
    address public constant FAILED_CALL_ADDRESS =
        address(0x000000000000000000000000000000000000dEaD);

    /**
     * @notice Returns the list of strategies that the AVS supports for restaking
     * @param serviceManager Address of AVS's ServiceManager contract
     */
    function getRestakeableStrategies(
        address serviceManager
    ) external view returns (address[] memory) {
        bytes memory data =
            abi.encodeWithSelector(IServiceManagerUI.getRestakeableStrategies.selector);
        return _makeCall(serviceManager, data);
    }

    /**
     * @notice Returns the list of strategies that the operator has potentially restaked on the AVS
     * @param serviceManager Address of AVS's ServiceManager contract
     * @param operator Address of the operator to get restaked strategies for
     */
    function getOperatorRestakedStrategies(
        address serviceManager,
        address operator
    ) external view returns (address[] memory) {
        bytes memory data = abi.encodeWithSelector(
            IServiceManagerUI.getOperatorRestakedStrategies.selector, operator
        );
        return _makeCall(serviceManager, data);
    }

    /**
     * @notice Internal helper function to make static calls
     * @dev Handles calls to contracts that don't implement the given function and to EOAs by
     *      returning a failed call address
     */
    function _makeCall(
        address serviceManager,
        bytes memory data
    ) internal view returns (address[] memory) {
        (bool success, bytes memory strategiesBytes) = serviceManager.staticcall(data);
        if (success && strategiesBytes.length > 0) {
            return abi.decode(strategiesBytes, (address[]));
        } else {
            address[] memory failedCall = new address[](1);
            failedCall[0] = FAILED_CALL_ADDRESS;
            return failedCall;
        }
    }
}
````

## File: src/SlashingRegistryCoordinator.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IPauserRegistry} from "eigenlayer-contracts/src/contracts/interfaces/IPauserRegistry.sol";
import {ISignatureUtilsMixin} from
    "eigenlayer-contracts/src/contracts/interfaces/ISignatureUtilsMixin.sol";
import {IStrategy} from "eigenlayer-contracts/src/contracts/interfaces/IStrategy.sol";
import {IAVSRegistrar} from "eigenlayer-contracts/src/contracts/interfaces/IAVSRegistrar.sol";
import {
    IAllocationManager,
    OperatorSet,
    IAllocationManagerTypes
} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISemVerMixin} from "eigenlayer-contracts/src/contracts/interfaces/ISemVerMixin.sol";
import {AllocationManager} from "eigenlayer-contracts/src/contracts/core/AllocationManager.sol";
import {SemVerMixin} from "eigenlayer-contracts/src/contracts/mixins/SemVerMixin.sol";

import {IBLSApkRegistry, IBLSApkRegistryTypes} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry, IStakeRegistryTypes} from "./interfaces/IStakeRegistry.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";
import {BN254} from "./libraries/BN254.sol";
import {SignatureCheckerLib} from "./libraries/SignatureCheckerLib.sol";
import {QuorumBitmapHistoryLib} from "./libraries/QuorumBitmapHistoryLib.sol";

import {OwnableUpgradeable} from "@openzeppelin-upgrades/contracts/access/OwnableUpgradeable.sol";
import {Initializable} from "@openzeppelin-upgrades/contracts/proxy/utils/Initializable.sol";
import {EIP712Upgradeable} from
    "@openzeppelin-upgrades/contracts/utils/cryptography/EIP712Upgradeable.sol";

import {Pausable} from "eigenlayer-contracts/src/contracts/permissions/Pausable.sol";
import {SlashingRegistryCoordinatorStorage} from "./SlashingRegistryCoordinatorStorage.sol";

/**
 * @title A `RegistryCoordinator` that has four registries:
 *      1) a `StakeRegistry` that keeps track of operators' stakes
 *      2) a `BLSApkRegistry` that keeps track of operators' BLS public keys and aggregate BLS public keys for each quorum
 *      3) an `IndexRegistry` that keeps track of an ordered list of operators for each quorum
 *      4) a `SocketRegistry` that keeps track of operators' sockets (arbitrary strings)
 *
 * @author Layr Labs, Inc.
 */
contract SlashingRegistryCoordinator is
    Initializable,
    SemVerMixin,
    Pausable,
    OwnableUpgradeable,
    SlashingRegistryCoordinatorStorage,
    EIP712Upgradeable,
    ISignatureUtilsMixin
{
    using BitmapUtils for *;
    using BN254 for BN254.G1Point;

    modifier onlyAllocationManager() {
        _checkAllocationManager();
        _;
    }

    modifier onlyEjector() {
        _checkEjector();
        _;
    }

    /// @dev Checks that `quorumNumber` corresponds to a quorum that has been created
    /// via `initialize` or `createQuorum`
    modifier quorumExists(
        uint8 quorumNumber
    ) {
        _checkQuorumExists(quorumNumber);
        _;
    }

    constructor(
        IStakeRegistry _stakeRegistry,
        IBLSApkRegistry _blsApkRegistry,
        IIndexRegistry _indexRegistry,
        ISocketRegistry _socketRegistry,
        IAllocationManager _allocationManager,
        IPauserRegistry _pauserRegistry,
        string memory _version
    )
        SlashingRegistryCoordinatorStorage(
            _stakeRegistry,
            _blsApkRegistry,
            _indexRegistry,
            _socketRegistry,
            _allocationManager
        )
        SemVerMixin(_version)
        Pausable(_pauserRegistry)
    {
        _disableInitializers();
    }

    /**
     *
     *                         EXTERNAL FUNCTIONS
     *
     */
    function initialize(
        address initialOwner,
        address churnApprover,
        address ejector,
        uint256 initialPausedStatus,
        address avs
    ) external initializer {
        __EIP712_init("AVSRegistryCoordinator", "v0.0.1");
        _transferOwnership(initialOwner);
        _setChurnApprover(churnApprover);
        _setPausedStatus(initialPausedStatus);
        _setEjector(ejector);
        _setAVS(avs);
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function createTotalDelegatedStakeQuorum(
        OperatorSetParam memory operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] memory strategyParams
    ) external virtual onlyOwner {
        _createQuorum(
            operatorSetParams,
            minimumStake,
            strategyParams,
            IStakeRegistryTypes.StakeType.TOTAL_DELEGATED,
            0
        );
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function createSlashableStakeQuorum(
        OperatorSetParam memory operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] memory strategyParams,
        uint32 lookAheadPeriod
    ) external virtual onlyOwner {
        _createQuorum(
            operatorSetParams,
            minimumStake,
            strategyParams,
            IStakeRegistryTypes.StakeType.TOTAL_SLASHABLE,
            lookAheadPeriod
        );
    }

    /// @inheritdoc IAVSRegistrar
    function registerOperator(
        address operator,
        address avs,
        uint32[] memory operatorSetIds,
        bytes calldata data
    ) external virtual override onlyAllocationManager onlyWhenNotPaused(PAUSED_REGISTER_OPERATOR) {
        require(supportsAVS(avs), InvalidAVS());
        bytes memory quorumNumbers = _getQuorumNumbers(operatorSetIds);

        (
            RegistrationType registrationType,
            string memory socket,
            IBLSApkRegistryTypes.PubkeyRegistrationParams memory params
        ) = abi.decode(
            data, (RegistrationType, string, IBLSApkRegistryTypes.PubkeyRegistrationParams)
        );

        /**
         * If the operator has NEVER registered a pubkey before, use `params` to register
         * their pubkey in blsApkRegistry
         *
         * If the operator HAS registered a pubkey, `params` is ignored and the pubkey hash
         * (operatorId) is fetched instead
         */
        bytes32 operatorId = _getOrCreateOperatorId(operator, params);

        if (registrationType == RegistrationType.NORMAL) {
            uint32[] memory numOperatorsPerQuorum = _registerOperator({
                operator: operator,
                operatorId: operatorId,
                quorumNumbers: quorumNumbers,
                socket: socket,
                checkMaxOperatorCount: true
            }).numOperatorsPerQuorum;

            // For each quorum, validate that the new operator count does not exceed the maximum
            // (If it does, an operator needs to be replaced -- see `registerOperatorWithChurn`)
            for (uint256 i = 0; i < quorumNumbers.length; i++) {
                uint8 quorumNumber = uint8(quorumNumbers[i]);

                require(
                    numOperatorsPerQuorum[i] <= _quorumParams[quorumNumber].maxOperatorCount,
                    MaxOperatorCountReached()
                );
            }
        } else if (registrationType == RegistrationType.CHURN) {
            // Decode registration data from bytes
            (
                ,
                ,
                ,
                OperatorKickParam[] memory operatorKickParams,
                SignatureWithSaltAndExpiry memory churnApproverSignature
            ) = abi.decode(
                data,
                (
                    RegistrationType,
                    string,
                    IBLSApkRegistryTypes.PubkeyRegistrationParams,
                    OperatorKickParam[],
                    SignatureWithSaltAndExpiry
                )
            );
            _registerOperatorWithChurn({
                operator: operator,
                operatorId: operatorId,
                quorumNumbers: quorumNumbers,
                socket: socket,
                operatorKickParams: operatorKickParams,
                churnApproverSignature: churnApproverSignature
            });
        } else {
            revert InvalidRegistrationType();
        }
    }

    /// @inheritdoc IAVSRegistrar
    function deregisterOperator(
        address operator,
        address avs,
        uint32[] memory operatorSetIds
    ) external override onlyAllocationManager onlyWhenNotPaused(PAUSED_DEREGISTER_OPERATOR) {
        require(supportsAVS(avs), InvalidAVS());
        bytes memory quorumNumbers = _getQuorumNumbers(operatorSetIds);
        _deregisterOperator({operator: operator, quorumNumbers: quorumNumbers});
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function updateOperators(
        address[] memory operators
    ) external override onlyWhenNotPaused(PAUSED_UPDATE_OPERATOR) {
        for (uint256 i = 0; i < operators.length; i++) {
            // create single-element arrays for the operator and operatorId
            address[] memory singleOperator = new address[](1);
            singleOperator[0] = operators[i];
            bytes32[] memory singleOperatorId = new bytes32[](1);
            singleOperatorId[0] = _operatorInfo[operators[i]].operatorId;

            uint192 currentBitmap = _currentOperatorBitmap(singleOperatorId[0]);
            bytes memory quorumNumbers = currentBitmap.bitmapToBytesArray();
            for (uint256 j = 0; j < quorumNumbers.length; j++) {
                // update the operator's stake for each quorum
                _updateOperatorsStakes(singleOperator, singleOperatorId, uint8(quorumNumbers[j]));
            }
        }
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function updateOperatorsForQuorum(
        address[][] memory operatorsPerQuorum,
        bytes calldata quorumNumbers
    ) external onlyWhenNotPaused(PAUSED_UPDATE_OPERATOR) {
        // Input validation
        // - all quorums should exist (checked against `quorumCount` in orderedBytesArrayToBitmap)
        // - there should be no duplicates in `quorumNumbers`
        // - there should be one list of operators per quorum
        BitmapUtils.orderedBytesArrayToBitmap(quorumNumbers, quorumCount);
        require(operatorsPerQuorum.length == quorumNumbers.length, InputLengthMismatch());

        // For each quorum, update ALL registered operators
        for (uint256 i = 0; i < quorumNumbers.length; ++i) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);

            // Ensure we've passed in the correct number of operators for this quorum
            address[] memory currQuorumOperators = operatorsPerQuorum[i];
            require(
                currQuorumOperators.length == indexRegistry.totalOperatorsForQuorum(quorumNumber),
                QuorumOperatorCountMismatch()
            );

            bytes32[] memory operatorIds = new bytes32[](currQuorumOperators.length);
            address prevOperatorAddress = address(0);
            // For each operator:
            // - check that they are registered for this quorum
            // - check that their address is strictly greater than the last operator
            // ... then, update their stakes
            for (uint256 j = 0; j < currQuorumOperators.length; ++j) {
                address operator = currQuorumOperators[j];

                operatorIds[j] = _operatorInfo[operator].operatorId;
                {
                    uint192 currentBitmap = _currentOperatorBitmap(operatorIds[j]);
                    // Check that the operator is registered
                    require(
                        BitmapUtils.isSet(currentBitmap, quorumNumber), NotRegisteredForQuorum()
                    );
                    // Prevent duplicate operators
                    require(operator > prevOperatorAddress, NotSorted());
                }

                prevOperatorAddress = operator;
            }

            _updateOperatorsStakes(currQuorumOperators, operatorIds, quorumNumber);

            // Update timestamp that all operators in quorum have been updated all at once
            quorumUpdateBlockNumber[quorumNumber] = block.number;
            emit QuorumBlockNumberUpdated(quorumNumber, block.number);
        }
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function updateSocket(
        string memory socket
    ) external {
        require(_operatorInfo[msg.sender].status == OperatorStatus.REGISTERED, NotRegistered());
        _setOperatorSocket(_operatorInfo[msg.sender].operatorId, socket);
    }

    /**
     *
     *                         EXTERNAL FUNCTIONS - EJECTOR
     *
     */

    /// @inheritdoc ISlashingRegistryCoordinator
    function ejectOperator(
        address operator,
        bytes memory quorumNumbers
    ) public virtual onlyEjector {
        lastEjectionTimestamp[operator] = block.timestamp;
        _kickOperator(operator, quorumNumbers);
    }

    /**
     *
     *                         EXTERNAL FUNCTIONS - OWNER
     *
     */

    /// @inheritdoc ISlashingRegistryCoordinator
    function setOperatorSetParams(
        uint8 quorumNumber,
        OperatorSetParam memory operatorSetParams
    ) external onlyOwner quorumExists(quorumNumber) {
        _setOperatorSetParams(quorumNumber, operatorSetParams);
    }

    /**
     * @notice Sets the churnApprover, which approves operator registration with churn
     * (see `registerOperatorWithChurn`)
     * @param _churnApprover the new churn approver
     * @dev only callable by the owner
     */
    function setChurnApprover(
        address _churnApprover
    ) external onlyOwner {
        _setChurnApprover(_churnApprover);
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function setEjector(
        address _ejector
    ) external onlyOwner {
        _setEjector(_ejector);
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function setAVS(
        address _avs
    ) external onlyOwner {
        _setAVS(_avs);
    }

    /// @inheritdoc ISlashingRegistryCoordinator
    function setEjectionCooldown(
        uint256 _ejectionCooldown
    ) external onlyOwner {
        uint256 prevEjectionCooldown = ejectionCooldown;
        ejectionCooldown = _ejectionCooldown;
        emit EjectionCooldownUpdated(prevEjectionCooldown, _ejectionCooldown);
    }

    /**
     *
     *                         INTERNAL FUNCTIONS
     *
     */

    /**
     * @notice Internal function to handle operator ejection logic
     * @param operator The operator to force deregister from the avs
     * @param quorumNumbers The quorum numbers to eject the operator from
     */
    function _kickOperator(address operator, bytes memory quorumNumbers) internal virtual {
        OperatorInfo storage operatorInfo = _operatorInfo[operator];
        // Only proceed if operator is currently registered
        require(operatorInfo.status == OperatorStatus.REGISTERED, OperatorNotRegistered());

        bytes32 operatorId = operatorInfo.operatorId;
        uint192 quorumsToRemove =
            uint192(BitmapUtils.orderedBytesArrayToBitmap(quorumNumbers, quorumCount));
        uint192 currentBitmap = _currentOperatorBitmap(operatorId);

        // Check if operator is registered for all quorums we're trying to remove them from
        if (quorumsToRemove.isSubsetOf(currentBitmap)) {
            _forceDeregisterOperator(operator, quorumNumbers);
        }
    }

    /**
     * @notice Register the operator for one or more quorums. This method updates the
     * operator's quorum bitmap, socket, and status, then registers them with each registry.
     */
    function _registerOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        string memory socket,
        bool checkMaxOperatorCount
    ) internal virtual returns (RegisterResults memory results) {
        /**
         * Get bitmap of quorums to register for and operator's current bitmap. Validate that:
         * - we're trying to register for at least 1 quorum
         * - the quorums we're registering for exist (checked against `quorumCount` in orderedBytesArrayToBitmap)
         * - the operator is not currently registered for any quorums we're registering for
         * Then, calculate the operator's new bitmap after registration
         */
        uint192 quorumsToAdd =
            uint192(BitmapUtils.orderedBytesArrayToBitmap(quorumNumbers, quorumCount));
        uint192 currentBitmap = _currentOperatorBitmap(operatorId);

        // call hook to allow for any pre-register logic
        _beforeRegisterOperator(operator, operatorId, quorumNumbers, currentBitmap);

        require(!quorumsToAdd.isEmpty(), BitmapEmpty());
        require(quorumsToAdd.noBitsInCommon(currentBitmap), AlreadyRegisteredForQuorums());
        uint192 newBitmap = uint192(currentBitmap.plus(quorumsToAdd));

        // Check that the operator can reregister if ejected
        require(
            lastEjectionTimestamp[operator] + ejectionCooldown < block.timestamp,
            CannotReregisterYet()
        );

        /**
         * Update operator's bitmap, socket, and status. Only update operatorInfo if needed:
         * if we're `REGISTERED`, the operatorId and status are already correct.
         */
        _updateOperatorBitmap({operatorId: operatorId, newBitmap: newBitmap});

        _setOperatorSocket(operatorId, socket);

        // If the operator wasn't registered for any quorums, update their status
        // and register them with this AVS in EigenLayer core (DelegationManager)
        if (_operatorInfo[operator].status != OperatorStatus.REGISTERED) {
            _operatorInfo[operator] = OperatorInfo(operatorId, OperatorStatus.REGISTERED);
            emit OperatorRegistered(operator, operatorId);
        }

        // Register the operator with the BLSApkRegistry, StakeRegistry, and IndexRegistry
        blsApkRegistry.registerOperator(operator, quorumNumbers);
        (results.operatorStakes, results.totalStakes) =
            stakeRegistry.registerOperator(operator, operatorId, quorumNumbers);
        results.numOperatorsPerQuorum = indexRegistry.registerOperator(operatorId, quorumNumbers);

        if (checkMaxOperatorCount) {
            for (uint256 i = 0; i < quorumNumbers.length; i++) {
                OperatorSetParam memory operatorSetParams = _quorumParams[uint8(quorumNumbers[i])];
                require(
                    results.numOperatorsPerQuorum[i] <= operatorSetParams.maxOperatorCount,
                    MaxOperatorCountReached()
                );
            }
        }

        // call hook to allow for any post-register logic
        _afterRegisterOperator(operator, operatorId, quorumNumbers, newBitmap);

        return results;
    }

    function _registerOperatorWithChurn(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        string memory socket,
        OperatorKickParam[] memory operatorKickParams,
        SignatureWithSaltAndExpiry memory churnApproverSignature
    ) internal virtual {
        require(operatorKickParams.length == quorumNumbers.length, InputLengthMismatch());

        // Verify the churn approver's signature for the registering operator and kick params
        _verifyChurnApproverSignature({
            registeringOperator: operator,
            registeringOperatorId: operatorId,
            operatorKickParams: operatorKickParams,
            churnApproverSignature: churnApproverSignature
        });

        // Register the operator in each of the registry contracts and update the operator's
        // quorum bitmap and registration status
        RegisterResults memory results = _registerOperator({
            operator: operator,
            operatorId: operatorId,
            quorumNumbers: quorumNumbers,
            socket: socket,
            checkMaxOperatorCount: false
        });

        // Check that each quorum's operator count is below the configured maximum. If the max
        // is exceeded, use `operatorKickParams` to deregister an existing operator to make space
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            OperatorSetParam memory operatorSetParams = _quorumParams[uint8(quorumNumbers[i])];

            /**
             * If the new operator count for any quorum exceeds the maximum, validate
             * that churn can be performed, then deregister the specified operator
             */
            if (results.numOperatorsPerQuorum[i] > operatorSetParams.maxOperatorCount) {
                _validateChurn({
                    quorumNumber: uint8(quorumNumbers[i]),
                    totalQuorumStake: results.totalStakes[i],
                    newOperator: operator,
                    newOperatorStake: results.operatorStakes[i],
                    kickParams: operatorKickParams[i],
                    setParams: operatorSetParams
                });

                bytes memory singleQuorumNumber = new bytes(1);
                singleQuorumNumber[0] = quorumNumbers[i];
                _kickOperator(operatorKickParams[i].operator, singleQuorumNumber);
            }
        }
    }

    /**
     * @dev Deregister the operator from one or more quorums
     * This method updates the operator's quorum bitmap and status, then deregisters
     * the operator with the BLSApkRegistry, IndexRegistry, and StakeRegistry
     * @param operator the operator to deregister
     * @param quorumNumbers the quorum numbers to deregister from
     * the core EigenLayer contract AllocationManager
     */
    function _deregisterOperator(address operator, bytes memory quorumNumbers) internal virtual {
        // Fetch the operator's info and ensure they are registered
        OperatorInfo storage operatorInfo = _operatorInfo[operator];
        bytes32 operatorId = operatorInfo.operatorId;
        uint192 currentBitmap = _currentOperatorBitmap(operatorId);

        // call hook to allow for any pre-deregister logic
        _beforeDeregisterOperator(operator, operatorId, quorumNumbers, currentBitmap);

        require(operatorInfo.status == OperatorStatus.REGISTERED, NotRegistered());

        /**
         * Get bitmap of quorums to deregister from and operator's current bitmap. Validate that:
         * - we're trying to deregister from at least 1 quorum
         * - the quorums we're deregistering from exist (checked against `quorumCount` in orderedBytesArrayToBitmap)
         * - the operator is currently registered for any quorums we're trying to deregister from
         * Then, calculate the operator's new bitmap after deregistration
         */
        uint192 quorumsToRemove =
            uint192(BitmapUtils.orderedBytesArrayToBitmap(quorumNumbers, quorumCount));
        require(!quorumsToRemove.isEmpty(), BitmapCannotBeZero());
        require(quorumsToRemove.isSubsetOf(currentBitmap), NotRegisteredForQuorum());
        uint192 newBitmap = uint192(currentBitmap.minus(quorumsToRemove));

        // Update operator's bitmap and status
        _updateOperatorBitmap({operatorId: operatorId, newBitmap: newBitmap});

        // If the operator is no longer registered for any quorums, update their status and deregister
        // them from the AVS via the EigenLayer core contracts
        if (newBitmap.isEmpty()) {
            _operatorInfo[operator].status = OperatorStatus.DEREGISTERED;
            emit OperatorDeregistered(operator, operatorId);
        }

        // Deregister operator with each of the registry contracts
        blsApkRegistry.deregisterOperator(operator, quorumNumbers);
        stakeRegistry.deregisterOperator(operatorId, quorumNumbers);
        indexRegistry.deregisterOperator(operatorId, quorumNumbers);

        // call hook to allow for any post-deregister logic
        _afterDeregisterOperator(operator, operatorId, quorumNumbers, newBitmap);
    }

    /**
     * @notice Helper function to handle operator set deregistration for OperatorSets quorums. This is used
     * when an operator is force-deregistered from a set of quorums.
     * Due to deregistration being possible in the AllocationManager but not in the AVS as a result of the
     * try/catch in `AllocationManager.deregisterFromOperatorSets`, we need to first check that the operator
     * is not already deregistered from the OperatorSet in the AllocationManager.
     * @param operator The operator to deregister
     * @param quorumNumbers The quorum numbers the operator is force-deregistered from
     */
    function _forceDeregisterOperator(
        address operator,
        bytes memory quorumNumbers
    ) internal virtual {
        allocationManager.deregisterFromOperatorSets(
            IAllocationManagerTypes.DeregisterParams({
                operator: operator,
                avs: avs,
                operatorSetIds: _getOperatorSetIds(quorumNumbers)
            })
        );
    }

    /**
     * @dev Helper function to update operator stakes and deregister operators with insufficient stake
     * This function handles two cases:
     * 1. Operators who no longer meet the minimum stake requirement for a quorum
     * 2. Operators who have been force-deregistered from the AllocationManager but not from this contract
     * (e.g. due to out of gas errors in the deregistration callback)
     * @param operators The list of operators to check and update
     * @param operatorIds The corresponding operator IDs
     * @param quorumNumber The quorum number to check stakes for
     */
    function _updateOperatorsStakes(
        address[] memory operators,
        bytes32[] memory operatorIds,
        uint8 quorumNumber
    ) internal virtual {
        bytes memory singleQuorumNumber = new bytes(1);
        singleQuorumNumber[0] = bytes1(quorumNumber);
        bool[] memory doesNotMeetStakeThreshold =
            stakeRegistry.updateOperatorsStake(operators, operatorIds, quorumNumber);
        for (uint256 j = 0; j < operators.length; ++j) {
            // If the operator does not have the minimum stake, they need to be force deregistered.
            if (doesNotMeetStakeThreshold[j]) {
                _kickOperator(operators[j], singleQuorumNumber);
            }
        }
    }

    /**
     * @notice Checks if the caller is the ejector
     * @dev Reverts if the caller is not the ejector
     */
    function _checkEjector() internal view {
        require(msg.sender == ejector, OnlyEjector());
    }

    function _checkAllocationManager() internal view {
        require(msg.sender == address(allocationManager), OnlyAllocationManager());
    }

    /**
     * @notice Checks if a quorum exists
     * @param quorumNumber The quorum number to check
     * @dev Reverts if the quorum does not exist
     */
    function _checkQuorumExists(
        uint8 quorumNumber
    ) internal view {
        require(quorumNumber < quorumCount, QuorumDoesNotExist());
    }

    /**
     * @notice Fetches an operator's pubkey hash from the BLSApkRegistry. If the
     * operator has not registered a pubkey, attempts to register a pubkey using
     * `params`
     * @param operator the operator whose pubkey to query from the BLSApkRegistry
     * @param params contains the G1 & G2 public keys of the operator, and a signature proving their ownership
     * @dev `params` can be empty if the operator has already registered a pubkey in the BLSApkRegistry
     */
    function _getOrCreateOperatorId(
        address operator,
        IBLSApkRegistryTypes.PubkeyRegistrationParams memory params
    ) internal returns (bytes32 operatorId) {
        return blsApkRegistry.getOrRegisterOperatorId(
            operator, params, pubkeyRegistrationMessageHash(operator)
        );
    }

    /**
     * @notice Validates that an incoming operator is eligible to replace an existing
     * operator based on the stake of both
     * @dev In order to churn, the incoming operator needs to have more stake than the
     * existing operator by a proportion given by `kickBIPsOfOperatorStake`
     * @dev In order to be churned out, the existing operator needs to have a proportion
     * of the total quorum stake less than `kickBIPsOfTotalStake`
     * @param quorumNumber `newOperator` is trying to replace an operator in this quorum
     * @param totalQuorumStake the total stake of all operators in the quorum, after the
     * `newOperator` registers
     * @param newOperator the incoming operator
     * @param newOperatorStake the incoming operator's stake
     * @param kickParams the quorum number and existing operator to replace
     * @dev the existing operator's registration to this quorum isn't checked here, but
     * if we attempt to deregister them, this will be checked in `_deregisterOperator`
     * @param setParams config for this quorum containing `kickBIPsX` stake proportions
     * mentioned above
     */
    function _validateChurn(
        uint8 quorumNumber,
        uint96 totalQuorumStake,
        address newOperator,
        uint96 newOperatorStake,
        OperatorKickParam memory kickParams,
        OperatorSetParam memory setParams
    ) internal view {
        address operatorToKick = kickParams.operator;
        bytes32 idToKick = _operatorInfo[operatorToKick].operatorId;
        require(newOperator != operatorToKick, CannotChurnSelf());
        require(kickParams.quorumNumber == quorumNumber, QuorumOperatorCountMismatch());

        // Get the target operator's stake and check that it is below the kick thresholds
        uint96 operatorToKickStake = stakeRegistry.getCurrentStake(idToKick, quorumNumber);
        require(
            newOperatorStake > _individualKickThreshold(operatorToKickStake, setParams),
            InsufficientStakeForChurn()
        );
        require(
            operatorToKickStake < _totalKickThreshold(totalQuorumStake, setParams),
            CannotKickOperatorAboveThreshold()
        );
    }

    /**
     * @notice Returns the stake threshold required for an incoming operator to replace an existing operator
     * The incoming operator must have more stake than the return value.
     */
    function _individualKickThreshold(
        uint96 operatorStake,
        OperatorSetParam memory setParams
    ) internal pure returns (uint96) {
        return operatorStake * setParams.kickBIPsOfOperatorStake / BIPS_DENOMINATOR;
    }

    /**
     * @notice Returns the total stake threshold required for an operator to remain in a quorum.
     * The operator must have at least the returned stake amount to keep their position.
     */
    function _totalKickThreshold(
        uint96 totalStake,
        OperatorSetParam memory setParams
    ) internal pure returns (uint96) {
        return totalStake * setParams.kickBIPsOfTotalStake / BIPS_DENOMINATOR;
    }

    /**
     * @notice Updates an operator's socket address in the SocketRegistry
     * @param operatorId The unique identifier of the operator
     * @param socket The new socket address to set for the operator
     * @dev Emits an OperatorSocketUpdate event after updating
     */
    function _setOperatorSocket(bytes32 operatorId, string memory socket) internal {
        socketRegistry.setOperatorSocket(operatorId, socket);
        emit OperatorSocketUpdate(operatorId, socket);
    }

    /// @notice verifies churnApprover's signature on operator churn approval and increments the churnApprover nonce
    function _verifyChurnApproverSignature(
        address registeringOperator,
        bytes32 registeringOperatorId,
        OperatorKickParam[] memory operatorKickParams,
        SignatureWithSaltAndExpiry memory churnApproverSignature
    ) internal {
        // make sure the salt hasn't been used already
        require(!isChurnApproverSaltUsed[churnApproverSignature.salt], ChurnApproverSaltUsed());
        require(churnApproverSignature.expiry >= block.timestamp, SignatureExpired());

        // set salt used to true
        isChurnApproverSaltUsed[churnApproverSignature.salt] = true;

        // check the churnApprover's signature
        SignatureCheckerLib.isValidSignature(
            churnApprover,
            calculateOperatorChurnApprovalDigestHash(
                registeringOperator,
                registeringOperatorId,
                operatorKickParams,
                churnApproverSignature.salt,
                churnApproverSignature.expiry
            ),
            churnApproverSignature.signature
        );
    }

    /**
     * @notice Creates a quorum and initializes it in each registry contract
     * @param operatorSetParams configures the quorum's max operator count and churn parameters
     * @param minimumStake sets the minimum stake required for an operator to register or remain
     * registered
     * @param strategyParams a list of strategies and multipliers used by the StakeRegistry to
     * calculate an operator's stake weight for the quorum
     */
    function _createQuorum(
        OperatorSetParam memory operatorSetParams,
        uint96 minimumStake,
        IStakeRegistryTypes.StrategyParams[] memory strategyParams,
        IStakeRegistryTypes.StakeType stakeType,
        uint32 lookAheadPeriod
    ) internal {
        // The previous quorum count is the new quorum's number,
        // this is because quorum numbers begin from index 0.
        uint8 quorumNumber = quorumCount;

        // Hook to allow for any pre-create quorum logic
        _beforeCreateQuorum(quorumNumber);

        // Increment the total quorum count. Fails if we're already at the max
        require(quorumNumber < MAX_QUORUM_COUNT, MaxQuorumsReached());
        quorumCount += 1;

        // Initialize the quorum here and in each registry
        _setOperatorSetParams(quorumNumber, operatorSetParams);

        // Create array of CreateSetParams for the new quorum
        IAllocationManagerTypes.CreateSetParams[] memory createSetParams =
            new IAllocationManagerTypes.CreateSetParams[](1);

        // Extract strategies from strategyParams
        IStrategy[] memory strategies = new IStrategy[](strategyParams.length);
        for (uint256 i = 0; i < strategyParams.length; i++) {
            strategies[i] = strategyParams[i].strategy;
        }

        // Initialize CreateSetParams with quorumNumber as operatorSetId
        createSetParams[0] = IAllocationManagerTypes.CreateSetParams({
            operatorSetId: quorumNumber,
            strategies: strategies
        });
        allocationManager.createOperatorSets({avs: avs, params: createSetParams});

        // Initialize stake registry based on stake type
        if (stakeType == IStakeRegistryTypes.StakeType.TOTAL_DELEGATED) {
            stakeRegistry.initializeDelegatedStakeQuorum(quorumNumber, minimumStake, strategyParams);
        } else if (stakeType == IStakeRegistryTypes.StakeType.TOTAL_SLASHABLE) {
            // For slashable stake quorums, ensure lookAheadPeriod is less than DEALLOCATION_DELAY
            require(
                AllocationManager(address(allocationManager)).DEALLOCATION_DELAY() > lookAheadPeriod,
                LookAheadPeriodTooLong()
            );
            stakeRegistry.initializeSlashableStakeQuorum(
                quorumNumber, minimumStake, lookAheadPeriod, strategyParams
            );
        }

        indexRegistry.initializeQuorum(quorumNumber);
        blsApkRegistry.initializeQuorum(quorumNumber);

        emit QuorumCreated({
            quorumNumber: quorumNumber,
            operatorSetParams: operatorSetParams,
            minimumStake: minimumStake,
            strategyParams: strategyParams,
            stakeType: stakeType,
            lookAheadPeriod: lookAheadPeriod
        });

        // Hook to allow for any post-create quorum logic
        _afterCreateQuorum(quorumNumber);
    }

    /**
     * @notice Record an update to an operator's quorum bitmap.
     * @param newBitmap is the most up-to-date set of bitmaps the operator is registered for
     */
    function _updateOperatorBitmap(bytes32 operatorId, uint192 newBitmap) internal {
        QuorumBitmapHistoryLib.updateOperatorBitmap(_operatorBitmapHistory, operatorId, newBitmap);
    }

    /// @notice Get the most recent bitmap for the operator, returning an empty bitmap if
    /// the operator is not registered.
    function _currentOperatorBitmap(
        bytes32 operatorId
    ) internal view returns (uint192) {
        return QuorumBitmapHistoryLib.currentOperatorBitmap(_operatorBitmapHistory, operatorId);
    }

    /**
     * @notice Returns the index of the quorumBitmap for the provided `operatorId` at the given `blockNumber`
     * @dev Reverts if the operator had not yet (ever) registered at `blockNumber`
     * @dev This function is designed to find proper inputs to the `getQuorumBitmapAtBlockNumberByIndex` function
     */
    function _getQuorumBitmapIndexAtBlockNumber(
        uint32 blockNumber,
        bytes32 operatorId
    ) internal view returns (uint32 index) {
        return QuorumBitmapHistoryLib.getQuorumBitmapIndexAtBlockNumber(
            _operatorBitmapHistory, blockNumber, operatorId
        );
    }

    /// @notice Returns the quorum numbers for the provided `OperatorSetIds`
    /// OperatorSetIds are used in the AllocationManager to identify operator sets for a given AVS
    function _getQuorumNumbers(
        uint32[] memory operatorSetIds
    ) internal pure returns (bytes memory) {
        bytes memory quorumNumbers = new bytes(operatorSetIds.length);
        for (uint256 i = 0; i < operatorSetIds.length; i++) {
            quorumNumbers[i] = bytes1(uint8(operatorSetIds[i]));
        }
        return quorumNumbers;
    }

    function _getOperatorSetIds(
        bytes memory quorumNumbers
    ) internal pure returns (uint32[] memory) {
        uint32[] memory operatorSetIds = new uint32[](quorumNumbers.length);
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            operatorSetIds[i] = uint32(uint8(quorumNumbers[i]));
        }
        return operatorSetIds;
    }

    function _setOperatorSetParams(
        uint8 quorumNumber,
        OperatorSetParam memory operatorSetParams
    ) internal {
        _quorumParams[quorumNumber] = operatorSetParams;
        emit OperatorSetParamsUpdated(quorumNumber, operatorSetParams);
    }

    function _setChurnApprover(
        address newChurnApprover
    ) internal {
        emit ChurnApproverUpdated(churnApprover, newChurnApprover);
        churnApprover = newChurnApprover;
    }

    function _setEjector(
        address newEjector
    ) internal {
        emit EjectorUpdated(ejector, newEjector);
        ejector = newEjector;
    }

    function _setAVS(
        address _avs
    ) internal {
        address prevAVS = avs;
        emit AVSUpdated(prevAVS, _avs);
        avs = _avs;
    }

    /// @dev Hook to allow for any pre-create quorum logic
    function _beforeCreateQuorum(
        uint8 quorumNumber
    ) internal virtual {}

    /// @dev Hook to allow for any post-create quorum logic
    function _afterCreateQuorum(
        uint8 quorumNumber
    ) internal virtual {}

    /// @dev Hook to allow for any pre-register logic in `_registerOperator`
    function _beforeRegisterOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        uint192 currentBitmap
    ) internal virtual {}

    /// @dev Hook to allow for any post-register logic in `_registerOperator`
    function _afterRegisterOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        uint192 newBitmap
    ) internal virtual {}

    /// @dev Hook to allow for any pre-deregister logic in `_deregisterOperator`
    function _beforeDeregisterOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        uint192 currentBitmap
    ) internal virtual {}

    /// @dev Hook to allow for any post-deregister logic in `_deregisterOperator`
    function _afterDeregisterOperator(
        address operator,
        bytes32 operatorId,
        bytes memory quorumNumbers,
        uint192 newBitmap
    ) internal virtual {}

    /**
     *
     *                         VIEW FUNCTIONS
     *
     */

    /// @notice Returns the operator set params for the given `quorumNumber`
    function getOperatorSetParams(
        uint8 quorumNumber
    ) external view returns (OperatorSetParam memory) {
        return _quorumParams[quorumNumber];
    }

    /// @notice Returns the operator struct for the given `operator`
    function getOperator(
        address operator
    ) external view returns (OperatorInfo memory) {
        return _operatorInfo[operator];
    }

    /// @notice Returns the operatorId for the given `operator`
    function getOperatorId(
        address operator
    ) external view returns (bytes32) {
        return _operatorInfo[operator].operatorId;
    }

    /// @notice Returns the operator address for the given `operatorId`
    function getOperatorFromId(
        bytes32 operatorId
    ) external view returns (address) {
        return blsApkRegistry.getOperatorFromPubkeyHash(operatorId);
    }

    /// @notice Returns the status for the given `operator`
    function getOperatorStatus(
        address operator
    ) external view returns (ISlashingRegistryCoordinator.OperatorStatus) {
        return _operatorInfo[operator].status;
    }

    /**
     * @notice Returns the indices of the quorumBitmaps for the provided `operatorIds` at the given `blockNumber`
     * @dev Reverts if any of the `operatorIds` was not (yet) registered at `blockNumber`
     * @dev This function is designed to find proper inputs to the `getQuorumBitmapAtBlockNumberByIndex` function
     */
    function getQuorumBitmapIndicesAtBlockNumber(
        uint32 blockNumber,
        bytes32[] memory operatorIds
    ) external view returns (uint32[] memory) {
        return QuorumBitmapHistoryLib.getQuorumBitmapIndicesAtBlockNumber(
            _operatorBitmapHistory, blockNumber, operatorIds
        );
    }

    /**
     * @notice Returns the quorum bitmap for the given `operatorId` at the given `blockNumber` via the `index`,
     * reverting if `index` is incorrect
     * @dev This function is meant to be used in concert with `getQuorumBitmapIndicesAtBlockNumber`, which
     * helps off-chain processes to fetch the correct `index` input
     */
    function getQuorumBitmapAtBlockNumberByIndex(
        bytes32 operatorId,
        uint32 blockNumber,
        uint256 index
    ) external view returns (uint192) {
        return QuorumBitmapHistoryLib.getQuorumBitmapAtBlockNumberByIndex(
            _operatorBitmapHistory, operatorId, blockNumber, index
        );
    }

    /// @notice Returns the `index`th entry in the operator with `operatorId`'s bitmap history
    function getQuorumBitmapUpdateByIndex(
        bytes32 operatorId,
        uint256 index
    ) external view returns (QuorumBitmapUpdate memory) {
        return _operatorBitmapHistory[operatorId][index];
    }

    /// @notice Returns the current quorum bitmap for the given `operatorId` or 0 if the operator is not registered for any quorum
    function getCurrentQuorumBitmap(
        bytes32 operatorId
    ) external view returns (uint192) {
        return _currentOperatorBitmap(operatorId);
    }

    /// @notice Returns the length of the quorum bitmap history for the given `operatorId`
    function getQuorumBitmapHistoryLength(
        bytes32 operatorId
    ) external view returns (uint256) {
        return _operatorBitmapHistory[operatorId].length;
    }

    /**
     * @notice Public function for the the churnApprover signature hash calculation when operators are being kicked from quorums
     * @param registeringOperatorId The id of the registering operator
     * @param operatorKickParams The parameters needed to kick the operator from the quorums that have reached their caps
     * @param salt The salt to use for the churnApprover's signature
     * @param expiry The desired expiry time of the churnApprover's signature
     */
    function calculateOperatorChurnApprovalDigestHash(
        address registeringOperator,
        bytes32 registeringOperatorId,
        OperatorKickParam[] memory operatorKickParams,
        bytes32 salt,
        uint256 expiry
    ) public view returns (bytes32) {
        // calculate the digest hash
        return _hashTypedDataV4(
            keccak256(
                abi.encode(
                    OPERATOR_CHURN_APPROVAL_TYPEHASH,
                    registeringOperator,
                    registeringOperatorId,
                    operatorKickParams,
                    salt,
                    expiry
                )
            )
        );
    }

    /**
     * @notice Returns the message hash that an operator must sign to register their BLS public key.
     * @param operator is the address of the operator registering their BLS public key
     */
    function pubkeyRegistrationMessageHash(
        address operator
    ) public view returns (BN254.G1Point memory) {
        return BN254.hashToG1(calculatePubkeyRegistrationMessageHash(operator));
    }

    /**
     * @notice Returns the message hash that an operator must sign to register their BLS public key.
     * @param operator is the address of the operator registering their BLS public key
     */
    function calculatePubkeyRegistrationMessageHash(
        address operator
    ) public view returns (bytes32) {
        return _hashTypedDataV4(keccak256(abi.encode(PUBKEY_REGISTRATION_TYPEHASH, operator)));
    }

    function supportsAVS(
        address _avs
    ) public view virtual returns (bool) {
        return _avs == address(avs);
    }

    /**
     * @notice Returns the domain separator used for EIP-712 signatures
     * @return The domain separator
     */
    function domainSeparator() external view virtual override returns (bytes32) {
        return _domainSeparatorV4();
    }
}
````

## File: src/SlashingRegistryCoordinatorStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IBLSApkRegistry} from "./interfaces/IBLSApkRegistry.sol";
import {IStakeRegistry} from "./interfaces/IStakeRegistry.sol";
import {IIndexRegistry} from "./interfaces/IIndexRegistry.sol";
import {IServiceManager} from "./interfaces/IServiceManager.sol";
import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {
    IAllocationManager,
    OperatorSet,
    IAllocationManagerTypes
} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";

abstract contract SlashingRegistryCoordinatorStorage is ISlashingRegistryCoordinator {
    /**
     *
     *                            CONSTANTS AND IMMUTABLES
     *
     */

    /// @notice The EIP-712 typehash for the `DelegationApproval` struct used by the contract
    bytes32 public constant OPERATOR_CHURN_APPROVAL_TYPEHASH = keccak256(
        "OperatorChurnApproval(address registeringOperator,bytes32 registeringOperatorId,OperatorKickParam[] operatorKickParams,bytes32 salt,uint256 expiry)OperatorKickParam(uint8 quorumNumber,address operator)"
    );
    /// @notice The EIP-712 typehash used for registering BLS public keys
    bytes32 public constant PUBKEY_REGISTRATION_TYPEHASH =
        keccak256("BN254PubkeyRegistration(address operator)");
    /// @notice The maximum value of a quorum bitmap
    uint256 internal constant MAX_QUORUM_BITMAP = type(uint192).max;
    /// @notice The basis point denominator
    uint16 internal constant BIPS_DENOMINATOR = 10000;
    /// @notice Index for flag that pauses operator registration
    uint8 internal constant PAUSED_REGISTER_OPERATOR = 0;
    /// @notice Index for flag that pauses operator deregistration
    uint8 internal constant PAUSED_DEREGISTER_OPERATOR = 1;
    /// @notice Index for flag pausing operator stake updates
    uint8 internal constant PAUSED_UPDATE_OPERATOR = 2;
    /// @notice The maximum number of quorums this contract supports
    uint8 internal constant MAX_QUORUM_COUNT = 192;

    /// @notice the Socket Registry contract that will keep track of operators' sockets (arbitrary strings)
    ISocketRegistry public immutable socketRegistry;
    /// @notice the BLS Aggregate Pubkey Registry contract that will keep track of operators' aggregate BLS public keys per quorum
    IBLSApkRegistry public immutable blsApkRegistry;
    /// @notice the Stake Registry contract that will keep track of operators' stakes
    IStakeRegistry public immutable stakeRegistry;
    /// @notice the Index Registry contract that will keep track of operators' indexes
    IIndexRegistry public immutable indexRegistry;

    /// EigenLayer contracts
    /// @notice the AllocationManager that tracks OperatorSets and Slashing in EigenLayer
    IAllocationManager public immutable allocationManager;

    /**
     *
     *                                    STATE
     *
     */

    /// @notice the current number of quorums supported by the registry coordinator
    uint8 public quorumCount;
    /// @notice maps quorum number => operator cap and kick params
    mapping(uint8 => OperatorSetParam) internal _quorumParams;
    /// @notice maps operator id => historical quorums they registered for
    mapping(bytes32 => QuorumBitmapUpdate[]) internal _operatorBitmapHistory;
    /// @notice maps operator address => operator id and status
    mapping(address => OperatorInfo) internal _operatorInfo;
    /// @notice whether the salt has been used for an operator churn approval
    mapping(bytes32 => bool) public isChurnApproverSaltUsed;
    /// @notice mapping from quorum number to the latest block that all quorums were updated all at once
    mapping(uint8 => uint256) public quorumUpdateBlockNumber;

    /// @notice the dynamic-length array of the registries this coordinator is coordinating
    /// @dev DEPRECATED: This slot is no longer used but kept for storage layout compatibility
    address[] private registries;
    /// @notice the address of the entity allowed to sign off on operators getting kicked out of the AVS during registration
    address public churnApprover;
    /// @notice the address of the entity allowed to eject operators from the AVS
    address public ejector;

    /// @notice the last timestamp an operator was ejected
    mapping(address => uint256) public lastEjectionTimestamp;
    /// @notice the delay in seconds before an operator can reregister after being ejected
    uint256 public ejectionCooldown;

    /// @notice The avs address for this AVS (used for UAM integration in EigenLayer)
    /// @dev NOTE: Updating this value will break existing OperatorSets and UAM integration.
    /// This value should only be set once.
    address public avs;

    constructor(
        IStakeRegistry _stakeRegistry,
        IBLSApkRegistry _blsApkRegistry,
        IIndexRegistry _indexRegistry,
        ISocketRegistry _socketRegistry,
        IAllocationManager _allocationManager
    ) {
        stakeRegistry = _stakeRegistry;
        blsApkRegistry = _blsApkRegistry;
        indexRegistry = _indexRegistry;
        socketRegistry = _socketRegistry;
        allocationManager = _allocationManager;
    }

    // storage gap for upgradeability
    // slither-disable-next-line shadowing-state
    uint256[38] private __GAP;
}
````

## File: src/SocketRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.12;

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";
import {SocketRegistryStorage} from "./SocketRegistryStorage.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

/**
 * @title A `Registry` that keeps track of operator sockets (arbitrary strings).
 * @author Layr Labs, Inc.
 */
contract SocketRegistry is SocketRegistryStorage {
    /// @notice A modifier that only allows the SlashingRegistryCoordinator to call a function
    modifier onlySlashingRegistryCoordinator() {
        require(msg.sender == slashingRegistryCoordinator, OnlySlashingRegistryCoordinator());
        _;
    }

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator
    ) SocketRegistryStorage(address(_slashingRegistryCoordinator)) {}

    /// @inheritdoc ISocketRegistry
    function setOperatorSocket(
        bytes32 _operatorId,
        string memory _socket
    ) external onlySlashingRegistryCoordinator {
        operatorIdToSocket[_operatorId] = _socket;
    }

    /// @inheritdoc ISocketRegistry
    function getOperatorSocket(
        bytes32 _operatorId
    ) external view returns (string memory) {
        return operatorIdToSocket[_operatorId];
    }
}
````

## File: src/SocketRegistryStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.12;

import {ISocketRegistry} from "./interfaces/ISocketRegistry.sol";

/**
 * @title Storage variables for the `SocketRegistry` contract.
 * @author Layr Labs, Inc.
 */
abstract contract SocketRegistryStorage is ISocketRegistry {
    /**
     *
     *                            CONSTANTS AND IMMUTABLES
     *
     */

    /// @notice The address of the SlashingRegistryCoordinator
    address public immutable slashingRegistryCoordinator;

    /**
     *
     *                                    STATE
     *
     */

    /// @notice A mapping from operator IDs to their sockets
    mapping(bytes32 => string) public operatorIdToSocket;

    constructor(
        address _slashingRegistryCoordinator
    ) {
        slashingRegistryCoordinator = _slashingRegistryCoordinator;
    }

    uint256[49] private __GAP;
}
````

## File: src/StakeRegistry.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {OperatorSet} from "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";

import {StakeRegistryStorage, IStrategy} from "./StakeRegistryStorage.sol";

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IStakeRegistry, IStakeRegistryTypes} from "./interfaces/IStakeRegistry.sol";

import {BitmapUtils} from "./libraries/BitmapUtils.sol";

/**
 * @title A `Registry` that keeps track of stakes of operators for up to 256 quorums.
 * Specifically, it keeps track of
 *      1) The stake of each operator in all the quorums they are a part of for block ranges
 *      2) The total stake of all operators in each quorum for block ranges
 *      3) The minimum stake required to register for each quorum
 * It allows an additional functionality (in addition to registering and deregistering) to update the stake of an operator.
 * @author Layr Labs, Inc.
 */
contract StakeRegistry is StakeRegistryStorage {
    using BitmapUtils for *;

    modifier onlySlashingRegistryCoordinator() {
        _checkSlashingRegistryCoordinator();
        _;
    }

    modifier onlyCoordinatorOwner() {
        _checkSlashingRegistryCoordinatorOwner();
        _;
    }

    modifier quorumExists(
        uint8 quorumNumber
    ) {
        _checkQuorumExists(quorumNumber);
        _;
    }

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        IDelegationManager _delegationManager,
        IAVSDirectory _avsDirectory,
        IAllocationManager _allocationManager
    )
        StakeRegistryStorage(
            _slashingRegistryCoordinator,
            _delegationManager,
            _avsDirectory,
            _allocationManager
        )
    {}

    /**
     *
     *                   EXTERNAL FUNCTIONS - REGISTRY COORDINATOR
     *
     */

    /// @inheritdoc IStakeRegistry
    function registerOperator(
        address operator,
        bytes32 operatorId,
        bytes calldata quorumNumbers
    ) public virtual onlySlashingRegistryCoordinator returns (uint96[] memory, uint96[] memory) {
        uint96[] memory currentStakes = new uint96[](quorumNumbers.length);
        uint96[] memory totalStakes = new uint96[](quorumNumbers.length);
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            _checkQuorumExists(quorumNumber);

            // Retrieve the operator's current weighted stake for the quorum, reverting if they have not met
            // the minimum.
            (uint96 currentStake, bool hasMinimumStake) =
                _weightOfOperatorForQuorum(quorumNumber, operator);
            require(hasMinimumStake, BelowMinimumStakeRequirement());

            // Update the operator's stake
            int256 stakeDelta = _recordOperatorStakeUpdate({
                operatorId: operatorId,
                quorumNumber: quorumNumber,
                newStake: currentStake
            });

            // Update this quorum's total stake by applying the operator's delta
            currentStakes[i] = currentStake;
            totalStakes[i] = _recordTotalStakeUpdate(quorumNumber, stakeDelta);
        }

        return (currentStakes, totalStakes);
    }

    /// @inheritdoc IStakeRegistry
    function deregisterOperator(
        bytes32 operatorId,
        bytes calldata quorumNumbers
    ) public virtual onlySlashingRegistryCoordinator {
        /**
         * For each quorum, remove the operator's stake for the quorum and update
         * the quorum's total stake to account for the removal
         */
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            _checkQuorumExists(quorumNumber);

            // Update the operator's stake for the quorum and retrieve the shares removed
            int256 stakeDelta = _recordOperatorStakeUpdate({
                operatorId: operatorId,
                quorumNumber: quorumNumber,
                newStake: 0
            });

            // Apply the operator's stake delta to the total stake for this quorum
            _recordTotalStakeUpdate(quorumNumber, stakeDelta);
        }
    }

    /// @inheritdoc IStakeRegistry
    function updateOperatorsStake(
        address[] memory operators,
        bytes32[] memory operatorIds,
        uint8 quorumNumber
    ) external virtual onlySlashingRegistryCoordinator returns (bool[] memory) {
        bool[] memory shouldBeDeregistered = new bool[](operators.length);

        /**
         * For each quorum, update the operator's stake and record the delta
         * in the quorum's total stake.
         *
         * If the operator no longer has the minimum stake required to be registered
         * in the quorum, the quorum number is added to `quorumsToRemove`, which
         * is returned to the registry coordinator.
         */
        _checkQuorumExists(quorumNumber);

        // Fetch the operators' current stake, applying weighting parameters and checking
        // against the minimum stake requirements for the quorum.
        (uint96[] memory stakeWeights, bool[] memory hasMinimumStakes) =
            _weightOfOperatorsForQuorum(quorumNumber, operators);

        int256 totalStakeDelta = 0;
        // If the operator no longer meets the minimum stake, set their stake to zero and mark them for removal
        /// also handle setting the operator's stake to 0 and remove them from the quorum
        for (uint256 i = 0; i < operators.length; i++) {
            if (!hasMinimumStakes[i]) {
                stakeWeights[i] = 0;
                shouldBeDeregistered[i] = true;
            }

            // Update the operator's stake and retrieve the delta
            // If we're deregistering them, their weight is set to 0
            int256 stakeDelta = _recordOperatorStakeUpdate({
                operatorId: operatorIds[i],
                quorumNumber: quorumNumber,
                newStake: stakeWeights[i]
            });

            totalStakeDelta += stakeDelta;
        }

        // Apply the delta to the quorum's total stake
        _recordTotalStakeUpdate(quorumNumber, totalStakeDelta);

        return shouldBeDeregistered;
    }

    /// @inheritdoc IStakeRegistry
    function initializeDelegatedStakeQuorum(
        uint8 quorumNumber,
        uint96 minimumStake,
        StrategyParams[] memory _strategyParams
    ) public virtual onlySlashingRegistryCoordinator {
        require(!_quorumExists(quorumNumber), QuorumAlreadyExists());
        _addStrategyParams(quorumNumber, _strategyParams);
        _setMinimumStakeForQuorum(quorumNumber, minimumStake);
        _setStakeType(quorumNumber, IStakeRegistryTypes.StakeType.TOTAL_DELEGATED);

        _totalStakeHistory[quorumNumber].push(
            StakeUpdate({
                updateBlockNumber: uint32(block.number),
                nextUpdateBlockNumber: 0,
                stake: 0
            })
        );
    }

    /// @inheritdoc IStakeRegistry
    function initializeSlashableStakeQuorum(
        uint8 quorumNumber,
        uint96 minimumStake,
        uint32 lookAheadPeriod,
        StrategyParams[] memory _strategyParams
    ) public virtual onlySlashingRegistryCoordinator {
        require(!_quorumExists(quorumNumber), QuorumAlreadyExists());
        _addStrategyParams(quorumNumber, _strategyParams);
        _setMinimumStakeForQuorum(quorumNumber, minimumStake);
        _setStakeType(quorumNumber, IStakeRegistryTypes.StakeType.TOTAL_SLASHABLE);
        _setLookAheadPeriod(quorumNumber, lookAheadPeriod);

        _totalStakeHistory[quorumNumber].push(
            StakeUpdate({
                updateBlockNumber: uint32(block.number),
                nextUpdateBlockNumber: 0,
                stake: 0
            })
        );
    }

    /// @inheritdoc IStakeRegistry
    function setMinimumStakeForQuorum(
        uint8 quorumNumber,
        uint96 minimumStake
    ) public virtual onlyCoordinatorOwner quorumExists(quorumNumber) {
        _setMinimumStakeForQuorum(quorumNumber, minimumStake);
    }

    /// @inheritdoc IStakeRegistry
    function setSlashableStakeLookahead(
        uint8 quorumNumber,
        uint32 _lookAheadBlocks
    ) external onlyCoordinatorOwner quorumExists(quorumNumber) {
        _setLookAheadPeriod(quorumNumber, _lookAheadBlocks);
    }

    /// @inheritdoc IStakeRegistry
    function addStrategies(
        uint8 quorumNumber,
        StrategyParams[] memory _strategyParams
    ) public virtual onlyCoordinatorOwner quorumExists(quorumNumber) {
        _addStrategyParams(quorumNumber, _strategyParams);

        uint256 numStratsToAdd = _strategyParams.length;

        address avs = registryCoordinator.avs();
        if (allocationManager.isOperatorSet(OperatorSet(avs, quorumNumber))) {
            IStrategy[] memory strategiesToAdd = new IStrategy[](numStratsToAdd);
            for (uint256 i = 0; i < numStratsToAdd; i++) {
                strategiesToAdd[i] = _strategyParams[i].strategy;
            }
            allocationManager.addStrategiesToOperatorSet({
                avs: avs,
                operatorSetId: quorumNumber,
                strategies: strategiesToAdd
            });
        }
    }

    /// @inheritdoc IStakeRegistry
    function removeStrategies(
        uint8 quorumNumber,
        uint256[] memory indicesToRemove
    ) public virtual onlyCoordinatorOwner quorumExists(quorumNumber) {
        uint256 toRemoveLength = indicesToRemove.length;
        require(toRemoveLength > 0, InputArrayLengthZero());

        StrategyParams[] storage _strategyParams = strategyParams[quorumNumber];
        IStrategy[] storage _strategiesPerQuorum = strategiesPerQuorum[quorumNumber];
        IStrategy[] memory _strategiesToRemove = new IStrategy[](toRemoveLength);

        for (uint256 i = 0; i < toRemoveLength; i++) {
            _strategiesToRemove[i] = _strategyParams[indicesToRemove[i]].strategy;
            emit StrategyRemovedFromQuorum(
                quorumNumber, _strategyParams[indicesToRemove[i]].strategy
            );
            emit StrategyMultiplierUpdated(
                quorumNumber, _strategyParams[indicesToRemove[i]].strategy, 0
            );

            // Replace index to remove with the last item in the list, then pop the last item
            _strategyParams[indicesToRemove[i]] = _strategyParams[_strategyParams.length - 1];
            _strategyParams.pop();
            _strategiesPerQuorum[indicesToRemove[i]] =
                _strategiesPerQuorum[_strategiesPerQuorum.length - 1];
            _strategiesPerQuorum.pop();
        }

        address avs = registryCoordinator.avs();
        if (allocationManager.isOperatorSet(OperatorSet(avs, quorumNumber))) {
            allocationManager.removeStrategiesFromOperatorSet({
                avs: avs,
                operatorSetId: quorumNumber,
                strategies: _strategiesToRemove
            });
        }
    }

    /// @inheritdoc IStakeRegistry
    function modifyStrategyParams(
        uint8 quorumNumber,
        uint256[] calldata strategyIndices,
        uint96[] calldata newMultipliers
    ) public virtual onlyCoordinatorOwner quorumExists(quorumNumber) {
        uint256 numStrats = strategyIndices.length;
        require(numStrats > 0, InputArrayLengthZero());
        require(newMultipliers.length == numStrats, InputArrayLengthMismatch());

        StrategyParams[] storage _strategyParams = strategyParams[quorumNumber];

        for (uint256 i = 0; i < numStrats; i++) {
            // Change the strategy's associated multiplier
            _strategyParams[strategyIndices[i]].multiplier = newMultipliers[i];
            emit StrategyMultiplierUpdated(
                quorumNumber, _strategyParams[strategyIndices[i]].strategy, newMultipliers[i]
            );
        }
    }

    /**
     *
     *                         INTERNAL FUNCTIONS
     *
     */
    function _getStakeUpdateIndexForOperatorAtBlockNumber(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 blockNumber
    ) internal view returns (uint32) {
        uint256 length = operatorStakeHistory[operatorId][quorumNumber].length;

        // Iterate backwards through operatorStakeHistory until we find an update that preceeds blockNumber
        for (uint256 i = length; i > 0; i--) {
            if (
                operatorStakeHistory[operatorId][quorumNumber][i - 1].updateBlockNumber
                    <= blockNumber
            ) {
                return uint32(i - 1);
            }
        }

        // If we hit this point, no stake update exists at blockNumber
        revert(
            "StakeRegistry._getStakeUpdateIndexForOperatorAtBlockNumber: no stake update found for operatorId and quorumNumber at block number"
        );
    }

    function _setMinimumStakeForQuorum(uint8 quorumNumber, uint96 minimumStake) internal {
        minimumStakeForQuorum[quorumNumber] = minimumStake;
        emit MinimumStakeForQuorumUpdated(quorumNumber, minimumStake);
    }

    /**
     * @notice Records that `operatorId`'s current stake for `quorumNumber` is now `newStake`
     * @return The change in the operator's stake as a signed int256
     */
    function _recordOperatorStakeUpdate(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint96 newStake
    ) internal returns (int256) {
        uint96 prevStake;
        uint256 historyLength = operatorStakeHistory[operatorId][quorumNumber].length;

        if (historyLength == 0) {
            // No prior stake history - push our first entry
            operatorStakeHistory[operatorId][quorumNumber].push(
                StakeUpdate({
                    updateBlockNumber: uint32(block.number),
                    nextUpdateBlockNumber: 0,
                    stake: newStake
                })
            );
        } else {
            // We have prior stake history - fetch our last-recorded stake
            StakeUpdate storage lastUpdate =
                operatorStakeHistory[operatorId][quorumNumber][historyLength - 1];
            prevStake = lastUpdate.stake;

            // Short-circuit in case there's no change in stake
            if (prevStake == newStake) {
                return 0;
            }

            /**
             * If our last stake entry was made in the current block, update the entry
             * Otherwise, push a new entry and update the previous entry's "next" field
             */
            if (lastUpdate.updateBlockNumber == uint32(block.number)) {
                lastUpdate.stake = newStake;
            } else {
                lastUpdate.nextUpdateBlockNumber = uint32(block.number);
                operatorStakeHistory[operatorId][quorumNumber].push(
                    StakeUpdate({
                        updateBlockNumber: uint32(block.number),
                        nextUpdateBlockNumber: 0,
                        stake: newStake
                    })
                );
            }
        }

        // Log update and return stake delta
        emit OperatorStakeUpdate(operatorId, quorumNumber, newStake);
        return _calculateDelta({prev: prevStake, cur: newStake});
    }

    /// @notice Applies a delta to the total stake recorded for `quorumNumber`
    /// @return Returns the new total stake for the quorum
    function _recordTotalStakeUpdate(
        uint8 quorumNumber,
        int256 stakeDelta
    ) internal returns (uint96) {
        // Get our last-recorded stake update
        uint256 historyLength = _totalStakeHistory[quorumNumber].length;
        StakeUpdate storage lastStakeUpdate = _totalStakeHistory[quorumNumber][historyLength - 1];

        // Return early if no update is needed
        if (stakeDelta == 0) {
            return lastStakeUpdate.stake;
        }

        // Calculate the new total stake by applying the delta to our previous stake
        uint96 newStake = _applyDelta(lastStakeUpdate.stake, stakeDelta);

        /**
         * If our last stake entry was made in the current block, update the entry
         * Otherwise, push a new entry and update the previous entry's "next" field
         */
        if (lastStakeUpdate.updateBlockNumber == uint32(block.number)) {
            lastStakeUpdate.stake = newStake;
        } else {
            lastStakeUpdate.nextUpdateBlockNumber = uint32(block.number);
            _totalStakeHistory[quorumNumber].push(
                StakeUpdate({
                    updateBlockNumber: uint32(block.number),
                    nextUpdateBlockNumber: 0,
                    stake: newStake
                })
            );
        }

        return newStake;
    }

    /**
     * @notice Adds `strategyParams` to the `quorumNumber`-th quorum.
     * @dev Checks to make sure that the *same* strategy cannot be added multiple times (checks against both against existing and new strategies).
     * @dev This function has no check to make sure that the strategies for a single quorum have the same underlying asset. This is a conscious choice,
     * since a middleware may want, e.g., a stablecoin quorum that accepts USDC, USDT, DAI, etc. as underlying assets and trades them as "equivalent".
     */
    function _addStrategyParams(
        uint8 quorumNumber,
        StrategyParams[] memory _strategyParams
    ) internal {
        require(_strategyParams.length > 0, InputArrayLengthZero());
        uint256 numStratsToAdd = _strategyParams.length;
        uint256 numStratsExisting = strategyParams[quorumNumber].length;
        require(
            numStratsExisting + numStratsToAdd <= MAX_WEIGHING_FUNCTION_LENGTH,
            InputArrayLengthMismatch()
        );
        for (uint256 i = 0; i < numStratsToAdd; i++) {
            // fairly gas-expensive internal loop to make sure that the *same* strategy cannot be added multiple times
            for (uint256 j = 0; j < (numStratsExisting + i); j++) {
                require(
                    strategyParams[quorumNumber][j].strategy != _strategyParams[i].strategy,
                    InputDuplicateStrategy()
                );
            }
            require(_strategyParams[i].multiplier > 0, InputMultiplierZero());
            strategyParams[quorumNumber].push(_strategyParams[i]);
            strategiesPerQuorum[quorumNumber].push(_strategyParams[i].strategy);
            emit StrategyAddedToQuorum(quorumNumber, _strategyParams[i].strategy);
            emit StrategyMultiplierUpdated(
                quorumNumber, _strategyParams[i].strategy, _strategyParams[i].multiplier
            );
        }
    }

    /// @notice Returns the change between a previous and current value as a signed int
    function _calculateDelta(uint96 prev, uint96 cur) internal pure returns (int256) {
        return int256(uint256(cur)) - int256(uint256(prev));
    }

    /// @notice Adds or subtracts delta from value, according to its sign
    function _applyDelta(uint96 value, int256 delta) internal pure returns (uint96) {
        if (delta < 0) {
            return value - uint96(uint256(-delta));
        } else {
            return value + uint96(uint256(delta));
        }
    }

    /// @notice Checks that the `stakeUpdate` was valid at the given `blockNumber`
    function _validateStakeUpdateAtBlockNumber(
        StakeUpdate memory stakeUpdate,
        uint32 blockNumber
    ) internal pure {
        /**
         * Check that the update is valid for the given blockNumber:
         * - blockNumber should be >= the update block number
         * - the next update block number should be either 0 or strictly greater than blockNumber
         */
        require(blockNumber >= stakeUpdate.updateBlockNumber, InvalidBlockNumber());
        require(
            stakeUpdate.nextUpdateBlockNumber == 0
                || blockNumber < stakeUpdate.nextUpdateBlockNumber,
            InvalidBlockNumber()
        );
    }

    /// Returns total Slashable stake for a list of operators per strategy that can have the weights applied based on strategy multipliers
    function _getSlashableStakePerStrategy(
        uint8 quorumNumber,
        address[] memory operators
    ) internal view returns (uint256[][] memory) {
        uint32 beforeBlock = uint32(block.number + slashableStakeLookAheadPerQuorum[quorumNumber]);

        uint256[][] memory slashableShares = allocationManager.getMinimumSlashableStake(
            OperatorSet(registryCoordinator.avs(), quorumNumber),
            operators,
            strategiesPerQuorum[quorumNumber],
            beforeBlock
        );

        return slashableShares;
    }

    /**
     * @notice This function computes the total weight of the @param operators in the quorum @param quorumNumber.
     * @dev this method DOES NOT check that the quorum exists
     * @return `uint96[] memory` The weighted sum of the operators' shares across each strategy considered by the quorum
     * @return `bool[] memory` True if the respective operator meets the quorum's minimum stake
     */
    function _weightOfOperatorsForQuorum(
        uint8 quorumNumber,
        address[] memory operators
    ) internal view virtual returns (uint96[] memory, bool[] memory) {
        uint96[] memory weights = new uint96[](operators.length);
        bool[] memory hasMinimumStakes = new bool[](operators.length);

        uint256 stratsLength = strategyParamsLength(quorumNumber);
        StrategyParams[] memory stratsAndMultipliers = strategyParams[quorumNumber];
        uint256[][] memory strategyShares;

        if (stakeTypePerQuorum[quorumNumber] == IStakeRegistryTypes.StakeType.TOTAL_SLASHABLE) {
            // get slashable stake for the operators from AllocationManager
            strategyShares = _getSlashableStakePerStrategy(quorumNumber, operators);
        } else {
            // get delegated stake for the operators from DelegationManager
            strategyShares =
                delegation.getOperatorsShares(operators, strategiesPerQuorum[quorumNumber]);
        }

        // Calculate weight of each operator and whether they contain minimum stake for the quorum
        for (uint256 opIndex = 0; opIndex < operators.length; opIndex++) {
            // 1. For the given operator, loop through the strategies and calculate the operator's
            // weight for the quorum
            for (uint256 stratIndex = 0; stratIndex < stratsLength; stratIndex++) {
                // get multiplier for strategy
                StrategyParams memory strategyAndMultiplier = stratsAndMultipliers[stratIndex];

                // calculate added weight for strategy and multiplier
                if (strategyShares[opIndex][stratIndex] > 0) {
                    weights[opIndex] += uint96(
                        strategyShares[opIndex][stratIndex] * strategyAndMultiplier.multiplier
                            / WEIGHTING_DIVISOR
                    );
                }
            }

            // 2. Check whether operator is above minimum stake threshold
            hasMinimumStakes[opIndex] = weights[opIndex] >= minimumStakeForQuorum[quorumNumber];
        }

        return (weights, hasMinimumStakes);
    }

    function _weightOfOperatorForQuorum(
        uint8 quorumNumber,
        address operator
    ) internal view virtual returns (uint96, bool) {
        address[] memory operators = new address[](1);
        operators[0] = operator;
        (uint96[] memory weights, bool[] memory hasMinimumStakes) =
            _weightOfOperatorsForQuorum(quorumNumber, operators);
        return (weights[0], hasMinimumStakes[0]);
    }

    /// @notice Returns `true` if the quorum has been initialized
    function _quorumExists(
        uint8 quorumNumber
    ) internal view returns (bool) {
        return _totalStakeHistory[quorumNumber].length != 0;
    }

    /**
     *
     *                         VIEW FUNCTIONS
     *
     */

    /// @inheritdoc IStakeRegistry
    function weightOfOperatorForQuorum(
        uint8 quorumNumber,
        address operator
    ) public view virtual quorumExists(quorumNumber) returns (uint96) {
        (uint96 stake,) = _weightOfOperatorForQuorum(quorumNumber, operator);
        return stake;
    }

    /// @inheritdoc IStakeRegistry
    function strategyParamsLength(
        uint8 quorumNumber
    ) public view returns (uint256) {
        return strategyParams[quorumNumber].length;
    }

    /// @inheritdoc IStakeRegistry
    function strategyParamsByIndex(
        uint8 quorumNumber,
        uint256 index
    ) public view returns (StrategyParams memory) {
        return strategyParams[quorumNumber][index];
    }

    /**
     *
     *                   VIEW FUNCTIONS - Operator Stake History
     *
     */

    /// @inheritdoc IStakeRegistry
    function getStakeHistoryLength(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (uint256) {
        return operatorStakeHistory[operatorId][quorumNumber].length;
    }

    /// @inheritdoc IStakeRegistry
    function getStakeHistory(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (StakeUpdate[] memory) {
        return operatorStakeHistory[operatorId][quorumNumber];
    }

    /// @inheritdoc IStakeRegistry
    function getCurrentStake(
        bytes32 operatorId,
        uint8 quorumNumber
    ) external view returns (uint96) {
        StakeUpdate memory operatorStakeUpdate = getLatestStakeUpdate(operatorId, quorumNumber);
        return operatorStakeUpdate.stake;
    }

    /// @inheritdoc IStakeRegistry
    function getLatestStakeUpdate(
        bytes32 operatorId,
        uint8 quorumNumber
    ) public view returns (StakeUpdate memory) {
        uint256 historyLength = operatorStakeHistory[operatorId][quorumNumber].length;
        StakeUpdate memory operatorStakeUpdate;
        if (historyLength == 0) {
            return operatorStakeUpdate;
        } else {
            operatorStakeUpdate = operatorStakeHistory[operatorId][quorumNumber][historyLength - 1];
            return operatorStakeUpdate;
        }
    }

    /// @inheritdoc IStakeRegistry
    function getStakeUpdateAtIndex(
        uint8 quorumNumber,
        bytes32 operatorId,
        uint256 index
    ) external view returns (StakeUpdate memory) {
        return operatorStakeHistory[operatorId][quorumNumber][index];
    }

    /// @inheritdoc IStakeRegistry
    function getStakeAtBlockNumber(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint96) {
        return operatorStakeHistory[operatorId][quorumNumber][_getStakeUpdateIndexForOperatorAtBlockNumber(
            operatorId, quorumNumber, blockNumber
        )].stake;
    }

    /// @inheritdoc IStakeRegistry
    function getStakeUpdateIndexAtBlockNumber(
        bytes32 operatorId,
        uint8 quorumNumber,
        uint32 blockNumber
    ) external view returns (uint32) {
        return _getStakeUpdateIndexForOperatorAtBlockNumber(operatorId, quorumNumber, blockNumber);
    }

    /// @inheritdoc IStakeRegistry
    function getStakeAtBlockNumberAndIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        bytes32 operatorId,
        uint256 index
    ) external view returns (uint96) {
        StakeUpdate memory operatorStakeUpdate =
            operatorStakeHistory[operatorId][quorumNumber][index];
        _validateStakeUpdateAtBlockNumber(operatorStakeUpdate, blockNumber);
        return operatorStakeUpdate.stake;
    }

    /**
     *
     *                     VIEW FUNCTIONS - Total Stake History
     *
     */

    /// @inheritdoc IStakeRegistry
    function getTotalStakeHistoryLength(
        uint8 quorumNumber
    ) external view returns (uint256) {
        return _totalStakeHistory[quorumNumber].length;
    }

    /// @inheritdoc IStakeRegistry
    function getCurrentTotalStake(
        uint8 quorumNumber
    ) external view returns (uint96) {
        return _totalStakeHistory[quorumNumber][_totalStakeHistory[quorumNumber].length - 1].stake;
    }

    /// @inheritdoc IStakeRegistry
    function getTotalStakeUpdateAtIndex(
        uint8 quorumNumber,
        uint256 index
    ) external view returns (StakeUpdate memory) {
        return _totalStakeHistory[quorumNumber][index];
    }

    /// @inheritdoc IStakeRegistry
    function getTotalStakeAtBlockNumberFromIndex(
        uint8 quorumNumber,
        uint32 blockNumber,
        uint256 index
    ) external view returns (uint96) {
        StakeUpdate memory totalStakeUpdate = _totalStakeHistory[quorumNumber][index];
        _validateStakeUpdateAtBlockNumber(totalStakeUpdate, blockNumber);
        return totalStakeUpdate.stake;
    }

    /// @inheritdoc IStakeRegistry
    function getTotalStakeIndicesAtBlockNumber(
        uint32 blockNumber,
        bytes calldata quorumNumbers
    ) external view returns (uint32[] memory) {
        uint32[] memory indices = new uint32[](quorumNumbers.length);
        for (uint256 i = 0; i < quorumNumbers.length; i++) {
            uint8 quorumNumber = uint8(quorumNumbers[i]);
            _checkQuorumExists(quorumNumber);
            require(
                _totalStakeHistory[quorumNumber][0].updateBlockNumber <= blockNumber,
                EmptyStakeHistory()
            );
            uint256 length = _totalStakeHistory[quorumNumber].length;
            for (uint256 j = 0; j < length; j++) {
                if (
                    _totalStakeHistory[quorumNumber][length - j - 1].updateBlockNumber
                        <= blockNumber
                ) {
                    indices[i] = uint32(length - j - 1);
                    break;
                }
            }
        }
        return indices;
    }

    /**
     * @notice Sets the stake type for the registry for a specific quorum
     * @param quorumNumber The quorum number to set the stake type for
     * @param _stakeType The type of stake to track (TOTAL_DELEGATED, TOTAL_SLASHABLE, or BOTH)
     */
    function _setStakeType(uint8 quorumNumber, IStakeRegistryTypes.StakeType _stakeType) internal {
        stakeTypePerQuorum[quorumNumber] = _stakeType;
        emit StakeTypeSet(_stakeType);
    }

    /**
     * @notice Sets the look ahead time for checking operator shares for a specific quorum
     * @param quorumNumber The quorum number to set the look ahead period for
     * @param _lookAheadBlocks The number of blocks to look ahead when checking shares
     */
    function _setLookAheadPeriod(uint8 quorumNumber, uint32 _lookAheadBlocks) internal {
        require(
            stakeTypePerQuorum[quorumNumber] == IStakeRegistryTypes.StakeType.TOTAL_SLASHABLE,
            QuorumNotSlashable()
        );
        uint32 oldLookAheadDays = slashableStakeLookAheadPerQuorum[quorumNumber];
        slashableStakeLookAheadPerQuorum[quorumNumber] = _lookAheadBlocks;
        emit LookAheadPeriodChanged(oldLookAheadDays, _lookAheadBlocks);
    }

    function _checkSlashingRegistryCoordinator() internal view {
        require(msg.sender == address(registryCoordinator), OnlySlashingRegistryCoordinator());
    }

    function _checkSlashingRegistryCoordinatorOwner() internal view {
        require(
            msg.sender == Ownable(address(registryCoordinator)).owner(),
            OnlySlashingRegistryCoordinatorOwner()
        );
    }

    function _checkQuorumExists(
        uint8 quorumNumber
    ) internal view {
        require(_quorumExists(quorumNumber), QuorumDoesNotExist());
    }
}
````

## File: src/StakeRegistryStorage.sol
````
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.27;

import {IDelegationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IDelegationManager.sol";
import {IAVSDirectory} from "eigenlayer-contracts/src/contracts/interfaces/IAVSDirectory.sol";
import {IAllocationManager} from
    "eigenlayer-contracts/src/contracts/interfaces/IAllocationManager.sol";
import {
    IStrategyManager,
    IStrategy
} from "eigenlayer-contracts/src/contracts/interfaces/IStrategyManager.sol";

import {ISlashingRegistryCoordinator} from "./interfaces/ISlashingRegistryCoordinator.sol";
import {IStakeRegistry, IStakeRegistryTypes} from "./interfaces/IStakeRegistry.sol";

/**
 * @title Storage variables for the `StakeRegistry` contract.
 * @author Layr Labs, Inc.
 * @notice This storage contract is separate from the logic to simplify the upgrade process.
 */
abstract contract StakeRegistryStorage is IStakeRegistry {
    /// @notice Constant used as a divisor in calculating weights.
    uint256 public constant WEIGHTING_DIVISOR = 1e18;
    /// @notice Maximum length of dynamic arrays in the `strategyParams` mapping.
    uint8 public constant MAX_WEIGHING_FUNCTION_LENGTH = 32;
    /// @notice Constant used as a divisor in dealing with BIPS amounts.
    uint256 internal constant MAX_BIPS = 10000;

    /// @notice The address of the Delegation contract for EigenLayer.
    IDelegationManager public immutable delegation;

    /// @notice The address of the Delegation contract for EigenLayer.
    IAVSDirectory public immutable avsDirectory;

    /// @notice the address of the AllocationManager for EigenLayer.
    IAllocationManager public immutable allocationManager;

    /// @notice the coordinator contract that this registry is associated with
    ISlashingRegistryCoordinator public immutable registryCoordinator;

    /// @notice In order to register for a quorum i, an operator must have at least `minimumStakeForQuorum[i]`
    /// evaluated by this contract's 'VoteWeigher' logic.
    mapping(uint8 => uint96) public minimumStakeForQuorum;

    /// @notice History of the total stakes for each quorum
    mapping(uint8 => StakeUpdate[]) internal _totalStakeHistory;

    /// @notice mapping from operator's operatorId to the history of their stake updates
    mapping(bytes32 operatorId => mapping(uint8 => StakeUpdate[])) internal operatorStakeHistory;

    /**
     * @notice mapping from quorum number to the list of strategies considered and their
     * corresponding multipliers for that specific quorum
     */
    mapping(uint8 quorumNumber => StrategyParams[]) public strategyParams;

    /// @notice mapping from quorum number to the list of strategies considered for that specific quorum
    mapping(uint8 quorumNumber => IStrategy[]) public strategiesPerQuorum;

    /// @notice mapping from quorum number to the IStakeRegistryTypes.StakeType for that specific quorum
    mapping(uint8 quorumNumber => IStakeRegistryTypes.StakeType) public stakeTypePerQuorum;

    /// @notice mapping from quorum number to the slashable stake look ahead time (in blocks)
    mapping(uint8 quorumNumber => uint32) public slashableStakeLookAheadPerQuorum;

    constructor(
        ISlashingRegistryCoordinator _slashingRegistryCoordinator,
        IDelegationManager _delegationManager,
        IAVSDirectory _avsDirectory,
        IAllocationManager _allocationManager
    ) {
        registryCoordinator = _slashingRegistryCoordinator;
        delegation = _delegationManager;
        avsDirectory = _avsDirectory;
        allocationManager = _allocationManager;
    }

    // storage gap for upgradeability
    // slither-disable-next-line shadowing-state
    uint256[43] private __GAP;
}
````
