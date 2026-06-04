# CMS-Aligned Network Specification

**Draft Community Specification**

**Editor's Draft, June 1, 2026**

**This version:** `can-spec/0.1-draft`
**Latest published version:** *none yet*
**Editor:** Liz Lewis (b.well Connected Health)
**Feedback:** via CMS Health Technology Ecosystem working groups

---

## Abstract

This document specifies the technical and operational requirements a Health Information Network MUST meet to be recognized as a **CMS-Aligned Network (CAN)** under the CMS Health Technology Ecosystem (HTE).

The specification covers the three core obligations of a CMS-Aligned Network, the three required connectivity pathways, patient matching, auto-registration via a dual-artifact model, query handling, National Provider Directory (NPD) publication, audit logging, security validation, fees, and accountability.

This specification deliberately covers **network obligations only**. Trust pathways for apps, EHRs, providers, and payers are referenced where they intersect network behavior but are specified elsewhere.

---

## Status of This Document

This is an editor's draft assembled from working-group materials. It has no normative force on its own. The authoritative source for CMS-Aligned status is the **CMS Interoperability Framework** published by CMS at <https://www.cms.gov/health-technology-ecosystem/interoperability-framework>. Where this document and the Framework conflict, the Framework controls.

This draft is offered as a consolidated rendering of network-side requirements so that implementers can evaluate conformance against a single artifact. Working-group input is welcome on operational specifics (auto-registration profiles, audit standards, dispute resolution, presumptive-eligibility scope).

---

## 1. Conformance

The keywords **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) when, and only when, they appear in all capitals.

A network conforms to this specification when it satisfies every **MUST** in §§ 3–13 and is in good standing under § 14.

---

## 2. Terminology

**CMS-Aligned Network (CAN)** — A Health Information Network recognized by CMS as meeting the obligations defined in this specification.

**Home Network** — The single CAN through which a given participant (app, data holder, delegated tech solution) is onboarded and held to be in good standing.

**Data Holder** — A HIPAA covered entity (provider organization or payer) that holds patient records and exposes them via a network.

**Tech Solution** — A patient-facing application, third-party delegated software, or other ecosystem participant that originates queries.

**Federal Trust Signal** — A federally grounded credential or attestation that travels with an actor (e.g., ONC certification, CARIN Code of Conduct, DirectTrust accreditation, DiME seal, IAL2 verification, Medicare App Library listing, NPD listing, HIPAA covered-entity status, X.509 credential).

**Good Standing** — As defined in § 4.3: completed home-network onboarding, current on obligations, no active unresolved complaints, passed operational health checks, not suspended.

**NPD** — National Provider Directory. The authoritative public registry of ecosystem participants, endpoints, and inter-network connections.

**RLS** — Record Locator Service.

**Use Case** — One of: patient access, treatment, payment, operations, prior authorization, payer-to-payer.

**Dynamic Client Registration (RFC 7591)** — The IETF protocol by which an OAuth 2.0 client registers itself with an authorization server programmatically, presenting a signed software statement at the `/register` endpoint. Used by both CMS-signed and UDAP registration flows in this specification.

**Software Statement (CMS-signed)** — A short-lived JWT signed by CMS asserting that a patient-facing app is listed (and in what status) in the Medicare App Library, and binding the app to its verified `jwks_uri`. Presented as the `software_statement` parameter during RFC 7591 Dynamic Client Registration.

**Software Statement (UDAP)** — An X.509-anchored signed JWT per the UDAP B2B Implementation Guide, used by payers, providers, and networks acting as clients to register at a CAN data holder. Trust is validated against the trust community CA recognized by the CMS-Aligned framework.

---

## 3. Architecture Overview

The HTE architecture establishes a **federal floor** that is mandatory for participants who choose to be CMS-Aligned. Above the floor, networks compete and differentiate freely.

### 3.1 Non-Goals

A CMS-Aligned Network is **NOT** required to:

