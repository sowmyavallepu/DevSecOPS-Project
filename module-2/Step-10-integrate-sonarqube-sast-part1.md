# **Lab Title**: Integrate SonarQube for SAST in DevSecOps Pipeline

**Description**: This lab provides a structured approach to integrate SonarQube SAST into a DevSecOps pipeline. This initial setup will lay the foundation to scan code for vulnerabilities automatically, enhancing the security of your development process.

---

### Step-by-Step Guide

#### **1. Open Project in Visual Studio Code**

1. Open Visual Studio Code.
2. Navigate to the project folder where you cloned the GitHub repository:
   ```plaintext
   C:\Dev-Sec\gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   ```
3. Verify you’re in the correct directory.

#### **2. Create the `.github` Folder**

1. In Visual Studio Code, right-click on the root project folder and select **New Folder**.
2. Name the folder `.github`.
3. Your folder structure should look like this:
   ```plaintext
    gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   └── .github
   ```

![Screenshot (37)](https://github.com/user-attachments/assets/2449e4d7-83a2-4398-840d-fda4d44192ea)


#### **3. Create the `workflows` Folder**

1. Inside the `.github` folder, create another new folder.
2. Name this folder `workflows`.
3. Your updated folder structure should look like this:
   ```plaintext
   gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   └── .github
       └── workflows
   ```

![Screenshot (38)](https://github.com/user-attachments/assets/6c9d2419-5c9e-45ec-a156-848704216047)


#### **4. Create the `gitops.yaml` File for Workflow Configuration**

1. Inside the `workflows` folder, create a new file named `gitops.yaml`.
2. Save the file; this is where you’ll define the GitHub Actions workflow configuration for integrating SonarQube SAST scanning.

---

![Screenshot (35)](https://github.com/user-attachments/assets/017eb423-8dc9-4d19-a280-28383ffa6f60)



### Summary

With these four foundational steps, you’ve set up the structure to implement a GitHub Actions workflow using `gitops.yaml`. The next steps would involve configuring this file to run security scans and ensure continuous integration of SAST into your DevSecOps pipeline.
