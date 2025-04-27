# SCENE Datalake 🎽🌊

Data Lake developed inside the [SCENE project](https://thesceneproject.eu/).  
SCENE is funded by the Horizon Europe Programme under Grant Agreement No. **101095303**.

The Data Lake includes the **SCENE Ontology**, covering knowledge areas from the film-making industry.

---

## 📚 Usage and Documentation

The SCENE Data Lake provides a **set of tools** including:
- Visual viewer for the ontology (WebVOWL)
- REST APIs for accessing the data lake
- Authentication and access management
- Data ingestion workflows

These tools are packaged as **Docker containers** and are publicly available on **DockerHub**.

You can deploy the SCENE Data Lake via **Docker Compose** by browsing the `deployment` folder.  
There are three installation options available:
- 🖥️ **Local installation on Windows**
- 🐧 **Local installation on Linux**
- 🌐 **Production deployment on a Linux server**

Each approach has its own **dedicated installation guide** available inside the corresponding subfolder.

📖 **Additional documentation** (User Manuals, Admin Manuals, FAQs, etc.) is available at:  
👉 [SCENE Data Lake Documentation on ReadTheDocs](https://scene-datalake-doc.readthedocs.io/en/latest/)

---

## ⚙️ WebVOWL: Optional Local Build

Although WebVOWL is provided as a Docker image, you can optionally **build your own version** (e.g., to customize the CSS, change ontology files):

```bash
docker build -t webvowl .
```

You will find the Dockerfile under the `docker-build/webvowl` folder.

---

## ⚡ Important Notes

- There is an **old version** of the platform under the `old/` folder. It is kept **only for historical reasons**.
- The old version requires manual edits to configuration files (`docker-compose.yml`, `default.conf`, etc.) — not recommended for production use.


---

## 🛡️ License

The SCENE Ontology and associated tools are distributed under the [Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

⚠️ Some third-party services integrated into the Data Lake (e.g., MinIO, Apache NiFi, Node-RED) have their **own licenses**.  
Please refer to their official sites for further details — all components used are open-source.

---

## 📬 Contact

- **Benjamin Molina** — [benmomo@upvnet.upv.es](mailto:benmomo@upvnet.upv.es)

---

# 🔗 Quick Links

- 🐙 GitHub Repository: https://github.com/benmomo/scene-datalake
- 📖 Documentation: https://scene-datalake-doc.readthedocs.io/en/latest/
- 🎬 SCENE Project Website: https://thesceneproject.eu/

---

👉 **This deployment assumes a single-server architecture.** For higher security, availability, or scaling, consider using advanced setups (Kubernetes, load balancers, database clustering).