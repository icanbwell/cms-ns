# CMS-Aligned Network Specification

**Draft Community Specification**

**Editor's Draft, May 27, 2026**

**This version:** `can-spec/0.1-draft`
**Latest published version:** *none yet*
**Editor:** Liz Lewis (b.well Connected Health)
**Feedback:** via CMS Health Technology Ecosystem working groups

---

## Abstract

This document specifies the technical and operational requirements a Health Information Network MUST meet to be recognized as a **CMS-Aligned Network (CAN)** under the CMS Health Technology Ecosystem (HTE).

The specification covers the three core obligations of a CMS-Aligned Network, the four permitted connectivity pathways, patient matching, query handling, National Provider Directory (NPD) publication, audit logging, security validation, fees, and accountability.

This specification deliberately covers **network obligations only**. Trust pathways for apps, EHRs, providers, and payers are referenced where they intersect network behavior but are specified elsewhere.

---

## Status of This Document

This is an editor's draft assembled from working-group materials. It has no normative force on its own. The authoritative source for CMS-Aligned status is the **CMS Interoperability Framework** published by CMS at <https://www.cms.gov/health-technology-ecosystem/interoperability-framework>. Where this document and the Framework conflict, the Framework controls.

This draft is offered as a consolidated, machine-readable rendering of network-side requirements so that implementers can evaluate conformance against a single artifact. Working-group input is genuinely welcome on operational specifics (auto-registration profiles, audit standards, dispute resolution, presumptive-eligibility scope).

---

## 1. Conformance

The keywords **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) when, and only when, they appear in all capitals.

A network conforms to this specification when it satisfies every **MUST** in §§ 3–12 and is in good standing under § 13.

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

---

## 3. Architecture Overview

The HTE architecture establishes a **federal floor** that is mandatory for participants who choose to be CMS-Aligned. Above the floor, networks compete and differentiate freely.

### 3.1 Non-Goals

A CMS-Aligned Network is **NOT** required to:

- sign a common agreement with other networks;
- peer commercially with every other CMS-Aligned Network;
- adopt a universal routing architecture;
- share liability with other networks;
- adopt common pricing or governance.

### 3.2 What Networks MUST Do

Every CMS-Aligned Network MUST:

1. Meet the three core obligations in § 4.
2. Support at least one of the four connectivity pathways in § 5 for each query type it handles.
3. Use the CMS-approved patient matching logic (§ 6).
4. Honor auto-registration and presumptive eligibility for participants in good standing on another home CAN (§ 7).
5. Respond to authorized queries without portal login (§ 8).
6. Publish to NPD (§ 9).
7. Produce audit logs accessible to patients (§ 10).
8. Maintain HITRUST or equivalent security validation (§ 11).
9. Comply with the fees floor (§ 12).
10. Remain accountable to CMS for ongoing compliance (§ 13).

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

### 4.4 Emergent Coverage

Ecosystem-wide coverage emerges from every CAN meeting § 4.1–4.3. A CAN **is not** responsible for guaranteeing ecosystem-wide coverage — only for its own three obligations.

A CAN **MAY** fulfill its obligations using any combination of the pathways in § 5.

---

## 5. Connectivity Pathways

A CAN **MUST** support at least one of the following four pathways for each query type it handles. A CAN **MAY** support more than one.

### 5.1 Pathway 1 — Intranetwork

The CAN serves queries against data holders that have contracted directly with the CAN as their home network.

**Assumptions:**
- The participant has a direct contract with the CAN.
- The CAN is in good standing as CMS-Aligned.

**Conformance:**
- The CAN **MUST** respond to authorized queries for any data holder on the network across applicable use cases.

### 5.2 Pathway 2 — Peering with a Partner Network

Bilateral, network-to-network exchange that goes deeper than the baseline federation interface in Pathway 3.

**Assumptions:**
- Voluntary, commercial, and bilateral.
- Not required by CMS-Aligned status.
- Both networks remain individually responsible for their conduct. No shared liability pool.
- The peering contract governs settlement, transit, and SLAs.

**Conformance:**
- Pathway 2 **MAY** be used by a CAN to satisfy its obligations in § 4 but **MUST NOT** be the *only* available pathway, because peering is voluntary and a partner network can withdraw.

### 5.3 Pathway 3 — Federated FHIR Queries via CMS-Aligned RLS Endpoints

The standardized broadcast/discovery path. Patient discovery and provider discovery via the federation interface.

**Assumptions:**
- Every CAN exposes a standardized RLS endpoint at a known address listed in NPD.
- The endpoint accepts authenticated requests from other CMS-Aligned Networks.
- Common patient matching (§ 6) applies.

**Conformance:**
- A CAN **MUST** expose a standardized RLS endpoint at the address published in NPD.
- A CAN **MUST** accept authenticated requests from other CMS-Aligned Networks on this endpoint.
- A CAN **MUST** apply the CMS patient matching rule (§ 6) to all queries received via Pathway 3.

