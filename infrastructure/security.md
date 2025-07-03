# Security Configuration

## Overview
Our infrastructure implements minimal security measures suitable for demo/development environments. All servers are hardened with basic protections against automated attacks.

## Implemented Security Measures

### Firewall (UFW)
- **Default policy**: Deny all incoming traffic
- **Open ports**: 
  - 80 (HTTP)
  - 443 (HTTPS) 
  - 1309 (SSH)
- **Status check**: `sudo ufw status`

### SSH Hardening
- **Port**: Changed from 22 to 1309
- **Authentication**: SSH keys only (password auth disabled)
- **Root login**: Disabled
- **Access**: Non-root user only

### Fail2Ban Protection
Active jails protect against brute force attacks
**SSH Protection**: 3 failed attempts within 10 minutes results in permanent ban

## Team Access

### SSH Connection
```bash
ssh -p 1309 username@server-ip
```

### Authorized Users
SSH keys configured for:
- khoa
- anh

## Security Monitoring

### Check for Compromise
```bash
# System load
uptime

# Network connections
ss -tuln | wc -l

# Listening services
netstat -tlnp

# Current users
who

# Failed login attempts
sudo lastb
```

### Expected Baseline
- Load average: < 0.5
- Network connections: < 20
- Only expected services listening (SSH:1309, HTTP:80, DNS:53)

## Security Limitations
⚠️ **Demo Environment Only**

## Emergency Procedures

### Suspected Compromise
1. Check system load: `uptime`
2. Identify suspicious processes: `htop` (sort by CPU)
3. Check network activity: `ss -tuln`
4. Review failed logins: `sudo lastb`
5. If compromised: destroy and recreate server

### Fail2Ban Management
```bash
# Check status
sudo fail2ban-client status

# Unban IP
sudo fail2ban-client set sshd unbanip IP_ADDRESS

# Check banned IPs
sudo fail2ban-client status sshd
```
