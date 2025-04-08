# 🐳 SCENE Datalake - Local Deployment (Windows)

This guide explains how to deploy the SCENE platform locally on a **Windows machine** using Docker Desktop.  
It is designed for **testing and development purposes**, simulating the production setup via `localhost` and self-signed certificates.


## 📁 Folder Structure

This folder contains everything needed to run the SCENE tools locally:

```
scene-datalake/
└── deployment/
    └── local/
        └── windows/
            ├── docker-compose.yml  # Main configuration to launch all services
            ├── default.conf        # NGINX reverse proxy configuratio
            ├── scene.env           # Environment variables for service passwords
            └── README.md           # This installation guide
            └── ...                 # Some folders with mounting purposes

```


## 🧰 Prerequisites

- You must have **Docker Desktop for Windows** installed.
- You must run your terminal or editor **as Administrator** to modify the hosts file and create certificates.
- Git or ZIP download access.



## 📦 Installation Steps

### 1. Clone or Download
```
git clone https://github.com/benmomo/scene-datalake.git
cd scene-datalake/deployment/local/windows
```

### 2. Configure Local Domains in `hosts` File

To simulate the domain-based access (as in production), we map local domains like `scene.local` to your own machine (`localhost`).

This allows you to use URLs such as:

- https://scene.local
- https://keycloak.scene.local
- https://s3api.scene.local
- and others...


#### 📝 Steps to Edit the `hosts` File (Windows)

1. **Open Notepad as Administrator**:
   - Press `Start`, type `Notepad`, right-click it, and choose **Run as administrator**.

2. **Open the hosts file**:
   - In Notepad, go to `File → Open`
   - Navigate to:  
     `C:\Windows\System32\drivers\etc`
   - Select `All Files (*.*)` and open the `hosts` file.

3. **Add the following lines at the bottom**:

```
127.0.0.1 scene.local 
127.0.0.1 s3api.scene.local 
127.0.0.1 s3ui.scene.local 
127.0.0.1 keycloak.scene.local 
127.0.0.1 jupyter.scene.local 
127.0.0.1 nodered.scene.local 
127.0.0.1 nifi.scene.local 
127.0.0.1 webvowl.scene.local
```

4. **Save the file** and close Notepad.

> ⚠️ You may need to restart Docker Desktop or your browser to apply the changes.


### 3. Generate Self-Signed SSL Certificates

Many services in the SCENE platform (like NiFi, Keycloak, Jupyter) require HTTPS.  
To simulate this in your local setup, you need to create **self-signed SSL certificates**.

These certificates will be used by the NGINX reverse proxy and other services.

#### 📁 Where to Place the Certificates

You should create a folder named `certs` inside `deployment/local/windows/`, like this:

```
scene-datalake/
└── deployment/
    └── local/
        └── windows/
            ├── certs
                ├── fullchain1.pem
                ├── privkey1.pem
```

#### 🛠️ Steps to Create the Certificates (Using Git Bash or WSL)

If you're using Git Bash, WSL, or any terminal with OpenSSL:

```
mkdir -p certs
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout certs/privkey1.pem \
  -out certs/fullchain1.pem \
  -subj "/C=EU/ST=Valencia/L=UPV/O=SCENE/CN=*.scene.local"
```

⚠️ If you're using PowerShell, OpenSSL must be installed and accessible via PATH.


✅ Result
You will have two files inside the certs directory:

- fullchain1.pem: the public certificate
- privkey1.pem: the private key

These will be automatically mounted by Docker when launching the services.


### 4. Define Credentials and Settings

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

- cker Compose will substitute these variables automatically when starting containers.
- l services are pre-configured to use the same simple password (scene) for demonstration and testing.
- In a production setup, never use shared credentials — this is for local dev only.


### 5. Launch the SCENE Platform

Once your environment is ready (hosts file, certificates, `.env` file), you're ready to run the platform locally.

#### ▶️ Start Everything with Docker Compose

Open a terminal in the `deployment/local/windows/` folder and run the following command:

```
docker-compose --env-file scene.env up -d
```

This will:

- Pull all the required images (only the first time)
- Create and start each container
- Set up networks, volumes, and environment variables
- Mount your local SSL certificates and config files into the containers

#### 🧪 Test the Setup

After a few minutes, open your browser and try the following address:

| Service          | URL                               |
|------------------|------------------------------------|
| Main NGINX page  | https://scene.local                |

This is the main front-end from which you will access the other endpoints of the services, accesible through the proxy

| Service          | URL                               |
|------------------|------------------------------------|
| MinIO (S3 UI)    | https://s3ui.scene.local           |
| MinIO (S3 API)   | https://s3api.scene.local          |
| Keycloak         | https://keycloak.scene.local       |
| Jupyter          | https://jupyter.scene.local        |
| Node-RED         | https://nodered.scene.local        |
| WebVowl          | https://webvowl.scene.local        |


> ⚠️ Because you're using self-signed certificates, your browser will show a warning.  
> Click “Advanced” → “Proceed to [domain]” to bypass it.

For some services, e.g., Keycloak, you might need a valid certificate (not self-signed). To avoid this, you might remove the HSTS profile in your browser. In Chrome it will be:
- Visit **chrome://net-internals/#hsts**
- In **Delete domain security policies** insert the domain (here *keycloak.scene.local*). Then Click on **Delete**

In Firefox just type **about:config**, search **hsts** and delete the HSTS cache

For other services, such as Jupyter providing examples, there might be some problems loading the theme in the **Chrome browser**. If this happens, just try **Firefox** for this particular service

#### 🛑 To Stop the Platform

You can stop all services with:

```
    docker-compose down
```

To stop (pause) the containers without deleting them:

```
    docker-compose stop
```


### 🎉 Done!

You’ve now successfully deployed the SCENE Data Lake platform locally for testing and development on Windows.




