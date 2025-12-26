---
hide:
  - navigation
  - toc
---

<div class="premium-hero">
  <div class="hero-glow" style="top: 10%; left: 20%;"></div>
  <div class="hero-glow" style="bottom: 10%; right: 20%; animation-delay: -5s;"></div>

  <div class="reveal">
    <h1>🚀 faneX-ID</h1>
    <p>Identity & Access Management, reimagined for the modern cloud. Secure, scalable, and open source.</p>

    <div style="display: flex; gap: 1rem; justify-content: center; flex-wrap: wrap;">
      <a href="https://fanex-id.fabiseitz.de" target="_blank" class="premium-btn btn-white">
        🎮 Try Demo
      </a>
      <a href="downloads/" class="premium-btn btn-outline">
        📥 Downloads
      </a>
      <a href="https://github.com/faneX-ID/core" target="_blank" class="premium-btn btn-outline">
        ⭐ Star on GitHub
      </a>
    </div>
  </div>
</div>

<div class="modern-grid reveal">
  <div class="modern-card">
    <span class="card-icon">👥</span>
    <h3 class="card-title">User Guide</h3>
    <p class="card-text">Learn how to manage your identity, set up MFA, and secure your account with faneX-ID.</p>
    <a href="users/" style="margin-top: 1rem; display: block; font-weight: 700;">Explore →</a>
  </div>

  <div class="modern-card">
    <span class="card-icon">💻</span>
    <h3 class="card-title">Developers</h3>
    <p class="card-text">Build custom integrations, explore our REST API, and contribute to the core platform.</p>
    <a href="developers/" style="margin-top: 1rem; display: block; font-weight: 700;">Build →</a>
  </div>

  <div class="modern-card">
    <span class="card-icon">⚙️</span>
    <h3 class="card-title">IT Admins</h3>
    <p class="card-text">Deployment guides for Docker, Kubernetes, and Hybrid Cloud environments.</p>
    <a href="it/" style="margin-top: 1rem; display: block; font-weight: 700;">Deploy →</a>
  </div>
</div>

<h2 style="text-align: center; margin-bottom: 3rem;" class="reveal">Built for the Enterprise</h2>

<div class="grid cards" markdown>

-   :material-shield-lock-outline:{ .lg .middle } __Zero Trust Core__

    Everything is verified. Secure your workforce with modern standards like WebAuthn and Passkeys.

-   :material-cloud-sync-outline:{ .lg .middle } __Hybrid Identity__

    Connect legacy Active Directory with modern Cloud Identity providers in minutes.

-   :material-robot-outline:{ .lg .middle } __Workflow Automation__

    Automate access requests and identity lifecycle events with our powerful workflow engine.

-   :material-api:{ .lg .middle } __API-First Architecture__

    Integrate faneX-ID into your existing stack with our developer-first RESTful APIs.

</div>

<div class="reveal" style="background: var(--px-primary-gradient); padding: 4rem; border-radius: 2rem; color: white; text-align: center; margin: 4rem 0;">
  <h2>Experience the Future of Identity</h2>
  <p style="margin-bottom: 2rem;">Stop choosing between security and user experience. Get both with faneX-ID.</p>
  <a href="releases/" class="premium-btn btn-white">🚀 Get Started Now</a>
</div>

<div class="stat-grid reveal" style="background: var(--md-default-bg-color--light); padding: 3rem; border-radius: 20px;">
  <div class="stat-item">
    <div class="stat-value">8+</div>
    <div class="stat-label">Core Projects</div>
  </div>
  <div class="stat-item" style="border-left: 1px solid var(--md-default-fg-color--lightest);">
    <div class="stat-value">100%</div>
    <div class="stat-label">Open Source</div>
  </div>
  <div class="stat-item" style="border-left: 1px solid var(--md-default-fg-color--lightest);">
    <div class="stat-value">60s</div>
    <div class="stat-label">Fast Deployment</div>
  </div>
</div>

<script>
  window.addEventListener('load', () => {
    ScrollReveal().reveal('.reveal', {
      distance: '50px',
      duration: 1000,
      interval: 200,
      origin: 'bottom',
      easing: 'cubic-bezier(0.5, 0, 0, 1)'
    });
  });
</script>
