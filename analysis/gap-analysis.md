# CJIS v6.0 to FedRAMP High — Control-by-Control Delta Analysis

Source data: `data/cjis-overlay.json` (OSCAL overlay with structured delta capture)

## Delta Controls

| Control | Category | Status |
|---------|----------|--------|
| AC-2 | Access Control | Pending |
| AT-2 | Training | Pending |
| AU-6 | Audit | Pending |
| IA-2 | Authentication | Complete |
| IA-5 | Authentication | Complete |
| IR-6 | Incident Response | Pending |
| MP-6 | Media Protection | Complete |
| PE-17 | Physical/Environmental | Pending |
| PS-3 | Personnel | Complete |
| PS-6 | Personnel | Complete |
| SC-12 | Encryption | Complete |
| SC-13 | Encryption | Complete |
| SC-28 | Encryption | Complete |

---

## Personnel Security

### PS-3 — Personnel Screening

**NIST 800-53 Rev 5 Control:** Screen individuals prior to authorizing access to the system; rescreen individuals in accordance with organization-defined conditions and frequency.

**CJIS v6.0 Reference:** Section 5.12 (Personnel Security)

#### FedRAMP High Baseline Requirement

FedRAMP High requires background investigations for personnel with access to the information system, commensurate with the risk level of the assigned position. The baseline defers to organization-defined parameters for:

- **Rescreening conditions** — the organization defines what triggers rescreening (role change, time-based, etc.)
- **Rescreening frequency** — the organization defines how often rescreening occurs

FedRAMP does not prescribe the *type* of background check. A standard OPM-tier investigation or commercial background check satisfies the baseline, as long as it matches the position risk designation. This flexibility is where CJIS diverges.

#### CJIS v6.0 Delta

CJIS eliminates the flexibility in screening method and scope:

- **Fingerprint-based background check** required — not just any background investigation, but specifically a fingerprint submission processed through state and national (FBI) criminal history repositories.
- **All CJI access** — applies to every individual with access to unencrypted CJI, regardless of position risk level. There is no "low-risk position" exemption.
- **No role exceptions** — contractors, vendor personnel, and CSP staff with logical or physical access to unencrypted CJI must complete fingerprint-based checks. A CSP system administrator who can access the database where CJI is stored is in scope, even if they never query CJI directly.
- **Pre-access requirement** — the fingerprint check must be completed and adjudicated *before* access is provisioned, not concurrently.

**Why this matters:** A CSP operating under FedRAMP High may have completed standard background checks for all personnel. Those checks satisfy FedRAMP but do not satisfy CJIS unless they included fingerprint submission to state and FBI repositories. This is a process gap, not a policy gap — the CSP likely already has a screening program, but it needs to be augmented with the fingerprint-specific requirement.

#### Implementation Guidance

1. **Establish a fingerprint submission process** with the state CJIS Systems Agency (CSA). Each state has a CSA that coordinates fingerprint-based background checks. The CSP must work with the subscribing agency's CSA to set up the submission channel.
2. **Identify all in-scope personnel.** Map every individual (employee, contractor, subcontractor) with logical or physical access to systems that store, process, or transmit unencrypted CJI. Include system administrators, database administrators, and operations staff — not just application users.
3. **Integrate into onboarding.** Add fingerprint submission as a gate in the personnel onboarding workflow. CJI access must not be provisioned until the fingerprint check is completed and adjudicated favorably.
4. **Define rescreening triggers.** Work with the CSA to determine rescreening conditions — typically role changes, periodic intervals (often every 5 years), or when the CSA mandates it.
5. **Maintain records.** Track fingerprint submission dates, adjudication results, and the link between each individual and their CJI access authorization.

#### Evidence Required

An auditor will expect to see:

- **Fingerprint submission records** for each individual with CJI access, showing submission to state and FBI repositories
- **Background check adjudication results** confirming favorable determination prior to CJI access
- **Personnel roster cross-referenced with CJI access list** — demonstrating that every person with CJI access has a completed fingerprint check on file
- **Onboarding procedures** documenting the fingerprint check as a prerequisite for CJI access provisioning
- **Rescreening schedule** and completion records showing the organization tracks and executes rescreening

#### Key Considerations

- **Encryption safe harbor:** If CJI is encrypted with agency-managed keys and the CSP personnel cannot access the decryption keys, those personnel may be outside the fingerprint check scope. This is a common architecture strategy to reduce the population requiring fingerprint checks, but the encryption implementation must be defensible (see SC-12, SC-28).
- **Subcontractor chains:** If the CSP uses subcontractors (e.g., managed services, data center staff), the fingerprint requirement flows down. The CSP must ensure subcontractors complete fingerprint checks or are architecturally excluded from CJI access.
- **State-specific variations:** While CJIS v6.0 sets the floor, individual state CSAs may impose additional screening requirements. The CSP should confirm requirements with each state CSA it serves.
- **Timeline:** Fingerprint-based checks can take weeks to complete depending on the state. Plan for this lead time in hiring and onboarding processes.

---

### PS-6 — Access Agreements

**NIST 800-53 Rev 5 Control:** Develop and document access agreements for organizational systems; review and update access agreements at organization-defined frequency; verify that individuals sign appropriate access agreements prior to being granted access and re-sign when agreements are updated.

**CJIS v6.0 Reference:** Section 5.12 (Personnel Security)

#### FedRAMP High Baseline Requirement

FedRAMP High requires signed access agreements before system access is granted. This includes:

- **Nondisclosure agreements** — protecting sensitive information
- **Acceptable use agreements** — defining permitted system use
- **Rules of behavior** — documenting user responsibilities and expected conduct

The baseline defers to the organization for:

- **Review/update frequency** — how often access agreements are reviewed and refreshed
- **Re-signing frequency** — how often users must re-sign to maintain access

Standard access agreements cover general system use, data handling, and security responsibilities. FedRAMP does not prescribe a specific agreement template for particular data types.

#### CJIS v6.0 Delta

CJIS requires a specific, additional agreement beyond standard access agreements:

