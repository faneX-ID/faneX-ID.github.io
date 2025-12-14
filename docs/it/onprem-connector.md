# OnPrem Connector Installation & Configuration

Complete guide for installing and configuring the faneX-ID OnPrem Connector.

## Quick Start

1. **Download** the connector for your platform
2. **Install** using the provided scripts
3. **Configure** `config.ini` with your API token
4. **Register** the connector in faneX-ID admin panel
5. **Start** the connector service

## Installation

### Windows Installation

1. Download the Windows release
2. Extract to `C:\Program Files\faneX-ID-Connector\`
3. Run PowerShell as Administrator:
   ```powershell
   cd "C:\Program Files\faneX-ID-Connector"
   .\install.bat
   ```
4. Configure `config.ini`
5. Start the service:
   ```cmd
   net start fanexid-connector
   ```

### Linux Installation

```bash
# Download
wget https://github.com/faneX-ID/onprem-connector/releases/latest/download/connector-linux.tar.gz

**Repository**: [faneX-ID/onprem-connector](https://github.com/faneX-ID/onprem-connector)

# Extract
tar -xzf connector-linux.tar.gz
cd fanexid-connector

# Install
sudo ./install.sh

# Configure
sudo nano /etc/fanexid-connector/config.ini

# Start service
sudo systemctl start fanexid-connector
sudo systemctl enable fanexid-connector
```

## Configuration

### Basic Configuration

```ini
[connector]
name = MyCompany-Connector
role = primary
version = 1.0.0

[cloud]
api_url = https://fanexid.example.com/api
api_token = fanexid_your_token_here
verify_ssl = true
timeout = 30

[network]
listen_host = 0.0.0.0
listen_port = 8080
allowed_ips = 10.0.0.0/8,192.168.0.0/16

[security]
token_refresh_interval = 3600
max_retries = 3
retry_delay = 5

[logging]
level = INFO
file = logs/connector.log
max_size = 10MB
backup_count = 5
```

### Network Modes

#### Private Network

```ini
[network]
mode = private
api_url = http://10.0.1.100:8000/api
verify_ssl = false
```

#### Internet

```ini
[network]
mode = internet
api_url = https://fanexid.example.com/api
verify_ssl = true
cert_file = /path/to/ca-cert.pem
```

## API Token Setup

See [SaaS Deployment Guide](saas-deployment.md) for detailed token setup instructions.

## Verification

### Check Connector Status

```bash
# Health check
curl http://localhost:8080/health

# Detailed status
curl http://localhost:8080/status
```

### Register in faneX-ID

Use the admin panel or API to register the connector. See [SaaS Deployment Guide](saas-deployment.md) for details.

## Troubleshooting

See [SaaS Deployment Guide - Troubleshooting](saas-deployment.md#troubleshooting) section.
