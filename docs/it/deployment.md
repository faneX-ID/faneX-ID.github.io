# Deployment Guide

This guide covers production deployment strategies for faneX-ID, including high availability, scaling, and best practices.

## Deployment Strategies

### Single Server Deployment

**Use Case:** Small to medium organizations, development/testing

**Architecture:**
```
┌─────────────────────────────────┐
│     Single Server               │
│  ┌──────────┐  ┌──────────┐    │
│  │ Frontend │  │ Backend  │    │
│  └──────────┘  └──────────┘    │
│       │              │          │
│       └──────┬───────┘          │
│              │                  │
│         ┌────▼────┐             │
│         │Database │             │
│         └─────────┘             │
└─────────────────────────────────┘
```

**Pros:**
- Simple setup
- Low resource requirements
- Easy maintenance

**Cons:**
- Single point of failure
- Limited scalability
- No redundancy

### High Availability Deployment

**Use Case:** Production environments requiring uptime

**Architecture:**
```
┌──────────────┐      ┌──────────────┐
│ Load Balancer│      │ Load Balancer│
└──────┬───────┘      └──────┬───────┘
       │                     │
   ┌───▼───┐             ┌───▼───┐
   │Frontend│            │Backend │
   │Server 1│            │Server 1│
   └───┬───┘             └───┬───┘
       │                     │
   ┌───▼───┐             ┌───▼───┐
   │Frontend│            │Backend │
   │Server 2│            │Server 2│
   └───────┘             └───┬───┘
                             │
                    ┌────────▼────────┐
                    │  Database       │
                    │  (Replicated)   │
                    └─────────────────┘
```

**Components:**
- Load balancer (nginx, HAProxy, or cloud LB)
- Multiple frontend instances
- Multiple backend instances
- Replicated database (PostgreSQL streaming replication)

### Container Orchestration

**Use Case:** Large-scale deployments, cloud environments

**Options:**
- **Kubernetes:** Full orchestration, auto-scaling
- **Docker Swarm:** Simpler orchestration
- **Cloud Services:** AWS ECS, Azure Container Instances, GCP Cloud Run

## Production Checklist

### Pre-Deployment

- [ ] Hardware requirements met
- [ ] Network configuration complete
- [ ] SSL certificates obtained
- [ ] Database configured and tested
- [ ] Backup strategy defined
- [ ] Monitoring tools configured
- [ ] Security hardening applied
- [ ] Documentation reviewed

### Deployment Steps

1. **Infrastructure Setup:**
   - Provision servers
   - Configure networking
   - Set up load balancer
   - Configure firewall rules

2. **Database Setup:**
   - Install PostgreSQL
   - Configure replication (if HA)
   - Set up backups
   - Test connectivity

3. **Application Deployment:**
   - Deploy containers/images
   - Configure environment variables
   - Run database migrations
   - Verify services start

4. **Load Balancer Configuration:**
   - Configure upstream servers
   - Set up SSL termination
   - Configure health checks
   - Test load balancing

5. **DNS Configuration:**
   - Point domain to load balancer
   - Configure SSL certificates
   - Test DNS resolution

6. **Verification:**
   - Health check endpoints
   - Functional testing
   - Performance testing
   - Security scanning

## Docker Compose Production Setup

### Production docker-compose.yml

```yaml
version: '3.8'

services:
  backend:
    image: ghcr.io/fanex-id/core/backend:latest
    restart: always
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/fanexiddb
      - SECRET_KEY=${SECRET_KEY}
      - DEBUG=false
    depends_on:
      - db
    networks:
      - fanexid-network

  frontend:
    image: ghcr.io/fanex-id/core/frontend:latest
    restart: always
    environment:
      - VITE_API_URL=https://api.yourdomain.com
    depends_on:
      - backend
    networks:
      - fanexid-network

  db:
    image: postgres:15
    restart: always
    environment:
      - POSTGRES_USER=fanexid
      - POSTGRES_PASSWORD=${DB_PASSWORD}
      - POSTGRES_DB=fanexiddb
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - fanexid-network

  nginx:
    image: nginx:alpine
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - frontend
      - backend
    networks:
      - fanexid-network

volumes:
  postgres-data:

networks:
  fanexid-network:
    driver: bridge
```

### Nginx Configuration

```nginx
upstream backend {
    server backend:8000;
}

upstream frontend {
    server frontend:3000;
}

server {
    listen 80;
    server_name yourdomain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    # Frontend
    location / {
        proxy_pass http://frontend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Backend API
    location /api {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

## Scaling Strategies

### Horizontal Scaling

**Frontend Scaling:**
- Deploy multiple frontend instances
- Use load balancer for distribution
- Stateless design allows easy scaling

**Backend Scaling:**
- Deploy multiple backend instances
- Use load balancer with session affinity (if needed)
- Ensure database connection pooling

### Vertical Scaling

- Increase server resources (CPU, RAM)
- Upgrade database server
- Optimize application performance

### Database Scaling

- **Read Replicas:** For read-heavy workloads
- **Connection Pooling:** Optimize database connections
- **Query Optimization:** Improve query performance
- **Caching:** Implement Redis for caching

## Monitoring & Logging

### Application Monitoring

1. **Health Endpoints:**
   - `/api/system/status` - System health
   - `/api/system/metrics` - Performance metrics

2. **Logging:**
   - Application logs
   - Access logs
   - Error logs
   - Audit logs

3. **Metrics:**
   - Response times
   - Error rates
   - Resource usage
   - Database performance

### Monitoring Tools

- **Prometheus + Grafana:** Metrics and visualization
- **ELK Stack:** Log aggregation and analysis
- **Sentry:** Error tracking
- **Uptime Monitoring:** External monitoring services

## Backup & Recovery

### Backup Strategy

1. **Database Backups:**
   - Daily full backups
   - Hourly incremental backups
   - Off-site backup storage

2. **Configuration Backups:**
   - Export system configuration
   - Backup SSL certificates
   - Document custom settings

3. **Application Backups:**
   - Container images
   - Configuration files
   - Custom integrations

### Recovery Procedures

1. **Database Recovery:**
   - Restore from backup
   - Verify data integrity
   - Test application functionality

2. **Full System Recovery:**
   - Restore infrastructure
   - Deploy application
   - Restore database
   - Verify functionality

## Security Considerations

1. **Network Security:**
   - Firewall rules
   - VPN access
   - DDoS protection

2. **Application Security:**
   - SSL/TLS encryption
   - Security headers
   - Rate limiting
   - Input validation

3. **Access Control:**
   - Strong authentication
   - Role-based access
   - Audit logging
   - Regular security audits

## Performance Optimization

1. **Caching:**
   - Redis for session storage
   - CDN for static assets
   - Application-level caching

2. **Database Optimization:**
   - Index optimization
   - Query optimization
   - Connection pooling

3. **Application Optimization:**
   - Code optimization
   - Resource optimization
   - Load balancing

## Maintenance Windows

1. **Schedule Maintenance:**
   - Plan during low-usage periods
   - Notify users in advance
   - Prepare rollback plan

2. **Update Procedures:**
   - Test in staging first
   - Backup before updates
   - Deploy during maintenance window
   - Verify functionality
   - Monitor for issues

---

**Ready for production?** Review the [Security Guide](security.md) and [Maintenance Guide](maintenance.md) for ongoing operations.


