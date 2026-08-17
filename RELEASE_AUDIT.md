# Release audit

Audit date: 2026-08-17

- Built from an explicit whitelist; no workspace-wide copy was used.
- No Git repository was initialized.
- Exact submission archive SHA-256:
  `f6824fa6bbc79368e358da847b1aca9597b0be79ee1afbde10f5b8adedb059ed`.
- The archive has exactly three regular members and no links.
- The extracted member hashes match `manifest.json` and the package-build
  receipt.
- The public ledger contains one early continuous ledger, one early progress
  summary, and 581 later preregistration/verdict records.
- No working conversation, agent handoff, credential file, environment file,
  private key, cache directory, editable report source, or personal absolute
  path was copied.
- Text and vendored-binary scans found no common API-key, access-token,
  password-assignment, bearer-token, or private-key signature.
- `SHA256SUMS` covers every released file except itself.

The final PDF intentionally includes its published LLM-assistance disclosure;
that authored disclosure is part of the report rather than a working transcript.
