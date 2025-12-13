# Downloads

Download the latest releases of faneX-ID for all supported platforms.

<div id="downloads-container">
  <p>Loading downloads...</p>
</div>

<script>
// Platform icons mapping
const platformIcons = {
  'windows': '🪟',
  'android': '🤖',
  'ios': '🍎',
  'linux': '🐧',
  'docker': '🐳',
  'macos': '💻',
  'msix': '📦',
  'apk': '📱',
  'ipa': '📱',
  'deb': '📦',
  'rpm': '📦',
  'tar': '📦',
  'zip': '📦'
};

// Detect platform from asset name
function detectPlatform(assetName) {
  const name = assetName.toLowerCase();
  if (name.includes('windows') || name.includes('.msix') || name.includes('.msixbundle')) return 'windows';
  if (name.includes('android') || name.includes('.apk')) return 'android';
  if (name.includes('ios') || name.includes('.ipa')) return 'ios';
  if (name.includes('linux') || name.includes('.deb') || name.includes('.rpm') || name.includes('.tar')) return 'linux';
  if (name.includes('docker') || name.includes('image')) return 'docker';
  if (name.includes('macos') || name.includes('.dmg')) return 'macos';
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
fetch('/docs/data/releases.json')
  .then(response => response.json())
  .then(data => {
    const container = document.getElementById('downloads-container');
    container.innerHTML = '';
    
    const lastUpdated = new Date(data.last_updated).toLocaleString();
    container.innerHTML += `<p style="text-align: center; color: #666; margin-bottom: 2rem;"><em>Last updated: ${lastUpdated}</em></p>`;
    
    const repos = Object.entries(data.repositories);
    
    // Group by platform
    const platformGroups = {
      'core': [],
      'windows': [],
      'android': [],
      'ios': [],
      'docker': [],
      'linux': [],
      'other': []
    };
    
    repos.forEach(([repoName, repoData]) => {
      // Process stable releases
      if (repoData.latest_stable && repoData.latest_stable.assets) {
        repoData.latest_stable.assets.forEach(asset => {
          const platform = detectPlatform(asset.name);
          platformGroups[platform].push({
            repo: repoName,
            release: repoData.latest_stable,
            asset: asset,
            platform: platform
          });
          if (repoName === 'core') {
            platformGroups['core'].push({
              repo: repoName,
              release: repoData.latest_stable,
              asset: asset,
              platform: platform
            });
          }
        });
      }
      
      // Process pre-releases
      if (repoData.latest_prerelease && repoData.latest_prerelease.assets) {
        repoData.latest_prerelease.assets.forEach(asset => {
          const platform = detectPlatform(asset.name);
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
    const platformOrder = ['core', 'windows', 'android', 'ios', 'linux', 'docker', 'macos', 'other'];
    const platformNames = {
      'core': 'Core Application',
      'windows': 'Windows',
      'android': 'Android',
      'ios': 'iOS',
      'linux': 'Linux',
      'docker': 'Docker',
      'macos': 'macOS',
      'other': 'Other'
    };
    
    platformOrder.forEach(platform => {
      const downloads = platformGroups[platform];
      if (downloads.length === 0) return;
      
      const section = document.createElement('div');
      section.style.cssText = 'margin-bottom: 3rem;';
      
      const title = document.createElement('h2');
      title.textContent = `${platformIcons[platform] || '📦'} ${platformNames[platform]}`;
      title.style.cssText = 'font-size: 1.8rem; margin-bottom: 1.5rem; padding-bottom: 0.5rem; border-bottom: 2px solid #667eea;';
      section.appendChild(title);
      
      // Group by repository
      const repoGroups = {};
      downloads.forEach(dl => {
        if (!repoGroups[dl.repo]) {
          repoGroups[dl.repo] = [];
        }
        repoGroups[dl.repo].push(dl);
      });
      
      Object.entries(repoGroups).forEach(([repo, repoDownloads]) => {
        const repoCard = document.createElement('div');
        repoCard.style.cssText = 'background: white; border: 1px solid #e0e0e0; border-radius: 12px; padding: 1.5rem; margin-bottom: 1.5rem; box-shadow: 0 2px 8px rgba(0,0,0,0.1);';
        
        const repoTitle = document.createElement('h3');
        repoTitle.textContent = repo === 'core' ? 'faneX-ID Core' : repo;
        repoTitle.style.cssText = 'font-size: 1.3rem; margin-bottom: 1rem; color: #333;';
        repoCard.appendChild(repoTitle);
        
        // Stable releases
        const stableDownloads = repoDownloads.filter(dl => !dl.prerelease);
        if (stableDownloads.length > 0) {
          const stableSection = document.createElement('div');
          stableSection.style.cssText = 'margin-bottom: 1rem;';
          
          stableDownloads.forEach(dl => {
            const downloadCard = document.createElement('div');
            downloadCard.style.cssText = 'display: flex; align-items: center; justify-content: space-between; padding: 1rem; background: #f5f5f5; border-radius: 8px; margin-bottom: 0.5rem;';
            
            const info = document.createElement('div');
            info.style.cssText = 'flex: 1;';
            
            const badge = document.createElement('span');
            badge.textContent = 'STABLE';
            badge.style.cssText = 'background: #4caf50; color: white; padding: 4px 8px; border-radius: 4px; font-size: 11px; font-weight: bold; margin-right: 8px;';
            
            const name = document.createElement('span');
            name.textContent = dl.asset.name;
            name.style.cssText = 'font-weight: 600; margin-right: 8px;';
            
            const size = document.createElement('span');
            size.textContent = `(${formatSize(dl.asset.size)})`;
            size.style.cssText = 'color: #666; font-size: 0.9rem;';
            
            const version = document.createElement('div');
            version.style.cssText = 'font-size: 0.85rem; color: #888; margin-top: 4px;';
            version.textContent = `Version: ${dl.release.tag} • Published: ${new Date(dl.release.published_at).toLocaleDateString()}`;
            
            info.appendChild(badge);
            info.appendChild(name);
            info.appendChild(size);
            info.appendChild(version);
            
            const downloadBtn = document.createElement('a');
            downloadBtn.href = dl.asset.download_url;
            downloadBtn.textContent = 'Download';
            downloadBtn.style.cssText = 'background: #667eea; color: white; padding: 0.5rem 1.5rem; border-radius: 6px; text-decoration: none; font-weight: 600; transition: background 0.2s;';
            downloadBtn.onmouseover = () => downloadBtn.style.background = '#5568d3';
            downloadBtn.onmouseout = () => downloadBtn.style.background = '#667eea';
            
            downloadCard.appendChild(info);
            downloadCard.appendChild(downloadBtn);
            stableSection.appendChild(downloadCard);
          });
          
          repoCard.appendChild(stableSection);
        }
        
        // Pre-releases
        const prereleaseDownloads = repoDownloads.filter(dl => dl.prerelease);
        if (prereleaseDownloads.length > 0) {
          const prereleaseSection = document.createElement('div');
          prereleaseSection.style.cssText = 'margin-top: 1rem; padding-top: 1rem; border-top: 1px solid #e0e0e0;';
          
          prereleaseDownloads.forEach(dl => {
            const downloadCard = document.createElement('div');
            downloadCard.style.cssText = 'display: flex; align-items: center; justify-content: space-between; padding: 1rem; background: #fff3e0; border-radius: 8px; margin-bottom: 0.5rem;';
            
            const info = document.createElement('div');
            info.style.cssText = 'flex: 1;';
            
            const badge = document.createElement('span');
            badge.textContent = 'PRE-RELEASE';
            badge.style.cssText = 'background: #ff9800; color: white; padding: 4px 8px; border-radius: 4px; font-size: 11px; font-weight: bold; margin-right: 8px;';
            
            const name = document.createElement('span');
            name.textContent = dl.asset.name;
            name.style.cssText = 'font-weight: 600; margin-right: 8px;';
            
            const size = document.createElement('span');
            size.textContent = `(${formatSize(dl.asset.size)})`;
            size.style.cssText = 'color: #666; font-size: 0.9rem;';
            
            const version = document.createElement('div');
            version.style.cssText = 'font-size: 0.85rem; color: #888; margin-top: 4px;';
            version.textContent = `Version: ${dl.release.tag} • Published: ${new Date(dl.release.published_at).toLocaleDateString()}`;
            
            info.appendChild(badge);
            info.appendChild(name);
            info.appendChild(size);
            info.appendChild(version);
            
            const downloadBtn = document.createElement('a');
            downloadBtn.href = dl.asset.download_url;
            downloadBtn.textContent = 'Download';
            downloadBtn.style.cssText = 'background: #ff9800; color: white; padding: 0.5rem 1.5rem; border-radius: 6px; text-decoration: none; font-weight: 600; transition: background 0.2s;';
            downloadBtn.onmouseover = () => downloadBtn.style.background = '#f57c00';
            downloadBtn.onmouseout = () => downloadBtn.style.background = '#ff9800';
            
            downloadCard.appendChild(info);
            downloadCard.appendChild(downloadBtn);
            prereleaseSection.appendChild(downloadCard);
          });
          
          repoCard.appendChild(prereleaseSection);
        }
        
        section.appendChild(repoCard);
      });
      
      container.appendChild(section);
    });
    
    // Summary
    const summary = document.createElement('div');
    summary.style.cssText = 'margin-top: 3rem; padding: 2rem; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); border-radius: 12px; color: white; text-align: center;';
    summary.innerHTML = `
      <h3 style="margin-bottom: 1rem;">📊 Download Statistics</h3>
      <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 1rem; margin-top: 1rem;">
        <div>
          <div style="font-size: 2rem; font-weight: bold;">${repos.length}</div>
          <div style="opacity: 0.9;">Repositories</div>
        </div>
        <div>
          <div style="font-size: 2rem; font-weight: bold;">${repos.reduce((sum, [, r]) => sum + (r.total_stable || 0), 0)}</div>
          <div style="opacity: 0.9;">Stable Releases</div>
        </div>
        <div>
          <div style="font-size: 2rem; font-weight: bold;">${repos.reduce((sum, [, r]) => sum + (r.total_prereleases || 0), 0)}</div>
          <div style="opacity: 0.9;">Pre-Releases</div>
        </div>
      </div>
    `;
    container.appendChild(summary);
  })
  .catch(error => {
    document.getElementById('downloads-container').innerHTML = 
      `<div style="text-align: center; padding: 2rem; color: #d32f2f;">
        <h3>❌ Error loading downloads</h3>
        <p>${error.message}</p>
        <p style="margin-top: 1rem; font-size: 0.9rem;">Please try refreshing the page or check back later.</p>
      </div>`;
  });
</script>

<style>
#downloads-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

@media (max-width: 768px) {
  #downloads-container {
    padding: 1rem;
  }
}
</style>

