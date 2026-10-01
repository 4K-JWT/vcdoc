# Thailand's VC stack

{/*
AGENT NOTES (hidden — not rendered on the site):

This page is a summary, not a translation. The Thai chapters are the single
source of truth. Where this page and the Thai text differ, the Thai text is
correct. Each item links to the Thai chapter that defines it.

Source chapters (paths under th/thai-vc-arf/ unless noted):
- Issuing a credential: 05-standards-and-compliance.md, 08-implementation-guidelines.md, 08.1-issuance-full-flow-detail.md
- Presenting a credential: 05-standards-and-compliance.md, 08-implementation-guidelines.md, 08.2-oid4vp-full-flow-detail.md
- The Trusted List: 06.5-trust-building-mechanism.md, 07-cross-ecosystem-interoperability.md, 09-trust-model-3.md, 12-key-management-and-trustlist-deployment.md, th/research/41-trustlist-serving-filecdn-etag-binary.md
- Revocation and status: 13-vc-status-and-revocation.md, 12-key-management-and-trustlist-deployment.md, 10-wallet-unit-attestation.md

Full source of truth (Thai): ARF index (th/thai-vc-arf/README.md).
*/}

This page describes Thailand's credential stack, in the terms a counterpart system uses to check compatibility.

## Current PoC stage

This page is the planned specification, not what is implemented today. The current proof of concept implements only:

1. A LoTE that holds all issuers, verifiers, and wallet providers.
2. A wallet app that trusts entities only through the Trusted List, with no certificates. Entities publish a public key in the Trusted List, either as a `did:web` reference or as a public key string.
3. Wallet Instance Attestation (WIA) is not implemented yet.

## Short version

The five building blocks:

- **Credential format:** The system supports multiple formats. v1.1 used JSON-LD per W3C VC Data Model, which is still supported, and adds IETF SD-JWT VC (`dc+sd-jwt`) as the recommended format for new issuance.
- **Issuance protocol:** OpenID for Verifiable Credential Issuance (OID4VCI) 1.0 Final, pre-authorized code flow.
- **Presentation protocol:** OpenID for Verifiable Presentations (OID4VP) 1.0 Final, with DCQL queries.
- **Trust:** a Trusted List (ETSI TS 119 602), signed by ETDA (สพธอ.) and served over a CDN. No CA chain.
- **Credential status:** IETF Token Status List, published by each issuer.

For a counterpart assessing convergence: Thailand uses the same OID4VCI, OID4VP, and SD-JWT VC stack, but a Thai Trusted List entry is not accepted in the EU by itself. Recognition needs a trust agreement, role and assurance mapping, a bridge list, and conformance evidence.

## Issuing a credential

Issuance runs OID4VCI 1.0 Final, pre-authorized code flow, and issues only SD-JWT VC (`dc+sd-jwt`).

Before the wallet asks the user for the OTP, it checks the issuer against the Trusted List (TC-3 early). If the issuer is not `active` for that credential type, the flow stops.

At assurance level IAL2.3 and above, the access token is bound to a wallet key with DPoP (RFC 9449).

The credential request carries the wallet's attestation, signed by the wallet provider. The issuer checks the provider on the Trusted List (TC-2) and validates the attestation before signing.

Before storing the VC, the wallet checks the issuer signature, the validity window, and the status bit (TC-3 final and TC-4).

```mermaid
sequenceDiagram
    participant U as User
    participant W as Wallet
    participant I as Issuer
    participant TL as Trusted List
    participant WP as Wallet Provider
    participant SL as Status List

    Note over U,I: Request and offer
    U->>I: requests credential (portal)
    I->>U: offer QR + OTP by SMS

    Note over W,TL: Discovery + TC-3 early
    W->>I: GET /.well-known/openid-credential-issuer
    I-->>W: metadata: dc+sd-jwt configurations
    W->>TL: TC-3 early: issuer active + type allowed
    W->>U: asks for OTP (tx_code)

    Note over W,I: OTP + token (DPoP)
    W->>I: POST /token + OTP + DPoP
    I-->>W: access_token (DPoP-bound) + c_nonce

    Note over W,WP: Credential + TC-2
    W->>I: POST /credential + proof + wallet_attestation
    I->>WP: TC-2: provider in list + WIA status

    Note over W,SL: Issue + TC-3/TC-4
    I->>W: issues SD-JWT VC (dc+sd-jwt)
    W->>TL: TC-3 final: signature + validity
    W->>SL: TC-4: read status bit
    SL-->>W: bit 0 (valid)
```

