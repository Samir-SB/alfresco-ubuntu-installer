# Alfresco installation in Ubuntu using ZIP Distribution Files

Alfresco Platform provides flexibility in deployment, accommodating various infrastructures and operational preferences. 

Below are several deployment approaches:

**[ZIP Distribution files](https://docs.alfresco.com/content-services/latest/install/zip/)**

Deploying Alfresco using ZIP distribution files involves manually configuring and installing Alfresco on servers. This approach allows for detailed customization of the installation process, making it suitable for environments where specific configurations or integrations are required.

**[Ansible](https://docs.alfresco.com/content-services/latest/install/ansible/)**

Ansible automation simplifies the deployment and management of Alfresco across multiple servers. Ansible playbooks automate the installation and configuration tasks, ensuring consistency and reducing deployment time. This method is ideal for environments requiring rapid deployment and scalability.

**[Containers](https://docs.alfresco.com/content-services/latest/install/containers/)**

Containerization of Alfresco leverages Docker and Kubernetes technologies, offering a modern approach to deployment:

   - **[Docker Compose](https://docs.alfresco.com/content-services/latest/install/containers/docker-compose/)**: Docker Compose simplifies the orchestration of multiple Alfresco services, such as Alfresco Content Repository, ActiveMQ, Elasticsearch, and others, defined in a single YAML file. It facilitates deployment in development, testing, and small-scale production environments.

   - **[Helm](https://docs.alfresco.com/content-services/latest/install/containers/helm/)**: Helm charts streamline the deployment of Alfresco on Kubernetes clusters. Helm manages Kubernetes applications through easy-to-use templates (charts) and package management. It enables scalability, version control, and rollback capabilities, making it suitable for production-grade deployments.

This project provides a sample **ZIP Distribution Files** configuration for deploying the Alfresco Platform.

## Contents

This project provides a collection of `bash` scripts designed to automate various installation and setup tasks on an Ubuntu system. Each script handles a specific component or service, ensuring a streamlined and repeatable setup process. Below is a list of the available scripts along with their descriptions:

1. **PostgreSQL Installation**
   - Script: [1-install_postgres.sh](scripts/1-install_postgres.sh)
   - Description: Installs and configures PostgreSQL, to be used as object-relational database system.

2. **Java Installation**
   - Script: [2-install_java.sh](scripts/2-install_java.sh)
   - Description: Installs Java Development Kit (JDK), essential for running Apache Tomcat and Java applications like Apache Solr, Apache ActiveMQ and Transform Service.

3. **Tomcat Installation**
   - Script: [3-install_tomcat.sh](scripts/3-install_tomcat.sh)
   - Description: Installs Apache Tomcat, to deploy Alfresco and Share web applications.

4. **ActiveMQ Installation**
   - Script: [4-install_activemq.sh](scripts/4-install_activemq.sh)
   - Description: Installs Apache ActiveMQ, to be used as messaging server.

5. **Alfresco Resources Download**
   - Script: [5-download_alfresco_resources.sh](scripts/5-download_alfresco_resources.sh)
   - Description: Downloads necessary resources for Alfresco, including web applications, search service and transform service.

6. **Alfresco Installation**
   - Script: [6-install_alfresco.sh](scripts/6-install_alfresco.sh)
   - Description: Installs Alfresco Community Edition, configuring Alfresco and Share web applications.

7. **Solr Installation**
   - Script: [7-install_solr.sh](scripts/7-install_solr.sh)
   - Description: Installs Apache Solr, to be used as search platform for indexing and searching data.

8. **Transform Service Installation**
   - Script: [8-install_transform.sh](scripts/8-install_transform.sh)
   - Description: Installs services required for document transformations within Alfresco.

9. **Start Services**
   - Script: [9-start_services.sh](scripts/9-start_services.sh)
   - Description: Starts all the installed services to ensure they are running correctly.

## Usage

Each script can be executed individually in a bash shell. Despiste user `ubuntu` is expected to be used, ensure you have the necessary permissions (e.g., using `sudo` where required).

```bash
bash scripts/1-install_postgres.sh
bash scripts/2-install_java.sh
bash scripts/3-install_tomcat.sh
bash scripts/4-install_activemq.sh
bash scripts/5-download_alfresco_resources.sh
bash scripts/6-install_alfresco.sh
bash scripts/7-install_solr.sh
bash scripts/8-install_transform.sh
```

Although the `9-start_services.sh` script includes the sequence for executing the services, it is recommended to run each line manually. This allows you to verify that each service is up and running correctly before proceeding to the next one.

```bash
sudo systemctl start postgresql
sudo systemctl status postgresql
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; preset: ena>
     Active: active (exited) since Mon 2024-07-29 09:46:15 UTC; 19s ago
```

```bash
sudo systemctl start activemq
sudo systemctl status activemq
● activemq.service - Apache ActiveMQ
     Loaded: loaded (/etc/systemd/system/activemq.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-07-29 09:46:56 UTC; 6s ago
```

```bash
sudo systemctl start transform
sudo systemctl status transform
● transform.service - Transform Application Container
     Loaded: loaded (/etc/systemd/system/transform.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-07-29 09:47:33 UTC; 8s ago
```

```bash
sudo systemctl start tomcat
sudo systemctl status tomcat
● tomcat.service - Apache Tomcat Web Application Container
     Loaded: loaded (/etc/systemd/system/tomcat.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-07-29 09:48:15 UTC; 7s ago
tail -f /home/ubuntu/tomcat/logs/catalina.out
...
29-Jul-2024 09:49:08.922 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [52678] milliseconds
```

```bash
sudo systemctl start solr
sudo systemctl status solr
● solr.service - Apache SOLR Web Application Container
     Loaded: loaded (/etc/systemd/system/solr.service; enabled; preset: enabled)
     Active: active (running) since Mon 2024-07-29 09:49:32 UTC; 11s ago
```

## Verification

Default credentials are `admin`/`admin`

* Alfresco Repository: http://localhost:8080/alfresco

* Share UI: http://localhost:8080/share
  - Search "budget" >> 8 results found
  - Access to document "Meeting Notes 2011-01-27.doc" in folder "Meeting Notes" of site "swsdp". PDF Preview must be available.