- **CJIS Security Addendum** — a binding legal agreement that must be executed by every individual with access to CJI. This is a separate document from standard rules of behavior or acceptable use agreements.
- **CJI-specific content** — the Security Addendum specifically addresses CJI handling rules, dissemination restrictions, sanctions for misuse or unauthorized disclosure, and obligations that survive termination of access.
- **Pre-access gate** — the signed addendum is a prerequisite for CJI access, not something that can be completed after access is provisioned.
- **Binding on individuals, not just organizations** — the addendum is signed by each person, not covered by an organizational MOU. An MOU between the agency and CSP is also required, but does not replace individual addendums.

**Why this matters:** A CSP with a FedRAMP High ATO has access agreements, but they are generic to the system. The CJIS Security Addendum is a *specific document* with *specific content* about CJI. A CSP cannot substitute its standard rules of behavior for the CJIS Security Addendum — both are required.

#### Implementation Guidance

1. **Obtain the CJIS Security Addendum template** from the state CSA. The FBI CJIS Division publishes a standard template, but state CSAs may have modified versions. Use the version required by the subscribing agency's CSA.
2. **Integrate addendum signing into onboarding.** Add the CJIS Security Addendum as a required step alongside (not replacing) standard access agreements. The addendum must be signed before CJI access is provisioned.
3. **Maintain signed addendums on file.** Store executed addendums in a retrievable format, linked to the individual's access record. Both physical and electronic signatures are typically acceptable.
4. **Track re-execution requirements.** When the Security Addendum terms are updated (e.g., by the CSA or FBI CJIS Division), all personnel with CJI access must re-sign. Track addendum versions and re-execution dates.
5. **Include in separation procedures.** When an individual's CJI access is revoked, document the termination of the addendum obligation and retain the signed addendum per the CSA's records retention requirements.

#### Evidence Required

An auditor will expect to see:

- **Signed CJIS Security Addendum** for every individual with CJI access — one per person, not a collective agreement
- **Addendum tracking log** showing execution dates, addendum version, and the individual's name and role
- **Evidence of pre-access signing** — demonstrating the addendum was signed before CJI access was granted (compare signing date to access provisioning date)
- **Re-execution records** showing personnel re-signed when addendum terms were updated
- **Organizational MOU/MOA** between the agency and CSP covering CJIS responsibilities (separate from individual addendums)

#### Key Considerations

- **The addendum is not optional.** Even if the CSP has comprehensive rules of behavior that cover similar ground, the CJIS Security Addendum is a specific, named document that auditors will look for by name.
- **Contractor and subcontractor coverage.** The addendum requirement applies to all personnel with CJI access, including third-party contractors. The CSP must ensure its vendors execute addendums if their personnel access CJI.
- **Relationship to PS-3.** The Security Addendum and fingerprint-based background check (PS-3) are complementary — both must be completed before CJI access is granted. The addendum is the *legal agreement*; the fingerprint check is the *screening verification*.
- **Records retention.** Retain signed addendums for the duration required by the CSA. Some states require retention beyond the period of CJI access.

---

## Identification and Authentication

### IA-2 — Identification and Authentication (Organizational Users)

**NIST 800-53 Rev 5 Control:** Uniquely identify and authenticate organizational users and associate that unique identification with processes acting on behalf of those users.

**CJIS v6.0 Reference:** Section 5.6 (Identification and Authentication)

#### FedRAMP High Baseline Requirement

FedRAMP High requires multi-factor authentication for all users through enhancements IA-2(1) (MFA for privileged accounts) and IA-2(2) (MFA for non-privileged accounts). The baseline requires:

- **Unique identification** — each user has a distinct identity, no shared accounts for accountability purposes
- **Multi-factor authentication** — two or more factors from: something you know, something you have, something you are
- **Network and local access** — MFA applies to both network (remote) and local access for privileged accounts

FedRAMP High does not prescribe a specific Authentication Assurance Level (AAL) from NIST SP 800-63-3, nor does it mandate phishing resistance. An organization using SMS-based OTP as a second factor satisfies the FedRAMP High baseline, even though SMS OTP is vulnerable to SIM-swapping and interception attacks.

#### CJIS v6.0 Delta

CJIS defines "Advanced Authentication" with specific requirements that narrow the acceptable MFA implementations:

- **AAL2 minimum** — per NIST SP 800-63-3, AAL2 requires multi-factor authentication using authenticators with proven possession through a cryptographic protocol or comparable mechanism. This eliminates some weaker MFA implementations that satisfy AAL1 but not AAL2.
- **Phishing-resistant authenticators preferred** — while AAL2 is the floor, CJIS guidance favors phishing-resistant methods (FIDO2/WebAuthn, PIV/CAC cards, hardware security keys). Software OTP tokens (TOTP apps) meet AAL2 but are not phishing-resistant.
- **Mandatory trigger conditions** — Advanced Authentication is required when accessing CJI from outside a physically secure location (defined by the agency) or when accessing CJI over any network. This means remote access to CJI always requires Advanced Authentication.
- **Two distinct factors required** — must use two of the three categories: something you know (password/PIN), something you have (token/smart card/phone), something you are (biometric). Two factors from the same category do not satisfy the requirement.

**Why this matters:** A CSP using basic MFA (e.g., password + SMS OTP) satisfies FedRAMP High but may not satisfy CJIS Advanced Authentication. The distinction is the assurance level — CJIS requires AAL2, which demands that the authenticator prove possession through a cryptographic protocol. SMS OTP does not meet AAL2 because the phone number is not a cryptographic authenticator. Organizations must evaluate their current MFA stack against NIST SP 800-63-3 AAL2 requirements, not just confirm that "MFA is enabled."

#### NIST SP 800-63-3 AAL Levels (Reference)

Understanding the AAL hierarchy is essential for evaluating whether current MFA satisfies CJIS:

- **AAL1** — single-factor or multi-factor, no cryptographic proof of possession required. Password-only or password + SMS OTP can satisfy AAL1.
- **AAL2** — multi-factor required, with at least one factor providing cryptographic proof of authenticator possession. Examples: TOTP app + password, hardware OTP token + password, smart card + PIN. *This is the CJIS floor.*
- **AAL3** — hardware-based cryptographic authenticator required, verifier impersonation resistance mandatory. Examples: PIV/CAC + PIN, FIDO2 hardware key + PIN. This exceeds CJIS requirements but is the strongest option.

