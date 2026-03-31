# Changelog

All notable changes to Data Sanitizer will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Selectable redaction categories in the UI for tailoring output to public sharing or internal debugging
- Safe defaults that keep secrets, credentials, personal data, network details, and user-specific paths redacted
- Optional UUID and interface-name masking for stricter sharing requirements
- Reset-to-defaults action for quickly restoring the recommended protection baseline

### Changed
- Documentation now explains which categories are safe to preserve for colleague-facing troubleshooting and which should stay redacted for external sharing

## [1.0.0] - 2026-03-03

### Added
- Initial release with comprehensive sensitive data detection
- Support for private keys, API tokens, credentials
- Email, domain, IP address (IPv4 & IPv6) detection
- File path redaction (Windows & Unix)
- Credit card validation with Luhn algorithm
- Finnish personal ID and IBAN detection
- Shell command pattern recognition (ssh, scp, rsync, sftp)
- Interactive diff preview with visual highlighting
- "Show only changed lines" toggle for large files
- Redaction summary with tag enumeration
- Multi-format export (copy text, copy diff, download file)
- Drag & drop file upload support (max 2 MB)
- Auto-sanitization on paste
- Interactive help modal with test sample
- Cross-browser compatibility (Chrome, Firefox, Safari)
- Zero external dependencies
- MIT License

### Features
- Client-side processing (100% privacy—no server uploads)
- Responsive web interface with Telia brand colors
- File type validation and encoding detection
- Status messages for user feedback
- Keyboard shortcuts for accessibility

### Known Limitations
- No backend processing (frontend only)
- 2 MB file size limit for browser memory
- Some context-specific secrets may be missed
- Requires modern browser with JavaScript enabled

---

## Version Format
- **[x.y.z]** - Semantic versioning
- **[Unreleased]** - Changes in development

## Categories
- **Added** - New features
- **Changed** - Changes in existing functionality
- **Deprecated** - Soon-to-be removed features
- **Removed** - Removed features
- **Fixed** - Bug fixes
- **Security** - Security improvements
