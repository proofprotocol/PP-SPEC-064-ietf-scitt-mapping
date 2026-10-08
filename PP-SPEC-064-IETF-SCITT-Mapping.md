# PP-SPEC-064: Proof of Efficacy Mapping to IETF SCITT

| Field | Value |
|---|---|
| Status | DRAFT v0.1 |
| Author | Craig Ellrod, Nebulonium, Inc. (dba HACKERverse®) |
| Date | October 8, 2026 |
| License | CC BY 4.0 |
| Maps to | IETF SCITT: RFC 9943 (SCITT Architecture), RFC 9942 (COSE Receipts), and draft-ietf-scitt-scrapi-11 (informative) |
| Series | Proof Protocol Framework Mapping Specifications |

---

## 1. Purpose

This specification defines how Proof Protocol artifacts, including proof records, ProofBundle™ envelopes, ProofStamp™ authorization tokens, and ProofRegister™ records, can be expressed as SCITT Signed Statements, registered with a SCITT Transparency Service, and carried as Transparent Statements with COSE Receipts.

IETF SCITT (Supply Chain Integrity, Transparency, and Trust) remains authoritative for its own terminology, data structures, identifiers, and architecture. This document defines a **Proof Protocol mapping** and does not supersede or modify SCITT.

## 2. Scope

SCITT supplies a standard model for signing statements about artifacts, registering them with an append-only Transparency Service, and returning Receipts that prove registration. Proof Protocol supplies the independent evidence model that determines what happened during a test and whether a selected control performed as claimed.

The SCITT documents mapped by this version are:

- **RFC 9943**, *An Architecture for Trustworthy and Transparent Digital Supply Chains*, Proposed Standard, June 2026, published from draft-ietf-scitt-architecture (final revision -22);
- **RFC 9942**, *CBOR Object Signing and Encryption (COSE) Receipts*, Proposed Standard, June 2026; and
- **draft-ietf-scitt-scrapi-11**, *Supply Chain Integrity, Transparency, and Trust (SCITT) Reference APIs*, Internet-Draft dated 26 June 2026, in the RFC Editor queue and not yet published as an RFC as of the date of this specification. SCRAPI references in this mapping are informative and SHOULD be re-checked when an RFC number is assigned.

RFC 9943 and draft-ietf-scitt-scrapi are products of the IETF SCITT Working Group. RFC 9942 is a product of the IETF COSE Working Group (published from draft-ietf-cose-merkle-tree-proofs).

Independent witnessing, proof validity, evidence capture, packaging, anchoring, and efficacy semantics are defined elsewhere in the Proof Protocol specification family.

## 3. Core Question

Proof of efficacy asks:

> **Was there a control, and did it work?**

SCITT can make the existence, issuer, and registration history of a Proof Protocol artifact transparent. It does not determine whether a control worked. This mapping defines how a proof record, ProofBundle™, ProofStamp™, or ProofRegister™ entry can be expressed in, carried by, or registered with SCITT without changing what constitutes proof.

A Proof Protocol result does **not** by itself establish legal, audit, regulatory, or standards conformance unless the applicable authority separately determines that the evidence is sufficient.

## 4. Metric Definitions

Against a defined corpus or test condition, each applicable empirical case is recorded as **blocked**, **detected**, **missed**, or **INVALID** where those verdicts are meaningful.

Relevant measurements can include containment rate, detection rate, miss rate, false-positive rate, robustness under bypass or failure conditions, version-level results, and **INVALID** when required evidence is incomplete or broken.

Not every upstream construct is empirical. Envelope, signature, receipt, credential, manifest, message, and registration constructs MUST NOT be converted into efficacy scores merely because they can carry or reference a proof record.

## 5. Evidence Produced

Mapped tests can produce:

- **Proof records** binding external context, test case, control, system/version, verdict, timestamp, and evidence references;
- **ProofStamp™** trusted timestamps bound to evidence/verdict objects;
- **ProofBundle™** packages containing proof records, metrics, corpus manifests, and environment/context;
- **ProofRegister™** records for issued proof artifacts; and
- corpus and environment manifests identifying the tested conditions.

## 6. Mapping to IETF SCITT

Exact upstream construct names, type URIs, and identifiers SHOULD be taken from the authoritative version being mapped and recorded in the ProofBundle™ where licensing permits. Proof Protocol artifact and field names are those defined in PP-SPEC-001 and PP-SPEC-003. This specification does not redefine upstream constructs.

