SCENE Datalake 
===

Data Lake  developed inside the [SCENE project](https://thesceneproject.eu/). 
SCENE has been financed by the Horizon Europe programme under Grant Agreement No 101095303

The data lake also includes the SCENE ontology,  covering knowledge areas from the film-making industry.




Usage and Documentation
===

The data lake provides a **set of tools** including a visual viewer, REST APIs for the associated data lakes, etc. 
They use Docker containers and are publicly available in DockerHub, except for Webvowl that yous should build, available in the **docker-build** folder:
$ docker build -t webvowl .

The **docker-compose** folder includes a docker-compose file that shows a basic usage of the whole set. You might test it in your own computer after setting up local configurations, including domains, valid SSL certificates, credentials, etc. Note that the provided docker-compose file serves as a template. You **must configure** it before being able to run it. Please edit it and check for any comment beginning with **SCENE**. As there is a proxy (nginx) you should also check the file located in the folder **frontend-ui-files/conf/default.conf**, and look for any comment beginning with **SCENE** 

The given docker-compose file is basic and could be improved for production environments (e.g., use of Kubernetes instead of docker-compose)


It is expected that more documentation will be available in the [Readthedocs](https://scene-datalake-doc.readthedocs.io/en/latest/) website, including a user manual. We will also try to automatize the installation process. 


License
===

The SCENE ontology and the set of tools are distributed through [Apache2](https://www.apache.org/licenses/LICENSE-2.0) license.


Contacts
===

Benjamin Molina <benmomo@upvnet.upv.es>
