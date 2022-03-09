# Deploying Node.js to Azure App Service

## Introduction
   This guide explains how to use GitHub Actions to build, test, and deploy a Node.js project to Azure App Service.

<details> 
   <summary> <strong>  Prerequisites </strong> </summary>

- [A GitHub account](https://github.com)
- [Source code](https://github.com/TrainingAssets/DevOps-Training-Assets/blob/vatsa/GitHub/labs/Source%20Code/Deployment-Source%20code.md)
- [Azure subscription](https://portal.azure.com)
   
</details>

<details>
   <summary> <strong> Task 1:  Creating the Azure App Service </strong> </summary>

1. [Login to Azure portal](https://portal.azure.com)

2. In the search bar, type **App Service**.

   ![image](https://user-images.githubusercontent.com/78465059/147928511-1783d1e2-ec19-4d7c-86bf-ae584b1058de.png)

3. Select an **App Service** from the list below.

   ![image](https://user-images.githubusercontent.com/78465059/147928609-741c5487-3bb4-4195-86b0-245c106e65f2.png)

4. Click on **Create**

   ![image](https://user-images.githubusercontent.com/78465059/147928713-7d96a2c7-6057-4d49-aa11-8c11c244dc50.png)

5. Fill in the required details

    **Project Details**
    - Choose Resource Group (if you don’t have one, you can click create new and specify the name).

   ![image](https://user-images.githubusercontent.com/78465059/147928867-a6b3e382-9677-4b09-be79-231f5f0d8090.png)
   
   **Instance Details**
   
   - Name of your app (we will use this name on the app URL)
   - choose Code.
   - Specify the app service language.
   - The OS for App service.
   - Specify where you want your app to be hosted.

   ![image](https://user-images.githubusercontent.com/78465059/147929069-295507d9-86d8-462f-a9e2-6600fbe7dd54.png)

6. Click on **Review + create** button.

   ![image](https://user-images.githubusercontent.com/78465059/147929188-24b13cbe-3c18-4b30-8aec-ada332f25ab7.png)

7. Click on **Create** button.
   
   ![image](https://user-images.githubusercontent.com/78465059/147929283-fe1e1827-aaa5-4caa-9f56-42c6830844fd.png)

8. The app is deployed successfully and then Click on **Go to resource**

   ![image](https://user-images.githubusercontent.com/78465059/147929723-8e53e242-63ac-4d99-897b-a1a1ddb5f2e4.png)

   ![image](https://user-images.githubusercontent.com/78465059/147929583-00a56fe5-66c8-4ec4-bc32-baeee9e0baf9.png)

</details>

<details>
<summary> <strong> Task 2:  Create a repository secret </strong> </summary>

1. Now you're going to create an encrypted secret to store the credentials. You'll create this secret at the repository level.

2. Navigate to GitHub and select your repository **Settings** tab.

   ![image](https://user-images.githubusercontent.com/78465059/147930505-3e243eb9-7cc1-47c7-b995-98423d5cd7b9.png)

3. Then select **Secrets**.

   ![image](https://user-images.githubusercontent.com/78465059/147930696-88b705b3-c6d8-4217-8d2e-059189fcec24.png)   

4. Select **New repository secret**

   ![image](https://user-images.githubusercontent.com/78465059/147930750-67012f39-2a7b-40b9-9ed1-7a54ab3518f0.png)

5. Open azure portal and download the Get Publish Profile

   ![image](https://user-images.githubusercontent.com/78465059/147930925-ffb425a2-b828-4e78-b6aa-131ba8f00369.png)
   
6. Copy the Publish profile file content and paste into value section

   ![image](https://user-images.githubusercontent.com/78465059/147931408-4fc7e915-7fd9-4561-a09e-03ed5b97a40e.png)

7. secret is added to repository

   ![image](https://user-images.githubusercontent.com/78465059/147931539-e927f56a-b778-4dfe-b3d2-353cb7ae55e5.png)

</details>

<details>
<summary> <strong> Task 3:  Create a Workflow </strong> </summary>

1. Click on **Action Tab**

   ![image](https://user-images.githubusercontent.com/78465059/147913068-7acd24ac-f783-4dc1-8a54-ee5540e5c89d.png)

2. Click on **set up a workflow yourself**

   ![image](https://user-images.githubusercontent.com/78465059/147913185-717c3a6c-4c2d-4cc6-a131-29dd3ada7bf6.png)

3. clear the file content and add new content to the file
   
   ![image](https://user-images.githubusercontent.com/78465059/147913500-9eadffa2-23ca-47d0-adb9-b6dd3d39f3fb.png)

```
    name: Nodeed.js build and deploy

    on:
      push:
        branches: [ master ]

    jobs:
      build-and-deploy:
        runs-on: ubuntu-latest
        steps:
        - name: Checkout
          uses: actions/checkout@master

        - name: Setup Node 12.x
          uses: actions/setup-node@v1
          with:
            node-version: '12.x'

        - name: 'Build and Test'
          run: |
            npm install
            npm run build --if-present
            npm run test --if-present

        - name: 'Deploy Azure Web App'
          uses: azure/webapps-deploy@v1
          with: 
            app-name: pocketcal
            publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
        
```

4. Click on **start commit**
   
   ![image](https://user-images.githubusercontent.com/78465059/147913719-f1d46e1f-73d8-4408-98d4-3041ac92a95a.png)
   
5. Enter the commit message

   ![image](https://user-images.githubusercontent.com/78465059/147913796-3299ee6a-e1b7-4015-b042-e55429a41489.png)

6. Click on commit new file

   ![image](https://user-images.githubusercontent.com/78465059/147913832-38c976c7-0ef4-4b7f-972c-8b1428d7d895.png)
</details> 

<details>
<summary> <strong> Task 4:  Viewing workflow results </strong> </summary>

1. Under your repository name, click **Actions**

2. In the left sidebar, click the workflow you want to see.
   
   ![image](https://user-images.githubusercontent.com/78465059/147914208-f2ec99ce-51b9-480b-a79f-eacda26d4cbf.png)

3. From the list of workflow runs, click the name of the run you want to see.

   ![image](https://user-images.githubusercontent.com/78465059/147914278-b9e5355f-bc0d-4b98-a39d-c57cd4993b7c.png)

4. Under **Jobs** , click the **Explore-GitHub-Actions** job.
  
   ![image](https://user-images.githubusercontent.com/78465059/147914357-2a4aa55f-878c-4f2f-91e7-768b99b894f5.png)

5. The log shows you how each of the steps was processed. Expand any of the steps to view its details.

   ![image](https://user-images.githubusercontent.com/78465059/147914424-dfd0b851-53d0-4bcf-8eae-22e4f7703864.png)

 For example, you can see the Azure Deploy history of the Node.js application:

![image](https://user-images.githubusercontent.com/78465059/147914503-3b952f19-b956-4dcb-a72f-de8f81b4ccbb.png)

</details>

<details>
<summary> <strong> Task 5:  View Deployment </strong> </summary>

1. [Login to Azure portal](https://portal.azure.com)

2. From the Azure portal, copy the web app URL.

   ![image](https://user-images.githubusercontent.com/78465059/147931903-0112fb68-77e1-4697-8404-e9b77aeb9cce.png)

3. Put the URL into your browser, then hit enter

   ![image](https://user-images.githubusercontent.com/78465059/147931982-aacb9f80-6b99-4c8f-bff1-a4a431f4b49a.png)
   
You will be able to notice successful deployment.

  ![image](https://user-images.githubusercontent.com/78465059/147932060-a1241bf1-0a02-4271-b28d-4189bef8c9da.png)
   
</details>
