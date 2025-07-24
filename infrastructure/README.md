# Cloud Infrastructure

**Status**: ✅ Complete
**Owner**: DevOps Team  
**Last Updated**: 22 July 2025

## Overview
Terraform + Ansible automated setup for DigitalOcean droplets running healthcare microservices via Docker Swarm.

## Stack
- **Infrastructure**: Ubuntu 24.04, 4 vCPU, 8GB RAM (Singapore)
- **Orchestration**: Docker Swarm, 2 replicas per service
- **Security**: SSH hardening, UFW + Cloudflare IP restrictions, Let's Encrypt SSL
- **Proxy**: Nginx with rate limiting and automated certificate renewal

## Structure
```
infrastructure/
├── terraform/main.tf         # Droplet + SSH keys
├── ansible/init_playbook.yml # Complete server setup  
├── swarm/compose.yml         # Service definitions
├── nginx/reverse_proxy.conf  # SSL proxy config
└── scripts/                  # IP restriction automation
```

## Services
- **IAM**: 2 replicas on port 8080
- **Patient**: 2 replicas on port 8081  
- **Test Order**: Disabled

## Usage
1. Set `DIGITALOCEAN_TOKEN` → `terraform/scripts/apply.sh`
2. Run `ansible/scripts/init.sh`
3. SSH to server → `/opt/microservices/scripts/deploy.sh`

---

# Nginx Reverse Proxy Configuration

Nginx reverse proxy configuration for routing traffic to microservices with SSL/TLS termination and Cloudflare integration.

## Configuration Details

### SSL/TLS Setup
- **Domain**: [microservices.khoa.email](https://microservices.khoa.email)
- **Certificate**: Let's Encrypt (automated renewal via certbot)
- **Ports**: 443 (HTTPS), 80 (HTTP redirect)

### Service Routing
All external API requests are path-based with URL rewriting:

| Public Path           | Internal Target                    | Service Name                      |
|-----------------------|------------------------------------|-----------------------------------|
| /auth/                | localhost:8080/iam/auth/          | Identity & Access Management      |
| /api/patients/        | localhost:8081/patient/patients/  | Patient Management                |
| /api/testorders/      | localhost:8082/testorder/         | Test Order Management             |

**URL Rewriting**: Nginx rewrites paths before forwarding to backend services.
Example: `/auth/login` → forwarded as `/iam/auth/login` to localhost:8080

### Configuration File
Located at: `/etc/nginx/sites-available/default` (copied from `nginx/reverse_proxy.conf`)

## Security Features
- **SSL/TLS**: Automatic HTTPS redirect with HSTS headers
- **Rate Limiting**: 5 req/s for site, 2 req/s for API endpoints
- **User Agent Blocking**: Blocks curl, wget, scanners, bots, automated tools
- **Cloudflare Integration**: Dynamic geo-blocking with real IP detection
- **Security Headers**: XSS protection, content type sniffing prevention, frame options
- **File Upload**: 10MB limit
- **Access Control**: Blocks dotfiles, actuator endpoints, Swagger UI
- **Method Restriction**: Only GET, POST, HEAD allowed
- **Server Info**: Nginx version hidden

## Cloudflare Integration
- **IP Allowlisting**: Only Cloudflare IPs allowed on port 443
- **Dynamic Updates**: Weekly cron job updates both nginx geo-blocking and UFW rules
- **Real IP Detection**: Uses CF-Connecting-IP header for accurate client IPs

## Deployment
1. Configuration deployed via Ansible to `/etc/nginx/sites-available/default`
2. Automatic nginx configuration testing and reload
3. Cloudflare IPs auto-generated at `/etc/nginx/conf.d/cloudflare-ips.conf`

## Monitoring
- Check status: `systemctl status nginx`
- View logs: `journalctl -u nginx`
- SSL certificate expiry: `certbot certificates`
- Test configuration: `nginx -t`

## Maintenance
- Certificate renewal: Fully automated via certbot cron
- IP updates: Weekly automated via `/opt/scripts/update-ips-nginx.sh`
- Configuration changes: Automatic validation and reload via Ansible

---

# Security Configuration

Multi-layered security configuration for healthcare microservices infrastructure with automated threat protection.

## Implemented Security Measures

### Network Security
- **UFW Firewall**: Default deny-all with selective access
  - Port 80/443: Cloudflare IPs only (dynamic allowlisting)
  - Port 1309: SSH access (custom port)
- **Rate Limiting**: Nginx-based request throttling (5 req/s site, 2 req/s API)
- **DDoS Protection**: Cloudflare proxy integration with real IP detection

### SSH Hardening
- **Custom Port**: 1309 (changed from default 22)
- **Authentication**: SSH keys only (password auth disabled)
- **Root Access**: Completely disabled

### Application Security
- **Docker Secrets**: Database credentials and sensitive configuration
- **SSL/TLS**: Let's Encrypt certificates with strong cipher suites and HSTS
- **Path Blocking**: Actuator, admin, debug, and Swagger endpoints blocked
- **User Agent Filtering**: Automated blocking of scanners, bots, and tools
- **Method Restriction**: Only GET, POST, HEAD requests allowed

### Access Control
- **Team SSH Keys**: Authorized access for khoa and anh
- **Service Isolation**: Docker Swarm network segmentation
- **Cloudflare Proxy**: All traffic routed through Cloudflare CDN

## Automated Security Updates
- **Cloudflare IPs**: Weekly nginx geo-blocking updates (Mondays 2:00 AM)
- **UFW Rules**: Weekly firewall synchronization (Mondays 2:30 AM)  
- **SSL Certificates**: Automated Let's Encrypt renewal (daily 12:00 PM)
- **System Updates**: Disabled automatic updates for stability

## Team Access
**SSH Connection**: `ssh -p 1309 <user>@<server-ip>`
**Authorized Users**: khoa, anh (SSH key authentication only)
