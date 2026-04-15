<!--
╔═══════════════════════════════════════════════════════════════════════════╗
║                                                                           ║
║   HNG DEVOPS STAGE 0 - SECURE HARDENED NGINX SERVER                   ║
║                                                                           ║
║   Built without Docker, containers, or automation tools                   ║
║   Just a bare Linux server and hands-on engineering                       ║
║                                                                           ║
╚═══════════════════════════════════════════════════════════════════════════╝
-->

# 🔐 HNG DevOps Stage 0 - Secure Nginx Server

[![Status](https://img.shields.io/badge/status-passed-brightgreen?style=for-the-badge)](https://hng.tech)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-22.04-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com)
[![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)](https://aws.amazon.com)
[![Nginx](https://img.shields.io/badge/Nginx-1.18.0-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://nginx.com)
[![Let's Encrypt](https://img.shields.io/badge/Let's_Encrypt-SSL-316AFE?style=for-the-badge&logo=letsencrypt&logoColor=white)](https://letsencrypt.org)
[![HNG](https://img.shields.io/badge/HNG-Internship-6A0DAD?style=for-the-badge)](https://hng.tech)

<div align="center">
  

**[Live Demo](https://mrrhng.mooo.com)** • **[API Endpoint](https://mrrhng.mooo.com/api)**

</div>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Architecture](#-architecture)
- [Technologies Used](#-technologies-used)
- [Quick Start](#-quick-start)
- [Detailed Setup](#-detailed-setup)
- [Configuration Files](#-configuration-files)
- [Testing & Verification](#-testing--verification)
- [Troubleshooting](#-troubleshooting)
- [Lessons Learned](#-lessons-learned)
- [Screenshots](#-screenshots)
- [Author](#-author)

---

## 🎯 Project Overview

This project demonstrates fundamental DevOps skills by provisioning a **bare Linux server from scratch**, securing it, and deploying a web application **without any automation tools or containers**.

### What I Built

| Component | Implementation |
|-----------|----------------|
| **Cloud Platform** | AWS EC2 (t2.micro, Ubuntu 22.04) |
| **Web Server** | Nginx 1.18.0 |
| **SSL Certificate** | Let's Encrypt (Certbot) |
| **Firewall** | UFW + AWS Security Groups |
| **DNS** | FreeDNS (mrrhng.mooo.com) |
| **Authentication** | SSH Key-based only |

### Endpoints

```http
GET https://mrrhng.mooo.com/
# Returns: Static HTML page with visible username

GET https://mrrhng.mooo.com/api
# Returns: 
{
  "message": "HNGI14 Stage 0",
  "track": "DevOps",
  "username": "Mr. React"
}
```
### 🏗️ Architecture
┌─────────────────────────────────────────────────────────────────────────────┐
│                              INTERNET                                        │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         FreeDNS (mrrhng.mooo.com)                            │
│                           A Record → Elastic IP                              │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              AWS EC2                                         │
│                         Ubuntu 22.04 LTS                                     │
│                         t2.micro (Free Tier)                                 │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│   ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐         │
│   │  AWS Security    │  │      UFW         │  │      SSH         │         │
│   │     Group        │  │    Firewall       │  │    Hardened      │         │
│   │                  │  │                  │  │                  │         │
│   │  ✅ Port 22      │  │  ✅ Port 22      │  │  ✅ Key-only     │         │
│   │  ✅ Port 80      │  │  ✅ Port 80      │  │  ✅ No root      │         │
│   │  ✅ Port 443     │  │  ✅ Port 443     │  │  ✅ No password  │         │
│   └──────────────────┘  └──────────────────┘  └──────────────────┘         │
│                                                                              │
│                          ┌─────────────────────┐                            │
│                          │       Nginx         │                            │
│                          │     Port 443        │                            │
│                          │      (SSL)          │                            │
│                          └──────────┬──────────┘                            │
│                                     │                                        │
│              ┌──────────────────────┼──────────────────────┐                │
│              ▼                      ▼                      ▼                │
│   ┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐       │
│   │     GET /        │   │    GET /api      │   │  Let's Encrypt   │       │
│   │                  │   │                  │   │     Certbot      │       │
│   │  index.html      │   │  JSON Response   │   │                  │       │
│   │  Port 443        │   │  Port 443        │   │  Auto-renewal    │       │
│   └──────────────────┘   └──────────────────┘   └──────────────────┘       │
│                                                                              │
│                          ┌─────────────────────┐                            │
│                          │    HTTP :80         │                            │
│                          │        │            │                            │
│                          │        ▼            │                            │
│                          │   301 Redirect      │                            │
│                          │   → HTTPS           │                            │
│                          └─────────────────────┘                            │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘

### 🛠️ Technologies Used
Infrastructure
Technology	Version	Purpose
AWS EC2	t2.micro	Cloud compute instance
Ubuntu	22.04 LTS	Operating system
Elastic IP	-	Static public IP address

### Web Server
Technology	Version	Purpose
Nginx	1.18.0	Web server and reverse proxy
Certbot	2.9.0	SSL certificate management
Let's Encrypt	-	Free SSL certificates

### Technology	Purpose
UFW	Host-based firewall
AWS Security Groups	Cloud-level firewall
SSH Key Pairs	Secure authentication
DNS
Service	Purpose
FreeDNS	Domain name resolution

### 🚀 Quick Start
bash

# Test the live endpoints
curl https://mrrhng.mooo.com/
curl https://mrrhng.mooo.com/api

# Check HTTP to HTTPS redirect
curl -I http://mrrhng.mooo.com/

# Expected output: HTTP/1.1 301 Moved Permanently

### 📝 Detailed Setup
1. AWS EC2 Provisioning
bash

# Launch Ubuntu 22.04 LTS (t2.micro)
# Configure Security Group:
# - SSH (22) - Your IP only
# - HTTP (80) - 0.0.0.0/0
# - HTTPS (443) - 0.0.0.0/0

# Allocate and associate Elastic IP
aws ec2 allocate-address --domain vpc
aws ec2 associate-address --instance-id <id> --allocation-id <alloc-id>

2. DNS Configuration (FreeDNS)
yaml

Domain: mrrhng.mooo.com
Record Type: A
Destination: [Your Elastic IP]
TTL: 60 seconds

3. SSH Hardening
bash

# Edit SSH configuration
sudo nano /etc/ssh/sshd_config

# Apply these settings
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers hngdevops ubuntu

# Restart SSH
sudo systemctl restart sshd

4. User Creation
bash

# Create hngdevops user
sudo adduser hngdevops
sudo usermod -aG sudo hngdevops

# Copy SSH keys
sudo mkdir -p /home/hngdevops/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys /home/hngdevops/.ssh/
sudo chown -R hngdevops:hngdevops /home/hngdevops/.ssh
sudo chmod 700 /home/hngdevops/.ssh
sudo chmod 600 /home/hngdevops/.ssh/authorized_keys

5. Passwordless Sudo Configuration
bash

# Create sudoers file
sudo visudo -f /etc/sudoers.d/hngdevops

# Add these lines
hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/sshd
hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/ufw
hngdevops ALL=(ALL) NOPASSWD: /bin/cat /etc/ssh/sshd_config

# Set correct permissions
sudo chmod 440 /etc/sudoers.d/hngdevops

6. UFW Firewall Configuration
bash

# Set defaults
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow required ports
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable firewall
sudo ufw enable

# Verify
sudo ufw status verbose

7. Nginx Installation & Configuration
bash

# Install Nginx
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx

# Create HTML file
sudo nano /var/www/html/index.html

index.html:
html

<!DOCTYPE html>
<html>
<head>
    <title>HNG DevOps Stage 0</title>
</head>
<body>
    <h1>Mr. React</h1>
    <p>HNGI14 DevOps Stage 0</p>
</body>
</html>

8. Nginx Configuration File
bash

sudo nano /etc/nginx/sites-available/hngdevops

Configuration:
nginx

server {
    listen 80;
    listen [::]:80;
    server_name mrrhng.mooo.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name mrrhng.mooo.com;

    ssl_certificate /etc/letsencrypt/live/mrrhng.mooo.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/mrrhng.mooo.com/privkey.pem;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;

    location / {
        root /var/www/html;
        index index.html;
    }

    location = /api {
        add_header Content-Type application/json always;
        return 200 '{"message":"HNGI14 Stage 0","track":"DevOps","username":"Mr. React"}';
    }
}

bash

# Enable site
sudo ln -s /etc/nginx/sites-available/hngdevops /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx

9. SSL Certificate (Let's Encrypt)
bash

# Install Certbot
sudo apt install certbot python3-certbot-nginx -y

# Obtain certificate
sudo certbot --nginx -d mrrhng.mooo.com

# Select option 2: Redirect HTTP to HTTPS

# Verify auto-renewal
sudo systemctl status snap.certbot.renew.service

📁 Configuration Files
/etc/ssh/sshd_config (Key excerpts)
ini

PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AllowUsers hngdevops ubuntu

/etc/sudoers.d/hngdevops
bash

hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/sshd
hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/ufw
hngdevops ALL=(ALL) NOPASSWD: /bin/cat /etc/ssh/sshd_config

UFW Rules
bash

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
443/tcp                    ALLOW IN    Anywhere

### 🧪 Testing & Verification
# Test HTTP redirect
curl -I http://mrrhng.mooo.com/

# Test HTTPS root
curl https://mrrhng.mooo.com/

# Test API endpoint
curl https://mrrhng.mooo.com/api

# Test API headers
curl -I https://mrrhng.mooo.com/api

# Check SSL certificate
openssl s_client -connect mrrhng.mooo.com:443 -servername mrrhng.mooo.com 2>/dev/null | openssl x509 -text | grep -A2 "Validity"

# Check security headers
curl -I https://mrrhng.mooo.com/ | grep -E "(Strict-Transport-Security|X-Frame-Options)"

🐛 Troubleshooting
Issue 1: Let's Encrypt Rate Limit

Error:
text

too many certificates (50) already issued for "crabdance.com" in the last 168h

Solution:

    Wait for rate limit reset (7 days)

    Switch to different FreeDNS subdomain (mooo.com, sytes.net, zapto.org)

    Use staging environment for testing: --test-cert

Issue 2: Grader Cannot Read SSH Config

Error:
text

unable to read sshd config

Solution:
bash

# Fix permissions
sudo chmod 644 /etc/ssh/sshd_config

# Add passwordless cat
echo "hngdevops ALL=(ALL) NOPASSWD: /bin/cat /etc/ssh/sshd_config" | sudo tee -a /etc/sudoers.d/hngdevops

Issue 3: UFW Password Prompt

Error:
text

sudo: a password is required

Solution:
bash

# Add passwordless sudo for UFW
echo "hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/ufw" | sudo tee -a /etc/sudoers.d/hngdevops
sudo chmod 440 /etc/sudoers.d/hngdevops

Issue 4: DNS NXDOMAIN

Error:
text

DNS problem: NXDOMAIN looking up A for domain.com

Solution:

    Verify A record exists in FreeDNS

    Wait 5-30 minutes for DNS propagation

    Test with dig domain.com +short

    Ensure Elastic IP is correctly associated

Issue 5: SSH Permission Denied

Error:
text

Permission denied (publickey)

Solution:
bash

# Verify permissions on server
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys

# Check SSH config
sudo grep -E "PermitRootLogin|PasswordAuthentication" /etc/ssh/sshd_config

# Restart SSH
sudo systemctl restart sshd

### 📚 Lessons Learned

1. File Permissions Matter Critically
File	Required Permission
/etc/ssh/sshd_config	644 (world-readable)
/etc/sudoers.d/*	440 (read-only for root)
~/.ssh/authorized_keys	600 (user only)
~/.ssh/	700 (user only)

2. Let's Encrypt Has Subtle Rate Limits

    50 certificates per registered domain per 7 days

    Shared FreeDNS domains (crabdance.com) get exhausted quickly

    Use less common suffixes (mooo.com, sytes.net, zapto.org)

3. DNS Propagation Is Not Instant

    FreeDNS: 5-30 minutes for updates

    Always verify with dig before running certbot

4. Passwordless Sudo Must Be Command-Specific
bash

# ✅ Correct
hngdevops ALL=(ALL) NOPASSWD: /usr/sbin/sshd

# ❌ Incorrect (won't work for grader)
hngdevops ALL=(ALL) NOPASSWD: ALL

### 👤 Author
<div align="center">

HNG Username: Mr. React

Stage: 0/10

Track: DevOps

### Connect With Me
[![GITHUB](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/mrr-01)]
[![LINKEDIN](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/victor-joseph-4a6917312)
