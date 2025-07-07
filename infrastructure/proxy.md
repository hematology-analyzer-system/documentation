# Proxy Configuration

## Overview
**Status**: ✅ Complete  
**Owner**: DevOps Team  
**Last Updated**: 05 July 2025

Nginx reverse proxy configuration for routing traffic to microservices with SSL/TLS termination.

## Configuration Details

### SSL/TLS Setup
- **Domain**: droplet.khoa.email
- **Certificate**: Let's Encrypt (auto-renewal)
- **Ports**: 443 (HTTPS), 80 (HTTP redirect)
- **Security**: Strong SSL configuration with HSTS

### Service Routing
All external API requests are prefixed with `/api/<service>` and forwarded to internal services.

| Public Path           | Internal Target              | Service Name                      |
|-----------------------|------------------------------|-----------------------------------|
| /api/iam/             | localhost:8080/iam/          | Identity & Access Management      |
| /api/patient/         | localhost:8081/patient/      | Patient Management                |
| /api/testorder/       | localhost:8082/testorder/    | Test Order Management             |

**URL Rewriting**: Nginx strips the `/api/` prefix before forwarding to the backend.
Example: `/api/iam/login` → forwarded as `/iam/login` to localhost:8080

### Configuration File
Located at: `infrastructure/nginx/reverse_proxy.conf`

## Security Features
- **SSL/TLS**: Automatic HTTPS redirect with HSTS headers
- **Security Headers**: XSS protection, content type sniffing prevention, frame options
- **File Upload**: 10MB limit
- **Access Control**: Blocks dotfiles, actuator endpoints, and Swagger UI
- **Method Restriction**: Only GET, POST, HEAD allowed
- **Server Info**: Nginx version hidden

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
