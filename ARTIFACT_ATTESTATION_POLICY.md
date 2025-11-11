# Singularity Organization - Artifact Attestation Policy

**Effective Date**: 2025-11-11
**Policy Owner**: Engineering Leadership
**Applies To**: All repositories in Singularity-ng organization

## Overview

This policy mandates the use of GitHub Artifact Attestations for all software releases within the Singularity organization to ensure supply chain security and build provenance.

## Requirements

### All Release Artifacts MUST:

1. **Have Build Provenance Attestations**
   - Use `actions/attest-build-provenance@v2` in release workflows
   - Minimum SLSA Build Level 2 compliance
   - Attestations must be verifiable via `gh attestation verify`

2. **Include SHA256 Checksums**
   - All release artifacts must have accompanying SHA256SUMS file
   - Checksums must be published alongside artifacts

3. **Use Organization Reusable Workflows**
   - Prefer `.github` org reusable workflows when available
   - Example: `Singularity-ng/.github/.github/workflows/rust-release-with-attestations.yml`

## Verification

Users must be able to verify artifacts:

```bash
# Verify attestation
gh attestation verify <artifact> -R Singularity-ng/<repo>

# Verify checksum
sha256sum -c SHA256SUMS
```

## Implementation

### For New Projects
- Use organization workflow templates that include attestations by default

### For Existing Projects
- Add attestation steps to existing release workflows
- See [rust-release-with-attestations.yml](.github/workflows/rust-release-with-attestations.yml) for reference

## Permissions Required

Workflows need these permissions:

```yaml
permissions:
  contents: write
  id-token: write      # Required for attestations
  attestations: write  # Required for attestations
```

## Exceptions

Exceptions require approval from:
- Security Team Lead
- Engineering Director

Document exceptions in repository's `SECURITY.md`.

## Benefits

- **Supply Chain Security**: Cryptographic proof artifacts came from our GitHub Actions
- **Compliance**: Meets SLSA Build Level 2 requirements
- **Trust**: Users can verify authenticity of our software
- **Audit Trail**: Complete provenance tracking

## Resources

- [GitHub Artifact Attestations Docs](https://docs.github.com/en/enterprise-cloud@latest/actions/security-for-github-actions/using-artifact-attestations)
- [Singularity-ng/.github](https://github.com/Singularity-ng/.github) - Organization workflows
- Internal: Slack #security channel for questions

## Enforcement

- **Automated**: Reusable workflows enforce attestations automatically
- **Code Review**: PRs modifying release workflows require security team review
- **Quarterly Audits**: Security team audits compliance across all repos

---

**Questions?** Contact security@singularity.com or post in #security Slack channel.
