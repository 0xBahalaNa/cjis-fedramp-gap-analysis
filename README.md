# CJIS v6.0 to FedRAMP High Gap Analysis

Identifies where the CJIS Security Policy v6.0 exceeds FedRAMP High baseline requirements on a control-by-control basis. CJIS v6.0 (December 2024) aligns with NIST 800-53 Rev 5 and becomes the audit standard on April 1, 2026. This project produces a structured delta analysis showing the specific controls where a law enforcement agency's cloud deployment must go beyond its FedRAMP High authorization to satisfy CJIS requirements. Built for GRC engineers, compliance analysts, and assessors working in public safety technology environments.

## Why This Matters

A CSP with a FedRAMP High ATO already satisfies the majority of CJIS v6.0 requirements — both frameworks derive from NIST 800-53 Rev 5. But "majority" is not "all." CJIS imposes additional requirements in specific control areas that reflect the sensitivity of Criminal Justice Information (CJI). An agency or CSP that assumes FedRAMP High equivalence without analyzing the deltas risks audit findings, delayed authorizations, or — in the worst case — unauthorized access to CJI.

This project makes those deltas explicit, traceable, and machine-readable.

## Gap Summary

The gap analysis distinguishes two categories of gaps between CJIS v6.0 and FedRAMP High:

- **Implementation-level deltas** — controls present in both baselines, where CJIS imposes stricter parameters, scope, or methodology (e.g., CJIS requires fingerprint-based background checks for PS-3, while FedRAMP allows the organization to define screening method).
- **Control-level gaps** — controls present in the CJIS v6.0 baseline but absent from FedRAMP High entirely. An agency running FedRAMP High must implement these from scratch to satisfy CJIS. These are concentrated in the NIST 800-53 Rev 5 privacy overlay, reflecting CJI's status as sensitive personal data.

### Implementation-Level Deltas

Controls where CJIS v6.0 imposes stricter requirements than the FedRAMP High baseline:

| NIST 800-53 Rev 5 | FedRAMP High | CJIS v6.0 Delta | Category |
|--------------------|:------------:|------------------|----------|
| PS-3 Personnel Screening | Background investigation | Fingerprint-based background check (state/national) required for all CJI access | Personnel |
| PS-6 Access Agreements | Signed rules of behavior | CJIS Security Addendum required before CJI access | Personnel |
| IA-2 Identification & Authentication | MFA required | AAL2 phishing-resistant MFA; Advanced Authentication for CJI access | Authentication |
| IA-5 Authenticator Management | Complexity/rotation per baseline | Minimum 8-char passwords, specific complexity rules, 90-day max lifetime | Authentication |
| MP-6 Media Sanitization | Sanitize before disposal/reuse | Prescriptive sanitization procedures per media type; physical destruction requirements for CJI media | Media Protection |
| SC-12 Cryptographic Key Management | FIPS-validated modules | Agency-managed encryption keys; FIPS 140-2/140-3 validated modules | Encryption |
| SC-13 Cryptographic Protection | FIPS-validated crypto | Minimum 128-bit symmetric / 2048-bit asymmetric key lengths for CJI | Encryption |
| SC-28 Protection of Info at Rest | Encryption at rest required | Agency-managed CMK required for CJI at rest; agency retains key revocation authority | Encryption |
| AU-6 Audit Record Review | Review/analysis per baseline | Weekly audit log review; 1-year minimum retention for CJI-related events | Audit |
| AC-2 Account Management | Account lifecycle per baseline | Quarterly access reviews for CJI-authorized users | Access Control |
| IR-6 Incident Reporting | Report to US-CERT | Additional reporting to CSO and FBI CJIS Division within required timeframes | Incident Response |
| PE-17 Alternate Work Site | Authorize alternate sites | Specific controls for remote CJI access locations | Physical/Environmental |
| AT-2 Awareness Training | Annual security training | CJIS Security Awareness Training within 6 months of CJI access, biennial refresher | Training |

### Control-Level Gaps

Controls in the CJIS v6.0 baseline that are not in FedRAMP High. Identified via OSCAL baseline comparison (CJIS v6.0: 302 controls, FedRAMP High: 410 controls, 287 shared, 15 CJIS-only). Documentation and OSCAL data for these controls are being added across subsequent releases.

| Count | Category | Controls |
|-------|----------|----------|
| 6 | Privacy (Retention, Quality, De-identification) | SI-12.1, SI-12.2, SI-12.3, SI-18, SI-18.4, SI-19 |
| 4 | PII Limitation (Audit, Physical, Access, Boundary) | AU-3.3, PE-8.3, AC-3.14, SC-7.24 |
| 3 | Training & Incident Response | AT-3.5, IR-2.3, IR-8.1 |
| 2 | Planning & Engineering | PL-9, SA-8.33 |

> **Note:** The implementation-level deltas table represents the initial scoping analysis. Additional controls may surface during full OSCAL baseline resolution.

## How an Auditor Uses This Output

A CJIS auditor reviewing a CSP's compliance posture starts with the FedRAMP High ATO package as a baseline, then uses this gap analysis to identify the specific controls requiring additional evidence or implementation. For each delta control, the analysis documents:

- **What FedRAMP High requires** — the baseline expectation already met
- **What CJIS v6.0 adds** — the additional or stricter requirement
- **Implementation guidance** — how to close the gap (policy, technical control, or process)
- **Evidence required** — what an auditor expects to see during a CJIS audit

This turns an ambiguous "is FedRAMP enough?" question into a concrete remediation checklist.

## FedRAMP 20x Alignment

The gap analysis data will be structured in OSCAL-compatible format, aligning with FedRAMP 20x compliance-as-code requirements. OSCAL profile comparison enables automated delta detection: import both the FedRAMP High profile and a CJIS v6.0 overlay, then programmatically identify where the CJIS profile adds parameters, constraints, or entirely new requirements. This machine-readable approach supports continuous compliance validation rather than point-in-time spreadsheet audits.

## CJIS v6.0 Context

CJIS Security Policy v6.0 was released in December 2024, completing the alignment with NIST 800-53 Rev 5. It replaces v5.9.x as the audit standard effective April 1, 2026. Key changes from v5.9.x include:

- Full adoption of NIST 800-53 Rev 5 control catalog (previously mapped to Rev 4)
- Updated Advanced Authentication requirements aligned with NIST SP 800-63-3
- Explicit FIPS 140-2/140-3 validation requirements for cryptographic modules
- Restructured policy sections mapping directly to 800-53 control families

For CSPs already operating under FedRAMP High, the v6.0 update is significant because it means CJIS and FedRAMP now share the same control catalog — making delta analysis cleaner and more precise than under v5.9.x.

## Project Structure

```
├── analysis/
│   └── gap-analysis.md             # Control-by-control delta analysis
├── data/
│   ├── fedramp-high-profile.json   # FedRAMP High baseline (OSCAL profile)
│   └── cjis-overlay.json           # CJIS v6.0 overlay (OSCAL profile)
├── scripts/
│   └── generate_gap_report.py      # Gap report generator (OSCAL → markdown)
├── output/
│   └── gap-report.md               # Generated gap report
├── README.md
└── LICENSE.txt
```

## License

MIT
