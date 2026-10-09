# Changelog

All notable changes to n8n-error-handling-pattern are documented here.
Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/)
Versioning: [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

## [Unreleased]

### Changed
- Clarified that the workflows are examples, corrected queue/masking descriptions, and removed unsupported production and client-result claims.

## [0.1.0] - 2024-08-15

### Added
- Initial repository scaffold with MIT license, CONTRIBUTING.md, and issue templates
- GitHub Actions CI for JSON validation (credential leak detection, active flag check)
- Stale issue bot configuration (auto-close after 30 days)
- `.gitignore` blocking credential files and n8n instance metadata
- README shell with architecture diagram placeholder
- Repository structure: `workflows/`, `payloads/`, `docs/`