- sign a common agreement with other networks;
- share liability with other networks;
- adopt common pricing or governance.

### 3.2 What Networks MUST Do

Every CMS-Aligned Network MUST:

1. Meet the three core obligations in § 4.
2. Support all three connectivity pathways in § 5.
3. Use the CMS-approved patient matching logic (§ 6).
4. Honor auto-registration and presumptive eligibility for participants in good standing on another home CAN (§ 7).
5. Implement authorization per patient preferences (§ 8).
6. Respond to authorized queries without portal login (§ 9).
7. Publish to NPD (§ 10).
8. Produce audit logs accessible to patients (§ 11).
9. Maintain HITRUST or equivalent security validation (§ 12).
10. Comply with the fees floor (§ 13).
11. Remain accountable to CMS for ongoing compliance (§ 14).

---

## 4. The Three Core Obligations

### 4.1 Respond to queries for data holders on the network

A CAN **MUST** respond when an authorized query reaches the network and a data holder on the network holds matching data, across every Use Case the network's participants engage in. This includes, at minimum, patient access, treatment, and payment queries within applicable use cases.

A CAN **MUST NOT** decline to respond solely because the originating query came from a participant whose home network is different.

### 4.2 Vouch for participants in good standing

A CAN **MUST** maintain the operational status of each of its onboarded participants and **MUST** report good standing (or lack thereof) when queried by another CAN or by NPD.

A CAN **MUST NOT** attest to federal legal compliance on behalf of its participants. Operational status only.

A CAN **MUST** suspend a participant's good-standing report when the participant has been suspended or flagged by the network, or has failed an operational health check that the network publishes.

### 4.3 Respond to credentialed tech solutions from other home networks

When a tech solution presents valid Federal Trust Signals **and** is reported in good standing on its home CAN, the receiving CAN **MUST** respond to its queries.

The receiving CAN **MUST NOT** impose duplicative trust gating on top of the Federal Trust Signals and the home-network good-standing report. Operational coordination (abuse contacts, rate-limit coordination, ops contacts, support channels) **MAY** be required.

---

## 5. Connectivity Pathways

A CAN **MUST** support all three of the following pathways.

### 5.1 Pathway 1 — Intranetwork

The CAN serves queries against data holders that have contracted directly with the CAN as their home network.

**Assumptions:**
- The participant has a direct contract with the CAN.
- The CAN is in good standing as CMS-Aligned.

**Conformance:**
- The CAN **MUST** respond to authorized queries for any data holder on the network across applicable use cases.

### 5.2 Pathway 2 — RLS Network Search ($match)

Discovery uses `$match` against the published RLS endpoints of other CMS-Aligned Networks. Data retrieval may use either **federated FHIR** (each responder serves its own data directly) or **brokered FHIR** (a network broker aggregates and returns data on behalf of multiple endpoints). Both retrieval modes are conformant.

**Assumptions:**
- Every CAN exposes a standardized RLS endpoint at a known address listed in NPD.
- The endpoint accepts authenticated `$match` requests from other CMS-Aligned Networks.
- Common patient matching (§ 6) applies.

**Conformance:**
- A CAN **MUST** expose a standardized RLS endpoint at the address published in NPD.
- A CAN **MUST** accept authenticated `$match` requests from other CMS-Aligned Networks on this endpoint.
- A CAN **MUST** apply the CMS patient matching rule (§ 6) to all queries received via Pathway 2.
- Data retrieval **MAY** use federated FHIR or brokered FHIR; both are conformant.

> **Note.** The exact wire profile of the RLS endpoint, the federation transport, and the authentication scheme between CANs are intentionally left to the CMS Interoperability Framework and the working-group operational profiles. The wire profile for brokered FHIR retrieval is an open question — see Appendix A.

### 5.3 Pathway 3 — Targeted Queries Against NPD

The connector queries a specific endpoint by NPI (or equivalent identifier) listed in NPD, when there is evidence the patient has data at that endpoint (e.g., a payer that received a claim from a specific provider).

