---
hide:
  - toc
---

# 📦 Releases

Comprehensive release information from all faneX-ID projects, including both **stable** and **pre-releases**, with detailed version information, dates, and download links.

!!! tip "Quick Downloads"
    Looking for direct download links organized by platform? Check out the [Downloads page](downloads.md) for all available platform downloads.

<style>
  .release-filter {
    display: flex;
    gap: 1rem;
    margin: 2rem 0;
    padding: 1rem;
    background: var(--md-default-bg-color--light);
    border-radius: 8px;
    flex-wrap: wrap;
  }
  .release-filter label {
    display: flex;
    align-items: center;
    cursor: pointer;
    font-weight: 500;
  }
  .release-filter input[type="radio"] {
    margin-right: 0.5rem;
  }
  .release-card {
    border: 1px solid var(--md-default-fg-color--lightest);
    border-radius: 12px;
    padding: 1.5rem;
    margin: 1.5rem 0;
    background: var(--md-default-bg-color);
    box-shadow: 0 2px 8px rgba(0,0,0,0.05);
    transition: all 0.3s ease;
  }
  .release-card:hover {
    box-shadow: 0 4px 16px rgba(0,0,0,0.1);
    transform: translateY(-2px);
  }
  .release-badge {
    display: inline-block;
    padding: 4px 12px;
    border-radius: 12px;
    font-size: 0.75rem;
    font-weight: 700;
    text-transform: uppercase;
    margin-right: 8px;
  }
  .badge-stable {
    background: #4caf50;
    color: white;
  }
  .badge-prerelease {
    background: #ff9800;
    color: white;
  }
  .release-header {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
    flex-wrap: wrap;
    gap: 0.5rem;
  }
  .release-title {
    font-size: 1.5rem;
    font-weight: 700;
    margin: 0;
    color: var(--md-primary-fg-color);
  }
  .release-meta {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
    margin: 1rem 0;
    padding: 1rem;
    background: var(--md-default-bg-color--light);
    border-radius: 8px;
  }
  .release-meta-item {
    display: flex;
    flex-direction: column;
  }
  .release-meta-label {
    font-size: 0.75rem;
    color: var(--md-default-fg-color--light);
    text-transform: uppercase;
    font-weight: 600;
    margin-bottom: 0.25rem;
  }
  .release-meta-value {
    font-size: 1rem;
    font-weight: 600;
    color: var(--md-default-fg-color);
  }
  .download-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    margin-top: 1rem;
  }
  .download-btn {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.75rem 1.25rem;
    background: var(--md-primary-fg-color);
    color: var(--md-primary-bg-color);
    border-radius: 8px;
    text-decoration: none;
    font-weight: 600;
    font-size: 0.9rem;
    transition: all 0.2s ease;
    border: none;
    cursor: pointer;
  }
  .download-btn:hover {
    background: var(--md-accent-fg-color);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  }
  .download-btn:active {
    transform: translateY(0);
  }
  .no-releases {
    text-align: center;
    padding: 3rem;
    color: var(--md-default-fg-color--light);
  }
  .loading {
    text-align: center;
    padding: 3rem;
    color: var(--md-default-fg-color--light);
  }
  .summary-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem;
    margin: 2rem 0;
  }
  .stat-card {
    background: var(--md-default-bg-color--light);
    padding: 1.5rem;
    border-radius: 8px;
    text-align: center;
  }
  .stat-value {
    font-size: 2rem;
    font-weight: 700;
    color: var(--md-primary-fg-color);
  }
  .stat-label {
    font-size: 0.875rem;
    color: var(--md-default-fg-color--light);
    margin-top: 0.5rem;
  }
</style>

<div class="release-filter">
  <label>
    <input type="radio" name="release-filter" value="all" checked onchange="filterReleases()">
    All Releases
  </label>
  <label>
    <input type="radio" name="release-filter" value="stable" onchange="filterReleases()">
    Stable Only
  </label>
  <label>
    <input type="radio" name="release-filter" value="prerelease" onchange="filterReleases()">
    Pre-Releases Only
  </label>
</div>

<div id="summary-stats" class="summary-stats" style="display: none;">
  <div class="stat-card">
    <div class="stat-value" id="total-stable">0</div>
    <div class="stat-label">Stable Releases</div>
  </div>
  <div class="stat-card">
    <div class="stat-value" id="total-prereleases">0</div>
    <div class="stat-label">Pre-Releases</div>
  </div>
  <div class="stat-card">
    <div class="stat-value" id="total-repos">0</div>
    <div class="stat-label">Repositories</div>
  </div>
</div>

<div id="releases-container">
  <div class="loading">Loading releases...</div>
</div>

<script>
let releasesData = null;

// Fetch and display releases
fetch('/docs/data/releases.json')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => {
    releasesData = data;
    renderReleases('all');
    updateSummaryStats();
  })
  .catch(error => {
    console.error('Error loading releases:', error);
    document.getElementById('releases-container').innerHTML = `
      <div class="no-releases">
        <p style="color: var(--md-typeset-a-color); font-weight: 600;">⚠️ Error loading releases</p>
        <p style="color: var(--md-default-fg-color--light);">${error.message}</p>
        <p style="margin-top: 1rem; font-size: 0.9rem;">The releases data may not be available yet. Please check back later.</p>
      </div>
    `;
  });

function filterReleases() {
  const filter = document.querySelector('input[name="release-filter"]:checked').value;
  renderReleases(filter);
}

