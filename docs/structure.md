# Documentation Structure

This page provides an overview of the faneX-ID documentation structure to help you navigate and find the information you need.

## 📚 Documentation Overview

### User Documentation

**Getting Started** (`users/getting-started.md`)
- First steps with faneX-ID
- Initial login and setup
- Understanding the dashboard

**Dashboard** (`users/dashboard.md`)
- Dashboard features
- Widgets and customization
- Quick actions

**Security** (`users/security.md`)
- Password management
- Two-factor authentication
- Passkey setup

**Configuration** (`users/configuration.md`)
- User settings
- Preferences
- Profile management

**Troubleshooting** (`users/troubleshooting.md`)
- Common issues
- Solutions
- Getting help

### IT Administrator Documentation

**Introduction** (`it/index.md`)
- IT administrator overview
- Responsibilities
- Key concepts

**Installation** (`it/installation.md`)
- System requirements
- Installation steps
- Initial configuration

**Deployment** (`it/deployment.md`)
- Deployment options
- Docker deployment
- Production setup

**SaaS & Hybrid Deployment** (`it/saas-deployment.md`)
- SaaS deployment models
- Hybrid architecture
- OnPrem connector setup
- Network configuration

**OnPrem Connector** (`it/onprem-connector.md`)
- Connector installation
- Configuration guide
- Troubleshooting

**Maintenance** (`it/maintenance.md`)
- System maintenance
- Updates and upgrades
- Backup procedures

**Security** (`it/security.md`)
- Security best practices
- Hardening guidelines
- Compliance

### Developer Documentation

**Introduction** (`developers/index.md`)
- Developer overview
- Getting started
- Development setup

**Architecture** (`developers/architecture.md`)
- System architecture
- Component overview
- Design patterns

**Repository Architecture** (`developers/repositories.md`)
- Repository structure
- Multi-repo setup
- Contribution guidelines

**Integrations** (`developers/integrations.md`)
- Integration development
- Integration store
- Best practices

**API Reference** (`developers/api.md`)
- REST API documentation
- Authentication
- Endpoints

**CI/CD Infrastructure** (`developers/cicd.md`)
- CI/CD pipelines
- Testing
- Deployment automation

## 📁 Directory Structure

```
docs/
├── assets/              # Images, logos, favicons
├── developers/          # Developer documentation
│   ├── index.md
│   ├── architecture.md
│   ├── repositories.md
│   ├── integrations.md
│   ├── api.md
│   └── cicd.md
├── it/                  # IT Administrator documentation
│   ├── index.md
│   ├── installation.md
│   ├── deployment.md
│   ├── saas-deployment.md
│   ├── onprem-connector.md
│   ├── maintenance.md
│   └── security.md
├── users/               # User documentation
│   ├── index.md
│   ├── getting-started.md
│   ├── dashboard.md
│   ├── security.md
│   ├── configuration.md
│   └── troubleshooting.md
├── downloads.md         # Download information
├── releases.md          # Release information
├── status.md            # System status
├── structure.md         # This file
└── index.md             # Homepage
```

## 🗺️ Navigation Guide

### For End Users

Start here:
1. [Getting Started](users/getting-started.md) - Learn the basics
2. [Dashboard](users/dashboard.md) - Understand the interface
3. [Security](users/security.md) - Secure your account
4. [Configuration](users/configuration.md) - Customize settings

### For IT Administrators

Start here:
1. [Introduction](it/index.md) - Understand your role
2. [Installation](it/installation.md) - Install faneX-ID
3. [Deployment](it/deployment.md) - Deploy to production
4. [SaaS & Hybrid Deployment](it/saas-deployment.md) - Set up hybrid deployments
5. [Security](it/security.md) - Secure your deployment

### For Developers

Start here:
1. [Introduction](developers/index.md) - Developer overview
2. [Architecture](developers/architecture.md) - Understand the system
3. [Integrations](developers/integrations.md) - Build integrations
4. [API Reference](developers/api.md) - Use the API

## 🔍 Quick Search

### Common Topics

**Installation & Setup**
- [Installation Guide](it/installation.md)
- [Docker Setup](it/deployment.md#docker-compose-production-setup)
- [OnPrem Connector](it/onprem-connector.md)

**Deployment Models**
- [On-Premises](it/deployment.md)
- [Cloud/SaaS](it/saas-deployment.md#internet-mode)
- [Hybrid](it/saas-deployment.md#hybrid-deployment-architecture)

**Security**
- [User Security](users/security.md)
- [Admin Security](it/security.md)
- [API Tokens](it/saas-deployment.md#step-2-generate-api-token)

**Development**
- [Integration Development](developers/integrations.md)
- [API Documentation](developers/api.md)
- [Architecture](developers/architecture.md)

**Troubleshooting**
- [User Issues](users/troubleshooting.md)
- [Connector Issues](it/saas-deployment.md#troubleshooting)
- [System Issues](it/maintenance.md)

## 📖 Documentation Features

- **Search**: Use the search bar to find specific topics
- **Navigation**: Use the sidebar to browse by category
- **Code Examples**: All code examples are syntax-highlighted
- **Interactive**: Try the live demo links where available
- **Versioned**: Documentation is versioned with releases

## 🤝 Contributing

Found an error or want to improve the documentation?

1. Navigate to the page
2. Click "Edit on GitHub" (top right)
3. Make your changes
4. Submit a pull request

## 📞 Support

- **Documentation Issues**: [GitHub Issues](https://github.com/faneX-ID/faneX-ID.github.io/issues)
- **General Questions**: [GitHub Discussions](https://github.com/faneX-ID/core/discussions)
- **Bug Reports**: [GitHub Issues](https://github.com/faneX-ID/core/issues)
