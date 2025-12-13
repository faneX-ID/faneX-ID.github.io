---
hide:
  - navigation
  - toc
---

<style>
  .hero-section {
    text-align: center;
    padding: 4rem 1rem;
    background: linear-gradient(135deg, #5c6bc0 0%, #3949ab 100%);
    color: white;
    border-radius: 1rem;
    margin-bottom: 2rem;
    box-shadow: 0 10px 30px rgba(0,0,0,0.15);
  }
  .hero-title {
    font-size: 3rem;
    font-weight: 800;
    margin-bottom: 1rem;
    line-height: 1.2;
    color: #ffffff !important;
  }
  .hero-subtitle {
    font-size: 1.5rem;
    opacity: 0.9;
    margin-bottom: 2.5rem;
    font-weight: 300;
    color: #e8eaf6 !important;
  }
  .hero-buttons {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
  }
  .btn {
    display: inline-block;
    padding: 0.8rem 2rem;
    border-radius: 50px;
    font-weight: 600;
    text-decoration: none !important;
    transition: transform 0.2s, box-shadow 0.2s;
  }
  .btn-primary {
    background: white;
    color: #3949ab !important;
  }
  .btn-secondary {
    background: rgba(255,255,255,0.2);
    color: white !important;
    backdrop-filter: blur(5px);
  }
  .btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
  }
  .stat-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin: 3rem 0;
  }
  .stat-card {
    background: var(--md-code-bg-color);
    padding: 1.5rem;
    border-radius: 0.5rem;
    text-align: center;
    border: 1px solid var(--md-default-fg-color--lightest);
  }
  .stat-value {
    font-size: 2rem;
    font-weight: bold;
    color: var(--md-primary-fg-color);
  }
  .stat-label {
    font-size: 0.9rem;
    color: var(--md-default-fg-color--light);
  }
</style>

<div class="hero-section">
  <h1 class="hero-title">faneX-ID</h1>
  <p class="hero-subtitle">The Next-Gen Identity Platform for the Modern Enterprise</p>
  <div class="hero-buttons">
    <a href="users/" class="btn btn-primary">Get Started</a>
    <a href="https://github.com/faneX-ID/core" class="btn btn-secondary">View on GitHub</a>
  </div>
</div>

<div class="stat-grid" id="github-stats">
  <!-- Dynamic content will be loaded here via JS -->
  <div class="stat-card">
    <div class="stat-value" id="latest-release">...</div>
    <div class="stat-label">Latest Stable</div>
  </div>
  <div class="stat-card">
    <div class="stat-value" id="latest-beta">...</div>
    <div class="stat-label">Latest Beta</div>
  </div>
    <div class="stat-card">
    <div class="stat-value" id="star-count">...</div>
    <div class="stat-label">GitHub Stars</div>
  </div>
</div>

<script>
  // Simple script to fetch GitHub releases
  // Note: This runs on the client side. Rate limits apply to unauthenticated requests.
  fetch("https://api.github.com/repos/faneX-ID/core/releases")
    .then(response => response.json())
    .then(releases => {
      const stable = releases.find(r => !r.prerelease);
      const beta = releases.find(r => r.prerelease);
      
      document.getElementById("latest-release").innerText = stable ? stable.tag_name : "v0.1.0";
      document.getElementById("latest-beta").innerText = beta ? beta.tag_name : "v0.2.0-beta";
    })
    .catch(e => {
       console.error("Failed to fetch releases", e);
       document.getElementById("latest-release").innerText = "v0.1.0";
       document.getElementById("latest-beta").innerText = "v0.2.0-beta";
    });

  fetch("https://api.github.com/repos/faneX-ID/core")
    .then(response => response.json())
    .then(repo => {
        document.getElementById("star-count").innerText = repo.stargazers_count ? repo.stargazers_count : "120+";
    })
     .catch(e => {
        document.getElementById("star-count").innerText = "120+";
    });
</script>

##  Why faneX-ID?

<div class="grid cards" markdown>

-   :material-shield-check: **Zero Trust Architecture**

    Identify every user and device. Enforce policies dynamically with **Passkeys** and **Adaptive MFA**.

-   :material-infinity: **Hybrid Mastery**

    Seamlessly bridge **Active Directory**, **LDAP**, and **Entra ID**. One identity, everywhere.

-   :material-api: **API-First Design**

    Everything is an API. Automate user provisioning, access requests, and compliance checks with ease.

-   :material-docker: **Cloud Native**

    Deploy anywhere. Docker, Kubernetes, or Bare Metal. Horizontally scalable by design.

</div>

<br>

##  The faneX Ecosystem

| Component | Status | Description |
| :--- | :--- | :--- |
| [**Core Platform**](https://github.com/faneX-ID/core) | ![Status](https://img.shields.io/badge/status-stable-green) | The heart of the system. |
| [**Standard Addons**](https://github.com/faneX-ID/integrations) | ![Status](https://img.shields.io/badge/status-active-blue) | Verified enterprise integrations. |
| [**User Docs**](users/) | ![Status](https://img.shields.io/badge/docs-comprehensive-green) | Guides for end-users and admins. |
| [**Dev Docs**](developers/) | ![Status](https://img.shields.io/badge/api-documented-blue) | Build your own plugins. |

<br>

##  Trusted Tech Stack

We use industry-standard technologies to ensure stability and ease of operation.

*   **Backend**: Python 3.12, FastAPI, SQLAlchemy
*   **Frontend**: React 18, TypeScript, Vite, TailwindCSS
*   **Database**: PostgreSQL
*   **Auth**: OAuth2, OIDC, WebAuthn (FIDO2)

---

<center>
<small>Copyright &copy; 2025 faneX-ID. Open Source MIT License.</small>
</center>

