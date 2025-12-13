# faneX-ID Documentation Site

[![Deploy](https://github.com/faneX-ID/faneX-ID.github.io/actions/workflows/deploy.yml/badge.svg)](https://github.com/faneX-ID/faneX-ID.github.io/actions/workflows/deploy.yml)
[![CI](https://github.com/faneX-ID/faneX-ID.github.io/actions/workflows/ci.yml/badge.svg)](https://github.com/faneX-ID/faneX-ID.github.io/actions/workflows/ci.yml)

Official documentation for [faneX-ID](https://faneX-ID.github.io) - Next-Generation Identity & Access Management.

Visit the live site: **[https://faneX-ID.github.io](https://faneX-ID.github.io)**

## 📚 About faneX-ID

**faneX-ID** is an enterprise-grade Identity & Access Management (IAM) platform built for modularity, security, and hybrid environments. It combines modern web technologies with robust legacy system integration capabilities.

### Key Features

- 🔐 **Zero Trust Architecture** with Passkeys, WebAuthn, and Adaptive MFA
- 🌐 **Hybrid Cloud Ready** - Bridge AD, LDAP, and Microsoft Entra ID
- 🚀 **API-First Design** for automation and integration
- 🔌 **Plugin Ecosystem** with official and community extensions
- 🏠 **Home Assistant Integration** for seamless deployment

## 🗂️ Repositories

- **Main Platform**: [faneX-ID/faneX-ID](https://github.com/faneX-ID/faneX-ID) - Core IAM platform
- **Official Integrations**: [faneX-ID/integrations](https://github.com/faneX-ID/integrations) - Enterprise integrations
- **Integration Examples**: [faneX-ID/integrations-example](https://github.com/faneX-ID/integrations-example) - Sample templates
- **Standard Workflows**: [faneX-ID/workflows](https://github.com/faneX-ID/workflows) - Automation templates
- **Workflow Examples**: [faneX-ID/workflows-example](https://github.com/faneX-ID/workflows-example) - Community patterns
- **Home Assistant Add-on**: [faneX-ID/homeassistant-addon](https://github.com/faneX-ID/homeassistant-addon) - HA integration
- **Integration Validation**: [faneX-ID/integration-validation](https://github.com/faneX-ID/integration-validation) - Quality tools

## 🛠️ Developing Documentation

This site is built with **MkDocs Material**.

### Prerequisites

```bash
pip install -r requirements.txt
```

### Local Preview

```bash
mkdocs serve
```

Then visit `http://127.0.0.1:8000` to preview the documentation locally.

### Building the Site

```bash
mkdocs build
```

The built site will be in the `site/` directory.

## 🤖 CI/CD & Automation

This repository uses GitHub Actions for automated deployment and quality checks:

- **Automatic Deployment**: Pushes to `main` automatically deploy to GitHub Pages
- **PR Validation**: Pull requests are validated with link checking and markdown linting
- **Dependabot**: Dependencies are automatically updated weekly

For detailed information, see the [CI/CD Infrastructure Documentation](.github/workflows/README.md).

## 📖 Documentation Structure

- **[User Guide](docs/users/)** - Getting started, dashboard, and security
- **[Developer Guide](docs/developers/)** - Architecture, integrations, and API reference

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the AGPL-3.0 license - see [LICENSE](https://github.com/faneX-ID/core/blob/main/LICENSE) for details.

---

**Built with ❤️ for the faneX-ID community**
