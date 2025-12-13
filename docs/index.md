---
hide:
  - navigation
  - toc
---

<style>
  .hero-section {
    text-align: center;
    padding: 5rem 2rem;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 50%, #f093fb 100%);
    color: white;
    border-radius: 1.5rem;
    margin-bottom: 3rem;
    box-shadow: 0 20px 60px rgba(102, 126, 234, 0.4);
    position: relative;
    overflow: hidden;
  }
  .hero-section::before {
    content: '';
    position: absolute;
    top: -50%;
    right: -50%;
    width: 200%;
    height: 200%;
    background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
    animation: pulse 15s ease-in-out infinite;
  }
  @keyframes pulse {
    0%, 100% { transform: scale(1) rotate(0deg); opacity: 0.3; }
    50% { transform: scale(1.1) rotate(180deg); opacity: 0.5; }
  }
  .hero-content {
    position: relative;
    z-index: 1;
  }
  .hero-title {
    font-size: 4rem;
    font-weight: 900;
    margin-bottom: 1rem;
    line-height: 1.1;
    color: #ffffff !important;
    text-shadow: 0 4px 20px rgba(0,0,0,0.3);
    letter-spacing: -0.02em;
  }
  .hero-subtitle {
    font-size: 1.75rem;
    opacity: 0.95;
    margin-bottom: 3rem;
    font-weight: 400;
    color: #f0f4ff !important;
    max-width: 700px;
    margin-left: auto;
    margin-right: auto;
  }
  .hero-buttons {
    display: flex;
    gap: 1.5rem;
    justify-content: center;
    flex-wrap: wrap;
  }
  .btn {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 1rem 2.5rem;
    border-radius: 50px;
    font-weight: 700;
    font-size: 1.1rem;
    text-decoration: none !important;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    box-shadow: 0 4px 14px rgba(0,0,0,0.15);
  }
  .btn-primary {
    background: linear-gradient(135deg, #ffffff 0%, #f0f4ff 100%);
    color: #667eea !important;
  }
  .btn-secondary {
    background: rgba(255,255,255,0.15);
    color: white !important;
    backdrop-filter: blur(10px);
    border: 2px solid rgba(255,255,255,0.3);
  }
  .btn-accent {
    background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    color: white !important;
  }
  .btn:hover {
    transform: translateY(-3px) scale(1.02);
    box-shadow: 0 8px 25px rgba(0,0,0,0.25);
  }
  .quick-links {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 2rem;
    margin: 3rem 0;
  }
  .quick-card {
    background: linear-gradient(135deg, var(--md-code-bg-color) 0%, var(--md-default-bg-color) 100%);
    padding: 2.5rem;
    border-radius: 1rem;
    text-align: center;
    border: 2px solid var(--md-default-fg-color--lightest);
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;
  }
  .quick-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 4px;
    background: linear-gradient(90deg, #667eea, #764ba2, #f093fb);
    transform: scaleX(0);
    transition: transform 0.3s ease;
  }
  .quick-card:hover::before {
    transform: scaleX(1);
  }
  .quick-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 40px rgba(102, 126, 234, 0.2);
    border-color: var(--md-primary-fg-color);
  }
  .quick-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
    display: block;
  }
  .quick-title {
    font-size: 1.5rem;
    font-weight: 700;
    margin-bottom: 0.75rem;
    color: var(--md-primary-fg-color);
  }
  .quick-desc {
    font-size: 1rem;
    color: var(--md-default-fg-color--light);
    margin-bottom: 1.5rem;
    line-height: 1.6;
  }
  .quick-link {
    display: inline-block;
    color: var(--md-primary-fg-color) !important;
    font-weight: 600;
    text-decoration: none !important;
    transition: all 0.2s ease;
  }
  .quick-link:hover {
    transform: translateX(5px);
  }
  .stat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 2rem;
    margin: 3rem 0;
  }
  .stat-card {
    background: linear-gradient(135deg, #667eea15 0%, #764ba215 100%);
    padding: 2rem;
    border-radius: 1rem;
    text-align: center;
    border: 2px solid var(--md-default-fg-color--lightest);
    transition: all 0.3s ease;
  }
  .stat-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 30px rgba(102, 126, 234, 0.15);
    border-color: var(--md-primary-fg-color);
  }
  .stat-value {
    font-size: 2.5rem;
    font-weight: 900;
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .stat-label {
    font-size: 1rem;
    color: var(--md-default-fg-color--light);
    margin-top: 0.5rem;
    font-weight: 600;
  }
  .repo-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1.5rem;
    margin: 2rem 0;
  }
  .repo-card {
    background: var(--md-code-bg-color);
    padding: 1.75rem;
    border-radius: 0.75rem;
    border: 1px solid var(--md-default-fg-color--lightest);
    transition: all 0.3s ease;
    display: flex;
    flex-direction: column;
  }
  .repo-card:hover {
    transform: translateY(-3px);
    box-shadow: 0 10px 25px rgba(0,0,0,0.1);
    border-color: var(--md-primary-fg-color);
  }
  .repo-name {
    font-size: 1.25rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
    color: var(--md-primary-fg-color);
  }
  .repo-desc {
    font-size: 0.95rem;
    color: var(--md-default-fg-color--light);
    flex-grow: 1;
    margin-bottom: 1rem;
    line-height: 1.5;
  }
  .repo-link {
    color: var(--md-primary-fg-color) !important;
    text-decoration: none !important;
    font-weight: 600;
    font-size: 0.9rem;
    transition: all 0.2s ease;
  }
  .repo-link:hover {
    text-decoration: underline !important;
  }
  .feature-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
    margin: 2rem 0;
  }
  .section-title {
    text-align: center;
    font-size: 2.5rem;
    font-weight: 800;
    margin: 3rem 0 2rem;
    background: linear-gradient(135deg, #667eea, #764ba2);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
</style>

<div class="hero-section">
  <div class="hero-content">
    <h1 class="hero-title">🚀 faneX-ID</h1>
    <p class="hero-subtitle">Next-Generation Identity & Access Management for Modern Enterprises</p>
    <div class="hero-buttons">
      <a href="users/" class="btn btn-primary">📖 User Guide</a>
      <a href="developers/" class="btn btn-accent">💻 Developer Docs</a>
      <a href="https://github.com/faneX-ID/faneX-ID" class="btn btn-secondary">⭐ GitHub</a>
    </div>
  </div>
</div>

<div class="stat-grid" id="github-stats">
  <div class="stat-card">
    <div class="stat-value" id="latest-release">...</div>
    <div class="stat-label">Latest Release</div>
  </div>
  <div class="stat-card">
    <div class="stat-value" id="latest-beta">...</div>
    <div class="stat-label">Beta Version</div>
  </div>
  <div class="stat-card">
    <div class="stat-value" id="star-count">...</div>
    <div class="stat-label">GitHub Stars</div>
  </div>
  <div class="stat-card">
    <div class="stat-value">8</div>
    <div class="stat-label">Active Repositories</div>
  </div>
</div>

<script>
  fetch("https://api.github.com/repos/faneX-ID/faneX-ID/releases")
    .then(response => response.json())
    .then(releases => {
      const stable = releases.find(r => !r.prerelease);
      const beta = releases.find(r => r.prerelease);
      document.getElementById("latest-release").innerText = stable ? stable.tag_name : "v1.0.0";
      document.getElementById("latest-beta").innerText = beta ? beta.tag_name : "v1.1.0-beta";
    })
    .catch(() => {
       document.getElementById("latest-release").innerText = "v1.0.0";
       document.getElementById("latest-beta").innerText = "v1.1.0-beta";
    });

  fetch("https://api.github.com/repos/faneX-ID/faneX-ID")
    .then(response => response.json())
    .then(repo => {
        document.getElementById("star-count").innerText = repo.stargazers_count || "0";
    })
    .catch(() => {
        document.getElementById("star-count").innerText = "0";
    });
</script>

## 🎯 Quick Access

<div class="quick-links">
  <div class="quick-card">
    <span class="quick-icon">👥</span>
    <h3 class="quick-title">For Users</h3>
    <p class="quick-desc">Get started with faneX-ID, manage your account, and learn about security features.</p>
    <a href="users/" class="quick-link">Explore User Guide →</a>
  </div>

  <div class="quick-card">
    <span class="quick-icon">🛠️</span>
    <h3 class="quick-title">For Developers</h3>
    <p class="quick-desc">Build integrations, explore the API, and contribute to the ecosystem.</p>
    <a href="developers/" class="quick-link">View Developer Docs →</a>
  </div>

  <div class="quick-card">
    <span class="quick-icon">🏠</span>
    <h3 class="quick-title">Home Assistant</h3>
    <p class="quick-desc">Deploy faneX-ID as a Home Assistant add-on for seamless integration.</p>
    <a href="https://github.com/faneX-ID/homeassistant-addon" class="quick-link">Get Add-on →</a>
  </div>
</div>

##  Why faneX-ID?

<div class="grid cards" markdown>

-   :material-shield-check: **Zero Trust Architecture**

    Identify every user and device. Enforce policies dynamically with **Passkeys**, **WebAuthn**, and **Adaptive MFA**.

-   :material-infinity: **Hybrid Cloud Ready**

    Seamlessly bridge **Active Directory**, **LDAP**, and **Microsoft Entra ID**. One identity, everywhere.

-   :material-api: **API-First Design**

    Everything is an API. Automate user provisioning, access requests, and compliance checks with ease.

-   :material-docker: **Cloud Native & Modular**

    Deploy anywhere: Docker, Kubernetes, or Bare Metal. Extend with custom integrations and workflows.

-   :material-puzzle: **Plugin Ecosystem**

    Official integrations, community extensions, and workflow automation out of the box.

-   :material-lock: **Enterprise Security**

    SAML 2.0, OAuth2, OIDC, RBAC, and comprehensive audit logging built-in.

</div>

<h2 class="section-title">🏗️ The faneX-ID Ecosystem</h2>

<div class="repo-grid">
  <div class="repo-card">
    <h3 class="repo-name">faneX-ID Core</h3>
    <p class="repo-desc">The main platform featuring full IAM capabilities, modern UI, and REST API.</p>
    <a href="https://github.com/faneX-ID/faneX-ID" class="repo-link">View Repository →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Official Integrations</h3>
    <p class="repo-desc">Verified enterprise integrations for LDAP, AD, Entra ID, SMTP, and more.</p>
    <a href="https://github.com/faneX-ID/integrations" class="repo-link">Browse Integrations →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Integration Examples</h3>
    <p class="repo-desc">Sample integrations and templates to kickstart your custom development.</p>
    <a href="https://github.com/faneX-ID/integrations-example" class="repo-link">View Examples →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Standard Workflows</h3>
    <p class="repo-desc">Pre-built workflow templates for common IAM automation scenarios.</p>
    <a href="https://github.com/faneX-ID/workflows" class="repo-link">Explore Workflows →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Workflow Examples</h3>
    <p class="repo-desc">Community workflow examples demonstrating alerting and automation patterns.</p>
    <a href="https://github.com/faneX-ID/workflows-example" class="repo-link">See Examples →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Home Assistant Add-on</h3>
    <p class="repo-desc">Official Home Assistant integration for easy deployment and configuration.</p>
    <a href="https://github.com/faneX-ID/homeassistant-addon" class="repo-link">Install Add-on →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Integration Validation</h3>
    <p class="repo-desc">Automated validation tools to ensure integration quality and compatibility.</p>
    <a href="https://github.com/faneX-ID/integration-validation" class="repo-link">View Tools →</a>
  </div>

  <div class="repo-card">
    <h3 class="repo-name">Documentation Site</h3>
    <p class="repo-desc">This documentation site built with MkDocs Material for comprehensive guides.</p>
    <a href="https://github.com/faneX-ID/faneX-ID.github.io" class="repo-link">Contribute →</a>
  </div>
</div>

## 💻 Technology Stack

<div class="grid cards" markdown>

-   **Backend Framework**

    Python 3.12 • FastAPI • SQLAlchemy • Pydantic

-   **Frontend Stack**

    React 18 • TypeScript • Vite • TailwindCSS

-   **Database & Cache**

    PostgreSQL • Redis • Alembic Migrations

-   **Authentication**

    OAuth2 • OIDC • SAML 2.0 • WebAuthn (FIDO2)

-   **Deployment**

    Docker • Kubernetes • Home Assistant Add-on

-   **Integrations**

    Python Modules • PowerShell Scripts • YAML Config

</div>

## 🚀 Getting Started

Ready to dive in? Choose your path:

<div class="hero-buttons" style="margin: 2rem 0;">
  <a href="users/" class="btn btn-primary">👤 I'm a User</a>
  <a href="developers/" class="btn btn-accent">👨‍💻 I'm a Developer</a>
  <a href="https://github.com/faneX-ID/homeassistant-addon" class="btn btn-secondary">🏠 Install Add-on</a>
</div>

---

<center>
<small>Copyright &copy; 2025 faneX-ID • Open Source [MIT License](https://github.com/faneX-ID/core/blob/main/LICENSE) • Built with ❤️ for the community</small>
</center>
