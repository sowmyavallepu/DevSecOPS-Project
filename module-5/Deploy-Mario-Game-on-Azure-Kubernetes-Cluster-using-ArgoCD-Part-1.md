

### **Goal**: Deploy the Super Mario Docker image to Azure Kubernetes Service (AKS) using ArgoCD.

### **Steps Involved**:

#### **Step 1: Create an Application in ArgoCD**

1. **Log in to ArgoCD:**
   Access your ArgoCD UI by navigating to the URL. 

   Example:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:80
   ```

   Now, visit `http://localhost:8080` to access the ArgoCD UI.

   - **Login** with the username `admin` and the password 

2. **Create a New Project in ArgoCD:**



   - In the ArgoCD UI, click on the **"Projects"** tab on the left sidebar.
   - Click the **"New Project"** button to create a new project.
   - **Project Name:** `supermariogamedeployment`
   - Leave the rest of the options as defaults.
   - Click **Create**.

3. **Create an Application in ArgoCD:**


   - Once the project is created, click on the **"Applications"** tab.
   - Click **"New App"** to create an application.
   
   Here's how to fill in the required fields:

   
   
   - **Application Name:** `supermariogame`
   - **Project:** `default` (as the project we created earlier is in the default group).
   - **Sync Policy:** Choose **Automatic** to enable auto-sync for continuous deployment.

   ![Screenshot (79)](https://github.com/user-attachments/assets/f26d6337-8ca5-4968-a071-71eff76b6c39)


   **Source Section:**
   - **Repository URL:** `https://github.com/RAHUL-AMBARAGONDA/gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo.git`
   - **Revision:** `HEAD` (which will use the latest commit from the default branch).
   - **Path:** `/` (this indicates that the Kubernetes manifests are at the root of the Git repository).

  ![Screenshot (80)](https://github.com/user-attachments/assets/9bdcc493-982a-4d5c-af6d-e2e4c4bcab40)
   
   **Destination Section:**
   - **Cluster URL:** `https://kubernetes.default.svc` (for the Kubernetes cluster within the current AKS cluster).
   - **Namespace:** `default` (use the `default` namespace for simplicity unless you wish to use a specific namespace).

   - After filling out the above fields, click **Create**.

 ![Screenshot (81)](https://github.com/user-attachments/assets/915dae6d-fa14-489f-b5f5-ec67ce49e911)

#### **Step 2: Sync the Application**

Once the application is created in ArgoCD, it will show up on the **Applications** dashboard:

1. Click on the newly created `supermariogame` application.
2. You'll see that the **Sync Status** is **OutOfSync** initially.
3. Click the **Sync** button to initiate the synchronization between your GitHub repository and the AKS cluster.

Once ArgoCD pulls the manifest and deploys the Mario game, it will show the **Sync Status** as **Synced**, indicating the deployment was successful.

![Screenshot (82)](https://github.com/user-attachments/assets/272d0bdf-1f37-4842-bfc0-1b14f22884ad)

---

### **Conclusion:**

With these steps, you have deployed the Super Mario game Docker image onto your Azure Kubernetes Service (AKS) cluster using ArgoCD. This approach follows **GitOps practices** where ArgoCD pulls changes from the GitHub repository, automates deployment to Kubernetes, and ensures that the cluster state is always in sync with the repository. 

If you want to see changes in real-time, any update in the repository will trigger the auto-sync feature, automatically updating the deployed Mario game in your AKS cluster.
