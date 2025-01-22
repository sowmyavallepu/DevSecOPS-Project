# Step 1: Create a Local Directory for Your Project

1. **Open Terminal or Command Prompt**.

2. **Create the Main Directory** for your project and navigate into it:
   ```bash
   mkdir DEV-SEC
   cd DEV-SEC
   ```

3. **Clone Your Existing Project**: If you have an existing repository, clone it:
   ```bash
   git clone https://github.com/sowmyavallepu/gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo.git
   cd gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   ```

4. **Confirm Directory Structure**: Verify that you see the `.github/workflows` folder and other files by running:
   ```bash
   ls -la
   ```

   **Sample output** should look similar to this:
   ```plaintext
   total 32
   drwxr-xr-x 6 root root 4096 Oct 31 10:28 .
   drwxr-xr-x 4 root root 4096 Oct 31 10:28 ..
   drwxr-xr-x 7 root root 4096 Oct 31 10:28 .git
   -rw-r--r-- 1 root root  168 Oct 30 11:21 README.md
   drwxr-xr-x 2 root root 4096 Oct 31 10:28 demo
   drwxr-xr-x 6 root root 4096 Oct 31 10:28 webapp
   ```

### Step 2: Stage and Commit the `gitops.yaml` File in Visual Studio

Since you've already created the `gitops.yaml` and sonar-project.properties file in Visual Studio, follow these steps to stage and commit it:

1. **Stage the Changes**:
   ```bash
   git add .github/workflows/gitops.yaml 
   ```
 ```bash
   git add sonar-project.properties 
   ```
2. **Commit the Changes**:
   ```bash
   git commit -m "Add GitHub Actions workflow for SonarQube SAST scan"
   ```

### Step 3: Push the Changes to GitHub

1. **Push to the Main Branch**:
   ```bash
   git push origin main
   ```

### Step 4: Verify Workflow Execution on GitHub

1. **Go to the Actions Tab** in your GitHub repository.

2. **Confirm the Workflow Run**:
   - The workflow defined in `gitops.yaml` should start automatically based on the push to the `main` branch.
   - You can view the logs and check for successful execution.

### Step 5: Review Results in SonarQube

1. **Navigate to SonarQube Dashboard** at `http://<your-azure-vm-ip>:9000`.

2. **Check for Analysis Results** to review any issues or vulnerabilities identified by the SAST scan.

---

This guide combines all the steps, from creating the local directory to pushing and verifying the workflow execution in GitHub. Once the workflow completes, you’ll see the results directly in SonarQube and GitHub’s Actions tab.
#### Step 6: Verify the GitHub Actions Workflow

1. **Navigate to the Actions Tab**:
   - Go to your GitHub repository on the web.
   - Click on the **Actions** tab.

2. **Check Workflow Status**:
   - You should see the SonarQube Scan job running.
   - Click on it to view the logs and confirm it completed successfully.

#### Step 7: Review SonarQube Results

1. **Return to SonarQube Dashboard**:
   - Go back to your SonarQube instance at `http://<your-azure-vm-ip>:9000`.

2. **View Project Analysis**:
   - Navigate to your project.
   - Review the results of the SAST analysis.
  
   - ![Screenshot (55)](https://github.com/user-attachments/assets/3b78ac3b-63c0-44c1-94ab-1b6197b20e51)


### Summary

You have successfully created a local folder structure, configured SonarQube for SAST integration using GitHub Actions, and pushed your code. You can now monitor your code quality and security directly from the SonarQube dashboard.

Feel free to upload any pictures related to this process or let me know if you need further assistance!
