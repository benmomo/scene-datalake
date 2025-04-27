# 🐳 SCENE Datalake - Production Server Deployment (Linux)

This document provides step-by-step instructions for installing and deploying the SCENE Data Lake platform on a Linux production server.



## 📁 Folder Structure

This folder contains everything needed to run the SCENE tools locally:

```
scene-datalake/
└── deployment/
    └── production/
        ├── docker-compose.yml  # Main configuration to launch all services
        ├── default.conf        # NGINX reverse proxy configuration
        ├── scene.env           # Environment variables for service passwords
        └── README.md           # This installation guide
        └── ...                 # Some folders with mounting purposes

```


## 🧰 Requirements

### 2.1. Hardware Requirements

- Linux server (e.g., Ubuntu 22.04 LTS)
- Public IP address (e.g., 158.42.188.110)

### 2.2. Network Requirements

- Public DNS records:
  - A record for `scene-dl.satrd.es` pointing to the server IP
  - Wildcard record `*.scene-dl.satrd.es` also pointing to the server IP

  *Note: We are assuming this domain name for the server, but you should change it accordingly*

### 2.3. Software Requirements

- Docker Engine
- Docker Compose
- Certbot (for SSL certificates)

### 2.4. User Requirements

- Root or sudo access to the server



## 📦 Installation Steps


### 3.1. Install Docker and Docker Compose

```
sudo apt update
sudo apt install docker.io docker-compose
```

### 3.2. Install Certbot

```
sudo apt install certbot
```

### 3.3. Generate SSL Certificates

Obtain wildcard SSL certificates using Let's Encrypt Certbot:

```
sudo certbot certonly --manual --preferred-challenges=dns \
  --email your-email@example.com --server https://acme-v02.api.letsencrypt.org/directory \
  --agree-tos -d "*.scene-dl.satrd.es"
```

> Follow the instructions to set the necessary DNS TXT record for domain verification.

After successful generation, locate your certificates, typically under `/etc/letsencrypt/live/`.

Copy them to the project's `certs` folder as:

```
./certs/fullchain1.pem
./certs/privkey1.pem
```


### 3.4. Generate Apache NiFi Certificates

Apache NiFi requires a custom keystore and truststore.
Use your SSL certificates to create these files or follow the official [NiFi TLS Toolkit Guide](https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html#tls-toolkit).

Place the resulting certificates into the `./nifi-certs/` directory.


### 3.5. Clone the Repository

```
git clone https://github.com/benmomo/scene-datalake.git
cd scene-datalake
```

### 3.6. Review Configuration Files

- Edit the `scene.env` file to adjust passwords if necessary.

To keep the configuration clean and flexible, all credentials and key settings are stored in a **central `.env` file**.

This file is already included as `scene.env` in this folder and is loaded automatically by Docker Compose.

#### 📝 Example `scene.env` File

```
# Admin user for most tools
ADMIN_USER=admin
ADMIN_PASSWORD=scene1234

# MinIO
MINIO_ROOT_USER=admin
MINIO_ROOT_PASSWORD=scene1234

# Keycloak
KEYCLOAK_ADMIN=admin
KEYCLOAK_ADMIN_PASSWORD=scene1234

# MongoDB
MONGO_INITDB_ROOT_USERNAME=admin
MONGO_INITDB_ROOT_PASSWORD=scene1234

# Mongo Express
ME_CONFIG_MONGODB_ADMINUSERNAME=admin
ME_CONFIG_MONGODB_ADMINPASSWORD=scene1234
ME_CONFIG_BASICAUTH_USERNAME=admin
ME_CONFIG_BASICAUTH_PASSWORD=scene1234

# Trino
TRINO_PASSWORD=scene1234
```

🔐 You can modify these values to suit your local needs before launching the platform.

📌 Notes

- Dock Compose will substitute these variables automatically when starting containers.
- all services are pre-configured to use the same simple password (scene) for demonstration and testing.
- In a production setup, never use shared credentials — this is for local dev only.



- Check the `docker-compose.yaml` and ensure domain names match your DNS setup.
- Review `frontend-ui-files/conf/default.conf` for nginx configuration.

> **Note:** If you use a domain different from `scene-dl.satrd.es`, you must adapt domain references accordingly.

### 3.7. Start the Deployment

Open a terminal in the `deployment/production/` folder and run the following command:

```
docker-compose --env-file scene.env up -d
```

This will:

- Pull all the required images (only the first time)
- Create and start each container
- Set up networks, volumes, and environment variables
- Mount your local SSL certificates and config files into the containers

## 4. Post-Installation Checks

Check that all containers are up and running:

```bash
docker ps
```

You should be able to access the following services:

- https://scene-dl.satrd.es (Portal)
- https://s3ui.scene-dl.satrd.es (MinIO UI)
- https://s3api.scene-dl.satrd.es (MinIO API)
- https://webvowl.scene-dl.satrd.es (WebVOWL)
- https://jupyter.scene-dl.satrd.es (Jupyter Notebooks)
- https://nodered.scene-dl.satrd.es (Node-RED)
- https://keycloak.scene-dl.satrd.es (Keycloak)
- https://nifi.scene-dl.satrd.es (Apache NiFi)

Default login credentials are defined in the `scene.env` file.


## 5. Useful Information

- **Firewall:** Configure your firewall to only expose required ports (e.g., 443).
- **Certificates Renewal:** Let's Encrypt certificates must be renewed periodically.
- **Volumes Backup:** Important data is stored in Docker volumes (`minio_data`, `jupyter_data`, etc.).
- **User Management:** For advanced NiFi user management, you can customize `authorizers.xml` and `login-identity-providers.xml`. See [Apache NiFi Security Docs](https://nifi.apache.org/docs/nifi-docs/html/administration-guide.html#user-authentication).

## 6. Troubleshooting

- **SSL issues:** Check if your certificates are correctly generated and copied.
- **Docker issues:** Run `docker logs <container>` to investigate problems.
- **Domain resolution issues:** Verify your DNS configuration.
- **Port conflicts:** Ensure ports 80, 443, 9000, 9001, 27017, 8080, 8443, and others are free.

---

> **Important Notice:** This deployment assumes a single-server architecture. For higher security, availability, or scaling, consult with your system/network administrators for additional setups like proxy servers, load balancers, or database clustering.






