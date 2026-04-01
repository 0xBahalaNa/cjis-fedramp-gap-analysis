# CJIS v6.0 to FedRAMP High — Control-by-Control Delta Analysis

Source data: `data/cjis-overlay.json` (OSCAL overlay with structured delta capture)

## Delta Controls

| Control | Category | Status |
|---------|----------|--------|
| AC-2 | Access Control | Pending |
| AT-2 | Training | Pending |
| AU-6 | Audit | Pending |
| IA-2 | Authentication | Pending |
| IA-5 | Authentication | Pending |
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