#### Implementation Guidance

1. **Inventory current MFA implementations.** Map each CJI access path to the authenticator types in use. Classify each against NIST SP 800-63-3 AAL levels. Any path using SMS OTP, email codes, or voice callbacks as the second factor likely falls below AAL2.
2. **Deploy AAL2-compliant authenticators.** TOTP apps (Authy, Google Authenticator, Microsoft Authenticator) meet AAL2 minimum. For stronger posture, deploy FIDO2/WebAuthn hardware keys or PIV/CAC cards, which meet AAL3 and provide phishing resistance.
3. **Enforce MFA at the application layer.** MFA must be enforced at the point of CJI access, not just at the network perimeter. If a user authenticates with MFA to a VPN but then accesses the CJI application with only a password, the Advanced Authentication requirement is not satisfied for the CJI access.
4. **Document authenticator classifications.** Maintain a record mapping each authenticator type to its AAL level, with references to NIST SP 800-63-3. This documentation is critical during audits.
5. **Address the physically secure location exception.** Work with the subscribing agency to define which locations qualify as "physically secure" for CJIS purposes. Users at physically secure locations accessing CJI over local networks may have different authentication requirements — but this exception must be formally documented and approved by the agency.

#### Evidence Required

An auditor will expect to see:

- **MFA configuration exports** from the identity provider showing enforcement for all CJI access paths
- **Authenticator type inventory** classified by AAL level per NIST SP 800-63-3
- **Authentication policy documentation** specifying CJIS Advanced Authentication requirements and which authenticator types are approved
- **Authentication logs** demonstrating MFA enforcement for CJI access events
- **Network diagrams** showing MFA enforcement points relative to CJI data flows
- **Physically secure location documentation** if the exception is claimed, including agency approval

#### Key Considerations

- **SMS OTP is the most common gap.** Many organizations implemented SMS-based MFA to satisfy FedRAMP. For CJIS, SMS does not meet AAL2. This is often the single highest-effort remediation item in the IA-2 delta.
- **SSO complicates the picture.** If users authenticate via SSO, the AAL level is determined by the IdP's authentication flow, not the downstream application. Verify the IdP enforces AAL2 for sessions that will access CJI.
- **Conditional access policies.** Consider using conditional access to require stronger authenticators (AAL3/phishing-resistant) for CJI access while allowing AAL2 for non-CJI systems. This reduces friction while exceeding CJIS minimums for the most sensitive access.
- **Grace period for migration.** If migrating from SMS OTP to AAL2-compliant authenticators, coordinate with the CSA on timeline expectations. An in-progress migration with a documented plan and deadline is better than no plan.

---

### IA-5 — Authenticator Management

**NIST 800-53 Rev 5 Control:** Manage system authenticators by verifying identity during initial distribution, establishing initial authenticator content, ensuring sufficient strength, implementing administrative procedures, changing defaults, and protecting authenticator content from unauthorized disclosure and modification.

**CJIS v6.0 Reference:** Section 5.6 (Identification and Authentication)

#### FedRAMP High Baseline Requirement

FedRAMP High requires authenticator management with organization-defined parameters for:

- **Refresh/change period** — the organization defines the time period for changing or refreshing authenticators by type
- **Change-triggering events** — the organization defines events that require authenticator changes (compromise, personnel change, etc.)

For password-based authenticators specifically, FedRAMP High enhancement IA-5(1) requires password complexity and rotation, but the specific values (minimum length, composition rules, rotation period, history depth) are left to the organization. FedRAMP provides guidance through its parameter requirements but does not dictate exact values for all parameters.

#### CJIS v6.0 Delta

CJIS replaces the org-defined flexibility with prescriptive password parameters:

- **Minimum 8 characters** — this is a floor, not a recommendation. Passwords shorter than 8 characters are non-compliant regardless of complexity.
- **Complexity: 3 of 4 categories** — passwords must include characters from at least three of: uppercase letters, lowercase letters, numeric digits, special characters. This is more prescriptive than "enforce complexity" — it defines exactly what complexity means.
- **90-day maximum lifetime** — passwords must be changed at least every 90 days. This is a hard ceiling — the organization cannot set a longer rotation period for CJI systems.
- **10-password history** — users cannot reuse any of their last 10 passwords. This prevents trivial rotation patterns (Password1 → Password2 → Password1).

**Why this matters — the 800-63B tension:** NIST SP 800-63B (Digital Identity Guidelines, 2017) explicitly *discourages* periodic password rotation, recommending instead that passwords be changed only when there is evidence of compromise. The reasoning is sound — forced rotation leads to weaker passwords (users increment a number, add a symbol) and does not measurably improve security when combined with breach detection.

However, CJIS v6.0 requires 90-day rotation. For CJI systems, **CJIS requirements take precedence over 800-63B guidance.** This is not a contradiction in the framework — CJIS is a policy overlay with specific operational requirements for CJI, while 800-63B is a guideline. When they conflict, the more restrictive policy wins for the data type it governs. An auditor will not accept "we follow 800-63B instead" as a justification for skipping 90-day rotation on CJI systems.

#### Implementation Guidance

1. **Configure the identity provider** to enforce CJIS password parameters. Set minimum length to 8, require 3 of 4 character categories, enforce 90-day maximum age, and set password history to 10. These settings must be applied to all accounts that access CJI.
2. **Verify SSO/federated IdP compliance.** If CJI access flows through SSO, the upstream IdP must enforce CJIS password policy. A downstream application cannot compensate for a weak upstream password policy.
3. **Document the 800-63B deviation.** Since CJIS password requirements conflict with 800-63B guidance, document this explicitly: "CJI systems enforce 90-day password rotation per CJIS Security Policy v6.0 Section 5.6, which takes precedence over NIST SP 800-63B rotation guidance for systems processing CJI." This preempts questions about why your password policy appears to contradict NIST guidance.
4. **Scope the policy to CJI systems.** Non-CJI systems can follow 800-63B guidance (no forced rotation). Apply CJIS password parameters only to systems and accounts that access CJI. This reduces user friction on non-CJI systems while maintaining compliance.
5. **Monitor for weak rotation patterns.** Even with history enforcement, users may rotate through predictable patterns (Summer2026!, Fall2026!, Winter2027!). Consider implementing password similarity checks or a deny-list of common patterns if the IdP supports it.

