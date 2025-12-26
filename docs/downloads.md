---
hide:
  - toc
---

# 📥 Downloads

Download the latest releases of faneX-ID for all supported platforms. All downloads are organized by operating system and platform for easy access.

!!! tip "Release Information"
    For detailed release notes and version history, check out the [Releases page](releases.md).


<div id="downloads-container" class="downloads-container reveal">
  <div class="loading">Loading downloads...</div>
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
// Platform icons and names
const platformConfig = {
  'core': { icon: '🚀', name: 'Core Application', order: 0 },
  'windows': { icon: '🪟', name: 'Windows', order: 1 },
  'android': { icon: '🤖', name: 'Android', order: 2 },
  'ios': { icon: '🍎', name: 'iOS', order: 3 },
  'linux': { icon: '🐧', name: 'Linux', order: 4 },
  'docker': { icon: '🐳', name: 'Docker', order: 5 },
  'macos': { icon: '💻', name: 'macOS', order: 6 },
  'other': { icon: '📦', name: 'Other', order: 7 }
};

// Detect platform from asset name
function detectPlatform(assetName) {
  const name = assetName.toLowerCase();
  if (name.includes('windows') || name.includes('.msix') || name.includes('.msixbundle') || name.includes('.exe')) return 'windows';
  if (name.includes('android') || name.includes('.apk')) return 'android';
  if (name.includes('ios') || name.includes('.ipa')) return 'ios';
  if (name.includes('linux') || name.includes('.deb') || name.includes('.rpm') || name.includes('.tar') || name.includes('.AppImage')) return 'linux';
  if (name.includes('docker') || name.includes('image') || name.includes('.tar.gz')) return 'docker';
  if (name.includes('macos') || name.includes('.dmg') || name.includes('mac')) return 'macos';
  if (name.includes('core') || name.includes('fanexid')) return 'core';
  return 'other';
}

// Format file size
function formatSize(bytes) {
  if (bytes === 0) return '0 B';
  const k = 1024;
  const sizes = ['B', 'KB', 'MB', 'GB'];
  const i = Math.floor(Math.log(bytes) / Math.log(k));
  return Math.round(bytes / Math.pow(k, i) * 100) / 100 + ' ' + sizes[i];
}

