# Hematology Analyzer Management
Microservices system for managing hematology laboratory analyzers and test data.

## Tech Stack
**Application Stack**
- Frontend: React + TypeScript + Next.js + Tailwind CSS
- Backend: Java Spring Boot
- Database: DigitalOcean Managed PostgreSQL
- Message Broker: RabbitMQ
- API Testing: Swagger, Postman
- Unit Testing: TBD

**Infrastructure & DevOps**
- Platform: DigitalOcean Droplets
- Containerization: Docker
- Orchestration: Docker Swarm
- Provisioning: Terraform + Ansible + Bash Scripts
- CI/CD: GitLab CI
- Reverse Proxy: Nginx with SSL/TLS
- TLS/SSL: Let's Encrypt (automated renewal)
- DNS & CDN: CloudFlare (IP-based access control)
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
