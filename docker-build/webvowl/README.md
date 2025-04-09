# 🌐 WebVOWL - Custom Docker Build for SCENE

This README explains how to build and customize a Docker container for **WebVOWL**, tailored to the SCENE project.  
The goal is to allow full control over branding, ontology integration, and UI behavior, beyond what the official repositories currently offer.

---

## 🧭 Overview

There are two approaches for integrating WebVOWL:

### 1. Prebuilt Image

Use the prebuilt Docker image hosted on DockerHub:

```
docker pull benmomo/scene-webvowl:latest
```

This version is stable and tested for SCENE.  
It’s used in the main `docker-compose.yaml`.


### 2. Custom Build (Developer Mode)

For advanced customization or adapting to other projects, build your own container.

> This is the **recommended approach for developers**.

*Notes*

- This is based on WebVOwl available at https://service.tib.eu/webvowl/. This repo is not active in the last years, but we didn't found any alternative
- The GitHub repo is availabe at https://github.com/VisualDataWeb/WebVOWL
- However, the converter from OWL/TTL to JSON (https://github.com/VisualDataWeb/OWL2VOWL) is decoupled in another app and it is not simple to bind them together (compiling and binding issues) 
- The GitHub repo also includes a Protege VOWWL plugin, but is states that it does not include all functionalites as the web app
- There is a Docker version at (https://github.com/VisualDataWeb/WebVOWL/tree/master/doc/Docker), but does not work:
	- by modifying the given Dockerfile, as the target WAR file is no longer available online. We used  webvowl_1.1.7.war (from an older project)
	- by executing it (docker run --name testw -v ./data:/data -p 8080:8080 testw) it is not able to connect with theOWL2VOWL converter
- We need to re-use webvowl_1.1.7.war and proceed with various steps




## 🛠 Custom Build Steps

The build uses a WAR file (`webvowl.war`) on top of an Apache Tomcat server.

### 1. Prepare Build Context

Inside this folder, you should have:

```
scene-datalake/
└── docker-build/
    └── webvowl/
        ├── Dockerfile
        ├── apache-tomcat-9.0.91.tar.gz        
        ├── webvowl.war           
        └── README.md           
        
```

Import original WAR file (webvowl.war) into the Eclipse Workspace

To keep the changes copy the following files before editing into and old files

- src/main/java/static/index.html 		  --> src/main/java/static/index.html.orig
- src/main/java/static/js/webvowl.app.js  --> src/main/java/static/js/webvowl.app.js.old
- src/main/java/static/css/webowl.app.css --> src/main/java/static/css/webowl.app.css.old



### 2. Customize the Web Interface (Optional)

You can modify the following files before building the WAR:

#### 🔧 `src/main/java/static/index.html`

Update accordingly:

- favicon (header)
- title (header)
- <div id="logo" class="noselect">
- <ul id="m_select" class="toolTipMenu noselect"> 
- <ul id="m_about" class="toolTipMenu">


#### 🔧 `src/main/java/static/js/webvowl.app.js`

Update ontology resolution logic:

```js
loadingModule.from_IRI_URL = function(fileName) {
  var filename = decodeURIComponent(fileName.slice("iri=".length));
  switch (filename) {
    case "schema":
      filename = "http://nginx/ontology/schemaorg.owl";
      break;
    ...
  }
};
```

#### 🎨 `src/main/java/static/css/webvowl.app.css`

- body {
    	/*background: rgb(24, 32, 42);*/
    	background: #b77126;   --> change yellow color with #141032 (dark blue)

- Search dark green (via /* */ as they were commented from before) and change into dark violet #15111b


- the light green color is directly in index.html
    	<aside id="detailsArea" style="background-color: #869734;">
    	change to #C5BDE1

- With all this changes we also need to change the font color, otherwise is is hardly to see


### 3. 🧱 Build the Docker Image

To build the customized WebVOWL image:

1. Open a terminal and navigate to the folder containing the Dockerfile:
```
docker build -t webvowl .
```

This command will:

- Start from a base ubuntu:24.04 image
- Install Java 8 and Apache Tomcat
- Remove any conflicting Java 11 installations
- Copy the WebVOWL WAR file into the webapps/ directory of Tomcat
- Expose port 8080 for local access

✅ Tip: Make sure that webvowl.war includes your customizations before building.


### 4. ▶️ Run the Container Locally

After building the image, you can test it by running:

```
docker run -d --name webvowl-test -p 8080:8080 webvowl
```

Then open your browser and go to:

```
http://localhost:8080/webvowl
```

You should see the customized WebVOWL interface running inside Tomcat.


### 5. 🚀 Push to DockerHub (Optional)

If you want to share your customized WebVOWL image or use it across different environments, you can push it to [DockerHub](https://hub.docker.com/).


#### 📝 Step-by-Step Instructions

1. **Tag the image** using your DockerHub username:

 ```
 docker tag webvowl yourdockerhubusername/webvowl
```

2. **Log in** to DockerHub (if not already logged in):

```
docker login
```

3. **Push** the image to your DockerHub repository:

```
docker push yourdockerhubusername/webvowl
```
Once pushed, the image will be available at:

https://hub.docker.com/r/yourdockerhubusername/webvowl

You can now reference it in your docker-compose.yaml or pull it from any machine:

```
docker pull yourdockerhubusername/webvowl
```
🔒 Tip: Make sure your DockerHub account is public if you want others to access your image, or configure access permissions accordingly for private repositories.

