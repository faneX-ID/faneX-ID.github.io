# CI/CD Infrastructure Documentation

This document describes the CI/CD infrastructure and automation workflows used in the faneX-ID project ecosystem.

## Overview

The faneX-ID project uses GitHub Actions for continuous integration and deployment across all repositories. The CI/CD infrastructure ensures code quality, automated testing, and seamless deployment.

## Core Workflows

### 1. PR Assistant Workflow

**Location**: `.github/workflows/pr-assistant.yml`

The PR Assistant workflow provides automated feedback and validation for pull requests:

- **Manifest Validation**: Validates integration and workflow manifests against the current schema
- **Code Quality Checks**: Runs linting and code style checks
- **Quality Score Validation**: Ensures quality scores are automatically calculated and not manually edited
- **Schema Compliance**: Verifies all manifests comply with the latest schema version from `faneX-ID/core`

**Trigger**: Pull request events (opened, synchronize, reopened)

### 2. PR Validation Workflow

**Location**: `.github/workflows/pr-validation.yml`

Comprehensive validation workflow that runs before PRs can be merged:

- **Integration Validation**: Validates integration structure and files
- **Workflow Validation**: Validates workflow definitions
- **Schema Validation**: Ensures compliance with manifest schemas
- **Version Compatibility**: Checks minimum core version requirements

**Trigger**: Pull request events

### 3. PR Autofix Workflow

**Location**: `.github/workflows/pr-autofix.yml`

Automatically fixes common formatting and style issues:

- **Code Formatting**: Auto-formats code using project standards
- **YAML Formatting**: Formats YAML files consistently
- **Manifest Formatting**: Ensures manifest files are properly formatted

**Trigger**: Pull request events

### 4. PR Thanks Workflow

**Location**: `.github/workflows/pr-thanks.yml`

Automated thank-you messages for contributors:

- **Welcome Messages**: Greets new contributors
- **Thank You Comments**: Appreciates contributions after merge
- **Community Engagement**: Encourages continued participation

**Trigger**: Pull request events

### 5. Housekeeping Workflow

**Location**: `.github/workflows/housekeeping.yml`

Maintenance and cleanup tasks:

- **Dependency Updates**: Checks for outdated dependencies
- **Security Scanning**: Scans for known vulnerabilities
- **Code Quality Monitoring**: Tracks code quality metrics
- **Documentation Updates**: Ensures documentation is up to date

**Trigger**: Scheduled (daily) and manual dispatch

### 6. Calculate Quality Workflow

**Location**: `.github/workflows/calculate-quality.yml` (integrations repositories)

Automatically calculates quality scores for integrations:

- **Code Analysis**: Analyzes code quality metrics
- **Documentation Review**: Checks documentation completeness
- **Test Coverage**: Evaluates test coverage
- **Quality Score Generation**: Generates `quality.json` files

**Trigger**: Push to main, pull request events

### 7. Integration Request Handler

**Location**: `.github/workflows/integration-request-handler.yml` (integrations repositories)

Automatically generates integration files from feature requests:

- **Issue Analysis**: Parses integration request issues
- **File Generation**: Generates manifest, code, and README files
- **PR Creation**: Creates pull requests with generated files
- **Issue Linking**: Links PRs back to original issues

**Trigger**: Issue events (new integration requests)

## Repository-Specific Workflows

### Documentation Site (`faneX-ID.github.io`)

- **Deploy Workflow**: Automatically deploys documentation to GitHub Pages
- **Fetch Releases Workflow**: Fetches release information from all repositories
- **CI Workflow**: Validates markdown and checks links

### Home Assistant Add-on (`homeassistant-addon`)

- **Build Add-on Workflow**: Builds Docker images for multiple architectures
- **YAML Validation**: Validates add-on configuration files
- **Dockerfile Validation**: Ensures Dockerfile correctness

### Intune Packaging (`Intune-packaging`)

- **PowerShell Validation**: Validates PowerShell scripts
- **XML Validation**: Validates Intune package XML files
- **Package Building**: Builds Intune packages

## Version Synchronization

The faneX-ID project uses a centralized version management system:

- **Source of Truth**: `faneX-ID/core/.github/versions.json`
- **HTTP-Based Sync**: Dependent repositories fetch version information via HTTP
- **Schema Base URL**: All schemas reference `faneX-ID/core` as the base URL
- **Automatic Updates**: Workflows automatically fetch the latest version information

## Bot Integration

The `faneX-ID/github-bot` provides automated assistance:

- **Workflow Retry**: Retries failed workflows on command
- **PR Management**: Provides status updates and feedback
- **Command Processing**: Responds to commands in PR comments
- **Status Reports**: Generates CI/CD status summaries

## Best Practices

1. **Always validate manifests** before committing
2. **Use PR templates** for consistent PR information
3. **Follow semantic versioning** for releases
4. **Keep dependencies updated** via housekeeping workflow
5. **Document changes** in PR descriptions
6. **Test locally** before pushing changes

## Troubleshooting

### Workflow Failures

- Check workflow logs in the Actions tab
- Verify required secrets are configured
- Ensure manifest schemas are up to date
- Check version compatibility requirements

### Version Sync Issues

- Verify `versions.json` exists in `faneX-ID/core`
- Check HTTP access to the raw content URL
- Ensure repository has access to private `core` repository (if needed)

### Quality Score Issues

- Ensure `quality.json` is not manually edited
- Run the calculate-quality workflow manually if needed
- Check that all required files exist (README, code, tests)

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [faneX-ID Repository Architecture](repositories.md)
- [Integration Development Guide](https://github.com/faneX-ID/core/blob/main/docs/INTEGRATION_DEV_GUIDE.md)


