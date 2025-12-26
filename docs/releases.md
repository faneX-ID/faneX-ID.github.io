---
hide:
  - toc
---

# 📦 Releases

Comprehensive release information from all faneX-ID projects, including both **stable** and **pre-releases**, with detailed version information, dates, and download links.

!!! tip "Quick Downloads"
    Looking for direct download links organized by platform? Check out the [Downloads page](downloads.md) for all available platform downloads.

<div class="release-filter reveal">
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

<div id="summary-stats" class="summary-stats reveal" style="display: none;">
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

<h2 id="summary-table-title" class="reveal" style="display: none;">📊 Release Overview</h2>
<div id="releases-table-container" class="releases-table-container reveal" style="display: none;">
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

<h2 id="detailed-releases-title" class="reveal" style="display: none;">🗂️ Detailed Release Info</h2>
<div id="releases-container" class="reveal">
  <div class="loading">Loading releases...</div>
</div>

<script>
  window.addEventListener('load', () => {
    ScrollReveal().reveal('.reveal', {
      distance: '30px',
      duration: 800,
      interval: 100,
      origin: 'bottom',
      easing: 'ease-out'
    });
  });
</script>

<script>
let releasesData = null;

// Fetch and display releases with cache-buster
fetch('../data/releases.json?v=' + new Date().getTime())
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
