# Continuing the GitOps Workflow Setup for Super Mario Docker Image

Following up on the previous steps, here’s how to create Docker Hub secrets and push the code to your GitHub repository. Once completed, you should be able to see the workflow running successfully in GitHub Actions and verify the Docker image in your Docker Hub repository.

---

## Steps Involved (Continued)

### Step 3: Create a Token in Docker Hub

1. Log in to [Docker Hub](https://hub.docker.com/).
2. Go to **Account Settings** and select **Security**.
3. Under **Access Tokens**, create a new token.
4. Copy the generated token as you will need it in the next step.

### Step 4: Add Secrets to GitHub Repository

1. Go to your GitHub repository.
2. Navigate to **Settings** > **Secrets and variables** > **Actions**.
3. Click on **New repository secret** and add the following secrets:

   - **DOCKERHUB_USERNAME**: Your Docker Hub username.
   - **DOCKERHUB_TOKEN**: Paste the token you created in Docker Hub.

### Step 5: Push the Workflow to GitHub

1. In your terminal, ensure all changes are committed and push the code to your remote GitHub repository:

    ```bash
    git add .
    git commit -m "Add GitHub Actions workflow for building and pushing Super Mario Docker image"
    git push origin main
    ```

2. Once pushed, GitHub Actions will automatically trigger the workflow to build and push the Docker image to Docker Hub whenever changes are made to the main branch.

### Step 6: Verify Workflow and Docker Hub Repository

1. **GitHub Actions**: After pushing the code, go to the **Actions** tab in your GitHub repository. You should see the workflow run with a green checkmark if it completes successfully. 

2. **Docker Hub Repository**: Log in to your Docker Hub account and go to your repository. You should see the Docker image with the specified tag (e.g., `1` for a static tag, or a unique timestamp if you followed the unique tagging instructions).

   - Example tag: `supermariogitopsproject:1` or `supermariogitopsproject:<timestamp>`

---

You’ve now completed the setup for a GitOps workflow that builds and pushes the Super Mario Docker image to Docker Hub. You can monitor each build's success on GitHub Actions and see the tagged images appear in your Docker Hub repository.
```