#### Evidence Required

An auditor will expect to see:

- **Password policy configuration exports** from the IdP or directory service showing all four CJIS parameters (8-char minimum, 3-of-4 complexity, 90-day max age, 10-password history)
- **Scope documentation** showing which systems/accounts are subject to CJIS password policy
- **IdP configuration screenshots** demonstrating enforcement (not just documentation — actual system settings)
- **Documentation mapping** CJIS password requirements to implemented configuration, line by line
- **800-63B deviation justification** documenting why periodic rotation is enforced despite 800-63B guidance

#### Key Considerations

- **MFA does not replace password policy.** Even with AAL2 MFA (IA-2), CJIS still requires compliant password parameters. MFA and password policy are independent requirements — satisfying one does not exempt the other.
- **Service accounts and API keys.** Clarify with the CSA whether service accounts that access CJI must follow the same password parameters. Service accounts typically use long-lived secrets or certificates, not user-interactive passwords. The 90-day rotation may apply differently to these.
- **Password manager compatibility.** Organizations should encourage (or require) password managers to help users generate and manage complex passwords that change every 90 days. This mitigates the weak-rotation-pattern problem.
- **Future CJIS updates.** CJIS v6.0 aligned with 800-53 Rev 5 but retained legacy password parameters from earlier versions. Future CJIS updates may reconcile with 800-63B guidance. Until then, enforce the current requirements.

---

## Encryption

Encryption is the most technically consequential delta between CJIS v6.0 and FedRAMP High. FedRAMP High requires encryption with FIPS-validated modules but allows the CSP to manage encryption keys. CJIS fundamentally changes the key management model: the law enforcement agency — not the CSP — must retain control over encryption keys for CJI. This has direct architectural implications for AWS KMS, storage service configuration, and cross-account access patterns.

These three controls are tightly coupled:
- **SC-12** governs *who manages the keys* and the key lifecycle
- **SC-13** governs *what algorithms and key strengths* are acceptable
- **SC-28** applies both to *data at rest*, requiring agency-managed keys for stored CJI

### SC-12 — Cryptographic Key Establishment and Management

**NIST 800-53 Rev 5 Control:** Establish and manage cryptographic keys when cryptography is employed within the system in accordance with organization-defined key management requirements.

**CJIS v6.0 Reference:** Section 5.10.1 (Encryption)

#### FedRAMP High Baseline Requirement

FedRAMP High requires cryptographic key management using FIPS-validated cryptographic modules. The baseline defers to organization-defined parameters for:

- **Key management requirements** — the organization defines requirements for key generation, distribution, storage, access, and destruction
- **Key lifecycle management** — the organization determines rotation schedules, revocation procedures, and key recovery mechanisms

Critically, FedRAMP High does not prescribe *who* manages the keys. A CSP using AWS-managed keys (e.g., `aws/s3`, `aws/ebs` default KMS keys) satisfies the FedRAMP baseline. The CSP manages the key lifecycle, the keys live in the CSP's infrastructure, and the customer trusts the CSP to handle key management properly. This is the standard shared responsibility model for encryption — the CSP provides the service, the customer uses it.

#### CJIS v6.0 Delta

CJIS breaks the standard shared responsibility model for key management:

- **Agency must manage encryption keys** — the law enforcement agency, not the CSP, must retain control over key creation, distribution, storage, rotation, and revocation for keys protecting CJI. This is not a suggestion — it is a mandatory requirement that changes the fundamental trust model.
- **Agency retains revocation authority** — the agency must have the ability to immediately revoke the CSP's access to CJI by revoking or rotating encryption keys. If the agency cannot unilaterally cut off access, the key management model does not satisfy CJIS.
- **FIPS 140-2 or FIPS 140-3 validated modules** — all cryptographic modules in the key management chain must be FIPS validated. This aligns with FedRAMP but is explicitly stated in CJIS to ensure no gaps in the validation chain (e.g., a key wrapping layer that uses a non-validated module).
- **Key lifecycle documentation** — the agency must have documented procedures for key creation, rotation, revocation, and destruction. These procedures must be agency-controlled, not just inherited from the CSP's default key management.

**Why this matters:** A CSP with a FedRAMP High ATO is already using FIPS-validated encryption, but the keys are CSP-managed. Moving to agency-managed keys requires architectural changes — different KMS key types, different key policies, different IAM trust relationships. This is not a policy update; it is an infrastructure change that affects every service that encrypts CJI.

#### Implementation Guidance — AWS KMS Architecture

1. **Use customer-managed CMKs (not AWS-managed keys).** In AWS KMS, this means creating keys of type `CUSTOMER_MANAGED_CMK`, not using the default `aws/s3`, `aws/ebs`, or `aws/rds` service keys. Customer-managed CMKs allow the key policy to be configured with agency-specific access controls.
2. **Configure key policies to restrict administrative access to agency IAM principals.** The KMS key policy is the primary access control. The `kms:*` administrative permissions must be granted only to IAM roles or users controlled by the agency — not to the CSP's account. This is the mechanism that gives the agency revocation authority.
3. **Implement cross-account key sharing if the CSP operates in a separate AWS account.** The agency creates the CMK in their account and grants the CSP's account `kms:Encrypt`, `kms:Decrypt`, `kms:GenerateDataKey`, and `kms:ReEncrypt` permissions via the key policy. The agency retains `kms:*` (full administrative control). To revoke access, the agency removes the CSP's account from the key policy or disables the key.
4. **Document the key lifecycle.** Create procedures for: key creation (who initiates, who approves), key rotation (automatic annual rotation via KMS, or manual rotation with a defined schedule), emergency key revocation (steps to disable or delete the CMK, expected impact on CJI availability), and key destruction (KMS scheduled deletion with a 7-30 day waiting period).
5. **Verify FIPS 140-2/3 validation.** AWS KMS uses FIPS 140-2 Level 2 validated HSMs (certificate numbers are published in AWS documentation). Document the certificate numbers and validation status. If using CloudHSM for additional control, it provides FIPS 140-2 Level 3 validated modules.
6. **Test revocation.** Periodically test the agency's ability to revoke CSP access by temporarily removing permissions from the key policy and verifying that CJI becomes inaccessible to CSP services. This test validates the revocation procedure and confirms no backdoor access paths exist.

