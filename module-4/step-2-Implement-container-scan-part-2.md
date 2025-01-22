# **Lab: Container Scan for Super Mario Docker Image part 2**

# **Goal:**
To pull the Super Mario Docker image from Docker Hub, run a container scan on the image using Grype, and allow the build to pass even if vulnerabilities are found.

---

### **Steps Involved:**

### **Part 1: Create the `run-container-scan-supermario-image.yaml` File**
1. **Navigate to your project directory** in Visual Studio Code (or your preferred code editor).

2. **Create the file**:
   - Inside the `.github/workflows/` directory, create a new YAML file called `run-container-scan-supermario-image.yaml`.

3. **Add the following code to the file**:

```yaml
name: "Run Container Scan on Super Mario Docker Image with Quality Gate"

on:
  push:
    branches:
      - main

env:
  VERSION: $(( $(cat version.txt) + 1 ))

jobs:
  run_container_image_scan_on_supermario_docker_image:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Login to Docker Hub
        run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USERNAME }}" --password-stdin

      - name: Get Docker Image from Docker Hub
        run: |
          docker pull docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}
          docker save -o supermariolatestdockerimage.tar docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}

      - name: Verify Tarball Exists
        run: |
          ls -lh supermariolatestdockerimage.tar

      - name: Set up Grype Cache Directory
        run: mkdir -p ~/.cache/grype

      - name: Cache Grype DB
        uses: actions/cache@v3
        with:
          path: ~/.cache/grype
          key: ${{ runner.os }}-grype-db-cache
          restore-keys: |
            ${{ runner.os }}-grype-db-cache

      - name: Install Grype
        run: |
          curl -sSfL https://github.com/anchore/grype/releases/download/v0.68.0/grype_0.68.0_linux_amd64.tar.gz | tar -xz -C /usr/local/bin

      - name: Run Grype Vulnerability Scan for Critical Issues
        run: |
          grype docker-archive://$(pwd)/supermariolatestdockerimage.tar --severity CRITICAL,HIGH
        continue-on-error: true  # This makes sure the build passes even with vulnerabilities
```

### **Part 2: Explanation of the Code**

- **Workflow Name**: "Run Container Scan on Super Mario Docker Image with Quality Gate"
  - This defines the name of the GitHub Actions workflow.

- **Trigger**: 
  - The workflow triggers on `push` events to the `main` branch.

- **Version Environment Variable**: 
  - The `VERSION` is calculated by incrementing the value in the `version.txt` file.

- **Steps in the Job**:
  - **Checkout Repository**: The first step checks out the repository code.
  - **Login to Docker Hub**: Logs into Docker Hub using secrets stored in GitHub (DOCKERHUB_TOKEN and DOCKERHUB_USERNAME).
  - **Get Docker Image**: Pulls the Docker image from Docker Hub and saves it as a tarball.
  - **Verify Tarball Exists**: Verifies that the tarball (`supermariolatestdockerimage.tar`) is present.
  - **Set up Grype Cache**: Creates a cache directory for Grype (a tool for scanning Docker images for vulnerabilities).
  - **Cache Grype DB**: Caches the Grype database to speed up future scans.
  - **Install Grype**: Installs Grype on the runner.
  - **Run Grype Vulnerability Scan**: Runs the Grype vulnerability scan against the Docker image tarball, focusing on `CRITICAL` and `HIGH` severity issues.
  - **`continue-on-error: true`**: Ensures that the workflow continues and **does not fail** even if vulnerabilities are found.

### **Part 3: Push the Changes to GitHub**

1. **Save the file** (`run-container-scan-supermario-image.yaml`) after adding the code.
   
2. **Commit and Push**:
   - Stage the changes by running:
   
     ```bash
     git add .github/workflows/run-container-scan-supermario-image.yaml
     ```

   - Commit the changes with a descriptive message:

     ```bash
     git commit -m "Add container scan workflow to check for vulnerabilities in Super Mario Docker image"
     ```

   - Push the changes to the `main` branch:

     ```bash
     git push origin main
     ```

### **Part 4: Verify the Workflow Execution**

1. **Navigate to the GitHub Repository**:
   - Go to the **Actions** tab of your GitHub repository.

2. **Verify the Workflow**:
   - You should see that the **workflow has started** automatically upon pushing to the `main` branch.
   - The workflow will run and pull the Super Mario Docker image from Docker Hub.
   - The vulnerabilities (if any) will be scanned using **Grype**, but since we set `continue-on-error: true`, the build will pass **even if vulnerabilities are found**.

3. **Check Logs**:
   - Click on the workflow run to view the logs.
   - You will see the Grype scan results, including details of any vulnerabilities found in the image. Despite the vulnerabilities, the workflow will **not fail** due to the `continue-on-error` setting.

---

### **Conclusion:**

In this lab, you successfully implemented a **container vulnerability scan** for a Docker image using Grype. You also configured the workflow to **pass even if vulnerabilities are found**, ensuring that the build continues without failure.

This is an important step in building secure CI/CD pipelines, where security checks are integrated but do not block development progress.
