### Step-by-Step Lab Guide to Create a VM in Azure (with Default Settings)

# 🌐 Step-by-Step Lab Guide to Create and Configure a VM in Azure

# 📋 Table of Contents

# Create a Virtual Machine in Azure

Step 1: Sign in to Azure portal

Step 2: Create a Virtual manchine 

Step 3: Configure Project  Details

Step 4: Configure Image and Size

Step 5: Configure Administrator Account

Step 6: Configure Inbound Port Rules

Step 7: Review and Create

Step 8: Access Your Virtual Machine

step 9: Configure Inbound Port Rules

step 10:Set Up SonarQube on Azure VM

step 11:System Requirements

step 12:Configuration Steps

step 13:Docker Compose Setup

step 14:Access SonarQube

Summary of Commands

#### Step 1: Sign in to the Azure Portal
1. Open your web browser and go to the [Azure Portal](https://portal.azure.com).
2. Sign in with your Azure account credentials.

#### Step 2: Create a Virtual Machine
1. In the Azure portal, click on **Virtual machines** from the left-hand menu.
2. Click on the **+ Create** button, then select **Azure virtual machine**.

#### Step 3: Configure Project Details

![Screenshot (20)](https://github.com/user-attachments/assets/8d17b505-70a8-44ee-a350-10acf543de0f)

1. **Subscription**: Select your subscription (e.g., **Free Trial**).
2. **Resource group**: Choose the previously created resource group (`demo-resource-group`).
3. **Virtual machine name**: Enter `sonar-vm`.
4. **Region**: Select **(Europe) UK West**.
5. **Availability options**: Select **No infrastructure redundancy required** (default).
6. **Security type**: Choose **Trusted launch virtual machines** (default).

#### Step 4: Configure Image and Size

![Screenshot (22)](https://github.com/user-attachments/assets/a5e78221-2fc6-403d-9617-083a2df3d131)

1. In the left-hand menu of your VM's overview page, scroll down and click on **Networking**.

#### Step 3: Configure Inbound Port Rules

![Screenshot (27)](https://github.com/user-attachments/assets/4a8fd619-d727-4698-9887-0dfa3efee41b)

1. In the **Networking** pane, you will see the **Inbound port rules** section.
2. By default, Azure may have created a rule for SSH (port 22). To add an additional rule, click on **Add inbound port rule**.

#### Step 4: Set Up the New Inbound Port Rule

![Screenshot (30)](https://github.com/user-attachments/assets/9a5f3af2-016b-410e-a8bd-4ed4c9fd31a4)

1. In the **Add inbound security rule** pane, configure the following settings:
   - **Destination port ranges**: Enter `9000`.
   - **Protocol**: Select **TCP** from the dropdown menu.
   - **Name**: Optionally, you can enter a name for this rule (e.g., `TCP-9000`).
   - **Priority**: Leave this at the default or adjust as needed (lower numbers indicate higher priority).
   - **Action**: Ensure this is set to **Allow**.

2. After entering the necessary information, click on **Add** to create the inbound port rule.

#### Step 5: Verify the Inbound Port Rule
1. After adding the rule, you will return to the **Networking** pane.
2. Under **Inbound port rules**, verify that the new rule for TCP port `9000` is listed alongside the existing rules.

### Conclusion
You have successfully configured the inbound port rules for your Azure VM, allowing access on TCP port 9000 in addition to SSH (port 22). This configuration enables external traffic to reach your applications that may be running on port 9000, facilitating better functionality and access. 
Here’s a detailed, step-by-step guide to set up SonarQube on a Dockerized environment, using a `docker-compose.yml` file that includes both SonarQube and PostgreSQL as a database.

---
# Setting Up SonarQube on an Azure Virtual Machine for Continuous Code Quality Analysis
**Title: Setting Up SonarQube on an Azure Virtual Machine for Continuous Code Quality Analysis**

In this lab, we’ll deploy SonarQube on an Azure Virtual Machine to enable continuous code quality analysis and ensure code meets specific security, maintainability, and reliability standards. SonarQube, a popular tool for static code analysis, helps detect bugs, vulnerabilities, and code smells early in the development process. 

We will:

1. Set up an Azure Virtual Machine as the environment for SonarQube.
2. Configure the VM with the necessary dependencies for SonarQube.
3. Install and run SonarQube on the VM.
4. Verify SonarQube access and configuration for integration into CI/CD pipelines.

By the end of this lab, we’ll have a fully functional SonarQube setup ready for integration into a DevSecOps workflow, allowing automated quality checks and security analysis for code as part of the CI/CD pipeline. This Azure-based setup serves as a scalable solution for enterprise-level quality and security enforcement, reducing risks associated with code deployment.

### Step 1: Verify System Requirements

1. **RAM**: Make sure the instance has at least **4 GB of RAM**.
2. **Port 9000**: Ensure **port 9000 is open** as this is the default port for accessing SonarQube.

---

### Step 2: Configure System Settings

SonarQube requires certain kernel settings for optimal operation. You’ll modify the system configuration file to add these settings.

1. **Open the sysctl configuration file**:
   ```bash
   sudo vi /etc/sysctl.conf
   ```
   This command opens the file in the `vi` editor with superuser privileges.

2. **Add the following configurations** to the end of the file:
   ```bash
   vm.max_map_count=262144
   fs.file-max=65536
   ```
   - `vm.max_map_count=262144`: Sets the maximum number of memory map areas a process may have. SonarQube requires this setting for efficient memory handling.
   - `fs.file-max=65536`: Sets the maximum number of file descriptors available to the system, ensuring SonarQube has adequate resources.

3. **Save and Exit**:
   - In `vi`, press `Esc`, then type `:wq` and press `Enter` to save and close the file.

4. **Apply the changes**:
   ```bash
   sudo sysctl -p
   ```
   This command reloads the kernel parameters, applying your changes immediately.

---

### Step 3: Set the Hostname for Clarity

1. **Set the hostname to “SonarQube”**:
   ```bash
   sudo hostnamectl set-hostname SonarQube
   ```
   Setting the hostname to “SonarQube” helps in identifying the instance in multi-instance environments.

---

### Step 4: Update the System Packages

1. **Update package information** to ensure you have the latest versions:
   ```bash
   sudo apt update
   ```

---

### Step 5: Install Docker Compose

SonarQube and PostgreSQL will be managed by Docker Compose, which requires installation if not already present.

1. **Install Docker Compose**:
   ```bash
   sudo apt install docker-compose -y
   ```
   The `-y` flag automatically confirms installation prompts.

---

### Step 6: Create the Docker Compose File

The `docker-compose.yml` file will define the configuration for both SonarQube and PostgreSQL services.

1. **Create and open the `docker-compose.yml` file**:
   ```bash
   sudo vi docker-compose.yml
   ```
   
2. **Copy and paste the following content into the file**:
   ```yaml
   version: "3"
   services:
     sonarqube:
       image: sonarqube:community
       restart: unless-stopped
       depends_on:
         - db
       environment:
         SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
         SONAR_JDBC_USERNAME: sonar
         SONAR_JDBC_PASSWORD: sonar
       volumes:
         - sonarqube_data:/opt/sonarqube/data
         - sonarqube_extensions:/opt/sonarqube/extensions
         - sonarqube_logs:/opt/sonarqube/logs
       ports:
         - "9000:9000"
     db:
       image: postgres:12
       restart: unless-stopped
       environment:
         POSTGRES_USER: sonar
         POSTGRES_PASSWORD: sonar
       volumes:
         - postgresql:/var/lib/postgresql
         - postgresql_data:/var/lib/postgresql/data
   volumes:
     sonarqube_data:
     sonarqube_extensions:
     sonarqube_logs:
     postgresql:
     postgresql_data:
   ```

   **Explanation of Configuration**:
 - **Services**:
 - `sonarqube`: Specifies SonarQube as a service using the community edition image. It restarts automatically unless explicitly stopped. It also depends on the `db` service.
- **Environment Variables**:
- `SONAR_JDBC_URL`: Connection string to the PostgreSQL database.
- `SONAR_JDBC_USERNAME` and `SONAR_JDBC_PASSWORD`: Credentials for SonarQube to access the PostgreSQL database.
- **Volumes**: Directories within the SonarQube container are mapped to Docker volumes for persistence.
- **Ports**: Exposes port `9000` for accessing SonarQube.
- `db`: Specifies PostgreSQL as a service, required by SonarQube for data storage. It also restarts automatically unless stopped.
- **Environment Variables**:
- `POSTGRES_USER` and `POSTGRES_PASSWORD`: Defines the database user and password for SonarQube.
- **Volumes**: Configures Docker volumes to store database files persistently.

- **Volumes**:
- Defines Docker volumes for data persistence.

3. **Save and Exit**:
- In `vi`, press `Esc`, then type `:wq` and press `Enter` to save and close.

---

### Step 7: Run Docker Compose

Now that the configuration file is ready, start the services using Docker Compose.

1. **Start the SonarQube and PostgreSQL services**:
   ```bash
   sudo docker-compose up -d
   ```
   - The `-d` flag runs the services in the background.
   - Docker Compose will download the required images and set up the services based on the `docker-compose.yml` configuration.

---

### Step 8: Verify SonarQube Status

1. **Check the logs** to ensure SonarQube is running correctly:
   ```bash
   sudo docker-compose logs --follow
   ```
   - This command will display real-time logs, allowing you to confirm that the services started without issues.
   - Look for logs indicating that SonarQube is up and listening on port 9000.
   - **Note**: Press `CTRL + C` to exit the log view once you’ve confirmed everything is working.

---

### Step 9: Access SonarQube UI

1. **Open a web browser** and go to the following URL to access SonarQube’s interface:
   ```
   http://<your_SonarQube_publicdns_name>:9000/
   ```
   - Replace `<your_SonarQube_publicdns_name>` with your instance's public DNS name or IP address.
   - Once the page loads, you should see the SonarQube login screen, indicating a successful setup.

---

### Summary of Commands

```bash
# Step 1: Edit sysctl configuration
sudo vi /etc/sysctl.conf

# Step 2: Set the hostname
sudo hostnamectl set-hostname SonarQube

# Step 3: Update system packages
sudo apt update

# Step 4: Install Docker Compose
sudo apt install docker-compose -y

# Step 5: Create docker-compose.yml file
sudo vi docker-compose.yml

# Step 6: Run Docker Compose
sudo docker-compose up -d

# Step 7: Check the logs
sudo docker-compose logs --follow
```

This completes the setup of SonarQube using Docker Compose, with PostgreSQL as the database. You should now have a fully operational SonarQube instance accessible via your browser.