// Fetch and display downloads with cache-buster
fetch('../data/releases.json?v=' + new Date().getTime())
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => {
    const container = document.getElementById('downloads-container');
    container.innerHTML = '';

    const lastUpdated = new Date(data.last_updated).toLocaleString();
    container.innerHTML += `<p style="text-align: center; color: var(--md-default-fg-color--light); margin-bottom: 2rem;"><em>Last updated: ${lastUpdated}</em></p>`;

    const repos = Object.entries(data.repositories);

    // Group by platform
    const platformGroups = {};

    repos.forEach(([repoName, repoData]) => {
      if (!repoData) return;

      // Process stable releases
      if (repoData.latest_stable && repoData.latest_stable.assets) {
        repoData.latest_stable.assets.forEach(asset => {
          const platform = detectPlatform(asset.name);
          if (!platformGroups[platform]) {
            platformGroups[platform] = [];
          }
          platformGroups[platform].push({
            repo: repoName,
            release: repoData.latest_stable,
            asset: asset,
            platform: platform,
            prerelease: false
          });
        });
      }

      // Process pre-releases
      if (repoData.latest_prerelease && repoData.latest_prerelease.assets) {
        repoData.latest_prerelease.assets.forEach(asset => {
          const platform = detectPlatform(asset.name);
          if (!platformGroups[platform]) {
            platformGroups[platform] = [];
          }
          platformGroups[platform].push({
            repo: repoName,
            release: repoData.latest_prerelease,
            asset: asset,
            platform: platform,
            prerelease: true
          });
        });
      }
    });

    // Display downloads by platform
    const sortedPlatforms = Object.keys(platformGroups)
      .sort((a, b) => {
        const orderA = platformConfig[a]?.order ?? 999;
        const orderB = platformConfig[b]?.order ?? 999;
        return orderA - orderB;
      });

    let totalDownloads = 0;
    let totalStable = 0;
    let totalPrereleases = 0;

    sortedPlatforms.forEach(platform => {
      const downloads = platformGroups[platform];
      if (downloads.length === 0) return;

      const config = platformConfig[platform] || { icon: '📦', name: platform };

      const section = document.createElement('div');
      section.className = 'platform-section';

      const header = document.createElement('div');
      header.className = 'platform-header';
      header.innerHTML = `
        <span class="platform-icon">${config.icon}</span>
        <h2 class="platform-title">${config.name}</h2>
      `;
      section.appendChild(header);

      const grid = document.createElement('div');
      grid.className = 'download-grid';

      // Sort: stable first, then by repo name
      downloads.sort((a, b) => {
        if (a.prerelease !== b.prerelease) return a.prerelease ? 1 : -1;
        return a.repo.localeCompare(b.repo);
      });

      downloads.forEach(dl => {
        totalDownloads++;
        if (dl.prerelease) {
          totalPrereleases++;
        } else {
          totalStable++;
        }

        const card = document.createElement('div');
        card.className = 'download-card';

        const headerDiv = document.createElement('div');
        headerDiv.className = 'download-header';
        headerDiv.innerHTML = `
          <span class="download-badge ${dl.prerelease ? 'badge-prerelease' : 'badge-stable'}">
            ${dl.prerelease ? 'PRE-RELEASE' : 'STABLE'}
          </span>
          <span class="repo-badge">${dl.repo === 'core' ? 'faneX-ID Core' : dl.repo}</span>
        `;
        card.appendChild(headerDiv);

        const name = document.createElement('div');
        name.className = 'download-name';
        name.textContent = dl.asset.name;
        card.appendChild(name);

        const meta = document.createElement('div');
        meta.className = 'download-meta';
        meta.innerHTML = `
          <div class="download-meta-item">
            <span class="download-meta-label">Version:</span>
            <span class="download-meta-value">${dl.release.tag}</span>
          </div>
          <div class="download-meta-item">
            <span class="download-meta-label">Size:</span>
            <span class="download-meta-value">${formatSize(dl.asset.size)}</span>
          </div>
          <div class="download-meta-item">
            <span class="download-meta-label">Published:</span>
            <span class="download-meta-value">${new Date(dl.release.published_at).toLocaleDateString()}</span>
          </div>
        `;
        card.appendChild(meta);

        const btn = document.createElement('a');
        btn.href = dl.asset.download_url;
        btn.target = '_blank';
        btn.className = 'download-btn';
        btn.textContent = '📥 Download';
        card.appendChild(btn);

        grid.appendChild(card);
      });

      section.appendChild(grid);
      container.appendChild(section);
    });

    // Summary statistics
    if (totalDownloads > 0) {
      const summary = document.createElement('div');
      summary.className = 'summary-stats';
      summary.innerHTML = `
        <div class="stat-item">
          <div class="stat-value">${totalDownloads}</div>
          <div class="stat-label">Total Downloads</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">${totalStable}</div>
          <div class="stat-label">Stable Releases</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">${totalPrereleases}</div>
          <div class="stat-label">Pre-Releases</div>
        </div>
        <div class="stat-item">
          <div class="stat-value">${sortedPlatforms.length}</div>
          <div class="stat-label">Platforms</div>
        </div>
      `;
      container.appendChild(summary);
    } else {
      container.innerHTML = `
        <div class="no-downloads">
          <h3>No downloads available</h3>
          <p>Check back later for available downloads.</p>
        </div>
      `;
    }
  })
  .catch(error => {
    console.error('Error loading downloads:', error);
    document.getElementById('downloads-container').innerHTML = `
      <div class="no-downloads">
        <h3 style="color: var(--md-typeset-a-color);">⚠️ Error loading downloads</h3>
        <p style="color: var(--md-default-fg-color--light);">${error.message}</p>
        <p style="margin-top: 1rem; font-size: 0.9rem;">The downloads data may not be available yet. Please check back later.</p>
      </div>
    `;
  });
</script>
