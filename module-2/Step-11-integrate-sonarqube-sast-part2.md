# **Step 5: Add Workflow Code to `gitops.yaml` File**

1. **Open `gitops.yaml`**: In Visual Studio Code, open the `gitops.yaml` file located in `.github/workflows`.

2. **Paste the Workflow Code**: Copy the code below and paste it into the `gitops.yaml` file:

   ```yaml
   name: "Run SAST Scan on SuperMario Game Project"

   on:
     push:
       branches:
         - main

   jobs:
     sonarqube_sast_scan:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout Repository
           uses: actions/checkout@v3
           with:
             fetch-depth: 0  # Shallow clones should be disabled for better analysis relevance

         - name: SonarQube Scan
           uses: sonarsource/sonarqube-scan-action@master
           env:
             SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
             SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
   ```

3. **Explanation of Each Section**:

   - **name**: Sets the name of the workflow as **"Run SAST Scan on SuperMario Game Project"**. This will be the name you see in GitHub Actions.
   - **on**: Specifies that the workflow triggers on pushes to the `main` branch.
   - **jobs**: Defines a job named `sonarqube_sast_scan` that runs on the latest Ubuntu environment.
   - **steps**:
     - **Checkout Repository**: Uses GitHub’s `actions/checkout@v3` to pull the repository code into the workflow, with `fetch-depth: 0` to ensure full repository depth, enhancing the accuracy of the SonarQube scan.
     - **SonarQube Scan**: Uses `sonarsource/sonarqube-scan-action@master` to run the scan. `SONAR_HOST_URL` and `SONAR_TOKEN` are pulled from GitHub Secrets to securely connect to your SonarQube instance.

4. **Save the File**: Save your `gitops.yaml` file in Visual Studio Code.

---

With this workflow added, your GitHub Actions will now trigger SonarQube scans on every push to the `main` branch, helping you identify and resolve potential security and quality issues automatically. 
