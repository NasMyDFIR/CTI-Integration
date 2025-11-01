# CTI Integration Lab

## Objective

To deploy and configure an OpenCTI server integrated with the OTX AlienVault threat intelligence feed to centralize, manage, and correlate indicators of compromise—such as IP addresses, domains, and file hashes—enhancing the detection and analysis of potential threats within a SOC environment.


### Skills Learned

- Acquired hands-on experience in deploying and configuring OpenCTI for centralized threat intelligence management.
- Learned to integrate external CTI feeds, specifically OTX AlienVault, for automated threat data ingestion.
- Developed skills in analyzing and correlating indicators of compromise (IOCs) such as IPs, domains, and file hashes.
- Gained practical understanding of SOC operations and threat detection workflows using real-world intelligence data.
- Enhanced Linux administration, API integration, and technical documentation capabilities.
  
### Tools Used
- OpenCTI – Open-source threat intelligence platform for managing and correlating CTI data.
- OTX AlienVault – External threat intelligence feed providing real-time indicators of compromise (IOCs).
- Splunk – Security Information and Event Management (SIEM) tool for log analysis, data correlation, and visualization.
- Ubuntu – Operating system used to host both the OpenCTI and Splunk servers.
- Docker & Docker Compose – Used for deploying and managing OpenCTI and its dependencies.
- API Integrations – Utilized for connecting OpenCTI with OTX AlienVault and Splunk.

---

## ⚙️ Project Implementation Steps

### **1. Set Up Virtual Machines**

Deploy two Ubuntu virtual machines:

* **VM 1:** OpenCTI Server
* **VM 2:** Splunk Server

Each will host their respective services in isolated environments.

---

### **2. Install Docker Compose**

On the OpenCTI VM, install Docker Compose to run containerized services:

```bash
sudo apt install docker-compose
```

---

### **3. Clone the OpenCTI Repository**

Clone the official OpenCTI GitHub repository to your OpenCTI server:

```bash
git clone https://github.com/OpenCTI-Platform/docker.git
```

Then navigate into the Docker directory:

```bash
cd docker
```

---

### **4. Configure the Environment File**

Rename the environment variables file:

```bash
mv .env.sample .env
```

Edit it with `nano`:

```bash
nano .env
```

Update the following fields:

* `OPENCTI_ADMIN_EMAIL` → Your admin/production email
* `OPENCTI_ADMIN_PASSWORD` → Desired password
* `OPENCTI_ADMIN_TOKEN` → Generate a UUID (use [uuidgenerator.net](https://www.uuidgenerator.net))
* `OPENCTI_BASE_URL` → OpenCTI server IP and port
* `MINIO_ROOT_PASSWORD` and `RABBITMQ_DEFAULT_PASS` → Secure passwords

---

### **5. Start OpenCTI Services**

Prepare and launch Docker containers:

```bash
sudo sysctl -w vm.max_map_count=1048575
sudo systemctl start docker.service
sudo docker-compose up -d
sudo docker-compose ps
```

Verify that all OpenCTI services are running successfully.

---

### **6. Access the OpenCTI Web Interface**

Open your browser and navigate to:

```
http://<OpenCTI_Server_IP>:8080
```

Log in using the admin email and password defined in your `.env` file.

---

### **7. Integrate OTX AlienVault Connector**

1. Create an **OTX AlienVault** account at [https://otx.alienvault.com](https://otx.alienvault.com).
2. From the OpenCTI GitHub repository, download the **AlienVault connector’s** `docker-compose.yml` file.
3. Edit the connector’s configuration file and update these fields:

   * `OPENCTI_TOKEN` → Same as in `.env`
   * `CONNECTOR_ID` → Generate a new UUID
   * `ALIENVAULT_APP_KEY` → Copy from your OTX account (API integrations section)

---

### **8. Restart Docker to Apply Changes**

```bash
sudo docker-compose stop
sudo docker-compose up -d
sudo docker-compose ps
```

Confirm all connector services are running.

---

### **9. Validate OTX AlienVault Integration**

Access the OpenCTI web interface again to ensure that **data ingestion from OTX AlienVault** has started successfully.

---

### **10. Configure Splunk**

1. Access the Splunk web interface:

   ```
   http://<Splunk_Server_IP>:8000
   ```
2. Navigate to:
   **Settings → Users and Authentication → Tokens**
3. Create a **new authentication token** for integration.

---

### **11. Add the Splunk Connector to OpenCTI**

From your SSH session, edit the Docker configuration to include the **OpenCTI-Splunk connector**, updating the following fields:

```yaml
OPENCTI_URL=opencti:8080
OPENCTI_TOKEN=<token_from_.env>
CONNECTOR_ID=<new_UUID>
SPLUNK_URL=<splunk_server_IP:port>
SPLUNK_TOKEN=<splunk_generated_token>
SPLUNK_SSL_VERIFY=false
```

---

### **12. Create a KV Lookup in Splunk**

In your Splunk web app:

* Navigate to **Settings → Knowledge → Lookups → Lookup Definitions → New Lookup Definition**
* Create a key-value lookup table to store correlated threat intelligence data.

---

### **13. Restart Docker and Verify Integration**

Restart all services:

```bash
sudo docker-compose stop
sudo docker-compose up -d
sudo docker-compose ps
```

Then, refresh the Splunk app and perform a **lookup query** to confirm that OpenCTI and Splunk are successfully integrated.

---

### ✅ **End Result**

You now have:

* A fully functional **OpenCTI server** collecting and managing CTI data.
* A **Splunk instance** ingesting and correlating threat intelligence from OpenCTI.
* A practical lab environment demonstrating your SOC-level skills in **CTI integration, SIEM configuration, and data correlation**.

---

<div>
    <img src="img src="https://i.imgur.com/NMvVGLQ.png" />
</div>

drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
