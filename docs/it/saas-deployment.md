# SaaS & Hybrid Deployment Guide

faneX-ID can be deployed as a Software-as-a-Service (SaaS) solution, allowing organizations to leverage cloud infrastructure while maintaining control over sensitive on-premises data through hybrid deployments.

## Overview

faneX-ID supports three deployment models:

1. **On-Premises**: Full control, all data stays local
2. **Cloud/SaaS**: Managed infrastructure, automatic scaling
3. **Hybrid**: Best of both worlds - cloud management with on-premises data

## Hybrid Deployment Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    faneX-ID Cloud Platform                  │
│  (SaaS Instance - Management, Orchestration, UI)          │
└───────────────────────┬───────────────────────────────────┘
                        │
                        │ HTTPS/TLS
                        │ API Token Auth
                        │
        ┌───────────────┴───────────────┐
        │                               │
┌───────▼────────┐            ┌─────────▼──────────┐
│ Primary        │            │ Backup             │
│ Connector      │            │ Connector          │
│ (OnPrem)       │            │ (OnPrem)            │
└───────┬────────┘            └─────────┬──────────┘
        │                               │
        │ Local Network                 │
        │                               │
┌───────▼────────┐            ┌─────────▼──────────┐
│ Active         │            │ Active              │
│ Directory      │            │ Directory           │
│ (OnPrem)       │            │ (OnPrem)            │
└────────────────┘            └─────────────────────┘
```

## OnPrem Connector Setup

### Step 1: Download and Install Connector

**Windows:**
1. Download the latest Windows release
2. Extract to `C:\Program Files\faneX-ID-Connector\`
3. Run `install.bat` as Administrator
4. Configure `config.ini`

**Linux:**
```bash
wget https://github.com/faneX-ID/onprem-connector/releases/latest/download/connector-linux.tar.gz
tar -xzf connector-linux.tar.gz
cd fanexid-connector
sudo ./install.sh
```

### Step 2: Generate API Token

1. Log into your faneX-ID SaaS instance
2. Navigate to **Admin → API Tokens**
3. Click **Create New Token**
4. Configure:
   - **Name**: Descriptive name (e.g., "Main Office Connector")
   - **Role**: Select appropriate RBAC role
   - **Permissions**: Configure specific permissions
   - **Expiration**: Set expiration date (optional)
   - **IP Whitelist**: Restrict to specific IPs (optional)
5. Copy the generated token (shown only once!)

### Step 3: Configure Connector

Edit `config.ini`:

```ini
[connector]
name = Main-Office-Connector
role = primary
version = 1.0.0

[cloud]
api_url = https://fanexid.example.com/api
api_token = fanexid_your_token_here
verify_ssl = true
timeout = 30

[network]
mode = internet  # or "private" for VPN/direct connection
listen_host = 0.0.0.0
listen_port = 8080
```

### Step 4: Register Connector

Register the connector in faneX-ID:

```bash
curl -X POST https://fanexid.example.com/api/plugins/onprem_connector/register_connector \
  -H "Authorization: Bearer YOUR_ADMIN_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "connector_id": "main-office-01",
    "name": "Main Office Connector",
    "api_url": "https://connector.example.com",
    "api_token": "fanexid_xxxxxxxxxxxx",
    "role": "primary",
    "priority": 1
  }'
