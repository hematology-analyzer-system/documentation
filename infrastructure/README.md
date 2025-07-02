# Healthcare Project Documentation

## Infrastructure

**Status**: ✅ Complete  
**Owner**: DevOps Team  
**Last Updated**: 02 July 2025

### Overview
Automated cloud infrastructure for hosting healthcare microservices using DigitalOcean + Docker.

### What's Implemented
- **Cloud Provisioning**: Terraform-managed DigitalOcean droplets (Ubuntu 22.04, Singapore region)
- **Configuration Management**: Ansible playbooks for Docker host setup
- **Team Access**: Multi-user SSH key management for secure server access
- **Container Runtime**: Docker + Docker Compose installed and configured
- **Development Tools**: Enhanced shell environment with monitoring utilities

### Infrastructure Components
```
infrastructure/
├── terraform/             # Cloud resource provisioning
│   ├── main.tf            # DO droplet + SSH key definitions  
│   ├── scripts/           # Deployment automation
│   └── keys/              # Team SSH public keys
└── ansible/               # Server configuration
    ├── init_playbook.yml  # Docker + tools installation
    └── scripts/           # Execution wrappers
```

### Usage
1. **Deploy**: `terraform/scripts/apply.sh` - Provisions servers and runs configuration
2. **Destroy**: `terraform/scripts/destroy.sh` - Tears down all resources
3. **Access**: SSH to servers using configured team keys

### Server Specifications
- **Instance**: s-2vcpu-8gb-amd (2 vCPU, 8GB RAM)
- **OS**: Ubuntu 22.04 LTS
- **Region**: Singapore (sgp1)
- **Software**: Docker, Docker Compose, development utilities