## Presenting a credential

Presentation runs OID4VP 1.0 Final, with DCQL instead of Presentation Exchange.

Before showing a consent screen, the wallet checks the verifier against the Trusted List (TC-1): is the verifier `active`, and are the requested claims and purpose within what it registered? If not, the wallet sends nothing.

The verifier then checks three things:

- TC-2: the wallet provider is on the Trusted List and the wallet attestation is valid and fresh. This check is mandatory for Thai government wallets. For a private wallet, it depends on the verifier's confidence level (standard or enhanced).
- TC-3: the issuer is on the Trusted List, and the issuer's DID resolves to the key that signed the credential.
- TC-4: the status bit is read from the issuer's status list.

A valid signature alone is not enough to accept the credential.

Identifiers: organisations use `did:web`, holders use `did:jwk`, and Thai identity rails such as NDID resolve as custom methods through the ETDA Trust Gateway (the universal resolver).

```mermaid
sequenceDiagram
    participant U as User
    participant W as Wallet
    participant V as Verifier
    participant TL as Trusted List
    participant R as DID Resolver
    participant SL as Status List

    Note over U,TL: Request + TC-1 wallet check
    U->>W: scans authorization QR (DCQL)
    W->>TL: TC-1: verifier role / status / scope
    TL-->>W: active / allowed attributes/purposes

    Note over W,V: Consent + presentation
    W->>U: consent: purpose + claims
    U-->>W: approves (selective disclosure)
    W->>V: vp_token (presentation + WIA-PoP)

    Note over V,TL: TC-2 wallet provider
    V->>TL: TC-2: wallet provider in list
    TL-->>V: provider active / WIA valid

    Note over V,TL: TC-3 issuer + keys (DID)
    V->>R: TC-3: resolve issuer DID (kid)
    R-->>V: DID document / verification key
    V->>TL: TC-3: issuer entry + key match

    Note over V,SL: TC-4 status + result
    V->>SL: TC-4: read status bit
    SL-->>V: bit 0 (valid)
    V->>W: granted / or 400 invalid_request
```

## The Trusted List

ETDA audits each organisation that wants to act as issuer, verifier, or wallet provider, then signs it into one list. The list is a signed JWT (ETSI TS 119 602), served on a CDN with no API key. The ETDA signature is the only reason to believe it.

An entry is one organisation with one or more roles. Every entity publishes a public key, either directly or as a DID document reference. A role carries the role type, status, validity, and per-role limits: issuers list the credential types they may issue, verifiers the claims and purposes they may request. Status must be `active` at both the organisation and role level.

Participants pull the list lazily, only when a connection needs a trust decision, and fail closed if the list cannot be pulled. Entity-level revocation reaches each entity at its next lazy pull.

The Trusted List, not the DID document, decides who may act: DID documents supply the keys used to verify signatures but do not grant permission. ETDA signs the list from FIPS 140-3 Level 3 HSMs under an M-of-N quorum, with an offline emergency key.

