# Security Scanning in KubeStellar

This document describes the security scanning implementation in KubeStellar, which includes multiple layers of security analysis to ensure code quality and identify potential vulnerabilities.

## Overview

KubeStellar implements a comprehensive security scanning strategy using multiple tools and approaches:

1. **CodeQL** - Static Application Security Testing (SAST) for Go code
2. **OSV-Scanner** - Open Source Vulnerability scanning for dependencies
3. **Trivy** - Container image vulnerability scanning
4. **License Scan** - License compliance checking

## CodeQL SAST Scanning

### What is CodeQL?

CodeQL is GitHub's semantic code analysis engine that allows you to query code as if it were data. It enables security researchers and developers to find vulnerabilities and errors in their code using queries written in the CodeQL language.

### Implementation Details

The CodeQL workflow is configured in `.github/workflows/codeql.yml` and includes:

- **Trigger**: Runs on pushes to main, pull requests, and weekly scheduled scans
- **Language Support**: Currently configured for Go
- **Analysis**: Uses GitHub's CodeQL action with autobuild for Go projects
- **Timeout**: 360 minutes to allow for comprehensive analysis
- **Permissions**: Minimal required permissions for security scanning

### How it Works

1. **Initialization**: Sets up the CodeQL environment for Go analysis
2. **Autobuild**: Automatically detects and builds the Go project
3. **Analysis**: Performs semantic analysis using CodeQL queries
4. **Results**: Uploads findings to GitHub's Security tab

### Viewing Results

CodeQL results are available in:
- GitHub Security tab for the repository
- Pull request security alerts
- GitHub Advanced Security dashboard (if enabled)

## OSV-Scanner

### What is OSV-Scanner?

OSV-Scanner is an open source vulnerability scanner that checks dependencies against the OSV database, which contains vulnerability information from various sources including GitHub Security Advisories, PyPA, RustSec, and more.

### Implementation Details

The OSV-Scanner workflow is configured in `.github/workflows/osv-scanner.yml` and includes:

- **Trigger**: Runs on pull requests, merge groups, pushes to main, and weekly scheduled scans
- **Scan Mode**: Different workflows for PRs vs scheduled scans
- **Scope**: Recursive scanning of the entire repository
- **Output**: SARIF format for integration with GitHub Security

### Supported Ecosystems

OSV-Scanner automatically detects and scans:
- Go modules (go.mod)
- Python packages (requirements.txt, setup.py, etc.)
- Node.js packages (package.json)
- Rust crates (Cargo.toml)
- And many other package ecosystems

## Trivy Container Scanning

### What is Trivy?

Trivy is a comprehensive security scanner that can scan for vulnerabilities in container images, file systems, Git repositories, and more.

### Implementation Details

The Trivy workflow is configured in `.github/workflows/trivy.yml` and includes:

- **Trigger**: Runs on pushes to main and weekly scheduled scans
- **Images Scanned**:
  - `kubestellar/core` - Main application image
  - `kubestellar/kubectl` - Kubectl tool image
- **Configuration**: Ignores unfixed vulnerabilities to focus on actionable items

### Scan Coverage

Trivy scans for:
- OS package vulnerabilities
- Language-specific package vulnerabilities
- Misconfigurations
- Secret leaks
- License compliance

## License Scanning

### Purpose

License scanning ensures that all dependencies comply with the project's license requirements and helps identify potential license conflicts.

### Implementation Details

The license scan workflow is configured in `.github/workflows/license-scan.yml` and includes:

- **Trigger**: Runs on pull requests and pushes to main
- **Allowed Licenses**: Apache-2.0, BSD variants, MIT, ISC, and others
- **Error Handling**: Continues on error due to known issues with deps.dev

## Workflow Integration

### CI/CD Integration

All security scans are integrated into the CI/CD pipeline:

1. **Pull Requests**: CodeQL and OSV-Scanner run on every PR
2. **Main Branch**: All scans run on pushes to main
3. **Scheduled**: Weekly scheduled scans for comprehensive coverage

### Security Alerts

Security findings are automatically:
- Displayed in pull requests
- Available in the Security tab
- Integrated with GitHub's security features

## Configuration

### Workflow Files

- `.github/workflows/codeql.yml` - CodeQL SAST scanning
- `.github/workflows/osv-scanner.yml` - OSV vulnerability scanning
- `.github/workflows/trivy.yml` - Container vulnerability scanning
- `.github/workflows/license-scan.yml` - License compliance scanning

### Customization

To customize the scanning:

1. **CodeQL**: Add custom queries in `.github/codeql/` directory
2. **OSV-Scanner**: Modify scan arguments in the workflow
3. **Trivy**: Adjust scan configuration and ignore rules
4. **License Scan**: Update allowed license list

## Best Practices

### For Developers

1. **Review Security Alerts**: Check security tab regularly
2. **Address Findings**: Fix vulnerabilities promptly
3. **Update Dependencies**: Keep dependencies up to date
4. **Follow Security Guidelines**: Adhere to security best practices

### For Maintainers

1. **Monitor Workflows**: Ensure scans are running successfully
2. **Review False Positives**: Configure ignore rules appropriately
3. **Update Tools**: Keep scanning tools up to date
4. **Document Changes**: Update this document when making changes

## Troubleshooting

### Common Issues

1. **Build Failures**: Check if Go modules are properly configured
2. **Timeout Issues**: Increase timeout for large codebases
3. **False Positives**: Configure appropriate ignore rules
4. **Permission Issues**: Verify workflow permissions

### Getting Help

- Check GitHub Actions logs for detailed error messages
- Review tool documentation for configuration options
- Contact the security team for complex issues

## References

- [CodeQL Documentation](https://codeql.github.com/)
- [OSV-Scanner Documentation](https://ossf.github.io/osv-scanner/)
- [Trivy Documentation](https://aquasecurity.github.io/trivy/)
- [GitHub Security Features](https://docs.github.com/en/code-security)
- [Envoy Gateway Security Implementation](https://github.com/envoyproxy/gateway) (Reference implementation) 