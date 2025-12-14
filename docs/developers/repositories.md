# faneX-ID Repository Architecture

faneX-ID consists of multiple interconnected repositories that work together to provide a complete identity management ecosystem. This document explains how each repository fits into the overall architecture and how they interact with each other.

## Overview

The faneX-ID ecosystem is built on a **hub-and-spoke architecture** where `faneX-ID/core` serves as the central hub (single source of truth) and all other repositories connect to it for schema definitions, version information, and standards.

## Core Repository

### `faneX-ID/core` - **The Foundation**

**Status:** 🔒 Private (will become public)
**Role:** Single Source of Truth

The main faneX-ID application repository containing the complete codebase and all standards.

#### Key Responsibilities

1. **Schema Definitions**
   - Integration manifest schemas (`integration-manifest.json`)
   - Workflow manifest schemas (`workflow-manifest.json`)
   - Quality score schemas (`quality-score.json`)
   - All schemas are versioned and backward-compatible

2. **Version Management**
   - Central `versions.json` file defining:
     - `manifest_schema_version`: Current integration manifest schema version
     - `workflow_schema_version`: Current workflow manifest schema version
     - `required_manifest_version`: Minimum required version for compatibility
     - `min_core_version`: Minimum faneX-ID core version required
   - All dependent repositories fetch this file via HTTP

3. **System Integrations**
   - Built-in integrations (GitHub, Mail, Active Directory, Microsoft Entra ID, etc.)
   - These are part of the core application and not installable separately

4. **Documentation & Specifications**
   - Core documentation
   - API specifications
   - Integration development guides
   - Workflow examples

#### Access Pattern

Even though `core` is private, public repositories can access:
- **Public Raw Content:** `https://raw.githubusercontent.com/faneX-ID/core/main/.github/versions.json`
- **Schema Files:** Via public raw content URLs
- **Releases:** Via GitHub API (when repository becomes public)

## Integration Repositories

### `faneX-ID/integrations` - **Official Integration Store**

**Purpose:** Production-ready, community-contributed integrations

**Contents:**
- Community-developed integrations (Slack, Microsoft Teams, Jira, Generic Webhook, Calendar Sync, etc.)
- Each integration includes:
  - `manifest.json`: Integration metadata and configuration schema
  - `integration.py`: Python implementation
  - `README.md`: Documentation
  - `quality.json`: Automatically calculated quality score

**Features:**
- **Quality Scoring:** Automatic calculation of integration quality (Bronze, Silver, Gold, Platinum)
- **Schema Validation:** PR Assistant validates all manifests against current schema from `core`
- **Version Sync:** Automatically fetches version requirements from `core`
- **Installation:** Integrations can be installed directly from the faneX-ID UI

**Workflow:**
1. Developer creates integration following schema from `core`
2. PR Assistant validates against current schema version
3. Quality score is automatically calculated
4. Integration is merged and available in the store

### `faneX-ID/integrations-example` - **Developer Learning Resource**

**Purpose:** Examples and templates for new integration developers

**Contents:**
- Step-by-step integration examples
- Template integrations with detailed comments
- Best practices and patterns
- Common integration patterns

**Target Audience:**
- Developers new to faneX-ID integration development
- Developers learning the manifest schema
- Developers looking for implementation examples

### `faneX-ID/integration-validation` - **Reusable Validation Action**

**Purpose:** GitHub Action for validating integration manifests

**Features:**
- Validates `manifest.json` files against schema from `core`
- Checks for required fields
- Validates data types and formats
- Can be used in any repository hosting integrations

**Usage:**
```yaml
- uses: faneX-ID/integration-validation@main
  with:
    manifest-path: 'my-integration/manifest.json'
    schema-version: '2.0.0'
```

## Workflow Repositories

### `faneX-ID/workflows` - **Official Workflow Store**

**Purpose:** Pre-built workflows for common automation scenarios

**Contents:**
- Community-contributed workflows
- Workflow manifests following the official schema
- Documentation and usage examples
- Multi-step automation workflows