Trusted List Example
1. Issuer: eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UiLCJpYXQiOjE3ODkwMzA2MDIsIng1YyI6WyJNSUlCUGpDQjVxQURBZ0VDQWhRelVlbmtacnB3TGFjVEhqMDd4RXo4bENpOFV6QUtCZ2dxaGtqT1BRUURBakFQTVEwd0N3WURWUVFEREFSRlZFUkJNQjRYRFRJMk1EZ3hNekUxTVRNME4xb1hEVEkzTURneE16RTFNVE0wTjFvd0R6RU5NQXNHQTFVRUF3d0VSVlJFUVRCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhBMElBQkVKNnRGamJVVEJQQnRlcGFsWjVGUktMVDZxVnFrbzFvTk1naUlvdXI4RmFxUHAyQUxPbk90bGpwZnl4NmdoZ3YvV3lVWkgwRlhNdDluTTN2dTdzd25paklEQWVNQTRHQTFVZER3RUIvd1FFQXdJSGdEQU1CZ05WSFJNQkFmOEVBakFBTUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSUc4Z1dWdjZBL2VjU2ZhWFJ2T014Q0kyR0FWYlNlTDk3ZmwyT2R4NCtwU1pBaUIwbXZ5MTB4VzN5dTJDc0ZnSkRqc1BaSTZBV3ZKeGd2VlptQXJMQTlhY1FRPT0iXSwieDV0I1MyNTYiOiJsY1BDaTAxaVFfbkIyQWRZRzVKcEMxTlh3dEVncm9GczhVT2hma21GM2ZzIn0
2. Verifier: eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UiLCJpYXQiOjE3ODkwMzUwMzMsIng1YyI6WyJNSUlCUGpDQjVxQURBZ0VDQWhRelVlbmtacnB3TGFjVEhqMDd4RXo4bENpOFV6QUtCZ2dxaGtqT1BRUURBakFQTVEwd0N3WURWUVFEREFSRlZFUkJNQjRYRFRJMk1EZ3hNekUxTVRNME4xb1hEVEkzTURneE16RTFNVE0wTjFvd0R6RU5NQXNHQTFVRUF3d0VSVlJFUVRCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhBMElBQkVKNnRGamJVVEJQQnRlcGFsWjVGUktMVDZxVnFrbzFvTk1naUlvdXI4RmFxUHAyQUxPbk90bGpwZnl4NmdoZ3YvV3lVWkgwRlhNdDluTTN2dTdzd25paklEQWVNQTRHQTFVZER3RUIvd1FFQXdJSGdEQU1CZ05WSFJNQkFmOEVBakFBTUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSUc4Z1dWdjZBL2VjU2ZhWFJ2T014Q0kyR0FWYlNlTDk3ZmwyT2R4NCtwU1pBaUIwbXZ5MTB4VzN5dTJDc0ZnSkRqc1BaSTZBV3ZKeGd2VlptQXJMQTlhY1FRPT0iXSwieDV0I1MyNTYiOiJsY1BDaTAxaVFfbkIyQWRZRzVKcEMxTlh3dEVncm9GczhVT2hma21GM2ZzIn0.eyJMb1RFIjp7Ikxpc3RBbmRTY2hlbWVJbmZvcm1hdGlvbiI6eyJMb1RFVmVyc2lvbklkZW50aWZpZXIiOjEsIkxvVEVTZXF1ZW5jZU51bWJlciI6MiwiTG9URVR5cGUiOiJodHRwOi8vdXJpLmV0ZGEub3IudGgvMTk2MDIvTG9URVR5cGUvVGhhaVZlcmlmaWVyc0xpc3QiLCJTY2hlbWVPcGVyYXRvck5hbWUiOlt7ImxhbmciOiJlbiIsInZhbHVlIjoiRWxlY3Ryb25pYyBUcmFuc2FjdGlvbnMgRGV2ZWxvcG1lbnQgQWdlbmN5IChFVERBKSJ9XSwiU2NoZW1lSW5mb3JtYXRpb25VUkkiOlt7ImxhbmciOiJlbiIsInVyaVZhbHVlIjoiaHR0cHM6Ly93d3cuZXRkYS5vci50aCJ9XSwiU3RhdHVzRGV0ZXJtaW5hdGlvbkFwcHJvYWNoIjoiaHR0cDovL3VyaS5ldGRhLm9yLnRoLzE5NjAyL1RoYWlWZXJpZmllcnNMaXN0L1N0YXR1c0RldG4vRVREQSIsIlNjaGVtZVR5cGVDb21tdW5pdHlSdWxlcyI6W3sibGFuZyI6ImVuIiwidXJpVmFsdWUiOiJodHRwOi8vdXJpLmV0ZGEub3IudGgvMTk2MDIvVGhhaVZlcmlmaWVycy9zY2hlbWVydWxlcy9FVERBIn1dLCJTY2hlbWVUZXJyaXRvcnkiOiJUSCIsIlNjaGVtZU5hbWUiOlt7ImxhbmciOiJlbiIsInZhbHVlIjoiVGhhaSBWZXJpZmllcnMgTGlzdCJ9XSwiSGlzdG9yaWNhbEluZm9ybWF0aW9uUGVyaW9kIjozNjUsIkRpc3RyaWJ1dGlvblBvaW50cyI6W10sIlNjaGVtZUV4dGVuc2lvbnMiOltdLCJMaXN0SXNzdWVEYXRlVGltZSI6IjIwMjYtMDktMTBUMTA6MTA6MzMuOTI4WiIsIk5leHRVcGRhdGUiOiIyMDI2LTA5LTExVDEwOjEwOjMzLjkyOFoifSwiVHJ1c3RlZEVudGl0aWVzTGlzdCI6W3siVHJ1c3RlZEVudGl0eUluZm9ybWF0aW9uIjp7IlRFTmFtZSI6W3sibGFuZyI6ImVuIiwidmFsdWUiOiJFbGVjdHJvbmljIFRyYW5zYWN0aW9ucyBEZXZlbG9wbWVudCBBZ2VuY3kgKEVUREEpIn1dfSwiVHJ1c3RlZEVudGl0eVNlcnZpY2VzIjpbeyJTZXJ2aWNlSW5mb3JtYXRpb24iOnsiU2VydmljZVR5cGVJZGVudGlmaWVyIjoiaHR0cDovL3VyaS5ldGRhLm9yLnRoLzE5NjAyL1N2Y1R5cGUvVmVyaWZpZXIiLCJTZXJ2aWNlTmFtZSI6W3sibGFuZyI6ImVuIiwidmFsdWUiOiJQcm9jaXZpcyBEZW1vIEJhbmsgVmVyaWZpZXIifV0sIlNlcnZpY2VEaWdpdGFsSWRlbnRpdHkiOnsiUHVibGljS2V5VmFsdWVzIjpbeyJrdHkiOiJFQyIsImNydiI6IlAtMjU2IiwieCI6IjVYemtpV0s4UWl1Y3N3SENLakk1S3dyM3JXZkZ6SDlBWElhN0FwMXpDeTgiLCJ5IjoidU9XYThGZU5nbTlyc0hiZmpBczZxTVZnb21Fa1JoMHl1QS1JRjhQUXFwRSJ9XSwiT3RoZXJJZHMiOlsiZGlkOndlYjp2ZXJpZmllci50b255aGVyZS53b3JrOnNzaTpkaWQtd2ViOnYxOmU1NDNlYTBlLTBhMTAtNGZmNS1iOGZhLTE0MTRmMGYzYWI5YyJdfX19XX1dfX0.u92cxz56yft9MttrtaOGJ7keqWsV8GX1BLfNQaMHToaQKnyrqcWBgkfrVQ5aRJHtociOro9EwGhWQrbiwHB-ag
3. Wallet Provider: eyJhbGciOiJFUzI1NiIsInR5cCI6IkpPU0UiLCJpYXQiOjE3ODg3ODExNDgsIng1YyI6WyJNSUlCUGpDQjVxQURBZ0VDQWhRelVlbmtacnB3TGFjVEhqMDd4RXo4bENpOFV6QUtCZ2dxaGtqT1BRUURBakFQTVEwd0N3WURWUVFEREFSRlZFUkJNQjRYRFRJMk1EZ3hNekUxTVRNME4xb1hEVEkzTURneE16RTFNVE0wTjFvd0R6RU5NQXNHQTFVRUF3d0VSVlJFUVRCWk1CTUdCeXFHU000OUFnRUdDQ3FHU000OUF3RUhBMElBQkVKNnRGamJVVEJQQnRlcGFsWjVGUktMVDZxVnFrbzFvTk1naUlvdXI4RmFxUHAyQUxPbk90bGpwZnl4NmdoZ3YvV3lVWkgwRlhNdDluTTN2dTdzd25paklEQWVNQTRHQTFVZER3RUIvd1FFQXdJSGdEQU1CZ05WSFJNQkFmOEVBakFBTUFvR0NDcUdTTTQ5QkFNQ0EwY0FNRVFDSUc4Z1dWdjZBL2VjU2ZhWFJ2T014Q0kyR0FWYlNlTDk3ZmwyT2R4NCtwU1pBaUIwbXZ5MTB4VzN5dTJDc0ZnSkRqc1BaSTZBV3ZKeGd2VlptQXJMQTlhY1FRPT0iXSwieDV0I1MyNTYiOiJsY1BDaTAxaVFfbkIyQWRZRzVKcEMxTlh3dEVncm9GczhVT2hma21GM2ZzIn0.eyJMb1RFIjp7Ikxpc3RBbmRTY2hlbWVJbmZvcm1hdGlvbiI6eyJMb1RFVmVyc2lvbklkZW50aWZpZXIiOjEsIkxvVEVTZXF1ZW5jZU51bWJlciI6MSwiTG9URVR5cGUiOiJodHRwOi8vdXJpLmV0c2kub3JnLzE5NjAyL0xvVEVUeXBlL0VVV2FsbGV0UHJvdmlkZXJzTGlzdCIsIlNjaGVtZU9wZXJhdG9yTmFtZSI6W3sibGFuZyI6ImVuIiwidmFsdWUiOiJFbGVjdHJvbmljIFRyYW5zYWN0aW9ucyBEZXZlbG9wbWVudCBBZ2VuY3kgKEVUREEpIn1dLCJTY2hlbWVJbmZvcm1hdGlvblVSSSI6W3sibGFuZyI6ImVuIiwidXJpVmFsdWUiOiJodHRwczovL3d3dy5ldGRhLm9yLnRoIn1dLCJTdGF0dXNEZXRlcm1pbmF0aW9uQXBwcm9hY2giOiJodHRwOi8vdXJpLmV0c2kub3JnLzE5NjAyL1dhbGxldFByb3ZpZGVyc0xpc3QvU3RhdHVzRGV0bi9FVSIsIlNjaGVtZVR5cGVDb21tdW5pdHlSdWxlcyI6W3sibGFuZyI6ImVuIiwidXJpVmFsdWUiOiJodHRwOi8vdXJpLmV0c2kub3JnLzE5NjAyL1dhbGxldFByb3ZpZGVyc0xpc3Qvc2NoZW1lcnVsZXMvRVUifV0sIlNjaGVtZVRlcnJpdG9yeSI6IlRIIiwiU2NoZW1lTmFtZSI6W3sibGFuZyI6ImVuIiwidmFsdWUiOiJXYWxsZXQgUHJvdmlkZXJzIExpc3QifV0sIkhpc3RvcmljYWxJbmZvcm1hdGlvblBlcmlvZCI6MzY1LCJEaXN0cmlidXRpb25Qb2ludHMiOltdLCJTY2hlbWVFeHRlbnNpb25zIjpbXSwiTGlzdElzc3VlRGF0ZVRpbWUiOiIyMDI2LTA5LTA3VDExOjM5OjA4LjM5NVoiLCJOZXh0VXBkYXRlIjoiMjAyNi0wOS0wOFQxMTozOTowOC4zOTVaIn0sIlRydXN0ZWRFbnRpdGllc0xpc3QiOltdfX0.Izh_JYqPikXvdYGKdYJY6I26VuWuLtWwCX1eKtvqxnVSMRsxxtV_K7Wx6lbp_1C7RemaFT52ZrAOZa5t1SmZ6g

## Revocation and status

Each issuer publishes its own Token Status List (`draft-ietf-oauth-status-list-18`). Status bits: 0 valid, 1 revoked, 2 suspended, 3 rotated (key rotation).

Issuers publish a change within 15 minutes (their publishing target, not an end-to-end guarantee). Verifiers read the status URL from the Trusted List entry, not from the credential, and read the status bit on every presentation (TC-4).

Revocation also happens above the credential level. If ETDA suspends or withdraws an issuer, all of that issuer's credentials stop verifying without any per-credential write, because the issuer's role record is no longer `active`. If a wallet provider revokes a wallet attestation, issuers that bound credentials to that wallet unit must revoke them in turn. Every credential also dies at `validUntil` without any of the above.

Emergency revocation reaches each entity at its next lazy pull, with no background polling and no push channel.
