# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 0.9.x   | :white_check_mark: |
| < 0.9   | :x:                |

## Reporting a Vulnerability

We take security seriously. If you discover a vulnerability in Docs Assembler, please report it privately so we can address it before public disclosure.

**Please do NOT open a public issue for security vulnerabilities.**

### How to report

Email us directly at **team@netoftrees.com** with:
- A clear description of the vulnerability
- Steps to reproduce (if applicable)
- Potential impact assessment
- Any suggested mitigation (optional)

### What to expect

- **Acknowledgement** within 48 hours
- **Initial assessment** within 5 business days
- **Resolution timeline** communicated once assessed
- **Credit** in release notes if you wish to be named (with your permission)

### Scope

This policy covers:
- The Docs Assembler VS Code extension
- The template repository and build pipeline
- Published documentation output (Markdown/HTML generation)

It does **not** cover:
- Third-party display tools implementing the Display Tool Contract
- Content authored by users of the extension
- GitHub Pages or other hosting platforms

## Security Considerations for Display Tool Authors

If you are building a display tool that consumes Docs Assembler output, please review the [Display Tool Contract](./DISPLAY_TOOLS.md) security section, which covers:

- Proxy allowlist validation for cross-origin fragment loading
- Path validation for proxied assets
- Rate limiting and abuse prevention
- Content-Type preservation for proxied media

## Disclosure Policy

We follow a coordinated disclosure approach:
1. We work with reporters to validate and fix the issue
2. We release a patched version
3. We publish an advisory after users have had reasonable time to update

We aim to complete this cycle within 90 days of initial report, though critical vulnerabilities may be expedited.
