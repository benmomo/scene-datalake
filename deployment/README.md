# 🎬 SCENE Datalake - Deployment Overview

Welcome to the SCENE Datalake Deployment Toolkit.  
This repository provides multiple options for deploying the SCENE platform depending on your environment and purpose.



## 📌 Deployment Targets

We currently support the following deployment scenarios:

### ✅ 1. Local Development - **Windows**

A complete local setup designed for testing and development on **Windows machines** using **Docker Desktop**.  
Includes configuration for local domain simulation (`*.scene.local`) and HTTPS access via self-signed certificates.

📂 Folder:  
`deployment/windows/`

📘 Setup Guide:  
Please check README file in this previous folder


### ✅ 2. Local Development - **Linux**

A similar setup adapted for **Linux workstations or servers**, also using **Docker Engine and Docker Compose**.  
Includes Linux-specific steps for configuring `/etc/hosts`, generating SSL certificates, and launching the platform.

📂 Folder:  
`deployment/linux/`

📘 Setup Guide:  
Please check README file in this previous folder



### 🚧 3. Production Server Deployment (Coming Soon)

We are preparing a dedicated deployment pipeline for secure **production environments**.  
This will include:

- Hardened configurations
- TLS certificates from a trusted CA
- Scalable and secure service configurations
- External storage and user authentication integration

📂 Future Folder:  
`deployment/server/`

📅 Status: *Planned*



## ⚙️ What’s Inside

Each deployment includes:

- `docker-compose.yaml`: Docker services and volumes
- `default.conf`: NGINX reverse proxy configuration
- `scene.env`: Centralized environment variables
- `README.md`: Installation and usage guide