**Assumptions:**
- The connector has direct query rights under HIPAA right of access, treatment/payment/operations purpose of use, or other applicable legal authority.
- NPD listing is authoritative for endpoint discovery.

**Conformance:**
- A CAN **MUST** publish its participants' endpoints to NPD in a form that supports targeted queries by NPI or equivalent identifier.
- A CAN **SHOULD** rate-limit or otherwise constrain unbounded discovery patterns (e.g., geographic broadcast) to prevent spamming of endpoints. The specific constraint mechanism is an open question — see Appendix A.

---

## 6. Patient Matching

A CAN **MUST** implement the CMS-approved patient matching logic specified in the CMS Interoperability Framework. The current MVP standard is the **27-combination matching rule**.

A CAN **MUST** respond when a query received via any pathway in § 5 matches a patient record across any of the 27 specified combinations.

> The exact field list and combination matrix are specified in the CMS Interoperability Framework, not duplicated here.

---

## 7. Auto-Registration and Presumptive Eligibility

### 7.1 Auto-Registration

Registration at any CAN data holder follows [RFC 7591 Dynamic Client Registration](https://www.rfc-editor.org/rfc/rfc7591), with a signed software statement presented at the standard `/register` endpoint as the `software_statement` parameter. Two artifact types are recognized and **MUST** be accepted:

1. **CMS-signed software statement** — **REQUIRED** for patient-facing apps registered in the Medicare App Library. A short-lived JWT (e.g., 24-hour TTL) signed by CMS, asserting Library status (active / suspended / delisted) and binding to the app's verified `jwks_uri`. Replaces per-network re-vetting of patient-facing apps. See Josh Mandel, "Software Statements for the Medicare App Library," May 26, 2026.
2. **UDAP software statement** — **REQUIRED** for all other participant types (payers, providers, networks acting as clients). An X.509-anchored signed JWT per the [UDAP B2B Implementation Guide](https://www.udap.org/udap-ig-b2b-health-apps). Trust validated against the trust community CA recognized by the CMS-Aligned framework.

Both artifact types reduce to RFC 7591 plumbing at the receiving authorization server. The server **SHOULD** route signature validation by issuer: CMS published JWKS for the CMS-signed artifact; UDAP trust community CA chain for the UDAP artifact.

When a participant has been onboarded by one home CAN using the applicable artifact type, every other CAN **MUST** register that participant on a defined timeline without redundant onboarding.

The timeline is set by the CMS Interoperability Framework.

A CAN **MUST NOT** impose duplicative trust gating on top of the federally grounded credentials. Operational coordination (abuse contacts, rate-limit, security procedures, support channels) **MAY** be coordinated.

### 7.2 Presumptive Eligibility

A participant that has met its trust requirements and is in good standing on one home CAN **MUST** be allowed by other CANs to operate for a default **90-day** presumptive-eligibility period without redundant onboarding.

After 90 days, presumptive eligibility transitions to ongoing recognition unless there is specific cause to suspend.

Three conditions are required:

1. The participant meets the trust requirements applicable to its actor type.
2. The home CAN's onboarding establishes that the participant works in production.
3. The participant is in good standing on the home CAN.

A CAN **MAY** suspend presumptive eligibility for cause, including operational abuse, security incidents, or a good-standing downgrade on the home CAN. A CAN that suspends **MUST** report the suspension to NPD and to the home CAN.

---

## 8. Authorization

> **Placeholder.** Authorization requirements — including patient consent, delegated authorization, and app-level permission scopes — are under active development by the Patient Preferences workgroup. This section will be populated when that workgroup publishes its recommendations.

---

## 9. Query Handling

### 9.1 No Portal Login

A CAN, and every EHR or data holder it routes to, **MUST** respond to authorized queries from properly credentialed parties without requiring portal login as a precondition.

For patient-directed access, IAL2 identity verification through a CMS-approved CSP (e.g., CLEAR, ID.me) plus app authorization is sufficient.

### 9.2 Respond Completely

When a query is authorized, the response **MUST** include all data the responder holds for the patient, structured and unstructured, within the applicable Use Case.

The minimum data scope for structured data is **USCDI v3** (or the version current at the time of the query, as specified by the CMS Interoperability Framework). Unstructured artifacts (clinical notes, scanned PDFs, imaging reports, encounter documents, faxes) **MUST** be included where they exist.

> No use case becomes a dead end.

### 9.3 Use Case Coverage

A CAN **MUST** respond to queries from the actor categories that apply to the use cases its participants engage in:

- patients seeking their own records;
- providers requesting clinical and claims data for treatment;
- payers (where applicable) requesting clinical data supporting claims from the last 60 days;
- payers querying for quality measure reporting;
- prior authorization queries (subject to CMS-0057-F deadlines).

If a network's participants engage in a use case, the network **MUST** support queries for that use case.

### 9.4 Purpose of Use Propagation

Every data request **MUST** declare why the data is being accessed. A CAN **MUST** support the HL7 Purpose of Use code set and apply the correct disclosure rules to each category.

The following codes are **REQUIRED**, aligned to the approved use cases in § 9.3:

| Code | Display | Level | Use case |
|---|---|---|---|
| `PATRQT` | Patient request | Granular | Patient access |
| `TREAT` | Treatment | High level | Treatment |
| `HPAYMT` | Healthcare payment | High level | Payment (including claims) |
| `CLMATTCH` | Claim attachment | Granular | Claim attachment within payment |
| `HOPERAT` | Healthcare operations | High level | Health care operations |
| `HQUALIPM` | Healthcare quality improvement | Granular | Quality measure reporting within operations |

Purpose of use **MUST** travel with the request to downstream systems.

When the requesting party is trusted and the purpose of use is properly declared, a CAN and its participants **MUST NOT** impose additional authorization requirements on top.

### 9.5 Patient-Contributed Data

A CAN **MUST** accept patient-contributed data (patient-reported outcomes, home device readings, symptom history, lifestyle data, notes) from patient-facing apps when the patient chooses to share, and **MUST** pass that information through to the appropriate data holder for inclusion in patient records or care use.

Patient choice governs whether patient-contributed data flows. Nothing in this section overrides patient control.

---

## 10. National Provider Directory Publication

A CAN **MUST** publish to NPD:

- its onboarded participants (apps, providers, payers, delegated tech solutions);
- its participants' endpoints in a form that supports Pathway 3 (targeted query by identifier);
- its inter-network connections;
- usage metrics by participant and by use case;
- trust-anchor metadata required for UDAP validation, including the trust community CA URL and any intermediate CA certificates recognized by the CMS-Aligned framework, so that receiving networks can validate UDAP software statements without bilateral out-of-band coordination.

A CAN **MUST** ingest and publish updates routinely. The ingest/refresh cadence is specified by the CMS Interoperability Framework.

NPD **MUST** also be queryable by any CAN, auditor, or participant to confirm an actor's listing and credentials. Trust travels with the actor because it is anchored in NPD as a public, queryable record that any CAN can read without bilateral verification.

---

## 11. Audit Logging

A CAN **MUST** produce audit logs for queries on its network, including:

- who accessed the data;
- when;
- for what declared purpose of use;
- which organizations were involved.

Audit logs **MUST** be organization-level at minimum.

A CAN **MUST** facilitate patient-facing audit access so patients can see, through their app, who queried their data.

EHRs facilitating ecosystem queries are subject to the same audit obligations as the CAN routing through them.

---

## 12. Security

A CAN **MUST** maintain **HITRUST certification or equivalent** security validation, as approved by CMS.

Security certification does **NOT** replace compliance with HIPAA, the Privacy Act, or applicable state laws.

Business Associate Agreements (BAAs) **MAY** be required even where data is not directly brokered (for example, when a participant queries an RLS endpoint under Pathway 3). Networks and participants **MUST** confirm their BAA obligations under HIPAA.

---

## 13. Fees and Economics

### 13.1 Patient-Directed Access

A CAN **MUST NOT** structure fees in a way that gates a patient's federal right to access their own data.

The Fees exception at [45 CFR 171.302](https://www.ecfr.gov/current/title-45/part-171/section-171.302) and the ONC information blocking framework establish this floor. Cost recovery is permitted; platform fees structured to defeat patient access are not.

### 13.2 Above the Floor

A CAN **MAY** set its own commercial terms for:

- premium services beyond baseline (enhanced identity verification, expanded data scope beyond USCDI, premium SLAs, real-time delivery, write-back capability, advanced patient matching, analytics, population health products);
- prior-authorization service offerings under CMS-0057-F;
- voluntary commercial peering arrangements with other CANs;
- value-added integration services.

### 13.3 Inter-Network Settlement

For patient-directed access traffic, inter-network settlement is **NOT** appropriate.

For other traffic types (treatment, payment, operations, prior auth, payer-to-payer), CANs **MAY** negotiate commercial peering arrangements with settlement, transit fees, or other terms above the federal floor.

> **Open question.** Whether a CAN may charge a data holder per query for required HTE use cases, and whether a CAN may charge a payer per query against a provider on the CAN, is not resolved in source documents. See Appendix A.

---

## 14. Accountability

A CAN is accountable to CMS for meeting the obligations in §§ 3–13. Persistent failure is grounds for delisting from CMS-Aligned status on the same footing as failing any other Framework criterion.

A CAN **MUST** publish operational metrics (response rates, query volumes, response times by use case) so apps and data holders can comparison-shop and so CMS can monitor adoption and performance. Network performance metrics appear in CMS scorecards (Framework criterion #19).

Outages and partial responses happen; the obligation is to meet published response standards over time, not to be perfect.

---

## 15. July 4, 2026 Framework Requirements

By **July 4, 2026**, a CAN **MUST** satisfy all four requirements below, as specified in the CMS Interoperability Framework.

### 15.1 FHIR API Access

A CAN **MUST** provide or facilitate access to data using FHIR APIs that adhere to the [HL7 FHIR US Core Implementation Guide](https://hl7.org/fhir/us/core), including:

- a complete and valid FHIR Capability Statement;
- USCDI v3 (or later) data elements with terminology compliance — laboratories coded in LOINC, medications in RxNorm, conditions in SNOMED CT.

A CAN **SHOULD** leverage [FHIR Bulk Data Exchange](https://hl7.org/fhir/uv/bulkdata) to reduce stress on existing systems and enable the exchange of full data records.

### 15.2 Chart Notes and Clinical Documents

A CAN **MUST** return chart notes and clinical documents — including radiology reports, scanned or faxed labs, and external specialist notes — in human-readable formats (PDF, TIFF, JPG) as FHIR attachments, as specified in USCDI v3.

### 15.3 Appointment and Encounter Notifications

A CAN **MUST** provide appointment and encounter notifications for outpatient, telehealth, emergency department, and inpatient encounters using FHIR Subscriptions, where such notifications are permitted by existing law.

> **Placeholder.** The specific notification profile and delivery requirements for this criterion are TBD, pending CAN role alignment by the working group.

### 15.4 Record Locator Service

A CAN **MUST** implement record locator functionality by collaborating with CMS to determine efficient and timely models that:

- reduce query load on the networks;
- aid understanding of data completeness.

Requests to the record locator service **MUST** be initiable by patients, providers, and value-based care organizations.

---

## Appendix A. Open Questions

These are gaps identified in source materials that this draft does not resolve. They are flagged here so working-group attention can converge.

| # | Open Question | Source |
|---|---|---|
| A1 | Wire profile of the standardized RLS / federation endpoint (transport, authentication, payload schema for `$match` requests and responses). | Framework defers; Connectivity Pathways doc notes this is the baseline interface but the operational profile is not fixed. |
| A1b | Wire profile for brokered FHIR retrieval under Pathway 2 — how a network broker aggregates responses from multiple RLS endpoints and returns them to the requesting CAN (payload shape, error handling, partial-response semantics). | Introduced by the shift from federated-only to dual retrieval modes in Pathway 2; not yet specified. |
| A2 | Mechanism for preventing endpoint-spamming under Pathway 3 (geo-search constraints, rate limits, query-shape rules). | Connectivity Pathways doc explicitly raises this as an unresolved question. |
| A3 | Whether networks may charge data holders per query for required HTE use cases, and the same for payer-to-provider queries. | Workgroup Alternative Proposal § 2.3 — raised but not resolved by CMS in source materials. |
| A4 | Definition and scope of the "on-ramp" intermediary role: separate ecosystem role or contracted vendor of the participant? | Workgroup Alternative Proposal § 2.2 — raised but not resolved. |
| A5 | Operational profile for auto-registration timeline and the exact handoff between home-network onboarding and presumptive eligibility at receiving networks. | HTE Reference doc Part II — "defined timeline" referenced but not specified. |
| A6 | Whether a single "Rules of the Road" document signed by all CANs is the right vehicle for cross-network operational standards, or whether criteria-based participation is sufficient. | Workgroup Alternative Proposal § 3.1 vs. HTE Reference doc Part I — disagreement; CMS chose criteria-based. |
| A7 | NPD ingest/refresh cadence, schema, and authoritative trust-registry behavior. | HTE Reference doc Part II references publication but the operational profile is open. |

> **Editor's note.** I'm flagging these honestly. A number of source documents speak in general terms ("defined timeline", "operational profile to be specified") without the technical detail an implementer needs. This draft does not fabricate that detail.

---

## Appendix B. Source Documents Used to Build This Draft

This draft was synthesized from three internal working documents and one public CMS resource. No external sources were invented.

- **HTE Network Framing Combined Reference (V2 draft).** Part I (the case for the HTE architecture), Part II (Rules of the Road), Part III (trust framework diagram). Internal working document.
- **Connectivity Pathways for Discovery.** Internal working document defining the original four pathways; this spec consolidates to three required pathways.
- **CMS Networks Workgroup — Meeting Summary and Alternative Ecosystem Proposal.** Internal working document. The alternative was not adopted; cited here for gap identification only.
- **CMS Interoperability Framework.** <https://www.cms.gov/health-technology-ecosystem/interoperability-framework>

---

## Appendix C. Normative References

References below appear in source materials. None are invented.

- [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) — Key words for use in RFCs to Indicate Requirement Levels.
- [RFC 7591](https://www.rfc-editor.org/rfc/rfc7591) — OAuth 2.0 Dynamic Client Registration Protocol.
- [UDAP B2B Implementation Guide](https://www.udap.org/udap-ig-b2b-health-apps) — Unified Data Access Profiles for B2B Health App Authorization; defines the X.509-anchored software statement used by payers, providers, and networks.
- [ONC 21st Century Cures Act Final Rule](https://www.healthit.gov/curesrule).
- [USCDI v3](https://www.healthit.gov/isa/united-states-core-data-interoperability-uscdi).
- [HL7 FHIR US Core](https://hl7.org/fhir/us/core).
- [CMS-0057-F — Advancing Interoperability and Improving Prior Authorization Final Rule](https://www.cms.gov/newsroom/fact-sheets/cms-advancing-interoperability-and-improving-prior-authorization-processes-final-rule-cms-0057-f).
- [ONC Information Blocking](https://www.healthit.gov/topic/information-blocking).
- [45 CFR 171.302 — Fees Exception](https://www.ecfr.gov/current/title-45/part-171/section-171.302).

---

*End of draft.*
