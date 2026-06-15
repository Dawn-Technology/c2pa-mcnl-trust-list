# Trust List for verifieermij.nl

This repository contains the trust list used by verifieermij.nl to determine
which issuer identities are trusted when verifying C2PA content credentials.

## Purpose

This repository contains trust policy inputs used during verification of C2PA
content credentials:

- certificate trust anchors for C2PA signatures,
- certificate trust anchors for TSA timestamp signatures,
- and (when applicable) an allowlist of issuer DIDs.

## Trust List Location

- `trust-list/C2PA-TRUST-LIST.pem`
- `trust-list/C2PA-TSA-TRUST-LIST.pem`
- `trust-list/ICA-ISSUERS-TRUST-LIST.txt`

## Trust List Files and Purpose

This repository contains three trust policy files, each with a different role:

1. `trust-list/C2PA-TRUST-LIST.pem`
   - PEM bundle of trusted X.509 certificates for C2PA signing chains.
   - Used to build and validate certificate trust for content credential
     signatures.

2. `trust-list/C2PA-TSA-TRUST-LIST.pem`
   - PEM bundle of trusted X.509 certificates for Time-Stamp Authority (TSA)
     chains.
   - Used to validate RFC 3161 timestamp token signing chains associated with
     credentials.

3. `trust-list/ICA-ISSUERS-TRUST-LIST.txt`
   - Allowlist of trusted issuer DIDs.
   - Applied only when a C2PA manifest contains an identity assertion with a
     DID.

## File Format

`ICA-ISSUERS-TRUST-LIST.txt` is a plain text file with one DID per line.

Rules:

1. Each non-empty line must contain a valid DID.
2. No JSON, YAML, or additional separators are used.
3. Duplicate entries should be avoided.
4. Keep entries stable and reviewable in pull requests.

Current examples in the list include:

- `did:jwk:...`
- `did:web:verify.contentauthenticity.org`
- `did:web:c2pa.org`

`C2PA-TRUST-LIST.pem` and `C2PA-TSA-TRUST-LIST.pem` use standard PEM encoding
for X.509 certificates. They should contain one or more certificate blocks in
the form:

```
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

Rules for PEM trust lists:

1. Include only trusted root/intermediate certificates intended for policy
   trust.
2. Keep certificate ordering stable where possible to improve reviewability.
3. Avoid duplicate certificate entries.
4. Do not include private keys or unrelated PEM object types.

## How verifieermij.nl Uses This List

At verification time, verifieermij.nl:

1. Uses `C2PA-TRUST-LIST.pem` to validate certificate trust chains for C2PA
   signatures.
2. Uses `C2PA-TSA-TRUST-LIST.pem` to validate certificate trust chains for TSA
   timestamp signatures.
3. If an identity assertion with an issuer DID is present, extracts that DID
   and compares it to entries in `ICA-ISSUERS-TRUST-LIST.txt`.
4. Marks the issuer/signing context as trusted only when applicable policy
   checks pass.

If the DID check is applicable and no match is found, the credential can still
be syntactically valid, but it is treated as not trusted by this policy.

## Maintenance

When adding or removing trust entries:

1. Open a pull request with a clear reason for the change.
2. Keep changes minimal (one issuer change per PR when possible).
3. Include the issuer owner/context in the PR description.
4. Request review from maintainers responsible for trust policy.

Recommended checks before merge:

- No malformed DID values.
- No accidental whitespace-only lines.
- No duplicate entries.
- PEM files contain valid certificate blocks only.
- No expired or unintended certificates are introduced without explicit review.

## Security Notes

- This file is security-sensitive. Treat changes as policy changes.
- Never add an issuer without ownership and trust validation.
- Prefer explicit removal when trust must be revoked.

## Versioning and Auditability

Git history is the audit trail for trust policy decisions. Every change should
be traceable to a review and rationale.
