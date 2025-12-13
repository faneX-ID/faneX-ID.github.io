# Releases

This page displays the latest releases from all faneX-ID projects, including both **stable** and **pre-releases**.

<div style="margin: 16px 0;">
  <label>
    <input type="radio" name="release-filter" value="all" checked onchange="filterReleases()">
    All Releases
  </label>
  <label style="margin-left: 16px;">
    <input type="radio" name="release-filter" value="stable" onchange="filterReleases()">
    Stable Only
  </label>
  <label style="margin-left: 16px;">
    <input type="radio" name="release-filter" value="prerelease" onchange="filterReleases()">
    Pre-Releases Only
  </label>
</div>

<div id="releases-container">
  <p>Loading releases...</p>
</div>

<script>
let releasesData = null;

// Fetch and display releases
fetch('/docs/data/releases.json')
  .then(response => response.json())
  .then(data => {
    releasesData = data;
    renderReleases('all');
  })
  .catch(error => {
    document.getElementById('releases-container').innerHTML =
      `<p style="color: red;">Error loading releases: ${error.message}</p>`;
  });

function filterReleases() {
  const filter = document.querySelector('input[name="release-filter"]:checked').value;
  renderReleases(filter);
}

function renderReleases(filter) {
  if (!releasesData) return;

  const container = document.getElementById('releases-container');
  container.innerHTML = '';

  const lastUpdated = new Date(releasesData.last_updated).toLocaleString();
  container.innerHTML += `<p><em>Last updated: ${lastUpdated}</em></p>`;

  const repos = Object.entries(releasesData.repositories);

  repos.forEach(([repoName, repoData]) => {
    // Skip if no releases match filter
    if (filter === 'stable' && !repoData.latest_stable) return;
    if (filter === 'prerelease' && !repoData.latest_prerelease) return;
    if (filter === 'all' && !repoData.latest_stable && !repoData.latest_prerelease) return;

    const card = document.createElement('div');
    card.className = 'release-card';
    card.style.cssText = 'border: 1px solid #ddd; border-radius: 8px; padding: 16px; margin: 16px 0;';

    const title = document.createElement('h3');
    title.textContent = repoName;
    card.appendChild(title);

    // Display stable releases
    if (repoData.latest_stable && (filter === 'all' || filter === 'stable')) {
      const stableSection = document.createElement('div');
      stableSection.style.cssText = 'margin-bottom: 16px;';
      stableSection.innerHTML = `
        <div style="display: flex; align-items: center; margin-bottom: 8px;">
          <span style="background: #4caf50; color: white; padding: 4px 8px; border-radius: 4px; font-size: 12px; font-weight: bold; margin-right: 8px;">STABLE</span>
          <strong>Latest Stable Release:</strong>
        </div>
        <div style="margin-left: 20px;">
          <p><strong>Version:</strong> <a href="${repoData.latest_stable.url}" target="_blank">${repoData.latest_stable.tag}</a></p>
          <p><strong>Published:</strong> ${new Date(repoData.latest_stable.published_at).toLocaleString()}</p>
          ${repoData.latest_stable.body ? `<p style="margin-top: 8px;">${repoData.latest_stable.body}</p>` : ''}
        </div>
      `;
      card.appendChild(stableSection);

      // Show additional stable releases if available
      if (repoData.stable_releases && repoData.stable_releases.length > 1 && filter === 'all') {
        const moreStable = document.createElement('details');
        moreStable.style.cssText = 'margin-top: 8px; margin-left: 20px;';
        moreStable.innerHTML = `
          <summary style="cursor: pointer; color: #666;">View ${repoData.stable_releases.length - 1} more stable release(s)</summary>
          <ul style="margin-top: 8px;">
            ${repoData.stable_releases.slice(1).map(r => `
              <li>
                <a href="${r.url}" target="_blank">${r.tag}</a>
                (${new Date(r.published_at).toLocaleDateString()})
              </li>
            `).join('')}
          </ul>
        `;
        card.appendChild(moreStable);
      }
    }

    // Display pre-releases
    if (repoData.latest_prerelease && (filter === 'all' || filter === 'prerelease')) {
      const prereleaseSection = document.createElement('div');
      prereleaseSection.style.cssText = 'margin-top: 16px; padding-top: 16px; border-top: 1px solid #eee;';
      prereleaseSection.innerHTML = `
        <div style="display: flex; align-items: center; margin-bottom: 8px;">
          <span style="background: #ff9800; color: white; padding: 4px 8px; border-radius: 4px; font-size: 12px; font-weight: bold; margin-right: 8px;">PRE-RELEASE</span>
          <strong>Latest Pre-Release:</strong>
        </div>
        <div style="margin-left: 20px;">
          <p><strong>Version:</strong> <a href="${repoData.latest_prerelease.url}" target="_blank">${repoData.latest_prerelease.tag}</a></p>
          <p><strong>Published:</strong> ${new Date(repoData.latest_prerelease.published_at).toLocaleString()}</p>
          <p style="color: #ff9800; font-size: 14px; margin-top: 4px;">⚠️ This is a pre-release. Use with caution.</p>
          ${repoData.latest_prerelease.body ? `<p style="margin-top: 8px;">${repoData.latest_prerelease.body}</p>` : ''}
        </div>
      `;
      card.appendChild(prereleaseSection);

      // Show additional pre-releases if available
      if (repoData.prereleases && repoData.prereleases.length > 1 && filter === 'all') {
        const morePrereleases = document.createElement('details');
        morePrereleases.style.cssText = 'margin-top: 8px; margin-left: 20px;';
        morePrereleases.innerHTML = `
          <summary style="cursor: pointer; color: #666;">View ${repoData.prereleases.length - 1} more pre-release(s)</summary>
          <ul style="margin-top: 8px;">
            ${repoData.prereleases.slice(1).map(r => `
              <li>
                <a href="${r.url}" target="_blank">${r.tag}</a>
                (${new Date(r.published_at).toLocaleDateString()})
              </li>
            `).join('')}
          </ul>
        `;
        card.appendChild(morePrereleases);
      }
    }

    // Show summary if no releases match filter
    if (!repoData.latest_stable && !repoData.latest_prerelease) {
      const noRelease = document.createElement('p');
      noRelease.textContent = 'No releases yet';
      noRelease.style.color = '#999';
      card.appendChild(noRelease);
    }

    // Version info
    if (repoData.version_info) {
      const versionInfo = document.createElement('div');
      versionInfo.style.cssText = 'margin-top: 16px; padding-top: 16px; border-top: 1px solid #eee;';
      versionInfo.innerHTML = `
        <p><strong>Version Info:</strong></p>
        <ul>
          <li>Schema Version: ${repoData.version_info.manifest_schema_version || 'N/A'}</li>
          <li>Min Core Version: ${repoData.version_info.min_core_version || 'N/A'}</li>
        </ul>
      `;
      card.appendChild(versionInfo);
    }

    container.appendChild(card);
  });

  // Show summary
  if (filter === 'all') {
    const totalStable = repos.reduce((sum, [, r]) => sum + (r.total_stable || 0), 0);
    const totalPrereleases = repos.reduce((sum, [, r]) => sum + (r.total_prereleases || 0), 0);
    const summary = document.createElement('div');
    summary.style.cssText = 'margin-top: 24px; padding: 16px; background: #f5f5f5; border-radius: 8px;';
    summary.innerHTML = `
      <h4>Summary</h4>
      <p>Total stable releases: <strong>${totalStable}</strong></p>
      <p>Total pre-releases: <strong>${totalPrereleases}</strong></p>
    `;
    container.appendChild(summary);
  }
}
</script>
