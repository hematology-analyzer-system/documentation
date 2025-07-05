# Proxy Configuration

## Overview
**Status**: ✅ Complete  
**Owner**: DevOps Team  
**Last Updated**: 05 July 2025

Nginx reverse proxy configuration for routing traffic to microservices with SSL/TLS termination.

## Configuration Details

### SSL/TLS Setup
- **Domain**: [droplet.khoa.email](https://droplet.khoa.email)
- **Certificate**: Let's Encrypt (auto-renewal)
- **Ports**: 443 (HTTPS), 80 (HTTP redirect)
- **Security**: Strong SSL configuration with HSTS

### Service Routing (via Nginx)

All external API requests are prefixed with `/api/<service>`.

| Public Path           | Internal Target              | Service Name                      |
|-----------------------|------------------------------|-----------------------------------|
| `/api/iam/`           | `localhost:8080/`            | Identity & Access Management      |
| `/api/patient/`       | `localhost:8081/`            | Patient Management                |
| `/api/testorder/`     | `localhost:8082/`            | Test Order Management             |

**Note**: Nginx strips the `/api/<service>/` prefix before forwarding to the backend.  
For example:  
`/api/iam/login` → forwarded as `/login` to `localhost:8080`


### Configuration File
Located at: `infrastructure/nginx/reverse_proxy.conf`

## Security Features
- SSL/TLS encryption for all traffic
- Proper proxy headers for backend services
- 404 response for undefined routes
- IPv6 support

## Deployment
1. Copy configuration to `/etc/nginx/sites-available/`
2. Create symbolic link to `/etc/nginx/sites-enabled/`
3. Test configuration: `nginx -t`
4. Reload: `systemctl reload nginx`

## Monitoring
- Check status: `systemctl status nginx`
- View logs: `journalctl -u nginx`
- SSL certificate expiry: `certbot certificates`

## Maintenance
- Certificate renewal: Automated via certbot
- Configuration updates: Restart nginx after changes
- Log rotation: Handled by logrotate
