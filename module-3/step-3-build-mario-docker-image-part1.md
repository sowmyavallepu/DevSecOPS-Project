# Step-by-Step Guide: Implementing GitOps for Super Mario Docker Image

This guide provides instructions for setting up a GitOps workflow to build and push the Super Mario Docker image to Docker Hub with a static tag using GitHub Actions.

---

## Steps Involved

### Step 1: Create a YAML File for GitOps Workflow

1. **Open Visual Studio Code** under the root folder of your project:
   ```
   DEV-SEC/gitops-practice-devsecops-sonarqube-sast-scan-supermario-repo
   ```

2. **Create a new YAML file** named `gitops-build-push-supermario-image.yaml` under the `.github/workflows` directory.

3. **Rename the existing file** `gitops.yaml` to `gitops-sast-sonar.yaml`.

4. **Modify the `gitops-sast-sonar.yaml` file** by updating the `on` section:
   - Change the `on` section to:
     ```yaml
     # on:
     #   push:
     #    branches:
           - main
     ```
   - This change specifies that the workflow should trigger on pushes to the `main` branch.

5. **Add the following code** to the `gitops-build-push-supermario-image.yaml` file:
   ```yaml
   name: "Build and push Super Mario Docker image with static tag to Docker Hub"

   on:
     push:
       branches:
         - main

   jobs:
     build_push_supermario_docker_image:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout Repository
           uses: actions/checkout@v3

         - name: Login to Docker Hub
           run: echo "${{ secrets.DOCKERHUB_TOKEN }}" | docker login -u "${{ secrets.DOCKERHUB_USERNAME }}" --password-stdin

         - name: Build and Push Docker Image
           run: |
             docker build -t docker.io/rahuldocker628/mariogitopsproject:1 .
             docker push docker.io/rahuldocker628/mariogitopsproject:1
   ```
   
6. **Save the files** after making the changes.

### Step 2: Commit and Push Changes to Remote Repository

1. **Open your terminal** and navigate to the root folder of your repository.

2. **Add the changes to the remote repository**:
    ```bash
    git add .github/workflows/gitops-build-push-supermario-image.yaml
    git add .github/workflows/gitops-sast-sonar.yaml
    git commit -m "Add GitOps workflow for Super Mario Docker image and update SonarQube workflow"
    git push origin main
    ```

### Step 3: Explanation of the Code

- **YAML File Creation**: The newly created YAML file (`gitops-build-push-supermario-image.yaml`) contains the workflow to build and push the Docker image to Docker Hub.

- **Triggering the Workflow**: The changes made to the existing `gitops-sast-sonar.yaml` file allow the workflow to trigger whenever there is a push to the `main` branch. This ensures that any changes to the code will automatically initiate a build process.

- **Automation Steps**:
  - **Checkout Repository**: The `actions/checkout@v3` step pulls the latest code from your repository.
  - **Login to Docker Hub**: This step logs into your Docker Hub account using secrets stored in your GitHub repository, ensuring that your credentials are kept secure.
  - **Build and Push Docker Image**: This command builds the Docker image with the specified tag and then pushes it to Docker Hub.

- **Automation Benefits**: By using GitHub Actions, you automate the CI/CD process, which reduces manual errors and increases deployment efficiency. The workflow can be further enhanced to include testing and security scans as needed.

---

Now you have set up the initial steps to automate the build and push process for your Super Mario Docker image, as well as updated the SonarQube workflow to trigger on main branch pushes.
```
