# PP-SPEC-064: Proof of Efficacy Mapping to IETF SCITT

**Status:** DRAFT v0.1
**Author:** Craig Ellrod / Nebulonium, Inc. / HACKERverse®
**License:** CC BY 4.0

**Normative specification:** [`PP-SPEC-064-IETF-SCITT-Mapping.md`](./PP-SPEC-064-IETF-SCITT-Mapping.md)

## Purpose

This repository defines a Proof Protocol mapping between IETF SCITT: RFC 9943 (SCITT Architecture), RFC 9942 (COSE Receipts), and draft-ietf-scitt-scrapi-11 (informative) and Proof Protocol evidence, efficacy, and proof semantics.

IETF SCITT is treated as an interoperability standard that can express, carry, or register Proof Protocol artifacts. Proof Protocol remains framework-agnostic and independently establishes whether a selected control performed as claimed.

## Repository contents

- `PP-SPEC-064-IETF-SCITT-Mapping.md`: normative mapping specification
- `README.md`: repository overview
- `LICENSE`: license for original Proof Protocol material
- `CONTRIBUTING.md`: contribution guidance
- `CITATION.cff`: citation metadata

## Architectural principle

> **Threat, control, audit, regulatory, and interoperability frameworks are pluggable inputs to Proof Protocol. Proof Protocol is framework-agnostic.**

External frameworks and standards identify what may be tested or how results may be carried. Proof Protocol independently establishes whether a control worked and what evidence proves the result.

## Ownership and external-framework notice

The Proof Protocol mapping is independently authored. IETF SCITT names, identifiers, trademarks, and upstream materials remain subject to their respective ownership and licensing. Mapping establishes interoperability, not dependency, endorsement, certification, or transfer of ownership. No affiliation, endorsement, certification, or sponsorship by the IETF, the IETF Trust, or the IETF SCITT Working Group is implied.

This specification references IETF RFC 9943, IETF RFC 9942, and the Internet-Draft draft-ietf-scitt-scrapi-11. Copyright (c) 2026 IETF Trust and the persons identified as the document authors. All rights reserved. (Notice source: RFC 9943, https://www.rfc-editor.org/rfc/rfc9943.html.) No IETF text or Code Components are reproduced.

Proof Protocol, ProofStamp, ProofBundle, and ProofRegister are trademarks of Nebulonium, Inc.
