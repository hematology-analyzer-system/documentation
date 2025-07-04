# Hematology Analyzer Management
Microservices system for managing hematology laboratory analyzers and test data.

## Tech Stack
**Application Stack**
- Frontend: React + TypeScript + Next.js + Tailwind CSS
- Backend: Java Spring Boot
- Database: PostgreSQL
- Message Broker: Redis Streams
- API Testing: Swagger, Postman
- Unit Testing: TBD
- Real-time Notifications: TBD

**Infrastructure & DevOps**
- Platform: DigitalOcean Droplets
- Containerization: Docker
- Deployment: Docker Compose (Single VM)
- Provisioning: Terraform + Ansible + Bash Scripts
- CI/CD: CircleCI
- Reverse Proxy: Nginx
- TLS/SSL: Certbot via Let's Encrypt
- DNS & CDN: CloudFlare
- Monitoring: SSH + htop + crontab
- Alerts: Uptime Robot

**Development**
- Git: GitLab with GitLab Flow
- Commits: Angular-style convention

## Documentation
### Infrastructure
- [Cloud Infrastructure](infrastructure/README.md) - DigitalOcean setup and provisioning
- [Security Configuration](infrastructure/security.md) - Firewall, SSH hardening, and monitoring
- [Nginx Configuration](infrastructure/proxy.md) - Reverse proxy and SSL setup

### Microservices
