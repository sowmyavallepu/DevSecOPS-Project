# Kubernetes Cluster Creation on Azure 🚀

A step-by-step guide to creating a Kubernetes cluster in Azure, optimized for **DevSecOps** and demo purposes.

---

## Table of Contents
1. [Access Kubernetes Creation Wizard](#1-access-the-kubernetes-cluster-creation-wizard-in-azure)
2. [Configure Basic Cluster Settings](#2-configure-basic-cluster-settings)
3. [Set Up Node Pools](#3-configuring-node-pools-for-kubernetes-cluster)
4. [Configure User Node Pool](#4-configuring-the-user-node-pool-in-kubernetes-cluster)
5. [Set Up Networking](#5-configuring-networking-for-the-kubernetes-cluster)
6. [Review & Deploy Cluster](#finalizing-cluster-configuration)

---

## 1. Access the Kubernetes Cluster Creation Wizard in Azure 🔑
- **Log in** to your Azure portal at [portal.azure.com](https://portal.azure.com).
- In the search bar, type **Kubernetes services** and select it.
- Click on **+ Create** to initiate the Kubernetes cluster setup.

![screenshot](https://github.com/user-attachments/assets/16710f9e-1934-4685-a688-181e64e0befa)

---

## 3. Configure Kubernetes Version and Upgrade Settings 🛠️
- **Kubernetes Version**: Choose **1.27.7** (or default version).
- **Automatic Upgrade**: **Disabled** for manual control.

  ![Screenshot (93)](https://github.com/user-attachments/assets/68db415b-cd42-467d-8f28-7f9310097220)


---

## 4. Set Authentication and Authorization Options 🔒
- **Authentication Method**: Choose **Local Accounts** with **Kubernetes RBAC** for Role-Based Access Control.

---

##  5. Configure Node Pools for Kubernetes Cluster 🖥️

### Understanding Node Pool Quotas and Limitations (Free Tier)
- **Free Tier Quotas**: Limited to 4 node pools, with 4 vCPUs max (e.g., 2 vCPUs each for two nodes).
- Suitable for lightweight applications or demos.

### Node Pool Configuration Steps

#### Delete Default System Node Pool (Optional)
> ⚠️ If using the Free Tier, delete the default system node pool to optimize capacity.

#### System Node Pool Setup
- **Name**: `demosysnode`
- **Mode**: **System**
- **OS SKU**: **Azure Linux**
- **Availability Zones**: **None**
- **Node Size**: **Standard B2pls v2** (2 vCPUs, 4 GiB memory)
- **Scale Method**: **Manual** or **Autoscale**
- **Node Count**: **1**
- **Max Pods per Node**: **30**

![screenshot](https://github.com/user-attachments/assets/d7b8c5bd-7508-4f29-a234-257bb1d493e9)


#### User Node Pool Setup
- **Name**: `demousermode`
- **Mode**: **User**
- **OS SKU**: **Azure Linux**
- **Availability Zones**: **Zone 1**
- **Node Size**: **Standard D2pds v5** (2 vCPUs, 8 GiB memory)
- **Scale Method**: **Autoscale**
- **Node Count**: Initial **3 nodes**

![screenshot](https://github.com/user-attachments/assets/4980320c-6c8d-46e4-95f6-4cbecd873a2e)


---
## 6. Configuring Networking for the Kubernetes Cluster 🌐

- **Public Access**: Use **Authorized IP Ranges** for restricted access.
- **Container Networking**: **kubenet**
- **VNet**: **Disabled** (default)
- **DNS Name Prefix**: `demoKubernetesCluster-dns`
- **Network Policy**: None
- **Load Balancer**: **Standard**

---

## Finalizing Cluster Configuration ✅

1. **Set Other Options to Default**: For simplicity in this demo, keep default values.
2. **Review and Create**: Click **Review + Create**.
   - Azure will validate your configuration; correct any errors if needed.
3. **Deploy**: Once validated, click **Create** to provision the cluster.

⏳ Deployment may take a few minutes. Once complete, you will see a confirmation.

Your Kubernetes cluster `demoKubernetesCluster` is now live, with networking security, node pools, and DNS prefix configured for immediate use in containerized applications on Azure!


---

Let me know if you need any further customization or details on deploying applications to this cluster!