> **Note.** The exact wire profile of the RLS endpoint, the federation transport, and the authentication scheme between CANs are intentionally left to the CMS Interoperability Framework and the working-group operational profiles. This specification fixes the *obligation* to expose and accept; it does not (yet) fix the transport profile. **Open question — see Appendix A.**

### 5.4 Pathway 4 — Targeted Queries Against NPD

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

When a tech solution holds the trust signals appropriate to its actor type (see § 2 "Federal Trust Signal"), holds an X.509 credential, and has been onboarded by one home CAN, every other CAN **MUST** register that tech solution on a defined timeline without redundant onboarding.

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

## 8. Query Handling

### 8.1 No Portal Login

A CAN, and every EHR or data holder it routes to, **MUST** respond to authorized queries from properly credentialed parties without requiring portal login as a precondition.

For patient-directed access, IAL2 identity verification through a CMS-approved CSP (e.g., CLEAR, ID.me) plus app authorization is sufficient.

### 8.2 Respond Completely

When a query is authorized, the response **MUST** include all data the responder holds for the patient, structured and unstructured, within the applicable Use Case.

The minimum data scope for structured data is **USCDI v3** (or the version current at the time of the query, as specified by the CMS Interoperability Framework). Unstructured artifacts (clinical notes, scanned PDFs, imaging reports, encounter documents, faxes) **MUST** be included where they exist.

> No use case becomes a dead end.

### 8.3 Use Case Coverage

A CAN **MUST** respond to queries from the actor categories that apply to the use cases its participants engage in:

- patients seeking their own records;
- providers requesting clinical and claims data for treatment;
- payers (where applicable) requesting clinical data supporting claims from the last 60 days;
- payer-to-payer flows for member transitions;
- prior authorization queries (subject to CMS-0057-F deadlines).

If a network's participants engage in a use case, the network **MUST** support queries for that use case.

### 8.4 Purpose of Use Propagation

Every data request **MUST** declare why the data is being accessed. A CAN **MUST** support the HL7 Purpose of Use code set and apply the correct disclosure rules to each category.

Purpose of use **MUST** travel with the request to downstream systems.

When the requesting party is trusted and the purpose of use is properly declared, a CAN and its participants **MUST NOT** impose additional authorization requirements on top.

### 8.5 Patient-Contributed Data

A CAN **MUST** accept patient-contributed data (patient-reported outcomes, home device readings, symptom history, lifestyle data, notes) from patient-facing apps when the patient chooses to share, and **MUST** pass that information through to the appropriate data holder for inclusion in patient records or care use.

Patient choice governs whether patient-contributed data flows. Nothing in this section overrides patient control.

---

## 9. National Provider Directory Publication

A CAN **MUST** publish to NPD:

- its onboarded participants (apps, providers, payers, delegated tech solutions);
- its participants' endpoints in a form that supports Pathway 4 (targeted query by identifier);
- its inter-network peering connections;
- usage metrics by participant and by use case.

A CAN **MUST** ingest and publish updates routinely. The ingest/refresh cadence is specified by the CMS Interoperability Framework.

NPD **MUST** also be queryable by any CAN, auditor, or participant to confirm an actor's listing and credentials. Trust travels with the actor because it is anchored in NPD as a public, queryable record that any CAN can read without bilateral verification.

---

## 10. Audit Logging

A CAN **MUST** produce audit logs for queries on its network, including:

- who accessed the data;
- when;
- for what declared purpose of use;
- which organizations were involved.

Audit logs **MUST** be organization-level at minimum.

A CAN **MUST** facilitate patient-facing audit access so patients can see, through their app, who queried their data.

EHRs facilitating ecosystem queries are subject to the same audit obligations as the CAN routing through them.

---

## 11. Security

A CAN **MUST** maintain **HITRUST certification or equivalent** security validation, as approved by CMS.

Security certification does **NOT** replace compliance with HIPAA, the Privacy Act, or applicable state laws.

Business Associate Agreements (BAAs) **MAY** be required even where data is not directly brokered (for example, when a participant queries an RLS endpoint under Pathway 4). Networks and participants **MUST** confirm their BAA obligations under HIPAA.

---

## 12. Fees and Economics

### 12.1 Patient-Directed Access

A CAN **MUST NOT** structure fees in a way that gates a patient's federal right to access their own data.

