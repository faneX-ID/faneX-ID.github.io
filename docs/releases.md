---
hide:
  - toc
---

# 📦 Releases

Comprehensive release information from all faneX-ID projects, including both **stable** and **pre-releases**, with detailed version information, dates, and download links.

!!! tip "Quick Downloads"
    Looking for direct download links organized by platform? Check out the [Downloads page](downloads.md) for all available platform downloads.

<style>
  .releases-table-container {
    overflow-x: auto;
    margin: 2rem 0;
    border-radius: 12px;
    border: 1px solid var(--md-default-fg-color--lightest);
    background: var(--md-default-bg-color);
  }
  .releases-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.9rem;
  }
  .releases-table th {
    background: var(--md-default-bg-color--light);
    color: var(--md-default-fg-color);
    padding: 12px 16px;
    text-align: left;
    font-weight: 600;
    border-bottom: 2px solid var(--md-default-fg-color--lightest);
  }
  .releases-table td {
    padding: 12px 16px;
    border-bottom: 1px solid var(--md-default-fg-color--lightest);
    color: var(--md-default-fg-color);
  }
  .releases-table tr:hover {
    background: var(--md-default-bg-color--light);
  }
  .release-filter {
    display: flex;
    gap: 1.5rem;
    margin: 2rem 0;
    padding: 1.25rem;
    background: var(--md-default-bg-color--light);
    border-radius: 12px;
    flex-wrap: wrap;
    align-items: center;
  }
  .release-filter label {
    display: flex;
    align-items: center;
    cursor: pointer;
    font-weight: 500;
    color: var(--md-default-fg-color);
    transition: color 0.2s;
  }
  .release-filter label:hover {
    color: var(--md-primary-fg-color);
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
    box-shadow: 0 4px 6px rgba(0,0,0,0.02);
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }
  .release-card:hover {
    box-shadow: 0 10px 20px rgba(0,0,0,0.08);
    transform: translateY(-4px);
    border-color: var(--md-primary-fg-color--light);
  }
  .release-badge {
    display: inline-block;
    padding: 4px 10px;
    border-radius: 20px;
    font-size: 0.7rem;
    font-weight: 800;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }
  .badge-stable {
    background: #e8f5e9;
    color: #2e7d32;
    border: 1px solid #c8e6c9;
  }
  [data-md-color-scheme="slate"] .badge-stable {
    background: rgba(46, 125, 50, 0.2);
    color: #81c784;
    border-color: rgba(129, 199, 132, 0.3);
  }
  .badge-prerelease {
    background: #fff3e0;
    color: #ef6c00;
    border: 1px solid #ffe0b2;
  }
  [data-md-color-scheme="slate"] .badge-prerelease {
    background: rgba(239, 108, 0, 0.2);
    color: #ffb74d;
    border-color: rgba(255, 183, 77, 0.3);
  }
  .release-header {
    display: flex;
    align-items: center;
    margin-bottom: 1.25rem;
    flex-wrap: wrap;
    gap: 0.75rem;
  }
  .release-title {
    font-size: 1.4rem;
    font-weight: 800;
    margin: 0;
    color: var(--md-primary-fg-color);
    letter-spacing: -0.02em;
  }
  .release-meta {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1rem;
    margin: 1.25rem 0;
    padding: 1.25rem;
    background: var(--md-default-bg-color--light);
    border-radius: 10px;
    border: 1px solid var(--md-default-fg-color--lightest);
  }
  .release-meta-item {
    display: flex;
    flex-direction: column;
  }
  .release-meta-label {
    font-size: 0.7rem;
    color: var(--md-default-fg-color--light);
    text-transform: uppercase;
    font-weight: 700;
    margin-bottom: 0.35rem;
    letter-spacing: 0.05em;
  }
  .release-meta-value {
    font-size: 0.95rem;
    font-weight: 600;
    color: var(--md-default-fg-color);
  }
  .download-buttons {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    margin-top: 1.25rem;
  }
  .download-btn {
    display: inline-flex;
    align-items: center;
    gap: 0.6rem;
    padding: 0.6rem 1.1rem;
    background: var(--md-primary-fg-color);
    color: var(--md-primary-bg-color);
    border-radius: 8px;
    text-decoration: none;
    font-weight: 600;
    font-size: 0.85rem;
    transition: all 0.2s ease;
    border: none;
  }
  .download-btn:hover {
    background: var(--md-accent-fg-color);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  }
  .summary-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
    gap: 1.25rem;
    margin: 2rem 0;
  }
  .stat-card {
    background: var(--md-default-bg-color);
    padding: 1.5rem;
    border-radius: 12px;
    text-align: center;
    border: 1px solid var(--md-default-fg-color--lightest);
    box-shadow: 0 2px 4px rgba(0,0,0,0.02);
  }
  .stat-value {
    font-size: 2.2rem;
    font-weight: 900;
    color: var(--md-primary-fg-color);
    line-height: 1;
  }
  .stat-label {
    font-size: 0.8rem;
    color: var(--md-default-fg-color--light);
    margin-top: 0.75rem;
    text-transform: uppercase;
    font-weight: 700;
    letter-spacing: 0.05em;
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

<h2 id="summary-table-title" style="display: none;">📊 Release Overview</h2>
<div id="releases-table-container" class="releases-table-container" style="display: none;">
  <table class="releases-table">
    <thead>
      <tr>
        <th>Repository</th>
        <th>Latest Stable</th>
        <th>Latest Pre-Release</th>
        <th>Released At</th>
      </tr>
    </thead>
    <tbody id="releases-table-body">
    </tbody>
  </table>
</div>

<h2 id="detailed-releases-title" style="display: none;">🗂️ Detailed Release Info</h2>
<div id="releases-container">
  <div class="loading">Loading releases...</div>
</div>

<script>
let releasesData = null;

// Fetch and display releases
fetch('/data/releases.json')
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
  const tableBody = document.getElementById('releases-table-body');
  const tableContainer = document.getElementById('releases-table-container');
  const summaryTitle = document.getElementById('summary-table-title');
  const detailedTitle = document.getElementById('detailed-releases-title');

  container.innerHTML = '';
  tableBody.innerHTML = '';

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

    // Add to table
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td><strong>${repoName}</strong></td>
      <td>${repoData.latest_stable ? `<span class="release-badge badge-stable">${repoData.latest_stable.tag}</span>` : '—'}</td>
      <td>${repoData.latest_prerelease ? `<span class="release-badge badge-prerelease">${repoData.latest_prerelease.tag}</span>` : '—'}</td>
      <td style="font-size: 0.8rem; color: var(--md-default-fg-color--light);">
        ${repoData.latest_stable ? new Date(repoData.latest_stable.published_at).toLocaleDateString() : (repoData.latest_prerelease ? new Date(repoData.latest_prerelease.published_at).toLocaleDateString() : '—')}
      </td>
    `;
    tableBody.appendChild(tr);

    // Create detailed card
    const card = document.createElement('div');
    card.className = 'release-card';
    card.id = `repo-${repoName.replace(/\W/g, '-')}`;

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
        <strong style="color: var(--md-default-fg-color);">Latest Stable Release</strong>
      `;
      stableSection.appendChild(stableHeader);

      const meta = document.createElement('div');
      meta.className = 'release-meta';
      meta.innerHTML = `
        <div class="release-meta-item">
          <div class="release-meta-label">Version</div>
          <div class="release-meta-value">
            <a href="${repoData.latest_stable.url}" target="_blank" style="color: var(--md-typeset-a-color); text-decoration: underline;">
              ${repoData.latest_stable.tag}
            </a>
          </div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Published At</div>
          <div class="release-meta-value">${new Date(repoData.latest_stable.published_at).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' })}</div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Time (UTC)</div>
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
          btn.innerHTML = `📥 ${asset.name} <span style="opacity: 0.7; font-size: 0.75rem; margin-left: 4px;">(${(asset.size / 1024 / 1024).toFixed(2)} MB)</span>`;
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
        <strong style="color: var(--md-default-fg-color);">Latest Pre-Release</strong>
      `;
      prereleaseSection.appendChild(prereleaseHeader);

      const meta = document.createElement('div');
      meta.className = 'release-meta';
      meta.innerHTML = `
        <div class="release-meta-item">
          <div class="release-meta-label">Version</div>
          <div class="release-meta-value">
            <a href="${repoData.latest_prerelease.url}" target="_blank" style="color: var(--md-typeset-a-color); text-decoration: underline;">
              ${repoData.latest_prerelease.tag}
            </a>
          </div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Published At</div>
          <div class="release-meta-value">${new Date(repoData.latest_prerelease.published_at).toLocaleDateString('en-US', { year: 'numeric', month: 'long', day: 'numeric' })}</div>
        </div>
        <div class="release-meta-item">
          <div class="release-meta-label">Time (UTC)</div>
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
          btn.innerHTML = `📥 ${asset.name} <span style="opacity: 0.7; font-size: 0.75rem; margin-left: 4px;">(${(asset.size / 1024 / 1024).toFixed(2)} MB)</span>`;
          downloads.appendChild(btn);
        });
        prereleaseSection.appendChild(downloads);
      }

      card.appendChild(prereleaseSection);
    }

    container.appendChild(card);
  });

  if (hasReleases) {
      tableContainer.style.display = 'block';
      summaryTitle.style.display = 'block';
      detailedTitle.style.display = 'block';
  } else {
    tableContainer.style.display = 'none';
    summaryTitle.style.display = 'none';
    detailedTitle.style.display = 'none';
    container.innerHTML = `
      <div class="no-releases" style="text-align: center; padding: 4rem; color: var(--md-default-fg-color--light);">
        <p style="font-size: 1.2rem; font-weight: 600;">No releases found matching the selected filter.</p>
      </div>
    `;
  }
}
</script>
