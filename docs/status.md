# Build Status

This page displays the current build status and version information from the faneX-ID core repository.

## Version Information

<div id="versions-container">
  <p>Loading version information...</p>
</div>

## Latest Releases Summary

<div id="releases-summary">
  <p>Loading releases summary...</p>
</div>

## Build Status

<div id="builds-container">
  <p>Loading build status...</p>
</div>

<script>
// Fetch and display version info
fetch('/docs/data/versions.json')
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('versions-container');
    
    if (data.error) {
      container.innerHTML = `<p style="color: red;">Error: ${data.error}</p>`;
      return;
    }
    
    const lastUpdated = new Date(data.last_updated).toLocaleString();
    container.innerHTML = `
      <p><em>Last updated: ${lastUpdated}</em></p>
      <div style="background: #f5f5f5; padding: 16px; border-radius: 8px; margin: 16px 0;">
        <h4>Current Versions</h4>
        <ul>
          <li><strong>Manifest Schema Version:</strong> ${data.versions.manifest_schema_version || 'N/A'}</li>
          <li><strong>Workflow Schema Version:</strong> ${data.versions.workflow_schema_version || 'N/A'}</li>
          <li><strong>Required Manifest Version:</strong> ${data.versions.required_manifest_version || 'N/A'}</li>
          <li><strong>Min Core Version:</strong> ${data.versions.min_core_version || 'N/A'}</li>
        </ul>
      </div>
    `;
  })
  .catch(error => {
    document.getElementById('versions-container').innerHTML = 
      `<p style="color: red;">Error loading version info: ${error.message}</p>`;
  });

// Fetch and display build status
fetch('/docs/data/builds.json')
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('builds-container');
    
    const lastUpdated = new Date(data.last_updated).toLocaleString();
    container.innerHTML = `<p><em>Last updated: ${lastUpdated}</em></p>`;
    
    if (data.workflows.length === 0) {
      container.innerHTML += '<p>No build information available.</p>';
      return;
    }
    
    data.workflows.forEach(workflow => {
      const card = document.createElement('div');
      card.className = 'build-card';
      card.style.cssText = 'border: 1px solid #ddd; border-radius: 8px; padding: 16px; margin: 16px 0;';
      
      const title = document.createElement('h4');
      title.textContent = workflow.name;
      card.appendChild(title);
      
      const runsList = document.createElement('ul');
      workflow.runs.forEach(run => {
        const runItem = document.createElement('li');
        const statusColor = run.conclusion === 'success' ? 'green' : 
                           run.conclusion === 'failure' ? 'red' : 'orange';
        runItem.innerHTML = `
          <a href="${run.html_url}" target="_blank">${run.head_branch || 'main'}</a>
          - <span style="color: ${statusColor}">${run.conclusion || run.status}</span>
          (${new Date(run.updated_at).toLocaleString()})
        `;
        runsList.appendChild(runItem);
      });
      
      card.appendChild(runsList);
      container.appendChild(card);
    });
  })
  .catch(error => {
    document.getElementById('builds-container').innerHTML = 
      `<p style="color: red;">Error loading build status: ${error.message}</p>`;
  });

// Fetch and display releases summary
fetch('/docs/data/releases.json')
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('releases-summary');
    
    const repos = Object.entries(data.repositories);
    let summaryHtml = '<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 16px; margin-top: 16px;">';
    
    repos.forEach(([repoName, repoData]) => {
      if (!repoData.latest_stable && !repoData.latest_prerelease) return;
      
      summaryHtml += `
        <div style="border: 1px solid #ddd; border-radius: 8px; padding: 12px;">
          <h4 style="margin-top: 0;">${repoName}</h4>
          ${repoData.latest_stable ? `
            <p>
              <span style="background: #4caf50; color: white; padding: 2px 6px; border-radius: 3px; font-size: 11px; font-weight: bold;">STABLE</span>
              <a href="${repoData.latest_stable.url}" target="_blank">${repoData.latest_stable.tag}</a>
            </p>
          ` : ''}
          ${repoData.latest_prerelease ? `
            <p>
              <span style="background: #ff9800; color: white; padding: 2px 6px; border-radius: 3px; font-size: 11px; font-weight: bold;">PRE-RELEASE</span>
              <a href="${repoData.latest_prerelease.url}" target="_blank">${repoData.latest_prerelease.tag}</a>
            </p>
          ` : ''}
        </div>
      `;
    });
    
    summaryHtml += '</div>';
    container.innerHTML = summaryHtml;
  })
  .catch(error => {
    document.getElementById('releases-summary').innerHTML = 
      `<p style="color: red;">Error loading releases summary: ${error.message}</p>`;
  });
</script>

