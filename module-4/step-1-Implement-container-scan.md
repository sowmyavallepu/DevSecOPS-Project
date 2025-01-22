Here's how to implement Part 1 of the container scan for the Super Mario Docker image, similar to the previous steps where we used Visual Studio Code to create and push the workflow to GitHub.

# Goal of this Lab:
To pull the Super Mario Docker image from Docker Hub, run a container scan on the Docker image, and pass/fail the build based on the vulnerabilities' severity.

---

### Steps Involved:

### Step 1: Create the Workflow File

1. **Open Visual Studio Code** and navigate to the root of your repository.
   
2. In the `.github/workflows` directory, **create a new YAML file** named `run-container-scan-supermario-image.yaml`.

3. Add the following code to the file:

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
             grype docker-archive://$(pwd)/supermariolatestdockerimage.tar --fail-on critical || exit 1
   ```

### Explanation of the Code:

- **Docker Pull and Save**: Pulls the Super Mario Docker image from Docker Hub using the version specified in `version.txt` and saves it as a tarball (`supermariolatestdockerimage.tar`).
- **Grype Vulnerability Scan**: Grype is used to scan the Docker image for vulnerabilities, specifically for critical issues. If critical issues are found, the workflow will fail (`--fail-on critical`).
- **Grype DB Caching**: Caches the Grype database to speed up subsequent scans.

### Step 2: Save and Push the Changes to GitHub

1. **Save the file** in Visual Studio Code (`run-container-scan-supermario-image.yaml`).
   
2. Open the terminal in the root of your repository and run the following Git commands to commit and push the workflow to your GitHub repository:

   ```bash
   git add .github/workflows/run-container-scan-supermario-image.yaml
   git commit -m "Add container scan workflow for Super Mario Docker image"
   git push origin main
   ```

---

### Outcome:

Once pushed to GitHub, this workflow will trigger whenever changes are made to the `main` branch. It will pull the latest version of the Super Mario Docker image, save it as a tarball, and run a Grype vulnerability scan. If any **critical** vulnerabilities are found, the build will fail, helping ensure that security issues are addressed before deployment.

This step adds an automated container security check, which is a crucial part of maintaining security in a CI/CD pipeline.

Let me know if any other clarifications or modifications are needed!
