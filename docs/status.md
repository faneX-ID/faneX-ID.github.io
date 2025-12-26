# Build Status

This page displays the current build status and version information from the faneX-ID core repository.


## 📌 Core Version Control

<div id="versions-container" class="reveal">
  <div class="loading-state">
    <p>Loading version information...</p>
  </div>
</div>

## 🌐 Project-wide Releases Summary

<div id="releases-summary" class="reveal">
  <div class="loading-state">
    <p>Loading releases summary...</p>
  </div>
</div>

## ⚙️ CI/CD Workflow Status

<div id="builds-container" class="status-grid reveal">
  <div class="loading-state">
    <p>Loading build status status...</p>
  </div>
</div>


<script>
// Fetch and display version info with cache-buster
fetch('/data/versions.json?v=' + new Date().getTime())
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('versions-container');

    if (data.error) {
      container.innerHTML = `<div class="status-card" style="border-color: #ff5252;"><p style="color: #ff5252; font-weight: 700;">Error: ${data.error}</p></div>`;
      return;
    }

    const lastUpdated = new Date(data.last_updated).toLocaleString();
    container.innerHTML = `
      <div class="status-card">
        <p style="font-size: 0.8rem; color: var(--md-default-fg-color--light); margin-bottom: 1rem;"><em>Last updated: ${lastUpdated}</em></p>
        <div class="version-list">
          <div class="version-item">
            <span class="version-label">Manifest Schema Version</span>
            <span class="version-value">${data.versions.manifest_schema_version || 'N/A'}</span>
          </div>
          <div class="version-item">
            <span class="version-label">Workflow Schema Version</span>
            <span class="version-value">${data.versions.workflow_schema_version || 'N/A'}</span>
          </div>
          <div class="version-item">
            <span class="version-label">Required Manifest Version</span>
            <span class="version-value">${data.versions.required_manifest_version || 'N/A'}</span>
          </div>
          <div class="version-item">
            <span class="version-label">Min Core Version</span>
            <span class="version-value">${data.versions.min_core_version || 'N/A'}</span>
          </div>
        </div>
        ${data.skipped ? `<div style="margin-top: 1rem; padding: 8px; background: var(--md-default-bg-color--light); border-radius: 4px; font-size: 0.8rem; color: #ffa000;">⚠️ Data limited (Core repo access required)</div>` : ''}
      </div>
    `;
  })
  .catch(error => {
    document.getElementById('versions-container').innerHTML =
      `<div class="status-card"><p style="color: #ff5252;">Error loading version info: ${error.message}</p></div>`;
  });

// Fetch and display build status with cache-buster
fetch('/data/builds.json?v=' + new Date().getTime())
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('builds-container');
    container.innerHTML = '';

    if (!data.workflows || data.workflows.length === 0) {
      container.innerHTML = '<div class="status-card" style="grid-column: 1/-1; text-align: center;"><p>No build information available.</p></div>';
      return;
    }

    data.workflows.forEach(workflow => {
      const card = document.createElement('div');
      card.className = 'status-card';

      const title = document.createElement('h4');
      title.textContent = workflow.name;
      card.appendChild(title);

      const runsDiv = document.createElement('div');
      runsDiv.className = 'build-status-list';

      workflow.runs.forEach(run => {
        const item = document.createElement('div');
        item.className = 'build-status-item';

        const statusClass = run.conclusion === 'success' ? 'status-success' :
                           run.conclusion === 'failure' ? 'status-failure' : 'status-pending';

        item.innerHTML = `
          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 0.5rem;">
            <a href="${run.html_url}" target="_blank" style="font-weight: 700; color: var(--md-typeset-a-color);">${run.head_branch || 'main'}</a>
            <span class="status-indicator ${statusClass}">${run.conclusion || run.status}</span>
          </div>
          <div style="font-size: 0.75rem; color: var(--md-default-fg-color--light);">
            ${new Date(run.updated_at).toLocaleString()}
          </div>
        `;
        runsDiv.appendChild(item);
      });

      card.appendChild(runsDiv);
      container.appendChild(card);
    });
  })
  .catch(error => {
    document.getElementById('builds-container').innerHTML =
      `<div class="status-card" style="grid-column: 1/-1;"><p style="color: #ff5252;">Error loading build status: ${error.message}</p></div>`;
  });

// Fetch and display releases summary with cache-buster
fetch('/data/releases.json?v=' + new Date().getTime())
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('releases-summary');
    container.innerHTML = '';

    const repos = Object.entries(data.repositories);
    let html = '<div class="status-grid">';

    repos.forEach(([repoName, repoData]) => {
      if (!repoData.latest_stable && !repoData.latest_prerelease) return;

      html += `
        <div class="status-card">
          <h4 style="margin-bottom: 1rem;">${repoName}</h4>
          ${repoData.latest_stable ? `
            <div style="margin-bottom: 0.75rem; display: flex; align-items: center; gap: 8px;">
              <span class="status-indicator status-success" style="padding: 1px 6px;">STABLE</span>
              <a href="${repoData.latest_stable.url}" target="_blank" style="font-weight: 600; font-size: 0.9rem;">${repoData.latest_stable.tag}</a>
            </div>
          ` : ''}
          ${repoData.latest_prerelease ? `
            <div style="display: flex; align-items: center; gap: 8px;">
              <span class="status-indicator status-pending" style="padding: 1px 6px;">PRE-RELEASE</span>
              <a href="${repoData.latest_prerelease.url}" target="_blank" style="font-weight: 600; font-size: 0.9rem;">${repoData.latest_prerelease.tag}</a>
            </div>
          ` : ''}
        </div>
      `;
    });

    html += '</div>';
    container.innerHTML = html;
  })
  .catch(error => {
    document.getElementById('releases-summary').innerHTML =
      `<div class="status-card"><p style="color: #ff5252;">Error loading releases summary: ${error.message}</p></div>`;
  });
</script>
