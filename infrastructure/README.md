# Cloud Infrastructure for Healthcare Microservices

**Status**: ✅ Production Ready  
**Owner**: DevOps Team  
**Last Updated**: 05 August 2025  
**Region**: Singapore (sgp1)

## Overview

Enterprise-grade Docker Swarm infrastructure on DigitalOcean with automated Terraform/Ansible provisioning for healthcare microservices. Features multi-node cluster, load balancing, SSL termination, and comprehensive security hardening.

## Architecture

### Infrastructure Components
- **Manager Node**: 4 vCPU, 8GB RAM, Ubuntu 24.04 LTS
- **Worker Node**: 2 vCPU, 4GB RAM, Ubuntu 24.04 LTS  
- **VPC**: Private networking (10.100.0.0/24) with internal service communication
- **Load Balancer**: DigitalOcean LB with Cloudflare integration
- **DNS**: Cloudflare-managed with automatic proxy and DDoS protection

### Service Architecture
```
Internet → Cloudflare → DigitalOcean LB → Nginx Proxy → Docker Swarm Services
```

## Services Configuration

### Resource Allocation
| Service | Replicas | CPU Limit | Memory Limit | Memory Reserve | Port |
|---------|----------|-----------|--------------|----------------|------|
| IAM Service | 2 | 1.0 | 896M | 640M | 8080 |
| Patient Service | 2 | 0.25 | 1.25G | 896M | 8081 |
| Test Order Service | 2 | 0.375 | 1.25G | 896M | 8082 |
| Nginx Proxy | 1 | - | - | - | 80 |

### Health Checks
- **Interval**: 30s
- **Timeout**: 10s  
- **Retries**: 3
- **Start Period**: 120s
- **Endpoint**: `/actuator/health`

## API Gateway & Routing

