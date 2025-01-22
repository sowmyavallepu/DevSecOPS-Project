# step-by-step guide for setting up the SonarQube integration, with the simplified sonar-project.properties configuration.
### **Step 5: Create a Project in SonarQube**

1. **Log in to SonarQube**:
   - Open SonarQube in your browser and sign in.

2. **Create a New Project**:
   - On the SonarQube dashboard, click **Create Project**.
   - For **Project Key**, enter `gitopsdevsecopspipeline`.
   - Provide a **Project Display Name** as desired.
   - Click **Set Up** to finalize the project.

3. **Record the Project Key**:
   - Note down the **Project Key** (`gitopsdevsecopspipeline`) for configuring your properties file in the next step.

---

### **Step 6: Create a `sonar-project.properties` File in Visual Studio Code**

1. **Open Your Repository**:
   - Open your project directory in Visual Studio Code.

2. **Create the `sonar-project.properties` File**:
   - In the root of your project, create a new file named `sonar-project.properties`.

3. **Add Configuration to `sonar-project.properties`**:
   - Paste the following line into the file:

     ```properties
     sonar.projectKey=gitopsdevsecopspipeline
     ```

4. **Save the File**:
   - Save this configuration in the project root directory.

---

### **Step 7: Create a Security Token in SonarQube**

1. **Generate a Token**:
   - In SonarQube, go to **My Account** > **Security**.
   - Under **Generate Tokens**, enter a name like `GitOpsToken` and click **Generate**.
   - Copy the token and save it securely (it will only be shown once).

---

### **Step 8: Set Up Secrets in GitHub**

1. **Open GitHub Repository**:
   - Go to your GitHub repository.

2. **Go to Settings**:
   - Select **Settings** from the main page.
  
   - ![Screenshot (48)](https://github.com/user-attachments/assets/b041d9e3-a18c-41f3-8706-cee77314bdc2)

3. **Add the SONAR_HOST_URL Secret**:
   - Under **Settings**, navigate to **Secrets and variables** > **Actions** > **New repository secret**.
   - Name it `SONAR_HOST_URL`, with the value being your SonarQube URL (e.g., `http://your-sonarqube-url`).
   - Click **Add secret**.

4. **Add the SONAR_TOKEN Secret**:
   - Again, go to **Secrets and variables** > **Actions** > **New repository secret**.
   - Name it `SONAR_TOKEN` and paste the token from Step 7.
   - Click **Add secret**.

![Screenshot (50)](https://github.com/user-attachments/assets/b769566c-b668-447e-8967-ddb6d6ad7a75)
---

This configuration enables SonarQube SAST scanning on the `gitopsdevsecopspipeline` project using minimal setup and repository secrets in GitHub.
