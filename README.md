# Developing a Comprehensive End-to-End DevSecOps Pipeline for Super Mario Game

![screenshot](https://github.com/sowmyavallepu/DevSecOPS-Project/blob/9ad0ca7bf44acf0aff2a6da3b4cd2a8b344069b2/Screenshot%20(7).png)
Designing and implementing a robust DevSecOps pipeline to ensure the security, scalability, and continuous integration of the Super Mario game.
"Leveraging best practices in DevSecOps to automate development, security testing, and deployment processes, enabling faster and more secure updates for the game.
"Integrating security at every stage of the pipeline, from code development to deployment, to safeguard the Super Mario game against potential vulnerabilities.
# Table Of Contents
1. [Project Introduction](Project-Introduction)
2. [Project Prerequisties](Project-Prerequisties)
3. [Tools and Technologies Used](Tools-and-Technologies-Used)
4. [Pipeline Architecture](Pipeline-Architecture)
5. [Step-by-Step Implementation](#step-by-step-implementation)
6. Repository Cloning and Setuu
7. GitHub Actions Configuration
8. SonarQube Setup for Static Application Security Testing (SAST)
9. Containerizing the Mario Game with Docker
10. Dynamic Tagging and Version Control Implementation
11. Deployment to Azure Kubernetes Service (AKS) Using ArgoCD
12. End-to-End Pipeline Validation and Testing
13. Summary and Key Takeaways

## 🎮Project Introduction
Secure DevSecOPS Pipeline for Infinite Mario Game
This project focuses on building a secure, automated DevSecOps pipeline to streamline the development, security, and deployment lifecycle of the Infinite Mario JavaScript game. It showcases the complete DevSecOps workflow, incorporating code quality checks, security scans, and automated container deployments within a Kubernetes environment. By implementing a robust CI/CD process, the pipeline ensures proactive vulnerability detection, efficient and secure deployments, and a resilient, cloud-based infrastructure optimized for adaptability and scalability.
# 🛠️ This project Aims to Deliver:
- **Automated Code Quality and Security:** Integration with SonarQube for Static Application Security Testing (SAST) to detect potential security issues early.
- **Container Security:** Use of Grype and other tools to scan container images for vulnerabilities.
- **Infrastructure-as-Code (IaC):** Efficient deployment within Azure Kubernetes Service (AKS).
- **Continuous Deployment:** Leveraging GitHub Actions for automated CI/CD workflows, ensuring rapid and reliable releases.

## 📝 Project Prerequisties
Ensure the following prerequisites are met:

- **Azure Account:** An active Azure account (free or paid). Sign up [here](https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account?icid=azurefreeaccount) .
- **Azure Kubernetes Service (AKS):** Knowledge of Kubernetes in Azure.
- **Docker Hub Account:** For managing container images.
- **Docker CLI:** Installed on your local machine. [Get Docker CLI Here](https://www.docker.com/products/docker-desktop/).
- **GitHub Account:** GitHub repository with Actions enabled.
- **SonarQube Account:** Active SonarQube instance for running SAST scans.
- **Basic Knowledge of:**

     ○ **DevSecOps and CI/CD Pipelines** (GitHub Actions or similar tools).
  
     ○ **AKS** (Basic Kubernetes concepts).
  
     ○ **Docker** (Creating Dockerfiles, building images).
  
     ○ **JavaScript** (Modifying the Infinite Mario game).
  

## ⚙️ Tools and Technologies Used

**Azure Kubernetes Service (AKS)**

**Docker and Docker Hub**

**SonarQube for SAST Scans**

**GitHub Actions for CI/CD**

**Grype for Container Security**

## 🏗️ Pipeline Architecture

![image alt](https://github.com/sowmyavallepu/DevSecOPS-Project/blob/af5f3afc552ed6117cf2268a3358e0bfb5b5f6d8/Screenshot%20(3).png)

## Pipeline Workflow:

- **Code Commit:** Push code changes to GitHub.
- **Build & Test:** Run unit tests and static code analysis using SonarQube.
- **Containerization:** Build and push Docker images to Docker Hub.
- **Security Scanning:** Scan images with Grype.
- **Deployment:** Deploy to AKS using ArgoCD.
## 🗂️Modules

### **Module 1: DevSecOps Case Study and Prerequisites**
- [Step 1: Creating Azure Free Trial](./module-1_prerequisites/1-azure-account.md)
- [Step 2: Creating AKS Cluster](./module-1_prerequisites/step-2-creating-AKS-Cluster.md)
- [Step 3: Install ArgoCD on AKS](./module-1_prerequisites/step-3-install-argocd-on-aks.md)

### **Module 2: Integrate Static Application Security Testing (SAST) with SonarQube**
- [Step 1: Install SonarQube on Azure VM Instance](module-2/Step-8-Install-Sonarqube-azure-vm.md)
- [Step 2: Clone Mario GitHub Repository](module-2/Step-9-Clone-mario-repo.md)
- [Step 3: Integrate SonarQube for SAST - Part 1](module-2/Step-10-integrate-sonarqube-sast-part1.md)
- [Step 4: Integrate SonarQube for SAST - Part 2](module-2/Step-11-integrate-sonarqube-sast-part2.md)
- [Step 5: Integrate SonarQube for SAST - Part 3](module-2/Step-12-integrate-sonarqube-sast-part3.md)
- [Step 6: Integrate Sonarqube for SAST - part-4](module-2/Step-13-integrate-sonarqube-sast-part4.md)
- [Step 7: Implement Quality Gates for SAST](module-2/Step-14-implement-quality-gates.md)

### **Module 3: Dockerization of Mario Game Project**
- [Step 1: Create DockerHub Account](module-3/step-1-Create-dockerhub-account.md)
- [Step 2: Write a Dockerfile for Mario Game](module-3/step-2-write-dockerfile.md)
- [Step 3: Build and Push Mario Docker Image - Part 1](module-3/step-3-build-mario-docker-image-part1.md)
- [Step 4: Build and Push Mario Docker Image - Part 2](module-3/step-4-build-push-mario-docker-image-part2.md)
- [Step 5: Implement Dynamic Tagging for Mario Docker Image](module-3/step-5-implement-dynamic-tagging.md)

### **Module 4: Implement Container Scan for Mario Game**
- [Step 1: Implement Container Scan - Part 1](module-4/step-1-Implement-container-scan.md)
- [step 2: Implement Container Scan - Part 2](module-4/step-2-Implement-container-scan-part-2.md)

### **Module 5: Deploy Mario Docker Game on Azure Kubernetes Cluster**
- [Step 1: Deploy Mario Game with ArgoCD - Part 1](module-5/Deploy-Mario-Game-On-Azure-Kubernetes-Cluster-using-argoCd-Part-2.md)
- [Step 2: Deploy Mario Game with ArgoCD - Part 2](module-5/Deploy-Mario-Game-on-Azure-Kubernetes-Cluster-using-ArgoCD-Part-1.md)

### **Module 6: Implement End-to-End DevSecOps Pipeline for Mario Game**
- [Step 1: understanding the Pipeline](module-6/step-1-understanding-end-to-end-devsecops-pipeline-for-mario-game.md)
- [Step 2: Hands-On: Part 1](module-6/step-2-hands-on-part-1-implement-end-to-end-devsecops-pipeline-for-mario-game.md)
- [Step 3: Hands-On: Part 2](module-6/step-3-hands-on-part-2-implement-end-to-end-devsecops-pipeline-for-mario-game.md)

---

## 🎯 Conclusion

In this project, we successfully built a complete DevSecOps pipeline for deploying the Infinite Mario Game. By integrating security at every stage of the CI/CD pipeline, from SAST to container security scans and seamless deployment with AKS, we demonstrated how DevSecOps practices help build secure, scalable applications. This ensures a secure and automated software development lifecycle that allows for continuous improvements and rapid delivery without compromising security.