function updateSummaryStats() {
  if (!releasesData) return;

  let totalStable = 0;
  let totalPrereleases = 0;
  let reposWithReleases = 0;

  Object.values(releasesData.repositories).forEach(repoData => {
    if (repoData && repoData.total_stable) {
      totalStable += repoData.total_stable;
      reposWithReleases++;
    }
    if (repoData && repoData.total_prereleases) {
      totalPrereleases += repoData.total_prereleases;
    }
  });

  document.getElementById('total-stable').textContent = totalStable;
  document.getElementById('total-prereleases').textContent = totalPrereleases;
  document.getElementById('total-repos').textContent = reposWithReleases;
  document.getElementById('summary-stats').style.display = 'grid';
}

function renderReleases(filter) {
  if (!releasesData) return;

  const container = document.getElementById('releases-container');
  container.innerHTML = '';

  const lastUpdated = new Date(releasesData.last_updated).toLocaleString();
  container.innerHTML += `<p style="text-align: center; color: var(--md-default-fg-color--light); margin-bottom: 2rem;"><em>Last updated: ${lastUpdated}</em></p>`;

  const repos = Object.entries(releasesData.repositories);
  let hasReleases = false;

  repos.forEach(([repoName, repoData]) => {
    if (!repoData) return;

    // Skip if no releases match filter
    if (filter === 'stable' && !repoData.latest_stable) return;
    if (filter === 'prerelease' && !repoData.latest_prerelease) return;
    if (filter === 'all' && !repoData.latest_stable && !repoData.latest_prerelease) return;

    hasReleases = true;

    const card = document.createElement('div');
    card.className = 'release-card';

    const header = document.createElement('div');
    header.className = 'release-header';

    const title = document.createElement('h3');
    title.className = 'release-title';
    title.textContent = repoName;
    header.appendChild(title);

    card.appendChild(header);

    // Display stable releases
    if (repoData.latest_stable && (filter === 'all' || filter === 'stable')) {
      const stableSection = document.createElement('div');
      stableSection.style.marginBottom = '1.5rem';

      const stableHeader = document.createElement('div');
      stableHeader.className = 'release-header';
      stableHeader.innerHTML = `
        <span class="release-badge badge-stable">STABLE</span>
        <strong>Latest Stable Release</strong>
      `;
      stableSection.appendChild(stableHeader);

      const meta = document.createElement('div');
      meta.className = 'release-meta';
      meta.innerHTML = `
        <div class="release-meta-item">
          <div class="release-meta-label">Version</div>
          <div class="release-meta-value">
            <a href="${repoData.latest_stable.url}" target="_blank" style="color: var(--md-typeset-a-color);">
              ${repoData.latest_stable.tag}
            </a>
          </div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Published</div>
          <div class="release-meta-value">${new Date(repoData.latest_stable.published_at).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' })}</div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Time</div>
          <div class="release-meta-value">${new Date(repoData.latest_stable.published_at).toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' })}</div>
        </div>
      `;
      stableSection.appendChild(meta);

      if (repoData.latest_stable.assets && repoData.latest_stable.assets.length > 0) {
        const downloads = document.createElement('div');
        downloads.className = 'download-buttons';
        repoData.latest_stable.assets.forEach(asset => {
          const btn = document.createElement('a');
          btn.href = asset.download_url;
          btn.target = '_blank';
          btn.className = 'download-btn';
          btn.innerHTML = `📥 ${asset.name} <span style="opacity: 0.8; font-size: 0.85rem;">(${(asset.size / 1024 / 1024).toFixed(2)} MB)</span>`;
          downloads.appendChild(btn);
        });
        stableSection.appendChild(downloads);
      }

      card.appendChild(stableSection);
    }

    // Display pre-releases
    if (repoData.latest_prerelease && (filter === 'all' || filter === 'prerelease')) {
      const prereleaseSection = document.createElement('div');

      const prereleaseHeader = document.createElement('div');
      prereleaseHeader.className = 'release-header';
      prereleaseHeader.innerHTML = `
        <span class="release-badge badge-prerelease">PRE-RELEASE</span>
        <strong>Latest Pre-Release</strong>
      `;
      prereleaseSection.appendChild(prereleaseHeader);

      const meta = document.createElement('div');
      meta.className = 'release-meta';
      meta.innerHTML = `
        <div class="release-meta-item">
          <div class="release-meta-label">Version</div>
          <div class="release-meta-value">
            <a href="${repoData.latest_prerelease.url}" target="_blank" style="color: var(--md-typeset-a-color);">
              ${repoData.latest_prerelease.tag}
            </a>
          </div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Published</div>
          <div class="release-meta-value">${new Date(repoData.latest_prerelease.published_at).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' })}</div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Time</div>
          <div class="release-meta-value">${new Date(repoData.latest_prerelease.published_at).toLocaleTimeString('en-US', { hour: '2-digit', minute: '2-digit' })}</div>
        </div>
      `;
      prereleaseSection.appendChild(meta);

      if (repoData.latest_prerelease.assets && repoData.latest_prerelease.assets.length > 0) {
        const downloads = document.createElement('div');
        downloads.className = 'download-buttons';
        repoData.latest_prerelease.assets.forEach(asset => {
          const btn = document.createElement('a');
          btn.href = asset.download_url;
          btn.target = '_blank';
          btn.className = 'download-btn';
          btn.innerHTML = `📥 ${asset.name} <span style="opacity: 0.8; font-size: 0.85rem;">(${(asset.size / 1024 / 1024).toFixed(2)} MB)</span>`;
          downloads.appendChild(btn);
        });
        prereleaseSection.appendChild(downloads);
      }

      card.appendChild(prereleaseSection);
    }

    container.appendChild(card);
  });

  if (!hasReleases) {
    container.innerHTML = `
      <div class="no-releases">
        <p>No releases found matching the selected filter.</p>
      </div>
    `;
  }
}
</script>
