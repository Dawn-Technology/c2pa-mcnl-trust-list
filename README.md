# Trust List for verifieermij.nl

This repository contains the trust list used by verifieermij.nl to determine
which issuer identities are trusted when verifying C2PA content credentials.

## Purpose

The trust list is an allowlist of issuer DIDs (Decentralized Identifiers).
During verification, an issuer is considered trusted only when its DID appears
in the list.

## Trust List Location

- `trust-list/ICA-ISSUERS-TRUST-LIST.txt`

## File Format

The trust list is a plain text file with one DID per line.

Rules:

1. Each non-empty line must contain a valid DID.
2. No JSON, YAML, or additional separators are used.
3. Duplicate entries should be avoided.
4. Keep entries stable and reviewable in pull requests.

Current examples in the list include:

- `did:jwk:...`
- `did:web:verify.contentauthenticity.org`
- `did:web:c2pa.org`

## How verifieermij.nl Uses This List

At verification time, verifieermij.nl:

1. Extracts the issuer DID from the credential/manifest chain.
2. Compares it to entries in the trust list.
3. Marks the issuer as trusted only when an exact match is found.

If no match is found, the credential can still be syntactically valid, but it
is treated as not trusted by this policy.

## Maintenance

When adding or removing issuers:

1. Open a pull request with a clear reason for the change.
2. Keep changes minimal (one issuer change per PR when possible).
3. Include the issuer owner/context in the PR description.
4. Request review from maintainers responsible for trust policy.

Recommended checks before merge:

- No malformed DID values.
- No accidental whitespace-only lines.
- No duplicate entries.

## Security Notes

- This file is security-sensitive. Treat changes as policy changes.
- Never add an issuer without ownership and trust validation.
- Prefer explicit removal when trust must be revoked.

## Versioning and Auditability

Git history is the audit trail for trust policy decisions. Every change should
be traceable to a review and rationale.
