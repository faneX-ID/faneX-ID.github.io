# Installation Guide

This guide provides step-by-step instructions for installing faneX-ID in various environments.

## Requirements

### Hardware Requirements

| Component | Minimum | Recommended | Production |
|-----------|---------|-------------|------------|
| **CPU** | 1 vCPU | 2 vCPU | 4+ vCPU |
| **RAM** | 1 GB | 4 GB | 8+ GB |
| **Storage** | 2 GB | 10 GB | 50+ GB |
| **Network** | 10 Mbps | 100 Mbps | 1 Gbps |

### Software Requirements

- **Operating System:**
  - Linux (Debian 12+, Ubuntu 22.04+, RHEL 9+)
  - Windows Server 2019+ (with WSL2 or Docker)
  - macOS (for development)

- **Container Runtime:**
  - Docker 20.10+
  - Docker Compose 2.0+

- **Database (Production):**
  - PostgreSQL 14+
  - SQLite (development only)

- **Network:**
  - Port 8000 (Backend API)
  - Port 3000 (Frontend, production)
  - Port 5173 (Frontend, development)

## Installation Methods

### Method 1: Docker Compose (Recommended)

#### Step 1: Clone Repository

```bash
git clone https://github.com/faneX-ID/core.git
cd core
```

#### Step 2: Configure Environment

```bash
cp .env.example .env
nano .env
```

**Required Environment Variables:**

```env
# Database Configuration
POSTGRES_PASSWORD=your_secure_password_here
POSTGRES_USER=fanexid
POSTGRES_DB=fanexiddb

# Application Settings
PROJECT_NAME=faneX-ID
DEBUG=false
SECRET_KEY=  # Auto-generated if empty

# Optional: GitHub Token (for private repo access)
GITHUB_TOKEN=your_github_token_here
```

#### Step 3: Start Services

```bash
docker-compose up -d --build
```

#### Step 4: Verify Installation

```bash
# Check container status
docker-compose ps

# View logs
docker-compose logs backend
docker-compose logs frontend

# Check backend health
curl http://localhost:8000/api/system/status
```

#### Step 5: Initial Login

1. View backend logs to get admin password:
   ```bash
   docker-compose logs backend | grep "INITIAL ADMIN"
   ```

2. Access frontend:
   - Development: http://localhost:5173
   - Production: http://localhost:3000

3. Login with:
   - Username: `admin`
   - Password: (from logs)

### Method 2: Home Assistant Add-on

#### Prerequisites

- Home Assistant installed
- HACS installed (optional, for easier installation)

#### Installation Steps

1. **Add Repository:**
   - Go to **Settings** → **Add-ons** → **Add-on Store**
   - Click three dots → **Repositories**
   - Add: `https://github.com/faneX-ID/homeassistant-addon`

2. **Install Add-on:**
   - Find **faneX-ID** in add-on list
   - Click **Install**
   - Wait for installation to complete

3. **Configure:**
   - Click **Configuration** tab
   - Enter GitHub token (required for private repo)
   - Configure other settings as needed
   - Click **Save**

4. **Start Add-on:**
   - Click **Start**
   - Wait for startup
   - Access via Home Assistant sidebar (Ingress)

### Method 3: Manual Installation

#### Backend Installation

1. **Install Python 3.13+**

2. **Install Dependencies:**
   ```bash
   cd backend
   pip install -r requirements.txt
   ```

3. **Configure Environment:**
   ```bash
   export DATABASE_URL="postgresql://user:pass@localhost/fanexiddb"
   export SECRET_KEY="your-secret-key"
   ```

4. **Run Migrations:**
   ```bash
   alembic upgrade head
   ```

5. **Start Backend:**
   ```bash
   uvicorn app.main:app --host 0.0.0.0 --port 8000
   ```

#### Frontend Installation

1. **Install Node.js 20+**

2. **Install Dependencies:**
   ```bash
   cd frontend
   npm install
   ```

3. **Build Frontend:**
   ```bash
   npm run build
   ```

4. **Serve Frontend:**
   ```bash
   # Development
   npm run dev

   # Production (using nginx or similar)
   npm run build
   # Serve dist/ directory
   ```

## Post-Installation Configuration

### 1. Admin Panel Setup

1. **Access Admin Panel:**
   - Navigate to `/admin` in browser
   - Login with admin credentials

2. **Configure Company:**
   - Set company name
   - Add support contact information
   - Configure base URL

3. **SSL Configuration:**
   - Generate self-signed certificate OR
   - Upload custom SSL certificate
   - Configure domain

### 2. Database Setup

#### PostgreSQL (Production)

1. **Create Database:**
   ```sql
   CREATE DATABASE fanexiddb;
   CREATE USER fanexid WITH PASSWORD 'secure_password';
   GRANT ALL PRIVILEGES ON DATABASE fanexiddb TO fanexid;
   ```

2. **Update Environment:**
   ```env
   DATABASE_URL=postgresql://fanexid:secure_password@localhost:5432/fanexiddb
   ```

3. **Run Migrations:**
   ```bash
   docker-compose exec backend alembic upgrade head
   ```

### 3. Integration Configuration

1. **Access Integrations:**
   - Go to **Integrations** page
   - Browse available integrations

2. **Install Integrations:**
   - Click **Install** on desired integration
   - Configure credentials
   - Test connection

3. **Configure System Integrations:**
   - Active Directory
   - Microsoft Entra ID
   - SMTP/Email
   - GitHub (for updates)

## Verification

### Health Checks

1. **Backend Health:**
   ```bash
   curl http://localhost:8000/api/system/status
   ```

2. **Frontend Access:**
   - Open browser to frontend URL
   - Verify login page loads

3. **Database Connection:**
   - Check backend logs for database errors
   - Verify migrations completed

### Functional Tests

1. **Login Test:**
   - Login with admin credentials
   - Verify dashboard loads

2. **API Test:**
   - Access API documentation: `http://localhost:8000/docs`
   - Test API endpoints

3. **Integration Test:**
   - Install test integration
   - Verify integration works

## Troubleshooting

### Common Issues

1. **Port Already in Use:**
   - Check what's using the port: `lsof -i :8000`
   - Change port in docker-compose.yml
   - Stop conflicting service

2. **Database Connection Failed:**
   - Verify database is running
   - Check connection string
   - Verify credentials
   - Check firewall rules

3. **Container Won't Start:**
   - Check logs: `docker-compose logs`
   - Verify environment variables
   - Check disk space
   - Review resource limits

4. **Frontend Not Loading:**
   - Check backend is running
   - Verify API URL in frontend config
   - Check browser console for errors
   - Verify CORS settings

## Next Steps

After successful installation:

1. **Security Hardening:** [Security Guide](security.md)
2. **Production Deployment:** [Deployment Guide](deployment.md)
3. **Monitoring Setup:** [Maintenance Guide](maintenance.md)
4. **Backup Configuration:** [Maintenance Guide](maintenance.md#backup-strategy)

---

**Installation complete?** Proceed to the [Deployment Guide](deployment.md) for production setup.
