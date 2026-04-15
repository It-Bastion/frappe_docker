# Deployment Process — Bastion ERPNext

## Overview

This document summarizes the deployment work completed during Week 2 of the Bastion project.

The objective of this phase was to:
- select and validate a VPS provider
- prepare the Linux server environment
- configure the local ERPNext development environment
- test remote deployment on the VPS
- validate access through browser and remote tools

---

## Monday–Tuesday — VPS Research & Selection

Providers reviewed:
- ADK Media
- Hostino.ma
- OVHcloud

Evaluation criteria:
- Datacenter location (Morocco)
- SLA uptime
- Support (FR/AR)
- Backup availability
- Compliance (Loi 09-08 / CNDP)

Minimum requirements:
- 4 vCPU
- 8 GB RAM
- 100 GB SSD

Result:
OVHcloud selected for reliability and scalability.

---

## Wednesday — Server Setup

SSH connection:
ssh ubuntu@<SERVER_IP>

System update:
sudo apt update && sudo apt upgrade -y

Docker installation:
sudo apt install -y docker.io docker-compose
sudo systemctl start docker
sudo systemctl enable docker

Firewall configuration:
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 5432/tcp
sudo ufw enable

Result:
- Server accessible
- Docker running
- Firewall configured

---

## Thursday — Local Environment

Tools:
- Docker Desktop
- VS Code (Docker, ESLint, GitLens, PostgreSQL)

Git config:
git config --global user.name "Yassine Souany"
git config --global user.email "it@bastiongroup.net"

Start environment:
docker compose up -d

Check:
docker ps

Site creation:
docker compose exec backend bench new-site monsite.local --db-root-username root --db-root-password admin --admin-password admin

Install ERPNext:
docker compose exec backend bench --site monsite.local install-app erpnext
docker compose exec backend bench use monsite.local
docker compose restart

Access:
http://127.0.0.1:8090

Login:
Administrator / admin

Result:
ERPNext working locally

---

## Friday — Deployment Test

Remote SSH via VS Code: OK

NGINX deployment:
docker run -d -p 8081:80 nginx

Check:
docker ps

Access:
http://152.228.131.124:8081/

Result:
Welcome to nginx!

---

## Final Status

- VPS ready
- Local ERPNext working
- Remote deployment validated

Next step:
Deploy ERPNext on VPS

---

Author:
Yassine Souany