**Features:**
- **Workflow Composition:** Combine multiple integrations
- **Conditional Logic:** Support for if/else conditions
- **Error Handling:** Built-in error handling and retry logic
- **Schema Validation:** Validates against workflow schema from `core`

**Example Use Cases:**
- Employee onboarding workflows
- Account lifecycle management
- Multi-system synchronization
- Automated provisioning and deprovisioning

### `faneX-ID/workflows-example` - **Workflow Examples**

**Purpose:** Example workflows demonstrating faneX-ID's capabilities

**Contents:**
- Complex workflow examples
- Best practices for workflow design
- Integration chain examples
- Error handling patterns
- Conditional logic examples

## Infrastructure & Tooling

### `faneX-ID/github-bot` - **Automation Bot**

**Purpose:** Automated PR and workflow management

**Features:**
- **Workflow Retry:** Retry failed workflows via PR comments
- **PR Feedback:** Provides status updates on pull requests
- **Cross-Repository Support:** Works across all faneX-ID repositories
- **Admin Commands:** Supports commands from repository admins

**Usage:**
- Automatically runs on PR events
- Responds to comments like: `@faneX-ID-bot retry workflow build`
- Provides status updates and summaries

**Configuration:**
- Configured via `config.yaml` in the repository
- Supports retryable workflows per repository
- Recognizes admins and repository owners

### `faneX-ID/homeassistant-addon` - **Home Assistant Integration**

**Purpose:** Official Home Assistant add-on for running faneX-ID

**Features:**
- One-click installation in Home Assistant
- Automatic updates
- Ingress support (access via HA sidebar)
- Configuration via Home Assistant UI
- Supports all Home Assistant architectures (amd64, arm64, armv7)

**How it Works:**
- Downloads and builds faneX-ID from `core` repository
- Uses GitHub token for private repository access
- Runs as a Home Assistant add-on container
- Provides web interface via Ingress

**Target Audience:**
- Home Assistant users
- Users wanting easy deployment
- Users preferring add-on management over Docker Compose

### `faneX-ID/faneX-ID.github.io` - **Documentation Website**

**Purpose:** Official documentation and website

**Features:**
- **Comprehensive Documentation:**
  - User guides
  - Developer guides
  - API reference
  - Integration development guide
  - Workflow examples

- **Release Information:**
  - Automatically fetches releases from all repositories
  - Displays stable and pre-releases
  - Shows version information from `core`
  - Updates every 6 hours or on new releases

- **Build Status:**
  - Displays build status from `core`
  - Shows workflow run information
  - Version compatibility information

**Technology:**
- Built with MkDocs Material
- Deployed via GitHub Pages
- Automatically updates on documentation changes

## Repository Interaction Flow

### Version Synchronization

```
┌─────────────────────────────────────────┐
│     faneX-ID/core (Private)            │
│                                         │
│  .github/versions.json                 │
│  ├── manifest_schema_version: "2.0.0"   │
│  ├── workflow_schema_version: "2.0.0"  │
│  └── min_core_version: "2025.12.0"     │
│                                         │
│  docs/specs/integrations-manifest/     │
│  └── schemas/                          │
│      ├── integration-manifest.json     │
│      ├── workflow-manifest.json        │
│      └── quality-score.json            │
└───────────────┬─────────────────────────┘
                │
                │ HTTP Fetch (Public Raw Content)
                │
    ┌───────────┼───────────┐
    │           │           │
    ▼           ▼           ▼
┌─────────┐ ┌─────────┐ ┌─────────┐
│integrat-│ │workflows│ │github-  │
│ions     │ │         │ │bot      │
│         │ │         │ │         │
│• PR     │ │• PR     │ │• Manages│
│  Assistant│  Assistant│   PRs   │
│  fetches│ │  fetches│ │         │
│  versions│ │  versions│ │         │
│• Validates│ • Validates│ │         │
│  against │ │  against │ │         │
│  schema  │ │  schema  │ │         │
└─────────┘ └─────────┘ └─────────┘
```

### PR Validation Flow

