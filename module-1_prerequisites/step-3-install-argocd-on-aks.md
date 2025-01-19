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
- 
