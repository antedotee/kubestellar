<!--security-start-->
## Security Announcements

Join the [kubestellar-security-announce](https://groups.google.com/u/1/g/kubestellar-security-announce) group for emails about security and major API announcements.

## SLSA Level 3 Compliance

KubeStellar implements SLSA (Supply-chain Levels for Software Artifacts) Level 3 to provide strong security guarantees for our build and release process.

### Security Guarantees

- **Build Integrity**: All builds are performed in isolated, ephemeral GitHub Actions environments
- **Provenance Authenticity**: Build provenance is cryptographically signed and tamper-proof
- **Supply Chain Transparency**: Complete Software Bill of Materials (SBOM) generated for all artifacts
- **Immutable References**: All builds use commit hashes instead of mutable tag references

### Verification Instructions

#### Binary Artifacts
```bash
# Download the release and verification files
wget https://github.com/kubestellar/kubestellar/releases/download/v0.x.x/kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz
wget https://github.com/kubestellar/kubestellar/releases/download/v0.x.x/kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz.sig
wget https://github.com/kubestellar/kubestellar/releases/download/v0.x.x/kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz.pem

# Verify the signature
cosign verify-blob \
  --certificate kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz.pem \
  --signature kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz.sig \
  kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz

# Verify checksums
sha256sum -c kubestellar_v0.x.x_COMMIT_checksums.txt
```

#### Container Images
```bash
# Verify container images
cosign verify ghcr.io/kubestellar/kubestellar/controller-manager:COMMIT_HASH
cosign verify ghcr.io/kubestellar/kubestellar/ocm-transport-controller:COMMIT_HASH

# Inspect SBOM
cosign download sbom ghcr.io/kubestellar/kubestellar/controller-manager:COMMIT_HASH
```

#### SLSA Provenance
```bash
# Download and verify SLSA provenance
slsa-verifier verify-artifact \
  --provenance-path kubestellar_v0.x.x.intoto.jsonl \
  --source-uri github.com/kubestellar/kubestellar \
  kubestellar_v0.x.x_COMMIT_linux_amd64.tar.gz
```

### Trust Model

Our security model is built on:
- **GitHub Actions Security**: Isolated, ephemeral build environments
- **Sigstore**: Keyless signing with transparency logs
- **SLSA Framework**: Industry-standard supply chain security
- **Cryptographic Verification**: All artifacts are cryptographically signed

### Security Features

- ✅ SLSA Build Level 3 compliant
- ✅ Cryptographic signing of all artifacts
- ✅ Software Bill of Materials (SBOM) generation
- ✅ Vulnerability scanning with govulncheck
- ✅ Immutable build references using commit hashes
- ✅ Transparent logging and audit trails
- ✅ Isolated build environments
- ✅ Dependency verification and integrity checks

## Report a Vulnerability

We're extremely grateful for security researchers and users that report vulnerabilities to the KubeStellar Open Source Community. All reports are thoroughly investigated by a set of community volunteers.

You can also email the private [kubestellar-security-announce@googlegroups.com](mailto:kubestellar-security-announce@googlegroups.com) list with the security details and the details expected for [all KubeStellar bug reports](https://github.com/kubestellar/kubestellar/blob/main/.github/ISSUE_TEMPLATE/bug_report.yaml).

### When Should I Report a Vulnerability?

- You think you discovered a potential security vulnerability in KubeStellar
- You are unsure how a vulnerability affects KubeStellar
- You think you discovered a vulnerability in another project that KubeStellar depends on
  - For projects with their own vulnerability reporting and disclosure process, please report it directly there


### When Should I NOT Report a Vulnerability?

- You need help tuning KubeStellar components for security
- You need help applying security related updates
- Your issue is not security related

## Security Vulnerability Response

Each report is acknowledged and analyzed by the maintainers of KubeStellar within 3 working days.

Any vulnerability information shared with Security Response Committee stays within KubeStellar project and will not be disseminated to other projects unless it is necessary to get the issue fixed.

As the security issue moves from triage, to identified fix, to release planning we will keep the reporter updated.

## Public Disclosure Timing

A public disclosure date is negotiated by the KubeStellar Security Response Committee and the bug submitter. We prefer to fully disclose the bug as soon as possible once a user mitigation is available. It is reasonable to delay disclosure when the bug or the fix is not yet fully understood, the solution is not well-tested, or for vendor coordination. The timeframe for disclosure is from immediate (especially if it's already publicly known) to a few weeks. For a vulnerability with a straightforward mitigation, we expect report date to disclosure date to be on the order of 7 days. The KubeStellar maintainers hold the final say when setting a disclosure date.
<!--security-end-->