1. **Developer creates PR** in `integrations` or `workflows`
2. **PR Assistant workflow triggers:**
   - Fetches `versions.json` from `core` via HTTP
   - Downloads current schema from `core`
   - Validates manifest against schema
   - Checks quality requirements
3. **Feedback provided:**
   - Success: PR approved for review
   - Failure: Detailed error messages with fixes

### Release Flow

1. **Release published** in any repository
2. **Documentation site workflow triggers:**
   - Fetches release information from all repositories
   - Updates `docs/.data/releases.json`
   - Website automatically displays new releases
3. **Version information:**
   - Fetches `versions.json` from `core`
   - Displays on status page
   - Shows compatibility information

## Repository Dependencies

### Direct Dependencies

| Repository | Depends On | Dependency Type |
|------------|-----------|-----------------|
| `integrations` | `core` | Schema versions, validation |
| `integrations-example` | `core` | Schema versions, examples |
| `workflows` | `core` | Schema versions, validation |
| `workflows-example` | `core` | Schema versions, examples |
| `integration-validation` | `core` | Schema definitions |
| `homeassistant-addon` | `core` | Source code, releases |
| `faneX-ID.github.io` | All repos | Release information, docs |
| `github-bot` | All repos | PR management, workflow retry |

### Dependency Management

- **No Circular Dependencies:** All repositories depend on `core`, but `core` doesn't depend on others
- **HTTP-Based:** Public repositories fetch information via HTTP (no Git submodules)
- **Version Pinning:** Each repository can specify minimum required versions
- **Backward Compatibility:** Schema versions maintain backward compatibility

## Best Practices

### For Integration Developers

1. **Always fetch latest schema** from `core` before creating integration
2. **Follow manifest schema** exactly as defined
3. **Include comprehensive README** with usage examples
4. **Test integration** before submitting PR
5. **Let quality score calculate automatically** (don't manually edit)

### For Workflow Developers

1. **Use official integrations** from `integrations` repository
2. **Follow workflow schema** from `core`
3. **Include error handling** in all workflows
4. **Document workflow purpose** and requirements
5. **Test with sample data** before publishing

### For Repository Maintainers

1. **Keep schemas in sync** with `core`
2. **Update version requirements** when `core` updates
3. **Monitor PR Assistant** for validation failures
4. **Maintain quality standards** for merged content
5. **Keep documentation updated**

## Repository Status

| Repository | Status | Public | Purpose |
|------------|--------|--------|---------|
| `core` | 🔒 Private | No | Main application, schemas, standards |
| `integrations` | ✅ Active | Yes | Production integrations |
| `integrations-example` | ✅ Active | Yes | Integration examples |
| `integration-validation` | ✅ Active | Yes | Validation GitHub Action |
| `workflows` | ✅ Active | Yes | Production workflows |
| `workflows-example` | ✅ Active | Yes | Workflow examples |
| `github-bot` | ✅ Active | Yes | Automation bot |
| `homeassistant-addon` | ✅ Active | Yes | Home Assistant add-on |
| `faneX-ID.github.io` | ✅ Active | Yes | Documentation website |

## Getting Started

### For Users

1. **Install faneX-ID:**
   - Via Docker Compose (from `core`)
   - Via Home Assistant Add-on (from `homeassistant-addon`)

2. **Browse Integrations:**
   - Visit `integrations` repository
   - Install via faneX-ID UI

3. **Browse Workflows:**
   - Visit `workflows` repository
   - Import and customize workflows

### For Developers

1. **Read Documentation:**
   - Start with `faneX-ID.github.io`
   - Review integration development guide

2. **Study Examples:**
   - Check `integrations-example` for integration patterns
   - Check `workflows-example` for workflow patterns

3. **Create Integration:**
   - Follow schema from `core`
   - Submit PR to `integrations`

4. **Create Workflow:**
   - Use integrations from `integrations`
   - Submit PR to `workflows`

## Links

- **All Repositories:** [faneX-ID Organization](https://github.com/orgs/faneX-ID/repositories)
- **Documentation:** [faneX-ID.github.io](https://faneX-ID.github.io/)
- **Core Repository:** [faneX-ID/core](https://github.com/faneX-ID/core) (Private)


