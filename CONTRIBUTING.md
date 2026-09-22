# Contributing

Thank you for your interest in contributing to **paste-elements-webjar**.

This project is part of **Tomshley LLC's OSS and IP strategy** and is part of PasteStack,
which augments the tools a consumer already uses rather than taking over its application
model. It is intentionally opinionated.

## Philosophy

- Packaging only: the jar carries an upstream release or pinned development package, unmodified
- The packaged version, its license and its notices always agree

Changes that increase complexity, size, or scope without strong justification are unlikely
to be accepted.

## What Contributions Are Welcome

- Packaging and verification fixes
- Documentation improvements

## What Is Out of Scope

- Patching upstream sources during packaging
- Raising `upstreamVersion` without moving the license and notice statements with it
- Updating the development pin partially: `upstream.development.version`,
  `upstream.development.url`, and `upstream.development.sha256` move together
- Changing project versions on develop or feature branches instead of using the repository's release flow
- Changes that contradict the project's architecture or philosophy
- Large refactors without prior discussion

## Process

1. Open an issue describing the change
2. Fork the repository
3. Create a focused merge request
4. Keep commits clean and scoped

Tomshley LLC reserves final decision-making authority on all contributions.
