Here's a detailed, step-by-step guide to clone the "Mario" GitHub repository, set it up on your local system, and ensure everything is properly organized. We'll walk through creating a project folder, cloning the repository, and verifying the setup in Visual Studio Code.

---

# **Lab Title**: DevSecOps SonarQube SAST Scan with Super Mario Repo

**Description**: This lab will guide you through setting up a DevSecOps project locally by cloning the Super Mario SonarQube SAST scan repository from GitHub, verifying the setup, and preparing the environment for future DevSecOps work.

---

### Step-by-Step Guide

1. **Open PowerShell or Terminal**  
   Start by opening your terminal (PowerShell on Windows or Terminal on Mac/Linux) to begin the setup.

2. **Create a Project Folder**  
   To keep the project files organized, create a folder where the cloned repository will be stored. Name it `Dev-Sec`.
   
   ```powershell
   mkdir C:\Dev-Sec
   cd C:\Dev-Sec
   ```
   
   This command creates the `Dev-Sec` folder on your `C:` drive and navigates into it.

3. **Fork the Repository**  
   - Go to the GitHub repository page: `https://github.com/asecurityguru/gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo`
   - Click the **Fork** button on the top-right to create a copy of the repository in your GitHub account.

4. **Clone the Repository**  
   After forking (if needed), use the HTTPS or SSH link to clone the repository to your local `Dev-Sec` folder. Run the following command:
   
   ```powershell
   git clone https://github.com/asecurityguru/gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo.git
   ```
   
   Expected output:
   ```plaintext
   Cloning into 'gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo'...
   remote: Enumerating objects: 231, done.
   remote: Counting objects: 100% (32/32), done.
   remote: Compressing objects: 100% (17/17), done.
   remote: Total 231 (delta 27), reused 15 (delta 15), pack-reused 199 (from 1)
   Receiving objects: 100% (231/231), 1.25 MiB | 1.64 MiB/s, done.
   Resolving deltas: 100% (68/68), done.
   ```

5. **Verify Cloning Success**  
   To confirm the repository was cloned, list the contents of the `Dev-Sec` directory using:

   ```powershell
   dir
   ```
   
   You should see a folder named `gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo` listed. Example output:

   ```plaintext
   Directory: C:\Dev-Sec

   Mode                 LastWriteTime         Length Name
   ----                 -------------         ------ ----
   d-----        10/30/2024  11:21 AM                gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   ```

6. **Navigate into the Cloned Directory**  
   Change to the repository directory to access its contents:
   
   ```powershell
   cd .\gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo\
   ```

7. **Open the Project in Visual Studio Code**  
   To open this project in Visual Studio Code, use the following command:
   
   ```powershell
   code .
   ```
   
   This will open the entire `gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo` project in Visual Studio Code, where you can browse and edit files.

8. **Locate and Review the README File**  
   In Visual Studio Code, find and open the `README.md` file. This file typically contains essential project details, instructions, code examples, and setup steps.

   ![Screenshot (33)](https://github.com/user-attachments/assets/64644b58-8992-4374-98a2-9e07cbb03c19)


### Summary

You have successfully cloned the GitHub repository, set it up in your `Dev-Sec` project folder, and opened it in Visual Studio Code for editing. The repository structure is now in place and ready for the next steps in configuring, testing, or extending this DevSecOps project. 