### Public Domain
**Primary**: [fhard.khoa.email](https://fhard.khoa.email)

### Path-Based Routing
| Public Endpoint | Internal Target | Service | URL Rewriting |
|----------------|-----------------|---------|---------------|
| `/api/iam/*` | `iam-service:8080/iam/*` | Identity & Access | `/api/iam/` → `/iam/` |
| `/api/patients/*` | `patient-service:8081/patient/patients/*` | Patient Management | `/api/patients/` → `/patient/patients/` |
| `/api/testorders/*` | `testorder-service:8082/testorder/*` | Test Orders | `/api/testorders/` → `/testorder/` |

### Load Balancer Configuration
- **SSL Termination**: Cloudflare origin certificate
- **Protocol**: HTTPS (443) → HTTP (80)
- **Health Check**: HTTP GET `/` every 10s
- **Firewall**: Cloudflare IPs only
- **Features**: HTTP→HTTPS redirect, real IP detection

## Security Implementation

### Network Security
- **VPC Isolation**: All traffic within private 10.100.0.0/24 network
- **Firewall Rules**:
  - SSH: Custom port (configurable via `variables.tf`)
  - HTTP/HTTPS: Cloudflare IPs only at load balancer
  - Swarm Communication: Ports 2377, 7946, 4789 (internal only)
  - ICMP: Ping allowed globally
- **DDoS Protection**: Cloudflare proxy with rate limiting

### Access Control  
- **SSH Authentication**: Ed25519 keys only, password auth disabled
- **Root Access**: Completely disabled
- **Authorized Users**: khoa, anh, phong
- **Connection**: `ssh -p <custom_port> <user>@<server_ip>`

### Application Security
- **Secrets Management**: Docker Swarm secrets for sensitive data
- **Path Blocking**: Actuator, admin, debug, Swagger endpoints blocked
- **Request Methods**: Only GET, POST, HEAD, PUT, DELETE allowed
- **File Upload**: 10MB limit
- **Headers**: Security headers (XSS, HSTS, CSP) enabled

## Project Structure

```
kchan139-infra-healthcare/
├── README.md
├── LICENSE
├── terraform/
│   ├── main.tf              # Droplets, SSH keys
│   ├── vpc.tf               # Virtual Private Cloud
│   ├── nodes.tf             # Manager + Worker configuration
│   ├── loadbalancer.tf      # LB + SSL certificate
│   ├── firewall.tf          # Security rules
│   ├── dns.tf               # Cloudflare DNS records
│   ├── variables.tf         # Input variables
│   ├── outputs.tf           # Output values
│   └── providers.tf         # Provider configuration
├── ansible/
│   ├── setup.yml            # Server provisioning
│   ├── playbook.yml         # Docker Swarm + secrets
│   ├── vars/
│   │   ├── secrets.yml      # Environment variables (encrypted)
│   │   └── example.secrets.yml
│   └── scripts/
│       ├── init.sh          # Full deployment script
│       └── re-run.sh        # Re-run playbooks only
└── swarm/
    ├── compose.yml          # Docker Swarm stack definition
    ├── nginx/
    │   ├── nginx.conf       # Reverse proxy configuration
    │   ├── wait-for-dns.sh  # Service discovery wait script
    │   └── html/            # Static welcome page
    └── scripts/
        ├── deploy.sh        # Deploy microservices stack
        └── cleanup.sh       # Remove all stacks
```

## Deployment Guide

### Prerequisites
- DigitalOcean API token
- Cloudflare API token & Zone ID
- SSH keys configured
- Terraform & Ansible installed

### 1. Infrastructure Provisioning

```bash
# Clone repository
git clone <repository_url>
cd kchan139-infra-healthcare

# Configure Terraform variables
cp terraform/terraform.tfvars.example terraform/terraform.tfvars
# Edit terraform.tfvars with your values

# Configure Ansible secrets
cp ansible/vars/example.secrets.yml ansible/vars/secrets.yml
ansible-vault edit ansible/vars/secrets.yml

# Create environment file
cp ansible/example.env ansible/.env
# Edit .env with SSH_PORT and USER_NAME

# Deploy infrastructure
cd terraform
terraform init
terraform plan
terraform apply

# Provision servers
cd ../ansible
./scripts/init.sh
# Enter vault password when prompted
# Enter sudo password for new user when prompted
```

### 2. Service Deployment

```bash
# SSH to manager node
ssh -p <custom_port> <username>@<manager_ip>

# Deploy microservices
cd /opt/microservices
./scripts/deploy.sh

# Verify deployment
docker service ls
docker stack ps microservices-stack
```

### 3. Verification & Monitoring

```bash
# Check service health
curl https://fhard.khoa.email/api/iam/actuator/health

# Monitor logs
docker service logs microservices-stack_iam-service
docker service logs microservices-stack_patient-service
docker service logs microservices-stack_testorder-service

# Check load balancer
curl -I https://fhard.khoa.email

# Verify SSL
openssl s_client -connect fhard.khoa.email:443 -servername fhard.khoa.email
```

## Operations & Maintenance

### Service Management
```bash
# Scale services
docker service scale microservices-stack_iam-service=3

# Update service
docker service update --image new-image:tag microservices-stack_iam-service

# Remove stack
./scripts/cleanup.sh

# Re-deploy
./scripts/deploy.sh
```

### Infrastructure Updates
```bash
# Re-run Ansible configuration
cd ansible
./scripts/re-run.sh

# Update Terraform infrastructure
cd terraform
terraform plan
terraform apply
```

### Monitoring Commands
```bash
# System resources
btop                                   # System monitor
docker system df                       # Docker disk usage
docker system events                   # Real-time events

# Service status
docker service ls                      # All services
docker stack ps microservices-stack    # Stack status
docker node ls                         # Swarm nodes

# Logs
journalctl -u docker                   # Docker daemon logs
docker service logs -f <service>       # Follow service logs
```

### Troubleshooting
```bash
# Check Swarm status
docker info | grep -A 10 "Swarm:"

# Inspect service
docker service inspect microservices-stack_iam-service

# Check network connectivity
docker exec -it $(docker ps -q -f "name=iam-service") ping patient-service

# Nginx configuration test
docker exec $(docker ps -q -f "name=nginx") nginx -t

# Secret verification
docker secret ls
```

## Network Configuration

### Firewall Rules (UFW)
```
Port 22: SSH (initial setup only)
Port <custom>: SSH (production)
Port 80: HTTP (from load balancer)
Port 2377: Docker Swarm management (internal)
Port 7946: Docker node communication (internal TCP/UDP)
Port 4789: Docker overlay network (internal UDP)
```

### Docker Networks
- **microservices-network**: Overlay network for service communication
- **Host networking**: Load balancer health checks
- **Bridge network**: Default Docker networking

## Backup & Recovery

### Automated Backups
- **Frequency**: Weekly (Tuesdays, 8:00 AM SGT)
- **Retention**: 4 weeks
- **Scope**: Full droplet snapshots

### Manual Backup
```bash
# Export Docker secrets
docker secret ls --format "table {{.ID}}\t{{.Name}}" > secrets-backup.txt

# Backup configurations
tar -czf config-backup.tar.gz /opt/microservices/

# Database backup (if applicable)
# Handled by external database service
```

### Disaster Recovery
1. **Infrastructure**: Re-run Terraform apply with backup snapshots
2. **Configuration**: Restore from Git repository
3. **Secrets**: Re-create Docker secrets from vault
4. **Services**: Deploy from latest images in GitLab registry

### Deployment Pipeline
1. Code push triggers GitLab CI
2. Docker images built and pushed to registry
3. Manual deployment via `docker service update`
4. Health checks verify deployment success

---

**Support**: For infrastructure issues, contact the DevOps person or create an issue in the repository.
