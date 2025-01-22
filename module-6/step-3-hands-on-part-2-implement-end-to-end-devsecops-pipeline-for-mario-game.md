### 🎮 Lab 3: Deploying SuperMario Image with Updated Controls  

In this lab, we ensured seamless deployment of the SuperMario game with updated controls by traversing every stage of our DevSecOps pipeline. This included static code analysis, Docker image management, container scanning, and automated Kubernetes deployment via GitOps.  

---

## 📝 Lab Steps  

### **Step 1: Define Secrets in GitHub Repository**  

Ensure the following secrets are configured in the repository for secure pipeline execution:  

| **Secret Name**      | **Description**                                |
|-----------------------|-----------------------------------------------|
| `SONAR_HOST_URL`      | SonarQube server URL.                         |
| `SONAR_TOKEN`         | Token for authentication with SonarQube.      |
| `DOCKERHUB_TOKEN`     | Token for DockerHub login.                    |
| `DOCKERHUB_USERNAME`  | Your DockerHub username.                      |
| `GIT_EMAIL`           | Email for Git configuration.                  |
| `GIT_USERNAME`        | Username for Git configuration.               |

---

### **Step 2: Push Changes to Remote Repository**  

- Push your updated workflow or game control code to the **main branch** to trigger the pipeline:  

  ```bash
  git add .
  git commit -m "Updated game controls and pipeline workflow"
  git push origin main
  ```

- This action will initiate the GitHub Actions workflow (`e2e-gitops.yaml`).  

---

### **Step 3: Validate Image and Deployment Files**  

1. **Verify New Docker Image Tag:**  
   - Check on [DockerHub](https://hub.docker.com/) to ensure a new tag is assigned to the SuperMario Docker image (e.g., `v2`, `v3`).  

2. **Validate `version.txt`:**  
   - Ensure the new image tag is stored in `version.txt`.  

   ```bash
   cat version.txt
   ```

3. **Confirm Update in `deployment.yaml`:**  
   - The `image` field in the `deployment.yaml` file should reference the updated tag:  

   ```yaml
   containers:
     - name: supermario-app
       image: rahuldocker628/mariogitopsproject:<new-tag>
   ```

---

### **Step 4: Wait for ArgoCD Refresh or Trigger Sync**  

- Wait for the **3-minute ArgoCD default refresh interval** to automatically detect and apply the deployment changes.  
- Alternatively, manually sync the application via the ArgoCD Web UI:  

  1. Navigate to the application managing your deployment.  
  2. Click **Sync** to ensure the latest changes are deployed.  

---

### **Step 5: Verify Deployment on AKS**  

1. **Check Running Pods:**  
   - Use `kubectl` to confirm that the updated image is deployed to AKS:  

     ```bash
     kubectl get pods -n <namespace>
     ```
![Screenshot (88)](https://github.com/user-attachments/assets/856b6588-ed92-4f24-b397-b54e70473c75)


2. **Test the SuperMario Game:**  
   - Access the game via its URL.  
   - Clear your browser cache to load the latest code changes.  
   - Play the game using the **updated controls**, ensuring the changes reflect in the deployed application.

     ![Screenshot (89)](https://github.com/user-attachments/assets/c2a49579-08e7-4b39-bb12-ad0328b0087f)


---

### ✨ **Final Note**  

Congratulations! 🎉 You have successfully deployed the SuperMario game with updated controls through a secure and automated DevSecOps pipeline.  

This deployment journey showcased how code changes traverse seamlessly through:  

- **SonarQube** for Static Application Security Testing (SAST) 🛡️.  
- **GitHub Actions** for pipeline execution 🚀.  
- **DockerHub** for container image management 🐳.  
- **Grype** for container image vulnerability scanning 🔍.  
- **ArgoCD** for GitOps-based continuous delivery to AKS 🎯.  

By testing and playing the game with the updated controls, you have confirmed that the entire process—from code to deployment—is robust, secure, and efficient. 🌟  

This accomplishment demonstrates the power of integrating security, automation, and reliability into your DevSecOps practices. Excellent work! 🙌
