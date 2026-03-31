# Test Cases for Data Sanitizer

This file documents expected behavior for various input patterns.

## Private Keys

### OpenSSH Private Key (RSA)
**Input:**
```
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEA1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJ
...
-----END RSA PRIVATE KEY-----
```
**Expected Output:** `[PRIVATE_KEY]`

### PEM Encoded Key
**Input:**
```
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQE...
-----END PRIVATE KEY-----
```
**Expected Output:** `[PRIVATE_KEY]`

## API Tokens

### GitLab PAT
**Input:** `glpat-xxAbCdEfGh1234567890`
**Expected Output:** `[GITLAB_PAT]`

### AWS Access Key
**Input:** `AKIAIOSFODNN7EXAMPLE`
**Expected Output:** `[AWS_ACCESS_KEY_ID]`

### JWT Token
**Input:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIn0.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ`
**Expected Output:** `[JWT]`

### Kubernetes Secret
**Input:** `token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`
**Expected Output:** `token: [KUBE_SECRET]`

## Credentials

### Password Assignment
**Input:** `password=MySecurePass123!`
**Expected Output:** `password=[REDACTED]`

**Input:** `DB_PASS=super_secret_db_pass`
**Expected Output:** `DB_PASS=[REDACTED]`

### API Key
**Input:** `apikey: dj84kd9fj0d92jd0j220djasdjkalsjdk`
**Expected Output:** `apikey: [REDACTED]`

## Network

### Email Address
**Input:** `admin@company.com`
**Expected Output:** `[EMAIL]`

### IPv4 Address
**Input:** `192.168.1.100`
**Expected Output:** `[IP]`

**Input:** Multiple in text: `Server 10.0.0.5 failed, contact 10.0.0.1`
**Expected Output:** `Server [IP] failed, contact [IP]`

### IPv6 Address
**Input:** `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
**Expected Output:** `[IP6]`

### FQDN / Domain Names
**Input:** `admin@server.company.com`
**Expected Output:** `[USER]@[FQDN]` (shell context) or `[EMAIL]` (email context)

**Input:** `https://api.example.com/endpoint`
**Expected Output:** `https://[FQDN]/endpoint`

**Input:** Standalone: `Connect to db.internal.company.fi`
**Expected Output:** `Connect to [FQDN]`

### MAC Address
**Input:** `00:1B:44:11:3A:B7`
**Expected Output:** `[MAC]`

**Input:** `00-1B-44-11-3A-B7`
**Expected Output:** `[MAC]`

## Shell Commands

### SSH Command
**Input:** `ssh deploy@app.example.com`
**Expected Output:** `ssh [USER]@[FQDN]`

### SCP Command
**Input:** `scp backup.tar.gz admin@backup.company.fi:/backups/`
**Expected Output:** `scp backup.tar.gz [USER]@[FQDN]:/backups/`

### Sudo with User
**Input:** `sudo -u postgres psql database`
**Expected Output:** `sudo -u [USER] psql database`

### Password for Username
**Input:** `password for deploy:`
**Expected Output:** `password for [USER]:`

### Shell Prompt
**Input:** `[user@server ~]$`
**Expected Output:** `[[USER]@[HOST] ~]$`

## File Paths

### Windows Path
**Input:** `C:\Users\john.doe\Documents\config.ini`
**Expected Output:** `[PATH]`

### Unix Path
**Input:** `/Users/john.doe/Library/Application Support/app/settings.json`
**Expected Output:** `[PATH]`

**Input:** `/etc/passwd`
**Expected Output:** `/etc/passwd` (system path, generally safe)

**Input:** `/home/username/private.key`
**Expected Output:** `[PATH]`

## Personal Identifiers

### Finnish Personal ID
**Input:** `010190A123A`
**Expected Output:** `[FINNISH_ID]`

### IBAN
**Input:** `FI1234567890123456`
**Expected Output:** `[IBAN]`

## Card Numbers

### Valid Credit Card (Luhn)
**Input:** `4532-1488-0343-6467`
**Expected Output:** `[CREDIT_CARD]`

### Invalid Credit Card (won't Luhn)
**Input:** `1234-5678-9012-3456`
**Expected Output:** `1234-5678-9012-3456` (unchanged - fails Luhn check)

## Identifiers Without Context

### UUID
**Input:** `550e8400-e29b-41d4-a716-446655440000`
**Expected Output (default settings):** `550e8400-e29b-41d4-a716-446655440000`

**Expected Output (UUID option enabled):** `[UUID]`

### Network Interface
**Input:** `eth0 failed, eth1 active`
**Expected Output (default settings):** `eth0 failed, eth1 active`

**Expected Output (Interface option enabled):** `[INTERFACE] failed, [INTERFACE] active`

## Option Profiles

### Public Sharing Defaults
Use the default UI options when preparing content for:

- public forums
- AI assistants
- vendor tickets
- screenshots shared outside the team

Expected behavior:

- secrets always redacted
- financial data redacted
- IPs, domains, emails, usernames, and file paths redacted
- UUIDs and interface names preserved unless explicitly enabled

### Internal Colleague Debugging
For trusted internal troubleshooting, UUIDs and interface names may be useful to keep visible.

Suggested behavior:

- leave defaults on for secrets, credentials, financial data, and user-identifying infrastructure data
- enable UUID/interface masking only if those identifiers are considered sensitive in your environment

## Edge Cases

### Multiple Patterns in One Line
**Input:** `ssh user@server.com with API key glpat-abc123def456ghi789jkl012`
**Expected Output:** `ssh [USER]@[FQDN] with API key [GITLAB_PAT]`

### Password in URL (should NOT be fully redacted)
**Input:** `https://user:password123@api.example.com/endpoint`
**Expected Output:** `https://user:password=[REDACTED]@[FQDN]/endpoint`

### Numbers That Aren't Secrets
**Input:** `Processing batch 12345 of 67890`
**Expected Output:** `Processing batch 12345 of 67890` (unchanged)

### Very Long Input (>100KB)
**Expected:** Should handle without performance degradation

### Special Characters
**Input:** `password>!@#$%^&*()`
**Expected Output:** Should match password pattern and redact value

---

## Test Validation

To validate changes:
1. Copy test cases above into the input area
2. Validate both default behavior and any optional category you enable
3. Check diff preview for each pattern
4. Verify redaction summary counts match expected tags
5. Look for false positives (things that shouldn't be redacted)
6. Look for false negatives (things that should be but aren't)

