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

1. **Image**: From the dropdown, select **Ubuntu Pro 24.04 LTS - x64 Gen2** (default).
2. **VM architecture**: Select **x64** (default).
3. **Size**: Click on **See all sizes**. Choose **Standard_B2s - 2 vCPUs, 4 GiB memory** (default).
4. **Enable Hibernation**: Ensure this is disabled as it does not support Trusted launch for Linux images (default).

#### Step 5: Configure Administrator Account

Authentication type: Select Password instead of SSH public key.
Username: Enter azureuser (default).
Password: Enter a strong password (ensure it meets Azure's password complexity requirements).

#### Step 6: Configure Inbound Port Rules
1. **Public inbound ports**: Select **Allow selected ports** (default).
2. **Select inbound ports**: Choose **SSH (22)** (default).

#### Step 7: Review and Create
1. After configuring all settings, click on **Review + create**.
2. Review your settings and ensure everything is correct.
3. Click on **Create** to provision the virtual machine.

#### Step 8: Access Your Virtual Machine
Once the VM is created, navigate to the Virtual machines section.

Click on sonar-vm to view its details.

To connect via SSH, use a terminal or SSH client. Run the following command (replace <public-ip> with the actual public IP of your VM):

bash
Copy code
ssh azureuser@<public-ip>
Note: You will be prompted to enter the password you set during the VM creation.

### Conclusion
You have successfully created a virtual machine in Azure named `sonar-vm` within the `demo-resource-group`, using default settings for all other sections. You can now access it using SSH and begin your work with this VM.

 ### Now we need to Configure Inbound Port Rules for Your Azure VM

#### Step 1: Navigate to Your Virtual Machine

1. In the left-hand menu, click on **Virtual machines**.
2. From the list of virtual machines, click on the name of your VM (e.g., **sonar-vm**).

#### Step 2: Go to Networking Settings

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