#### Evidence Required

An auditor will expect to see:

- **Key management policy** documenting agency key control responsibilities, not delegated to the CSP
- **KMS configuration exports** showing customer-managed CMKs (not AWS-managed default keys) for all CJI-related services
- **Key policy documents** showing agency IAM principals have administrative access and the CSP has only usage permissions
- **FIPS 140-2/3 validation certificates** for all cryptographic modules in the key management chain (AWS KMS HSM certificates)
- **Key rotation configuration** showing automatic or scheduled rotation with documented procedures
- **Emergency key revocation procedures** and test results demonstrating the agency can immediately cut off CSP access to CJI
- **Cross-account architecture diagram** (if applicable) showing key ownership in the agency's account and usage grants to the CSP's account

#### Key Considerations

- **AWS-managed keys vs. customer-managed CMKs.** This is the single most important distinction. AWS-managed keys (the default when you enable encryption on S3, EBS, RDS, etc.) are created and managed by AWS in your account but you cannot modify their key policies. Customer-managed CMKs give you full control over the key policy — this is what CJIS requires. Migrating from AWS-managed to customer-managed CMKs may require re-encrypting existing data.
- **CloudHSM as an alternative.** For agencies requiring FIPS 140-2 Level 3 (instead of Level 2), AWS CloudHSM provides dedicated HSMs. CloudHSM keys are managed entirely by the agency and never leave the HSM in plaintext. This exceeds CJIS requirements but adds operational complexity and cost.
- **Key policy vs. IAM policy.** In AWS KMS, the key policy is the authoritative access control. An IAM policy alone cannot grant access to a CMK if the key policy does not allow it. Agencies should rely on key policies (not just IAM policies) to enforce access controls — this ensures that even a compromised IAM administrator cannot access the CMK without key policy changes.
- **Relationship to SC-28.** SC-12 governs *who manages the keys*; SC-28 governs *where those keys are applied* (data at rest). An agency cannot satisfy SC-28 without first satisfying SC-12 — you cannot have agency-managed encryption at rest without agency-managed keys.
- **Multi-region considerations.** If CJI is replicated across AWS regions, the CMK must be replicated or separate CMKs must be created in each region. AWS KMS multi-Region keys can simplify this, but each replica must have the same agency-controlled key policy.

---

### SC-13 — Cryptographic Protection

**NIST 800-53 Rev 5 Control:** Determine the cryptographic uses required for the system; implement the required types of cryptography for each specified use.

**CJIS v6.0 Reference:** Section 5.10.1 (Encryption)

#### FedRAMP High Baseline Requirement

FedRAMP High requires FIPS-validated cryptography for information protection. The baseline defers to organization-defined parameters for:

- **Cryptographic uses** — the organization determines which data flows and storage locations require cryptographic protection
- **Types of cryptography** — the organization selects the specific algorithms and key lengths, as long as they are FIPS-validated

FedRAMP High does not prescribe minimum key lengths. An organization using AES-128 or AES-256 both satisfy the baseline, as long as the implementation uses a FIPS-validated module. Similarly, RSA-2048 and RSA-4096 are both acceptable. The baseline focuses on *validation status*, not *cryptographic strength* beyond what FIPS validation requires.

#### CJIS v6.0 Delta

CJIS adds prescriptive minimum key lengths on top of the FIPS validation requirement:

- **128-bit minimum symmetric key length** — AES-128 is the floor. AES-192 and AES-256 exceed the requirement. Any symmetric cipher below 128-bit is non-compliant for CJI, regardless of FIPS validation status.
- **2048-bit minimum asymmetric key length** — RSA-2048 is the floor. RSA-3072 and RSA-4096 exceed the requirement. For elliptic curve cryptography (ECC), the equivalent strength is approximately 224-bit ECC (NIST P-224 or higher), though NIST P-256 and P-384 are more commonly deployed.
- **FIPS 140-2 or FIPS 140-3 validated modules** — same as FedRAMP, but CJIS explicitly states this to close any ambiguity. Every cryptographic operation protecting CJI (encryption, decryption, hashing, signing, key exchange) must use a FIPS-validated module.
- **Applies to all CJI data states** — these minimums apply to CJI at rest, in transit, and in use. TLS configurations, disk encryption, database encryption, and any other cryptographic protection for CJI must meet these floors.

**Why this matters:** Most modern AWS services default to AES-256 for symmetric encryption and support RSA-2048+ for asymmetric operations, so the key length minimums are usually satisfied by default configurations. The real risk is in edge cases: legacy TLS configurations that include weaker cipher suites, custom application-layer encryption that uses shorter keys, or third-party integrations that negotiate down to weaker algorithms. The audit obligation is to *prove* compliance across all CJI data paths, not just assume it.

#### Implementation Guidance

1. **Audit all encryption configurations across CJI data paths.** Inventory every service and component that encrypts CJI — storage (S3, EBS, RDS, DynamoDB), transit (TLS, VPN, API gateways), and application-layer encryption. Document the algorithm and key length for each.
2. **Verify TLS configurations.** Ensure TLS configurations enforce FIPS-approved cipher suites with adequate key lengths. In AWS, this means using security policies that exclude weak ciphers. For ALB/NLB, use TLS security policies that enforce TLS 1.2+ with AES-128-GCM or AES-256-GCM cipher suites. Disable any cipher suite using keys below 128-bit or algorithms not on the FIPS-approved list.
3. **Check storage encryption defaults.** AWS KMS defaults to AES-256 for symmetric CMKs. S3 SSE-KMS, EBS encryption, and RDS encryption all use AES-256 when configured with KMS CMKs. Verify this by checking the key spec on each CMK (`SYMMETRIC_DEFAULT` = AES-256-GCM in AWS KMS).
4. **Review asymmetric key usage.** If using asymmetric keys for signing, key exchange, or certificate-based authentication, verify RSA-2048 minimum. For ECC, verify NIST P-256 or higher. Check TLS certificates, SSH keys, and any application-level digital signatures.
5. **Document FIPS validation chain.** For each cryptographic module, record the CMVP certificate number, validation level, and the specific operations it covers. AWS publishes certificate numbers for KMS, CloudHSM, and other services in their FIPS compliance documentation.
6. **Establish a cipher suite policy.** Create a documented list of approved cipher suites for CJI systems. Reference NIST SP 800-52 (TLS guidelines) for recommended cipher suites. Block any configuration change that introduces a cipher suite below the minimums.

