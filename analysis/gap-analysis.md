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
| MP-6 | Media Protection | Pending |
| PE-17 | Physical/Environmental | Pending |
| PS-3 | Personnel | Complete |
| PS-6 | Personnel | Complete |
| SC-12 | Encryption | Pending |
| SC-13 | Encryption | Pending |
| SC-28 | Encryption | Pending |

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
