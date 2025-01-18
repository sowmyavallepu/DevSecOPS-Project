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