```

## Network Configuration

### Private Network Mode

For connections within a private network (VPN, direct connection):

**Advantages:**
- Lower latency
- No internet dependency
- Can use HTTP (if network is trusted)

**Configuration:**
```ini
[network]
mode = private
api_url = http://10.0.1.100:8000/api  # Internal IP
verify_ssl = false  # Optional for private networks
```

**Setup:**
1. Ensure faneX-ID is accessible via private network
2. Configure VPN or direct network connection
3. Use internal IP addresses
4. Optionally disable SSL verification (only for trusted networks)

### Internet Mode

For connections over the public internet:

**Advantages:**
- No VPN required
- Works from anywhere
- Standard HTTPS security

**Configuration:**
```ini
[network]
mode = internet
api_url = https://fanexid.example.com/api
verify_ssl = true
cert_file = /path/to/ca-cert.pem  # Optional custom CA
```

**Setup:**
1. Ensure faneX-ID has valid SSL certificate
2. Configure firewall rules
3. Use HTTPS only
4. Enable IP whitelisting for additional security

## Primary/Backup Configuration

Configure multiple connectors for high availability:

### Primary Connector

```ini
[connector]
name = Primary-Connector
role = primary
priority = 1
```

### Backup Connector

```ini
[connector]
name = Backup-Connector
role = backup
priority = 2
```

The faneX-ID platform automatically:
- Uses primary connector by default
- Fails over to backup if primary is unavailable
- Load balances between connectors (if configured)

## Security Best Practices

### Token Security

1. **Store Securely**
   - Use environment variables in production
   - Encrypt configuration files
   - Never commit tokens to version control

2. **Rotate Regularly**
   - Set expiration dates
   - Rotate tokens every 90 days
   - Revoke compromised tokens immediately

3. **Limit Permissions**
   - Use RBAC to grant minimal required permissions
   - Use IP whitelisting when possible
   - Monitor token usage in audit logs

### Network Security

1. **Use HTTPS**
   - Always use HTTPS for internet connections
   - Verify SSL certificates
   - Use custom CA certificates if needed

2. **Firewall Rules**
   - Restrict connector ports to necessary IPs
   - Use VPN for private network connections
   - Implement network segmentation

3. **Monitoring**
   - Monitor connection health
   - Set up alerts for failures
   - Review audit logs regularly

## Troubleshooting

### Connection Issues

**Problem:** Cannot connect to cloud platform

**Solutions:**
- Verify `api_url` is correct
- Check network connectivity (`ping`, `curl`)
- Verify firewall rules
- Check DNS resolution

### Authentication Errors

**Problem:** Token authentication fails

**Solutions:**
- Verify token hasn't expired
- Check token is active in admin panel
- Verify token permissions
- Regenerate token if needed

### High Latency

**Problem:** Slow response times

**Solutions:**
- Check network bandwidth
- Verify connector location
- Consider using backup connector in different location
- Optimize network path

## Monitoring

### Health Checks

The connector exposes health check endpoints:

```bash
# Basic health check
curl http://localhost:8080/health

# Detailed status
curl http://localhost:8080/status

# Prometheus metrics
curl http://localhost:8080/metrics
```

### faneX-ID Integration

The `onprem_connector` integration automatically:
- Checks connector health every 5 minutes
- Updates connector status
- Logs connection issues
- Triggers alerts on failures

## Examples

### Example 1: Single Office Setup

```
Cloud Platform (SaaS)
    ↓
Primary Connector (Main Office)
    ↓
Active Directory (OnPrem)
```

### Example 2: Multi-Office with Backup

```
Cloud Platform (SaaS)
    ↓
    ├─→ Primary Connector (Office A)
    │       ↓
    │   AD (Office A)
    │
    └─→ Backup Connector (Office B)
            ↓
        AD (Office B)
```

### Example 3: Internet + VPN Hybrid

```
Cloud Platform (SaaS)
    ↓
    ├─→ Connector A (Internet)
    │       ↓
    │   AD (Remote Office)
    │
    └─→ Connector B (VPN)
            ↓
        AD (HQ)
```

## Support

- **Repository**: [faneX-ID/onprem-connector](https://github.com/faneX-ID/onprem-connector)
- **Documentation**: [OnPrem Connector Installation Guide](onprem-connector.md)
- **Issues**: [GitHub Issues](https://github.com/faneX-ID/onprem-connector/issues)
- **Community**: [GitHub Discussions](https://github.com/faneX-ID/onprem-connector/discussions)
