# Step-by-Step Guide to Create and Configure Quality Gates in SonarQube

#### Step 1: Create a Quality Gate

1. **Log into SonarQube**:
   Open your web browser and navigate to your SonarQube instance at `http://<your-azure-vm-ip>:9000`.

2. **Navigate to Quality Gates**:
   - From the main dashboard, click on **Quality Gates** in the top navigation menu.

3. **Create a New Quality Gate**:
   - Click on the **Create** button.
  
   - ![Screenshot (57)](https://github.com/user-attachments/assets/0300e072-8319-48f6-b4df-278b183ea490)

   - Enter the name for the new quality gate,  `customsupermario`.
   - Click on **Create** to save your new quality gate.
   - 
   - Set it as default gate
  
   - ![Screenshot (61)](https://github.com/user-attachments/assets/9c3c7dc9-c860-4ea7-aeb1-b339be68979c)

#### Step 2: Add Rules to the Quality Gate

1. **Unlock Editing**:
   - Find your newly created quality gate in the list and click on it.
   - Click the **Unlock editing** button to enable changes.

2. **Add Conditions**:
   - Click on **Add Condition** to define rules for the quality gate.
  
   - ![Screenshot (59)](https://github.com/user-attachments/assets/29a990c3-830a-4540-919f-0873fc64d62c)
    
   - Select **Coverage** from the dropdown list.
   - Set the condition to **< 90%**. This means that if the line coverage is below 90%, the quality gate will fail.
  
  ![Screenshot (60)](https://github.com/user-attachments/assets/6820aef3-d9a1-4e91-a399-3d706aedd2c6)


3. **Set as Default**:
   - After adding the condition, click the **Set as Default** button to make this quality gate the default for your projects.

#### Step 3: Update `gitops.yaml` File

1. **Navigate to Your Project Folder**:
   Open your terminal and navigate to the directory where your `gitops.yaml` file is located.

2. **Edit the `gitops.yaml` File**:
   Open the `gitops.yaml` file in your preferred text editor:
   ```bash
   nano gitops.yaml
   ```

3. **Add Quality Gate Check**:
   Add the following lines under the appropriate job section:
   ```yaml
   # If you wish to fail your job when the Quality Gate is red, uncomment the
   # following lines. This would typically be used to fail a deployment.
   - name: SonarQube Quality Gate Check
     uses: sonarsource/sonarqube-quality-gate-action@master
     timeout-minutes: 5
     env:
       SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
   ```

4. **Save and Exit**:
   If using `nano`, press `CTRL + X`, then `Y`, and `ENTER` to save your changes.

#### Step 4: Commit and Push Your Changes

1. **Stage Changes**:
   ```bash
   git add gitops.yaml
   ```

2. **Commit Changes**:
   ```bash
   git commit -m "Add SonarQube Quality Gate check"
   ```

3. **Push Changes**:
   ```bash
   git push origin main
   ```

#### Step 5: Run the GitHub Actions Workflow

1. **Navigate to the Actions Tab**:
   - Go to your GitHub repository on the web.
   - Click on the **Actions** tab.

2. **Trigger the Workflow**:
   If the workflow is set to run on push, it will automatically trigger. If not, you can manually trigger it.

3. **Monitor the Workflow Status**:
   - Click on the running job to view the logs.
   - Check for any failures related to the quality gate.
  
   - ![Screenshot (62)](https://github.com/user-attachments/assets/e2aa3843-6e54-4b3e-a98f-b8d594732aa2)


#### Step 6: Review Results in SonarQube

1. **Return to SonarQube Dashboard**:
   - Go back to your SonarQube instance at `http://<your-azure-vm-ip>:9000`.

2. **View Project Analysis**:
   - Navigate to your project.
   - Check the analysis results. If the line coverage is below 90%, you should see that the scan has failed due to the quality gate condition.

3. **Confirm Quality Gate Status**:
   - Look for a message indicating that the quality gate has failed. This confirms that your quality gate is functioning correctly.
  
     ![Screenshot (64)](https://github.com/user-attachments/assets/d0bd9fc1-5728-4058-a81f-aae5667d0681)


### Summary

You have successfully created a quality gate in SonarQube, configured rules, updated your `gitops.yaml` file to check for quality gate status during the CI/CD pipeline, and verified the results of the scan. This setup helps ensure that your code meets quality standards before it is deployed.

Feel free to upload any pictures related to this process or let me know if you need further assistance!