| SCITT construct | Proof Protocol artifact/treatment | Notes |
|---|---|---|
| Artifact | The tested product, agent, or system version identified in the proof record. A ProofBundle™ is a separate subject, identified by digest. | A SCITT Artifact is any item moving along a supply chain. The tested system and the proof artifact about it MUST NOT be conflated. |
| Statement (payload with a declared media type) | A proof record, ProofBundle™ `packet.json`, `proofregister.json`, or `proofstamp.json`. | The payload `content_type` SHOULD identify the Proof Protocol object type and specification version (`proof_protocol.spec_version`). |
| Signed Statement (COSE_Sign1, CWT Claims `iss` and `sub`) | The Proof Protocol artifact signed by its producer. `sub` SHOULD carry a stable identifier such as the Proof Record ID (`PR-YYYY-NNNNN`) or `verifier.root_hash`. | RFC 9943 permits detached or hashed payloads, so the ProofBundle™ MAY remain in its own storage and be bound by digest. |
| Issuer | The party that signs the Statement: the ProofBundle™ assembler, the witness, or HACKERverse® for `proofstamp.json`. | Issuer identity is not witness independence. Independence is established under PP-SPEC-001 Section 7 and the PP-SPEC-005 eligibility gates. |
| Registration Policy | A Transparency Service policy MAY require `packet.json` schema validity, presence of `nist-pulse.json` and `witness.json`, and a declared `verifier.verdict`. | Acceptance under a Registration Policy is an admission decision, not a Proof of Efficacy determination. |
| Transparency Service and Verifiable Data Structure | An external append-only registration point that MAY complement ProofRegister™. | ProofRegister™ remains the canonical Proof Protocol ledger (PP-SPEC-003 Section 8). SCITT registration is additional, not a substitute. |
| Registration (SCRAPI Register Signed Statement, `POST /entries`) | Submission of a signed Proof Protocol artifact after ProofBundle™ assembly. | Registration occurs after execution and MUST NOT be presented as the pre-execution commitment recorded in `nist-pulse.json`. |
| Receipt (COSE Receipt, RFC 9942; `vds`, `vdp`, inclusion proof) | Supplemental anchoring evidence recorded alongside `proofregister.json`, for example through the PP-SPEC-001 `extensions` mechanism. | A Receipt proves inclusion in a Verifiable Data Structure. It does not prove that the tested control worked. |
| Transparent Statement (Signed Statement with `receipts` header, label 394) | Portable form of a registered Proof Protocol artifact. | Verifying a Transparent Statement does not replace the PP-SPEC-003 Section 6 verification procedure. |
| Consistency proof and Append-only Log | Supports audit of the registration history of issued proof artifacts, including superseded or revoked ProofStamp™ tokens. | Useful for detecting equivocation in registration history. |
| Relying Party and Auditor | Buyer, auditor, regulator, insurer, or requesting agent consuming the proof artifact. | A Relying Party evaluating an efficacy claim MUST also apply Proof Protocol verification. |
| Transparency Service keys (SCRAPI `/.well-known/scitt-keys`) | Key identifiers used to verify Receipts SHOULD be recorded with the ProofBundle™ metadata. | Informative pending SCRAPI publication as an RFC. |

## 7. Interoperability Rules

1. The authoritative upstream source, edition/version, and applicable identifier SHOULD be recorded.
2. Upstream identifiers and terminology MUST NOT be silently redefined.
3. Proof Protocol verdicts are Proof Protocol results; they are not upstream certifications, audit opinions, legal determinations, or endorsements.
4. A control's presence, configuration, activation, detection, and efficacy are distinct facts.
5. A policy decision, alert, log entry, or control trigger does not by itself establish efficacy when the claim concerns a downstream protected outcome.
6. Target, application, tool, SIEM, vendor, service, resource, or equivalent evidence SHOULD complete the evidence round trip when required to establish the result.
7. Missing required evidence MUST yield **INVALID**, not PASS.
8. Material changes to the tested system, policy, control, environment, mapping, or corpus SHOULD trigger retesting where they can affect the result.
9. Format, envelope, transport, and registration properties MUST remain distinguishable from empirically tested efficacy assertions.
10. Expressing a Proof Protocol artifact in SCITT MUST NOT drop the evidence references, verdict, version context, or **INVALID** semantics of the source artifact.
11. External-format validity under SCITT, such as a valid signature, receipt, credential, manifest, or message, is not by itself Proof of Efficacy.
12. The native Proof Protocol artifact remains authoritative. Any expression of it in SCITT SHOULD reference it by digest (for example `verifier.root_hash` or the SHA-256 digest of `packet.json`) and by Proof Record ID where one exists.
13. SCITT registration time, Receipt time, or log position MUST NOT be substituted for the pre-execution commitment anchored in `nist-pulse.json`.

## 8. Framework-Agnostic Architecture

