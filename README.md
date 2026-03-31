## ☕ Support
If you like this project, consider supporting:
<a href="https://www.buymeacoffee.com/Kjorri" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

[![Buy Me a Coffee](https://img.buymeacoffee.com/button-api/?text=Buy me a coffee&emoji=☕&slug=YOURNAME&button_colour=FFDD00&font_colour=000000&font_family=Arial&outline_colour=000000&coffee_colour=ffffff)](https://www.buymeacoffee.com/Kjorri)
# Data Sanitizer

A lightweight, client-side web tool for removing sensitive data from logs, configs, and command outputs before sharing them safely with AI assistants or in public forums.

## Quick Start

1. Open `data-sanitizer.html` in any modern web browser
2. Paste text or drag & drop a file into the input area
3. Adjust the redaction options if you need to preserve safe debugging identifiers
4. Review the diff preview and redaction summary
5. Copy or download the sanitized output

## Features

- **Zero Trust**: All processing happens in your browser—nothing is sent to servers
- **Selectable Redaction Options**: Choose which categories to remove based on where you plan to share the output
- **Comprehensive Redaction**: Detects and masks:
  - Private keys (SSH, TLS)
  - API tokens (GitHub, GitLab, AWS, Kubernetes)
  - Email addresses and domain names
  - IP addresses (IPv4 & IPv6)
  - File system paths (Windows & Unix)
  - Credit cards (with Luhn validation)
  - Personal identifiers (Finnish ID, IBAN)
  - MAC addresses, UUIDs, and more
- **Diff Preview**: See exactly what changed with visual highlighting
- **Multiple Export Options**: Copy cleaned text, download file, or copy diff
- **Large File Support**: Handles files up to 2 MB
- **No Dependencies**: Pure HTML/CSS/JavaScript—no build steps needed

### Redaction Profiles and Safe Defaults

The sanitizer now groups redaction categories so you can keep useful debugging context when sharing internally:

- **Always enabled**: secrets and credentials such as private keys, JWTs, AWS keys, tokens, and password values
- **Enabled by default**: financial data, IPs, MAC addresses, FQDNs, emails, `user@host` patterns, and user-specific file paths
- **Disabled by default**: UUIDs and interface names, which are often useful for colleague-facing debugging and ticket triage

Recommended use:

- **Public forums / AI prompts**: keep the default settings enabled
- **Internal colleague debugging**: optionally leave UUIDs and interface names visible
- **Security-sensitive review**: keep all default protections on and manually inspect the diff before sharing


## Usage Examples

### Sharing a Docker error log
```bash
# Before sanitization (risky to share)
[user@prod-server ~]$ docker logs api-container 2>&1 | head -20
Error connecting to db.company.com:5432
User: dbadmin, Password: MySecurePass123!
...

# After sanitization (safe to share)
Error connecting to [FQDN]:5432
User: [USER], Password: [REDACTED]
...
```

### Preparing code snippet for ChatGPT
Paste your AWS configuration with credentials → sanitizer replaces keys with `[AWS_ACCESS_KEY_ID]` → safe to paste into AI prompt

### Redacting server logs before posting to Stack Overflow
Upload your syslog → diff preview shows all changes → download sanitized version

## What Gets Redacted

| Tag | Pattern |
|-----|---------|
| `[PRIVATE_KEY]` | SSH/TLS private keys |
| `[JWT]` | JSON Web Tokens |
| `[GITLAB_PAT]` | GitLab Personal Access Tokens |
| `[AWS_ACCESS_KEY_ID]` | AWS access key IDs |
| `[KUBE_SECRET]` | Kubernetes secrets |
| `[EMAIL]` | Email addresses |
| `[IP]` / `[IP6]` | IPv4 and IPv6 addresses |
| `[FQDN]` | Fully qualified domain names |
| `[USER]@[HOST]` | Shell prompts and SSH commands |
| `[PATH]` | File system paths |
| `[MAC]` | MAC addresses |
| `[UUID]` | UUIDs |
| `[CREDIT_CARD]` | Valid credit cards (Luhn check) |
| `[FINNISH_ID]` | Finnish personal IDs |
| `[IBAN]` | IBAN numbers |

## Selectable Categories

The options panel groups patterns into these categories:

- **Secrets & Keys**: private keys, JWTs, GitLab PATs, AWS access keys, Kubernetes secrets, password and token assignments
- **Financial Data**: credit cards, Finnish personal IDs, IBANs
- **IP Addresses**: IPv4 and IPv6 addresses
- **MAC Addresses**: hardware identifiers
- **Domain Names**: fully qualified domain names and hostnames inside URLs
- **Email Addresses**: email recipients and senders
- **Username@Host**: shell prompts, SSH/SCP/SFTP/rsync user and host references, `sudo -u`, `password for user`
- **File Paths**: Windows and user-specific Unix paths
- **UUIDs**: request and transaction identifiers, off by default
- **Interface Names**: `eth0`, `vlan100`, `ge-0/0/0`, off by default

## Features Explained

### Redaction Options Panel

Use the options panel to tune the output for the audience:

- Leave defaults on for anything leaving your team
- Enable UUID/interface masking when identifiers should not be shared
- Disable those optional categories when they help debugging and do not expose sensitive data in your environment
- Use **Reset to Defaults** to return to the safe baseline

### Changed Lines Toggle
By default, the diff shows all lines. Check "Show only changed lines" to focus on redacted content in large files.

### Redaction Summary
See a count of each redaction type (e.g., `[EMAIL]: 5`, `[IP]: 2`) to validate the sanitization worked as expected.

### File Upload
- Drag & drop files or click to select
- Max file size: 2 MB
- Supports: `.txt`, `.log`, `.md`, `.csv`, `.json`, `.yaml`, `.yml`, `.ini`, `.conf`

### Auto-Clean on Paste
Paste text into the input area → automatic sanitization after 60ms

## Browser Compatibility

Works on all modern browsers:
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Safari on iOS 14+

## Tips for Best Results

1. **Always review the diff** before sharing—no automation catches 100% of context-specific secrets
2. **Check the redaction summary** to ensure all expected patterns were caught
3. **Use the defaults for public sharing** and only relax optional categories for trusted internal debugging
4. **Test with the sample** in the help menu to verify behavior
5. **For large files**, use the "only changed lines" toggle to focus review effort

## Development

No build process required. The tool is a single HTML file:
- HTML structure
- CSS (Telia brand colors)
- Vanilla JavaScript (no frameworks)

### Customization

Edit the `scrub()` function in `data-sanitizer.html` to:
- Add new regex patterns
- Adjust redaction tags
- Change color scheme (CSS variables at top)

## License

MIT License - See LICENSE file for details

## Disclaimer

This tool helps reduce accidental exposure of sensitive data, but **no automated tool is perfect**. Always manually review output before sharing sensitive information. For highly confidential data, consider alternative approaches.

## Contributing

Found a pattern we miss? Want to add support for a new secret format?
- Report issues with specific examples
- Suggest new regex patterns
- Test edge cases

---

**Made for sharing sensitive context safely with AI and colleagues** 🔒
