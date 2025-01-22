# **Lab: Deploy Mario Game on Azure Kubernetes Service using ArgoCD - Part 2 (with Changes)**

---

# **Goal of this Lab:**
- To deploy the Super Mario game Docker image to Azure Kubernetes Service (AKS) using **ArgoCD** and manage it using GitOps principles.
- Once the game is deployed, we'll interact with the game through the browser, and demonstrate the changes we made to the game startup behavior (pressing 'Y' to start). In the next lab, we'll automate further changes via a DevSecOps pipeline.

---

### **Steps Involved:**

---

#### **Step 2: Add `deployment.yaml` to Your GitHub Repository**

1. In your **GitHub repository**, add a file named `deployment.yaml` at the root level. This file contains the deployment configuration for your Super Mario game and the service configuration. Here's the content for `deployment.yaml`:

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: supermariogame-deployment
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: supermariogame
     template:
       metadata:
         labels:
           app: supermariogame
       spec:
         containers:
         - image: rahuldocker628/mariogitopsproject:2
           name: supermariogame-container
           ports:
           - containerPort: 8080
   ---
   apiVersion: v1
   kind: Service
   metadata:
     name: supermariogame-service
   spec:
     selector:
       app: supermariogame
     ports:
     - protocol: TCP
       port: 8600  # <-- Change this line to your desired port
       targetPort: 8080
     type: LoadBalancer
   ```

   **Explanation of `deployment.yaml`:**
   - The **Deployment** resource defines the container image (`rahuldocker628/mariogitopsproject:2`) and the desired state for the `supermariogame` application.
   - The **Service** resource exposes the deployment through a **LoadBalancer** with the port `8600` mapped to container port `8080`.

2. **Push the Changes to the Remote GitHub Repository:**
   After adding the `deployment.yaml` file, push the changes to the remote GitHub repository.

   ```bash
   git add deployment.yaml
   git commit -m "Add deployment.yaml for Super Mario Game"
   git push origin main
   ```

---

#### **Step 3: Wait for ArgoCD to Sync the Changes**

1. ArgoCD **auto-sync** will automatically detect the changes in your GitHub repository.
2. By default, ArgoCD refreshes every 3 minutes, so wait for at least 3-4 minutes for the application to sync and deploy.

   You can verify the sync status in the ArgoCD UI by going to the **Applications** tab and checking the sync status of the `supermariogame` application. The status should change from **OutOfSync** to **Synced** once the deployment is successfully applied.

---

#### **Step 4: Copy the LoadBalancer IP from ArgoCD UI**

1. Once the application is synced and deployed, you can access the external IP of the service through **ArgoCD UI**.
   
2. Go to the **ArgoCD UI**, and select the `supermariogame` application.

3. Navigate to the **Resources** tab, and find the **Service** resource. You will see the `supermariogame-service` listed there.

4. Under the **EXTERNAL-IP** column, you should find the **LoadBalancer** IP address. This IP will be used to access the game.

   ![Screenshot (83)](https://github.com/user-attachments/assets/41753acf-f78c-41d9-8f99-68cdf9c4dd12)

   
   ![Screenshot (84)](https://github.com/user-attachments/assets/7193e2ac-cace-4920-b98b-39dfc1b2fc15)



---

#### **Step 5: Open the Mario Game in the Browser**

1. **Copy the LoadBalancer IP** from the ArgoCD UI (from the previous step).
   
2. **Open your Browser**, and paste the copied LoadBalancer IP followed by the port `8600` in the address bar. For example:

   ```
   http://<EXTERNAL-IP>:8600
   ```

3. The **Super Mario Game** will open in the browser. As per the changes you made to the game, it will prompt you to **press 'Y'** to start the game.

   **Note:** This change was made to the game code, where the game now requires the player to press **'Y'** to start.

4. After pressing **'Y'**, you will be able to play the game.

   ![Screenshot (87)](https://github.com/user-attachments/assets/6bcfa36a-a8d8-43d9-a588-b3a220b6df59)


---

#### **Step 6: Modifying the Game Behavior for Next Lab**

In this lab, we’ve modified the game to make it prompt players to **press 'Y'** to start. 

**Next Steps (in the next lab)**:
- In the upcoming lab, we will further modify the game so that instead of pressing **'Y'** to start, players will press **'S'**.
- We'll automate the game change using a **DevSecOps pipeline** to ensure a seamless CI/CD process with security measures integrated.

---

### **Conclusion:**

At the end of this lab:
- You successfully created an application in ArgoCD and connected it to your GitHub repository.
- You added a `deployment.yaml` file containing Kubernetes manifests to deploy the Super Mario game.
- ArgoCD automatically synced the changes, and the application was deployed to your AKS cluster.
- You accessed the deployed Mario game via the LoadBalancer IP and port `8600`, where you modified the game to press **'Y'** to start.

In the next lab, we will continue enhancing the game by automating game changes via a **DevSecOps pipeline**, and modify the game to use **'S'** instead of **'Y'** as the start button.

Let me know if you need further assistance!