> **Threat, control, audit, regulatory, and interoperability frameworks are pluggable inputs to Proof Protocol. Proof Protocol is framework-agnostic.**

External frameworks can identify **what to test or examine**, or how a result is carried. Proof Protocol independently establishes **whether the control worked and what evidence proves that result**.

No external framework is required for Proof Protocol to operate. An implementation MAY use IETF SCITT, another recognized framework or standard, a proprietary threat model, several frameworks simultaneously, or no external framework when the test condition is otherwise sufficiently defined.

Adding, replacing, muting, or removing a framework mapping does not alter the Proof Protocol architecture, evidence model, Proof of Efficacy determination, ProofBundle™, ProofStamp™, ProofRegister™, or independent corroboration requirements.

A framework mapping therefore establishes **interoperability**, not architectural dependency.

## 9. Relationship to Proof Protocol

This mapping is part of the Proof Protocol specification family maintained by Nebulonium, Inc.

The relationship is intentionally asymmetric:

> **IETF SCITT supplies a standard envelope, registration, and receipt model for signed statements about artifacts. Proof Protocol supplies the evidence model, and the proof artifacts, for determining whether a selected control performed as claimed.**

No affiliation, endorsement, certification, or sponsorship by the IETF, the IETF Trust, or the IETF SCITT Working Group is implied.

## 10. Source Framework, Attribution, and License

IETF SCITT is external work of the Internet Engineering Task Force, developed in the SCITT Working Group; RFC 9942 was developed in the COSE Working Group. RFC 9943 and RFC 9942 are IETF Stream documents on the Internet Standards Track. draft-ietf-scitt-scrapi-11 is an Internet-Draft and is a work in progress until published as an RFC.

IETF documents are subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents (https://trustee.ietf.org/license-info). The RFC 9943 copyright notice states that the document is "subject to BCP 78 and the IETF Trust's Legal Provisions Relating to IETF Documents" in effect on the date of publication, and that "Code Components extracted from this document must include Revised BSD License text" as described in Section 4.e of the Trust Legal Provisions (https://www.rfc-editor.org/rfc/rfc9943.html). Under Section 3.c.iii of the Trust Legal Provisions, unmodified excerpts of IETF Documents may be reproduced provided "each such portion is clearly attributed to IETF and identifies the RFC or other IETF Document" (https://trustee.ietf.org/assets/trust-legal-provisions/). The Trust Legal Provisions do not license modification of IETF Documents outside the IETF Standards Process.

This Proof Protocol mapping is independently authored and licensed under **CC BY 4.0**. It references SCITT construct names, COSE header parameter names and labels, and SCRAPI endpoint paths for interoperability and does not relicense IETF material.

Where upstream material is copyrighted, licensed with conditions, or access-controlled, contributors MUST NOT copy upstream normative text into this repository merely to make the mapping self-contained. Reference identifiers and independently authored descriptions should be used.

### Required attribution

The notice quoted below is the copyright notice carried by RFC 9943, verbatim:

> Copyright (c) 2026 IETF Trust and the persons identified as the document authors. All rights reserved.

Source: RFC 9943, Copyright Notice, https://www.rfc-editor.org/rfc/rfc9943.html. RFC 9942 and draft-ietf-scitt-scrapi carry their own IETF Trust notices, which SHOULD be checked at https://www.rfc-editor.org/info/rfc9942 and https://datatracker.ietf.org/doc/draft-ietf-scitt-scrapi/ before any excerpt is reproduced.

Attribution line used by this repository:

> This specification references IETF RFC 9943 (An Architecture for Trustworthy and Transparent Digital Supply Chains), IETF RFC 9942 (CBOR Object Signing and Encryption (COSE) Receipts), and the Internet-Draft draft-ietf-scitt-scrapi-11. Copyright (c) 2026 IETF Trust and the persons identified as the document authors. All rights reserved.

**What this specification reproduces:** construct names (for example Signed Statement, Transparent Statement, Receipt, Registration Policy), COSE header parameter names and labels, media type and endpoint identifiers, and short factual statements of document status. It does **not** reproduce RFC or Internet-Draft text, CDDL, or Code Components. No excerpt requiring the Section 3.c.iii attribution and no Code Component requiring the Revised BSD License text is included. Any future excerpt MUST be unmodified, clearly attributed to IETF, and identify the RFC or Internet-Draft.

## 11. Versioning

This mapping is versioned independently of IETF SCITT. Publication of SCRAPI as an RFC, any update to RFC 9943 or RFC 9942, or errata affecting mapped constructs SHOULD trigger a mapping review and, where necessary, a new version identifying the RFC numbers and draft revisions mapped.

---

*Proof Protocol · proofprotocol.io · CC BY 4.0*