#### Evidence Required

An auditor will expect to see:

- **Cryptographic inventory** mapping every CJI data path (at rest, in transit) to the algorithm and key length in use
- **FIPS 140-2/3 validation certificates** for each cryptographic module, with CMVP certificate numbers
- **TLS configuration exports** showing cipher suites, protocol versions, and key lengths for all endpoints handling CJI
- **KMS key specifications** showing symmetric key algorithm (AES-256) and any asymmetric key specs (RSA-2048+)
- **Cipher suite policy document** listing approved algorithms and minimum key lengths for CJI systems
- **Configuration scan results** from tools like SSL Labs, `nmap --script ssl-enum-ciphers`, or AWS Config rules showing no weak ciphers in use

#### Key Considerations

- **AES-256 is the practical default.** AWS KMS symmetric keys are AES-256-GCM. S3, EBS, RDS, and DynamoDB encryption all use AES-256 when configured with KMS. The 128-bit floor is unlikely to be a problem in AWS-native services — the risk is in custom or third-party components.
- **TLS is where gaps hide.** While storage encryption is typically AES-256, TLS configurations can inadvertently include weaker cipher suites from older security policies. Audit all load balancers, API gateways, and CloudFront distributions for cipher suite compliance.
- **ECC equivalence.** CJIS specifies 2048-bit asymmetric minimum, which is RSA-specific. For ECC, use NIST-recommended equivalent strength: P-256 (128-bit security level) meets the minimum, P-384 (192-bit) exceeds it. Document the equivalence rationale.
- **Relationship to SC-12 and SC-28.** SC-13 defines the *strength* of the cryptography; SC-12 defines *who manages* it; SC-28 defines *where it applies* for data at rest. All three must be satisfied together — using AES-256 (SC-13) with a CSP-managed key (fails SC-12) for data at rest (SC-28) would not satisfy CJIS.
- **Crypto agility.** As NIST post-quantum cryptography standards mature, agencies should plan for algorithm migration. CJIS v6.0 does not yet require post-quantum algorithms, but awareness of the transition timeline is a forward-looking consideration.

---

### SC-28 — Protection of Information at Rest

**NIST 800-53 Rev 5 Control:** Protect the confidentiality and integrity of information at rest.

**CJIS v6.0 Reference:** Section 5.10.1 (Encryption)

#### FedRAMP High Baseline Requirement

FedRAMP High requires encryption at rest for information stored on the system. The baseline requires protection of both confidentiality and integrity, with the organization defining:

- **Information requiring protection** — the organization determines which data at rest requires cryptographic protection
- **Protection mechanisms** — the organization selects the specific encryption method (e.g., full-disk encryption, database-level encryption, object-level encryption)

FedRAMP High is satisfied by enabling encryption on storage services using any FIPS-validated method. AWS-managed default encryption (e.g., S3 default encryption with `aws/s3` KMS key, EBS encryption with `aws/ebs` KMS key) satisfies the FedRAMP baseline. The key management is transparent to the customer — AWS creates, rotates, and manages the keys.

#### CJIS v6.0 Delta

CJIS adds a specific key ownership requirement that changes the entire encryption-at-rest architecture:

- **Agency-managed CMK required** — all CJI at rest must be encrypted with a customer master key controlled by the law enforcement agency. CSP-managed default encryption keys (AWS-managed keys like `aws/s3`, `aws/ebs`) are **insufficient** — they do not give the agency control over the key.
- **Agency retains key revocation authority** — the agency must be able to immediately render CJI inaccessible by disabling or deleting the CMK. This is the "kill switch" requirement — if the agency-CSP relationship ends or a breach occurs, the agency must be able to unilaterally cut off access to CJI by acting on the encryption key.
- **Applies to all storage locations** — every location where CJI is stored must use the agency-managed CMK. This includes primary databases, backups, replicas, caches, logs containing CJI, temporary files, and any other persistent storage. A single S3 bucket using the default `aws/s3` key while others use the agency CMK creates a compliance gap.
- **Encryption is not optional for CJI at rest** — unlike FedRAMP, which allows the organization to determine which data requires encryption at rest, CJIS mandates encryption for all CJI at rest with no exceptions.

**Why this matters:** This is the control that makes the PS-3 encryption safe harbor work. If CJI is encrypted at rest with agency-managed keys and the CSP cannot access those keys, CSP personnel may be outside the scope of fingerprint-based background checks (PS-3). But this only works if the encryption implementation is airtight — agency-managed keys, no CSP access to key administrative operations, and verifiable key revocation capability. SC-28 is where the rubber meets the road for the agency-managed encryption architecture.

#### Implementation Guidance — AWS Storage Services

