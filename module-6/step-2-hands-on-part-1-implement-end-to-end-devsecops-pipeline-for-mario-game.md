Here’s an updated version of the **README.md** with relevant icons added for better visual appeal and clarity. Icons are represented using Unicode emojis and SVG links for a professional look in platforms supporting rich markdown.

---

# 🎮 Module 8: End-to-End DevSecOps Pipeline for Mario Game  

In this module, we test our DevSecOps pipeline by introducing a functional change to the SuperMario game controls. This provides an opportunity to validate how the pipeline handles modifications while ensuring the integrity and security of the deployment.  

---

## 🎯 Objective  

To build, secure, and automate a DevSecOps pipeline that:  
1. 🔍 Detects and validates code changes.  
2. 🛡️ Performs static code analysis (SAST).  
3. 📦 Builds, scans, and pushes a Docker image.  
4. ⚙️ Updates Kubernetes deployment files automatically.  

---

## 📝 Step-by-Step Implementation  

### 🔧 Step 1: Modify Game Controls  

1. 🕹️ Update the game controls so that the **"S" key** is used for movement.  
2. Replace the `webapp` folder in your repository with the updated code.  
3. Commit and push the changes to the `main` branch:  

   ```bash
   git add webapp
   git commit -m "Updated Mario game controls to use 'S' key"
   git push origin main
   ```

---

### ⚙️ Step 2: Workflow File  

Add the following workflow file, `e2e-gitops.yaml`, to the `.github/workflows/` directory. This workflow automates the entire process, from SAST to deployment:  

```yaml
name: "Run SAST, Build and push supermario image, scan image, Update deployment and version txt files"

on:
  push:
    branches:
      - main

env:
  VERSION: $(( $(cat version.txt) + 1 ))

jobs:

  sonarqube_sast_scan:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0  # Shallow clones should be disabled for better analysis relevance

      - name: SonarQube Scan 🛡️
        uses: sonarsource/sonarqube-scan-action@master
        env:
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

  build_push_supermario_docker_image:
    runs-on: ubuntu-latest
    needs: sonarqube_sast_scan

    steps:
      - name: Checkout Repository 📥
        uses: actions/checkout@v3

      - name: Login to Docker Hub 🔐
        run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USERNAME }}" --password-stdin

      - name: Build and Push Docker Image 📦
        run: |
          docker build -t docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }} .
          docker push docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}

  run_container_image_scan_on_supermario_docker_image:
    runs-on: ubuntu-latest
    needs: build_push_supermario_docker_image

    steps:
      - name: Checkout Repository 📥
        uses: actions/checkout@v3

      - name: Login to Docker Hub 🔐
        run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USERNAME }}" --password-stdin

      - name: Get Docker Image from Docker Hub 🖼️
        run: |
          docker pull docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}
          docker save -o supermariolatestdockerimage.tar docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}

      - name: Verify Tarball Exists 📂
        run: |
          ls -lh supermariolatestdockerimage.tar

      - name: Set up Grype Cache Directory 💾
        run: mkdir -p ~/.cache/grype

      - name: Cache Grype DB 🗂️
        uses: actions/cache@v3
        with:
          path: ~/.cache/grype
          key: ${{ runner.os }}-grype-db-cache
          restore-keys: |
            ${{ runner.os }}-grype-db-cache

      - name: Install Grype 🛠️
        run: |
          curl -sSfL https://github.com/anchore/grype/releases/download/v0.68.0/grype_0.68.0_linux_amd64.tar.gz | tar -xz -C /usr/local/bin

      - name: Run Grype Vulnerability Scan for Critical Issues 🛡️
        run: |
          grype docker-archive://$(pwd)/supermariolatestdockerimage.tar --fail-on critical || true
          
  update_k8s_yaml_version_file_with_latest_image_tag:
    runs-on: ubuntu-latest
    needs: run_container_image_scan_on_supermario_docker_image

    steps:
      - name: Checkout Repository 📥
        uses: actions/checkout@v3

      - name: Set Git Config ⚙️
        run: |
          git config --global user.email "${{ secrets.GIT_EMAIL}}"
          git config --global user.name "${{ secrets.GIT_USERNAME}}"

      - name: Update Deployment YAML 📄
        run: |
          git pull
          sed -i "s|image: rahuldocker628/mariogitopsproject:.*$|image: rahuldocker628/mariogitopsproject:${{ env.VERSION }}|" deployment.yaml
          echo ${{ env.VERSION }} > version.txt
          CURRENT_VERSION=$(cat version.txt)
          git add deployment.yaml version.txt
          git commit -m "Updated deployment yaml and version txt file with supermario image tag to ${CURRENT_VERSION}"
          git push
```

---

### 📊 Step 3: Verify Pipeline Execution  

Monitor the **Actions Tab** in your GitHub repository to track the workflow's execution:  

1. **SonarQube SAST Scan 🛡️** validates code quality.  
2. **Docker Image Build and Push 📦** automates container creation.  
3. **Grype Scan 🔍** ensures no critical vulnerabilities exist.  
4. **Kubernetes Deployment Update 🔄** auto-applies the latest image.  

---

### 🏁 Outcome  

By completing this module, you have a fully automated, secure, and scalable DevSecOps pipeline for your SuperMario game.  

🚀 **Next Steps:** Continue enhancing the pipeline with dynamic security testing (DAST) and infrastructure as code (IaC) security scans.
