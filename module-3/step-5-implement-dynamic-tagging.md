# Step 5: Build and Push Super Mario Game Docker Image with Dynamic Tag

## Goal of this step:

To build and push the Super Mario Game Docker image with a dynamic tag to Docker Hub.

---

## Steps Involved:

### Step 1: Add a `version.txt` File

1. At the root of your repository, create a new file named `version.txt`.
2. Add the number `1` to this file. This file will be used to track the version of your Docker image.

### Step 2: Modify the GitHub Actions Workflow

1. Open your `gitops-build-push-supermario-image.yaml` file located under `.github/workflows/`.
2. Update the contents of the file with the following code:

   ```yaml
   name: "Build and push Super Mario Docker image with dynamic tag to Docker Hub"

   on:
     push:
       branches:
         - main

   env:
     VERSION: $(( $(cat version.txt) + 1 ))

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
             docker build -t docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }} .
             docker push docker.io/rahuldocker628/mariogitopsproject:${{ env.VERSION }}
   ```

   **Explanation of the Code:**
   - **Dynamic Tagging**: The `VERSION` environment variable is set by reading the `version.txt` file and incrementing its value by `1`. This allows the Docker image to be tagged dynamically based on the current version.
   - **Docker Build and Push**: The image is built and tagged using the incremented version number, ensuring that each build has a unique tag.

### Step 3: Push the Changes to the Remote GitHub Repository

1. Open your terminal and navigate to the root of your repository.
2. Run the following commands to add, commit, and push your changes:

   ```bash
   git add version.txt .github/workflows/gitops-build-push-supermario-image.yaml
   git commit -m "Add versioning for Docker image"
   git push origin main
   ```

---

After completing these steps, the GitHub Actions workflow will automatically trigger, build the Docker image, and push it to your Docker Hub repository with the incremented tag based on the value in `version.txt`.

This process enables efficient version management for your Docker images, ensuring that each build is uniquely identified.
```

You can copy and paste this markdown content directly into your README file on GitHub. Let me know if you need any further modifications!
