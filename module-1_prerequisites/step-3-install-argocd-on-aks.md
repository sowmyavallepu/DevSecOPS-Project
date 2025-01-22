# **Module 2: Installing ArgoCD on AKS**

**Step 1: Connect to AKS Cluster**

```bash
az aks get-credentials ---resource-group <demo-resource-group> --name <demoKubernetesCluster>
```

- **Purpose**: Connects your local machine to your AKS (Azure Kubernetes Service) cluster by retrieving the cluster credentials.
- **Explanation**:
  - `az aks get-credentials` is an Azure CLI command that fetches and configures the kubectl credentials.
  - `--resource-group <resource-group>` specifies the Azure resources group where your AKS cluster is hosted.
  - `--name <aks-cluster-name>` is the name of ther AKS cluster you want to connect to.

**Step:2: Create a Namespace argocd

```bash
kubectl create namespace argocd
```
- **Purpose**: Creates a separate namespace in kuberenetes called `argocd` .
- Explanation :
  - `kubectl create namespace` is a Kubernetes command to create a new namespace.
  - Namespaces in Kubernetes are used to organize resources and separate workloads for different environments or applications.
 
**Step 3: Install ArgoCD in the `argocd` Namespace**

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- **Purpose**: Installs ArgoCD using predefined YAML configurations from the ArgoCD GitHub repository.
- **Explanation**:
  - `kubectl apply -n argocd` applies the YAML configuration to the `argocd` namespace.
  - `-f` specifies the file or URL containing the YAML configuration.
  - The URL points to ArgoCD’s installation manifest, which defines all the necessary resources (pods, services, roles) needed to run ArgoCD on Kubernetes.

**Step 4: Verify ArgoCD Resources**

```bash
kubectl get all -n argocd
```
- **Purpose**: Lists all resources (pods, services, etc.) created by ArgoCD in the `argocd` namespace.
- **Explanation**:
  - `kubectl get all` retrieves all resource types, including pods, services, deployments, etc.
  - `-n argocd` limits the command to display resources only within the `argocd` namespace.
  - This command helps verify that ArgoCD components have been correctly deployed.
 
**Step 5: Set Up LoadBalancer for External Access**

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
- **Purpose**: Changes the `argocd-server` service type to LoadBalancer, allowing external access.
- **Explanation**:
  - `kubectl patch svc` updates the configuration of the specified service.
  - `argocd-server` is the name of the service to be patched.
  - `-n argocd` specifies the `argocd` namespace.
  - `-p '{"spec": {"type": "LoadBalancer"}}'` updates the service spec to type `LoadBalancer`, which provides a public IP.

**Step 6: Retrieve LoadBalancer IP and Port**

```bash
kubectl get svc -n argocd
```
- **Purpose**: Retrieves service details, including the external IP address of the `argocd-server` service.
- **Explanation**:
  - `kubectl get svc` lists all services.
  - `-n argocd` limits the output to the `argocd` namespace.
  - The external IP and port are necessary for accessing the ArgoCD UI from a browser.

**Step 7: Access ArgoCD in Browser**

   - **Explanation**: After getting the IP and port, you can enter it in the format `http://<External-IP>:<Port>` in a browser to access the ArgoCD UI.
   - If you see a "Not Secure" warning, proceed by selecting **Advanced** and clicking **Proceed** to open the UI.

   - ![R](https://github.com/user-attachments/assets/a55c850d-a05d-43aa-9b30-090db317255c)
     
   - below is the image of argo cd ui:
     
   - ![Screenshot (16)](https://github.com/user-attachments/assets/417f520f-e37b-44b3-a572-693f2a625ca5)

**Step 8: Log into ArgoCD**

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```
- **Purpose**: Retrieves the initial admin password for ArgoCD login.
- **Explanation**:
  - `kubectl -n argocd get secret argocd-initial-admin-secret` retrieves the ArgoCD admin secret from the `argocd` namespace.
  - `-o jsonpath="{.data.password}"` extracts the password field.
  - `base64 -d` decodes the password, which is stored in base64.
  - `echo` prints the password to the terminal for easy copying.
 
  - ![Screenshot (17)](https://github.com/user-attachments/assets/6e37d212-b47d-4aa9-b27d-e02e85dadfb6)

By this we successfully completed installing argocd on aks cluster


