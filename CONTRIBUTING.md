# Contributing to Data Sanitizer

Thank you for your interest in contributing! This guide will help you understand how to contribute effectively.

## How to Contribute

### Reporting Issues

Found a pattern we don't catch? Have a suggestion? Please:

1. **Check existing issues** to avoid duplicates
2. **Provide specific examples** of text that should be sanitized
3. **Describe expected vs. actual behavior**
4. **Include browser/OS information** if relevant

**Example issue:**
```
Title: SSH keys not sanitized
Description: When I paste an OpenSSH private key, it shows [PRIVATE_KEY] 
but Ed25519 keys with BEGIN OPENSSH PRIVATE KEY are not caught.
Example: [provide redacted example]
```

### Suggesting New Patterns

Want to add support for a new secret format?

1. **Describe the pattern** (structure, examples)
2. **Provide test cases** (before/after examples)
3. **Suggest a tag name** (e.g., `[GITHUB_TOKEN]`)
4. **Explain the use case** (how commonly is it seen?)

**Example suggestion:**
```
Title: Add GitHub token detection
Pattern: ghp_[A-Za-z0-9]{36}
Example input: ghp_1234567890abcdefghijklmnopqrstuvwxyz
Expected output: [GITHUB_TOKEN]
```

### Code Contributions

Improving the sanitizer? Here's how:

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Test your changes** thoroughly
4. **Follow the style** of existing code
5. **Commit clearly**: `git commit -m "Add XYZ pattern detection"`
6. **Push and create a pull request**

### What to Improve

Areas where contributions are most welcome:

- **New regex patterns** for missing secret types
- **Performance optimizations** for large files
- **UI/UX improvements** (accessibility, responsiveness)
- **Documentation** (examples, edge cases)
- **Test cases** for validation
- **Browser compatibility** fixes
- **Localization** (translations, regional formats)

### Code Style Guide

- Use 4-space indentation
- Keep lines under 100 characters where reasonable
- Add comments for complex regex patterns
- Use clear variable names
- Test in multiple browsers

### Pattern Development

When adding a new regex pattern:

1. **Be specific** - avoid over-matching (e.g., don't flag all numbers as secrets)
2. **Test edge cases** - uppercase, lowercase, with/without separators
3. **Consider context** - look ahead/behind for shell commands or common prefixes
4. **Document** - add a comment explaining the pattern
5. **Add to help modal** - update the "What Gets Redacted" section

**Example pattern addition:**

```javascript
// GitHub Fine-grained PAT: ghp_* (36 chars after prefix)
[/ghp_[A-Za-z0-9]{36}\b/g, "[GITHUB_TOKEN]"],

// GitHub OAuth token: gho_* (48 chars after prefix)
[/gho_[A-Za-z0-9]{48}\b/g, "[GITHUB_OAUTH]"],
```

### Testing

Before submitting:

1. **Test with the sample data** in the help modal
2. **Try edge cases** (very long input, special characters)
3. **Check the diff preview** carefully
4. **Verify redaction summary** counts
5. **Test file upload** if you modified file handling
6. **Test in different browsers** (at least Chrome and Firefox)

### Pull Request Process

1. **Update CHANGELOG.md** with your changes
2. **Update README.md** if adding new features
3. **Include test examples** in your PR description
4. **Keep commits focused** - one feature per PR if possible
5. **Respond to feedback** promptly

### Code of Conduct

Be respectful and constructive:
- Assume good intent
- Provide specific feedback
- Welcome diverse perspectives
- Help others learn

## Questions?

If you're unsure about something:
- Check existing issues/discussions
- Look at closed PRs for similar patterns
- Ask clearly and provide context

---

**Happy contributing! Your help makes Data Sanitizer better for everyone.** 🎉