1. **Create a dedicated CMK for CJI encryption.** In the agency's AWS account (or in a dedicated key management account), create a customer-managed symmetric CMK. Set the key policy to grant administrative access (`kms:*`) only to agency IAM principals. Grant the CSP's account or roles only usage permissions (`kms:Encrypt`, `kms:Decrypt`, `kms:GenerateDataKey`, `kms:DescribeKey`).
2. **Configure S3 bucket encryption.** For S3 buckets containing CJI, set the default encryption to use the agency-managed CMK (SSE-KMS with the CMK ARN). Apply a bucket policy that denies `s3:PutObject` without the `x-amz-server-side-encryption-aws-kms-key-id` header matching the CMK ARN. This prevents objects from being uploaded with the wrong key.
3. **Configure EBS volume encryption.** For EC2 instances processing CJI, enable EBS encryption using the agency-managed CMK. Set the default EBS encryption key in the account to the agency CMK. Existing volumes encrypted with AWS-managed keys must be re-encrypted — create a snapshot, copy the snapshot with the new CMK, and create a new volume from the re-encrypted snapshot.
4. **Configure RDS encryption.** RDS instances storing CJI must be encrypted with the agency-managed CMK. RDS encryption is set at instance creation and cannot be changed — if an existing instance uses the wrong key, the migration path is: snapshot → copy snapshot with agency CMK → restore to new instance from re-encrypted snapshot.
5. **Configure DynamoDB encryption.** For DynamoDB tables containing CJI, set encryption to use the agency-managed CMK (not the default `AWS_OWNED_CMK` or `AWS_MANAGED_CMK`).
6. **Address backup encryption.** AWS Backup, S3 replication, RDS automated backups, and EBS snapshots must all use the agency-managed CMK. Verify that backup configurations inherit the source encryption key or are explicitly configured to use the agency CMK.
7. **Inventory all CJI storage locations.** Create and maintain a complete inventory of every storage location containing CJI, mapped to the encryption key protecting it. Include primary storage, backups, replicas, logs, and caches. This inventory is critical for audit evidence and for verifying that no CJI exists outside the agency-managed CMK's protection.

#### Evidence Required

An auditor will expect to see:

- **Storage encryption configuration** for every service containing CJI, showing the agency-managed CMK ARN/ID (not AWS-managed key ARNs)
- **KMS key policy** showing agency-only administrative access and CSP limited to usage permissions
- **CJI storage inventory** mapping every storage location to its encryption key — demonstrating complete coverage
- **Key rotation configuration and logs** showing the CMK is rotated per agency policy
- **Key revocation test results** demonstrating that disabling the CMK renders CJI inaccessible across all storage locations
- **Bucket/volume/instance encryption configuration exports** from AWS CLI or Config showing encryption settings for each resource
- **Backup encryption verification** showing backups, snapshots, and replicas use the agency-managed CMK

#### Key Considerations

- **Migration from AWS-managed to customer-managed CMKs.** This is often the highest-effort item. S3 objects can be re-encrypted in place using S3 Batch Operations with a copy operation specifying the new CMK. EBS volumes and RDS instances require snapshot-copy-restore workflows. Plan for downtime and data validation during migration.
- **The encryption safe harbor for PS-3.** SC-28 with agency-managed keys is the foundation of the encryption safe harbor referenced in PS-3 (Personnel Screening). If CSP personnel cannot access the CMK, they cannot decrypt CJI, and they may be outside the scope of fingerprint-based background checks. But this only holds if the key policy is airtight — any administrative access by CSP IAM principals undermines the safe harbor.
- **Cost implications.** Customer-managed CMKs in AWS KMS cost $1/month per key plus per-request charges. CloudHSM (if used for FIPS 140-2 Level 3) costs significantly more (~$1.50/hour per HSM). The cost is modest relative to the compliance benefit, but should be budgeted.
- **Logging and monitoring.** Enable CloudTrail logging for all KMS API calls related to the CJI CMK. Monitor for unexpected `kms:DisableKey`, `kms:ScheduleKeyDeletion`, or key policy changes. Alarm on any CMK administrative action not initiated by an authorized agency principal.
- **Relationship to SC-12.** SC-28 cannot be satisfied without SC-12. The agency-managed CMK requirement (SC-28) depends on agency key management (SC-12). These controls must be implemented together — you cannot have agency-managed encryption at rest without agency-managed key lifecycle procedures.

---

## Media Protection

### MP-6 — Media Sanitization

**NIST 800-53 Rev 5 Control:** Sanitize organization-defined system media prior to disposal, release out of organizational control, or release for reuse using organization-defined sanitization techniques and procedures; employ sanitization mechanisms with the strength and integrity commensurate with the security category or classification of the information.

**CJIS v6.0 Reference:** Section 5.8 (Media Protection)

#### FedRAMP High Baseline Requirement

FedRAMP High requires media sanitization before disposal or reuse, but defers heavily to organization-defined parameters for:

- **System media scope** — the organization defines which media types are subject to sanitization before disposal, release, or reuse
- **Sanitization techniques and procedures** — the organization selects the sanitization methods, as long as the strength is commensurate with the security category of the information

FedRAMP High requires sanitization that is proportional to the data classification, and it references NIST SP 800-88 (Guidelines for Media Sanitization) as guidance. However, the baseline does not mandate specific sanitization methods per media type. An organization could clear, purge, or destroy media — the choice is left to the organization based on their risk assessment and the information's sensitivity. There is no explicit requirement for witnessed destruction or per-event sanitization records beyond what the organization defines in its own procedures.

#### CJIS v6.0 Delta

CJIS replaces the organizational flexibility with prescriptive sanitization requirements tied to media type:

- **NIST 800-88 Purge level minimum for electronic media** — electronic media containing CJI must be sanitized to at least the Purge level as defined in NIST SP 800-88. Purge renders data recovery infeasible using state-of-the-art laboratory techniques. Clear level (which protects against simple non-invasive recovery) is insufficient for CJI. This means standard "quick format" or single-pass overwrite may not satisfy the requirement — the method must achieve Purge-level assurance.
- **Physical destruction when Purge is not achievable** — media that cannot be reliably sanitized to Purge level must be physically destroyed. Approved destruction methods include shredding, incineration, and degaussing to the point of destruction (for magnetic media). This applies to damaged media, media with firmware-level storage (SSDs with wear-leveling that cannot guarantee complete overwrite), and any media where Purge-level sanitization cannot be verified.
- **Paper and microform: cross-cut shred or incinerate** — paper documents and microform (microfiche, microfilm) containing CJI must be cross-cut shredded or incinerated. Strip-cut shredding is insufficient — cross-cut produces particles small enough to prevent reconstruction. The shred size should align with NSA/CSS EPL-listed shredder specifications for classified material handling (typically 1mm x 5mm or smaller), though CJIS does not mandate a specific particle size.
- **Witnessed or verified sanitization** — sanitization events must be witnessed by an authorized individual or verified through testing. "Verified" means the sanitization result is confirmed (e.g., attempting data recovery on a sanitized drive to confirm data is irrecoverable). This is more rigorous than FedRAMP's general requirement, which does not mandate per-event witnessing.
- **Sanitization records** — each sanitization event must be documented with: media identifier (serial number, asset tag), media type, sanitization method used, date of sanitization, and identity of the person who performed and witnessed/verified the sanitization. These records must be retained per the organization's records retention policy.