The Fees exception at [45 CFR 171.302](https://www.ecfr.gov/current/title-45/part-171/section-171.302) and the ONC information blocking framework establish this floor. Cost recovery is permitted; platform fees structured to defeat patient access are not.

### 12.2 Above the Floor

A CAN **MAY** set its own commercial terms for:

- premium services beyond baseline (enhanced identity verification, expanded data scope beyond USCDI, premium SLAs, real-time delivery, write-back capability, advanced patient matching, analytics, population health products);
- prior-authorization service offerings under CMS-0057-F;
- peering arrangements with other CANs (Pathway 2);
- value-added integration services.

### 12.3 Inter-Network Settlement

For patient-directed access traffic, inter-network settlement is **NOT** appropriate.

For other traffic types (treatment, payment, operations, prior auth, payer-to-payer), CANs **MAY** negotiate commercial peering arrangements with settlement, transit fees, or other terms above the federal floor.

> **Open question.** Whether a CAN may charge a data holder per query for required HTE use cases, and whether a CAN may charge a payer per query against a provider on the CAN, is not resolved in source documents. See Appendix A.

---

## 13. Accountability

A CAN is accountable to CMS for meeting the obligations in §§ 3–12. Persistent failure is grounds for delisting from CMS-Aligned status on the same footing as failing any other Framework criterion.

A CAN **MUST** publish operational metrics (response rates, query volumes, response times by use case) so apps and data holders can comparison-shop and so CMS can monitor adoption and performance. Network performance metrics appear in CMS scorecards (Framework criterion #19).

Outages and partial responses happen; the obligation is to meet published response standards over time, not to be perfect.

---

## 14. Framework Criterion #13 — July 4, 2026

Framework criterion #13 requires CMS-Aligned Networks to provide or facilitate FHIR or FHIR-mediated API access adhering to US Core (USCDI v3+) by **July 4, 2026**, with related criteria for chart notes, attachments, and notifications on the same date.

A CAN **MUST** be capable, by that date, of:

- responding to patient-access queries from patient-facing apps;
- responding to payer queries for clinical data tied to claims from the prior 60 days;
- supporting payer-to-payer flows for member transitions;
- providing the foundational infrastructure for prior authorization under CMS-0057-F (compliance deadline January 2027).

The substantive priorities for the July milestone are **patient access** and **payer use cases**.

---

## Appendix A. Open Questions

These are gaps identified in source materials that this draft does not resolve. They are flagged here so working-group attention can converge.

| # | Open Question | Source |
|---|---|---|
| A1 | Wire profile of the standardized RLS / federation endpoint (transport, authentication, payload schema). | Framework defers; Connectivity Pathways doc notes this is the baseline interface but the operational profile is not fixed. |
| A2 | Mechanism for preventing endpoint-spamming under Pathway 4 (geo-search constraints, rate limits, query-shape rules). | Connectivity Pathways doc explicitly raises this as an unresolved question. |
| A3 | Whether networks may charge data holders per query for required HTE use cases, and the same for payer-to-provider queries. | Workgroup Alternative Proposal § 2.3 — raised but not resolved by CMS in source materials. |
| A4 | Definition and scope of the "on-ramp" intermediary role: separate ecosystem role or contracted vendor of the participant? | Workgroup Alternative Proposal § 2.2 — raised but not resolved. |
| A5 | Operational profile for auto-registration timeline and the exact handoff between home-network onboarding and presumptive eligibility at receiving networks. | HTE Reference doc Part II — "defined timeline" referenced but not specified. |
| A6 | Whether a single "Rules of the Road" document signed by all CANs is the right vehicle for cross-network operational standards, or whether criteria-based participation is sufficient. | Workgroup Alternative Proposal § 3.1 vs. HTE Reference doc Part I — these documents disagree, and CMS has chosen criteria-based participation; the gap is whether that choice produces enough operational uniformity. |
| A7 | NPD ingest/refresh cadence, schema, and authoritative trust-registry behavior. | HTE Reference doc Part II references publication but the operational profile is open. |

**I'm flagging these honestly.** A number of source documents speak in general terms ("defined timeline", "operational profile to be specified") without the technical detail an implementer needs. This draft does not fabricate that detail.

---

## Appendix B. Source Documents Used to Build This Draft

This draft was synthesized from three internal working documents and one public CMS resource. No external sources were invented.

- **HTE Network Framing Combined Reference (V2 draft).** Part I (the case for the HTE architecture), Part II (Rules of the Road), Part III (trust framework diagram). Internal working document.
- **Connectivity Pathways for Discovery.** Internal working document defining the four pathways.
- **CMS Networks Workgroup — Meeting Summary and Alternative Ecosystem Proposal.** Internal working document. The alternative was not adopted; cited here for gap identification only.
- **CMS Interoperability Framework.** <https://www.cms.gov/health-technology-ecosystem/interoperability-framework>

---

## Appendix C. Normative References

References below appear in source materials. None are invented.

- [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) — Key words for use in RFCs to Indicate Requirement Levels.
- [ONC 21st Century Cures Act Final Rule](https://www.healthit.gov/curesrule).
- [USCDI v3](https://www.healthit.gov/isa/united-states-core-data-interoperability-uscdi).
- [HL7 FHIR US Core](https://hl7.org/fhir/us/core).
- [CMS-0057-F — Advancing Interoperability and Improving Prior Authorization Final Rule](https://www.cms.gov/newsroom/fact-sheets/cms-advancing-interoperability-and-improving-prior-authorization-processes-final-rule-cms-0057-f).
- [ONC Information Blocking](https://www.healthit.gov/topic/information-blocking).
- [45 CFR 171.302 — Fees Exception](https://www.ecfr.gov/current/title-45/part-171/section-171.302).

---

*End of draft.*
