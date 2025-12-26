---
hide:
  - toc
---

# 📥 Downloads

Download the latest releases of faneX-ID for all supported platforms. All downloads are organized by operating system and platform for easy access.

!!! tip "Release Information"
    For detailed release notes and version history, check out the [Releases page](releases.md).

<style>
  .downloads-container {
    max-width: 1200px;
    margin: 0 auto;
  }
  .platform-section {
    margin-bottom: 3rem;
  }
  .platform-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    margin-bottom: 1.5rem;
    padding-bottom: 1rem;
    border-bottom: 3px solid var(--md-primary-fg-color);
  }
  .platform-icon {
    font-size: 2.5rem;
  }
  .platform-title {
    font-size: 2rem;
    font-weight: 700;
    color: var(--md-primary-fg-color);
    margin: 0;
  }
  .download-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 1.5rem;
  }
  .download-card {
    background: var(--md-default-bg-color);
    border: 2px solid var(--md-default-fg-color--lightest);
    border-radius: 12px;
    padding: 1.5rem;
    transition: all 0.3s ease;
    box-shadow: 0 2px 8px rgba(0,0,0,0.05);
  }
  .download-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.15);
    border-color: var(--md-primary-fg-color);
  }
  .download-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 1rem;
  }
  .download-badge {
    display: inline-block;
    padding: 4px 10px;
    border-radius: 12px;
    font-size: 0.7rem;
    font-weight: 700;
    text-transform: uppercase;
  }
  .badge-stable {
    background: #4caf50;
    color: white;
  }
  .badge-prerelease {
    background: #ff9800;
    color: white;
  }
  .download-name {
    font-size: 1.1rem;
    font-weight: 600;
    color: var(--md-default-fg-color);
    margin: 0.5rem 0;
    word-break: break-word;
  }
  .download-meta {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    margin: 1rem 0;
    padding: 1rem;
    background: var(--md-default-bg-color--light);
    border-radius: 8px;
  }
  .download-meta-item {
    display: flex;
    justify-content: space-between;
    font-size: 0.9rem;
  }
  .download-meta-label {
    color: var(--md-default-fg-color--light);
    font-weight: 500;
  }
  .download-meta-value {
    color: var(--md-default-fg-color);
    font-weight: 600;
  }
  .download-btn {
    display: block;
    width: 100%;
    padding: 1rem;
    background: var(--md-primary-fg-color);
    color: var(--md-primary-bg-color);
    text-align: center;
    text-decoration: none;
    border-radius: 8px;
    font-weight: 700;
    font-size: 1rem;
    transition: all 0.2s ease;
    margin-top: 1rem;
  }
  .download-btn:hover {
    background: var(--md-accent-fg-color);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,0,0,0.2);
  }
  .download-btn:active {
    transform: translateY(0);
  }
  .summary-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;
    margin: 3rem 0;
    padding: 2rem;
    background: linear-gradient(135deg, var(--md-primary-fg-color) 0%, var(--md-accent-fg-color) 100%);
    border-radius: 16px;
    color: white;
  }
  .stat-item {
    text-align: center;
  }
  .stat-value {
    font-size: 2.5rem;
    font-weight: 900;
    margin-bottom: 0.5rem;
  }
  .stat-label {
    font-size: 1rem;
    opacity: 0.9;
  }
  .loading {
    text-align: center;
    padding: 3rem;
    color: var(--md-default-fg-color--light);
  }
  .no-downloads {
    text-align: center;
    padding: 3rem;
    color: var(--md-default-fg-color--light);
  }
  .repo-badge {
    display: inline-block;
    padding: 2px 8px;
    background: var(--md-primary-fg-color--light);
    color: var(--md-primary-bg-color);
    border-radius: 4px;
    font-size: 0.75rem;
    font-weight: 600;
    margin-left: 0.5rem;
  }
</style>

<div id="downloads-container" class="downloads-container">
  <div class="loading">Loading downloads...</div>
</div>

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

// Fetch and display downloads
fetch('/data/releases.json')
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