**Why this matters:** A CSP operating under FedRAMP High likely has a media sanitization procedure, but it may rely on the CSP's own risk assessment to determine methods per media type. CJIS removes that discretion for CJI media — the methods are prescribed, destruction is mandatory in specific cases, and every event must be documented. For CSPs using cloud-only infrastructure (no physical media they control), the primary impact is on the agency side and on any hybrid components. For CSPs managing physical infrastructure, this affects hardware lifecycle management, decommissioning procedures, and vendor relationships for destruction services.

#### Implementation Guidance

1. **Create a CJI media sanitization matrix.** Map each media type to the approved sanitization method:

   | Media Type | Sanitization Method | Standard |
   |------------|-------------------|----------|
   | HDD (magnetic) | Purge (secure erase / cryptographic erase) or physical destruction (degauss + shred) | NIST 800-88 Purge |
   | SSD / Flash | Cryptographic erase (if supported with verification) or physical destruction (shred/incinerate) | NIST 800-88 Purge |
   | Magnetic tape | Degauss to point of destruction or incinerate | NIST 800-88 Destroy |
   | Optical media (CD/DVD) | Physical destruction (shred/incinerate) | NIST 800-88 Destroy |
   | Paper documents | Cross-cut shred or incinerate | CJIS v6.0 §5.8 |
   | Microform | Cross-cut shred or incinerate | CJIS v6.0 §5.8 |
   | Mobile devices | Cryptographic erase + factory reset (if verified) or physical destruction | NIST 800-88 Purge |

2. **Implement witnessed destruction procedures.** Designate authorized witnesses for sanitization events. For in-house sanitization, the witness must be present during the process. For third-party destruction vendors, require certificates of destruction that include media serial numbers, destruction method, date, and witness signatures.
3. **Establish a sanitization log.** Create a standardized log template capturing: media identifier, media type, data classification (CJI), sanitization method, date/time, operator name, witness/verifier name, and verification result (pass/fail). Retain logs per the agency's records retention requirements.
4. **Address SSD sanitization challenges.** SSDs with wear-leveling algorithms may retain data in overprovisioned cells even after a standard wipe. For SSDs containing CJI, cryptographic erase (where the encryption key is destroyed, rendering all data irrecoverable) is the preferred Purge method. If the SSD does not support verified cryptographic erase, physical destruction is required. Document the SSD manufacturer's sanitization capabilities and verification methods.
5. **Contract requirements for third-party destruction.** If using a third-party media destruction vendor, the contract must require: NIST 800-88 compliance, certificates of destruction per media item, chain-of-custody documentation during transport, and the right to audit the vendor's destruction process. The vendor's employees who handle CJI media may be subject to CJIS personnel requirements (PS-3, PS-6) if they have access to unencrypted CJI during the destruction process.
6. **Cloud-specific considerations.** For CJI stored entirely in AWS, the CSP (AWS) handles physical media sanitization under their shared responsibility model. AWS publishes media sanitization procedures in their SOC 2 reports and follows NIST 800-88 guidelines. The agency should obtain AWS's media sanitization attestation and verify it meets CJIS Purge/Destroy requirements. For any agency-controlled media (laptops, removable drives, backup tapes), the agency is directly responsible for CJIS-compliant sanitization.

#### Evidence Required

An auditor will expect to see:

- **Media sanitization policy** specific to CJI, referencing NIST 800-88 and CJIS v6.0 requirements
- **Sanitization matrix** mapping media types to approved methods (as described above)
- **Sanitization logs** with media identifiers, methods, dates, and witness signatures for each event
- **Certificates of destruction** from third-party destruction vendors, per media item
- **Verification records** showing sanitization effectiveness was confirmed (e.g., recovery attempt results)
- **Third-party vendor contracts** with NIST 800-88 compliance requirements and audit rights
- **CSP media sanitization attestation** (for cloud-hosted CJI) — AWS SOC 2 or equivalent documentation showing media sanitization procedures meet NIST 800-88 Purge/Destroy levels

#### Key Considerations

- **Cloud vs. on-premises distinction.** For fully cloud-hosted CJI, the physical media sanitization responsibility falls primarily on the CSP (AWS). The agency's responsibility shifts to: (1) obtaining the CSP's sanitization attestation, (2) verifying the attestation meets CJIS requirements, and (3) sanitizing any agency-controlled endpoints or removable media. This does not eliminate the requirement — it shifts where it applies.
- **SSD overprovisioning is the biggest technical risk.** Standard overwrite methods that work on HDDs do not guarantee complete data removal on SSDs due to wear-leveling and overprovisioned NAND cells. Cryptographic erase (ATA Secure Erase Enhanced or NVMe Format with crypto erase) is the only reliable Purge method for SSDs. If the SSD does not support verified cryptographic erase, physical destruction is the only compliant option.
- **Degaussing does not work on SSDs.** Degaussing is effective only on magnetic media (HDDs, tapes). SSDs use NAND flash storage, which is not affected by magnetic fields. An organization that includes "degaussing" as a sanitization method for all media types has a gap — SSDs must be handled separately.
- **Chain of custody for off-site destruction.** If media is transported to a destruction facility, document the chain of custody from the point of removal to the point of destruction. Any gap in the chain creates a period where CJI media is unaccounted for — this is an audit finding.
- **Relationship to SC-28 (encryption safe harbor).** If CJI at rest is encrypted with agency-managed keys (SC-28), cryptographic erase becomes a stronger sanitization option — destroying the encryption key renders the data irrecoverable regardless of whether the physical media is sanitized. This is another benefit of the agency-managed key architecture. However, cryptographic erase alone may not satisfy CJIS if the agency or CSA requires physical destruction for certain media types — confirm with the CSA.
