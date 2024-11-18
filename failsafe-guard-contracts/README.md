### FailSafe Guard Smart Contracts

#### Overview of FailSafe Guard

FailSafe Guard provides the following security features for [Safe multi-signature Wallets](https://safe.global/). After completing [the onboarding steps for FailSafe Guard](https://doc.getfailsafe.com/docs/failsafe-attestation-service/branches/main/8xsw0cm9oogeh-enrollment-of-safe-contract-for-attestation), any one of the signatories can configure a security policy on their Safe Wallet. The security policy can include the following security conditions:

1. Signatory's Geolocation: An allowed IP address range for each signatory’s wallet address.

2. Threat Intelligence: This policy utilizes [FailSafe Radar](https://app.getfailsafe.com/radar), a threat intelligence solution that provides a risk score for a given wallet address based on its on-chain behavior and anomalous transaction patterns.

    The user can configure one of the three risk threshold levels:

    1. High
    2. Medium
    3. Low

  - A risk threshold level of Low indicates that the system only tolerates a low risk score for the signatory’s wallet to allow the transaction.
  - A risk threshold level of High indicates that the system will tolerate a high risk score for the signatory’s wallet to allow the transaction.

3. Time-Based Restrictions: This policy defines the maximum duration allowed (in days) for the signatories to complete the approvals and execute the transaction.

For more details and step-by-step instructions on configuring the security policy, please refer to the guide [Adjusting Attestation Conditions](https://doc.getfailsafe.com/docs/failsafe-attestation-service/vjhemxtku31p0-adjusting-attestation-conditions).

#### Overview of FailSafe Guard Smart Contracts

1. `failsafe-guard-contracts/external` contains interfaces used by FailSafe Guard. The main interface, `Guard`, is defined in `failsafe-guard-contracts/external/GuardManager.sol`. This interface defines the signatures of the `checkTransaction` and `checkModuleTransaction` functions as per the guidelines from the Safe team for writing [Safe Guard contracts](https://docs.safe.global/advanced/smart-account-guards).

2. `failsafe-guard-contracts/guard` contains the source code for FailSafe Guard contracts.
   1. `failsafe-guard-contracts/guard/AttestationGuardFactory.sol` is the factory contract used by the FailSafe Guard cloud system to deploy individual Guard contract instances.
   2. `failsafe-public/blob/main/failsafe-guard-contracts/guard/AttestationGuard.sol` is deployed and attached to a FailSafe Guard user's Safe Wallet as part of the onboarding flow. After activating the FailSafe Guard contract, multi-signature transactions executed on the Safe Wallet will be attested by the FailSafe Guard cloud system via the `attestHash` function if all security conditions configured by the FailSafe Guard admin user are met. When the multi-signature transaction is executed by the Safe Wallet contract, it will call the `checkTransaction` function, which will return `true` only if the transaction being executed was attested by the FailSafe Guard cloud system.

Please refer to [FailSafe Attestation Guard Architecture](https://doc.getfailsafe.com/docs/failsafe-attestation-service/unihxlqj9rhpm-fail-safe-attestation-architecture) for the full architecture of FailSafe Guard.
