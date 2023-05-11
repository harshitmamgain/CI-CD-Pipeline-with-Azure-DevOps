# CI/CD Pipeline with Azure DevOps for .NET Application

In this project, I have implemented an automated build and release pipeline for the 'Helloworldapp' application using Azure DevOps. The application is deployed on the Azure WebApp. The Continuous Integration (CI) and Continuous Deployment (CD) processes are set up to automatically trigger whenever code is pushed to the Azure Repos. This ensures that any changes made to the code are immediately integrated, built, and deployed to the Azure WebApp without manual intervention. This streamlined approach enables faster and more frequent deployments, enhancing the agility and efficiency of the development and deployment cycle.

The complete pipeline has been designed to ensure a smooth and controlled deployment process. To ensure maximum quality and reliability, I have added an approval step before each component of the release pipeline. This means that before proceeding to the next stage, such as building, testing, or deploying the application, an approval is required from the designated personnel. This allows for thorough validation and verification of the changes being introduced. 

Within the production environment, I have implemented a Staging Slot for the WebApp. Leveraging this staging slot, the entire application is seamlessly hosted, ensuring a smooth transition during deployment. By performing a slot swap, Azure orchestrates the process meticulously, guaranteeing zero downtime for the application as it undergoes publication.


# Overview
 - The services that were used to implement this Projects are as follows:
     - Azure DevOps
     - Azure Repos
     - Azure Pipelines 
     - App Service
     - Deployment Slots
     - Service Principals
     - Role Based Access Control (RBAC)
   
# Detailed Steps
 ## Creating and then Pushing the Application to Azure Repos
  - To create a simple application named 'HelloworldApp', I executed the following commands on my local machine to create the .NET application:
   
    >dotnet new sln -o HelloworldApp <br>
    >cd HelloworldApp <br>
    >dotnet new mvc -n HelloWorldApp.Web <br>
    >dotnet sln HelloworldApp.sln add HelloWorldApp.Web\HelloworldApp.Web.csproj <br>
    >dotnet restore <br>
    >dotnet build --no-restore --configuration release <br>
    >dotnet D:\Project\HelloworldApp\HelloWorldApp.Web\bin\Release\net7.0\HelloworldApp.Web.dll
 
![2](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/6b8f5a93-6c2d-480b-acec-d6eefc499146)
  
 - The application is available on the Local host - http://localhost:5000/
 
 ![3](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/fa5d6b75-023d-4b91-99a3-b04dbdebaf27)  
- Now initialising GIT, Adding on the local machine to push the application on the Azure Repository.
    
    >git init <br>
    >git status <br>
    >git add . <br>
    >git commit -m "Intial Status" <br>
    >git remote add origin https://hmamgain79@dev.azure.com/hmamgain79/CI%20CD%20Pipeline%20for%20.NET%20Application/_git/CI%20CD%20Pipeline%20for%20.NET%20Application <br>
    >git push -u origin --all
 

 ![2](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/70a23731-049c-4b9c-bf41-cf0aba199d34)
    
 - The application has now been successfully pushed and is available in the Azure Repos, making it readily available for further development and collaboration.

 ![4](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/96b5b752-e9ca-491a-a8b1-4dd95594dfb3)
 
 ## Create a Continuous Integration Pipeline. 
  - Using the classic editor to create a Continuous Integration pipeline, which allows for seamless integration of code changes, automated builds, and continuous delivery of the application.
 
 ![5](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/88eca3c7-c6a5-44d7-885c-7641445c00cc)
  
  - Build Pipeline Specifications
     - Name - HellowebApp Build Pipeline <br>
       Agent Pool – Azure Pipelines <br>
       Agent Specification - ubuntu-20.04 <br>
       Artifact name – drop <br>
       Path to publish - $(build.artifactstagingdirectory)

(The Agent will perform Restore, Build and finally Publsih the Application to the Artifact named 'drop'. I will utilise this Artifact during the Release Pipeline. I have disabled the 'Test' job, as this is not needed for this project.) 
 
![5 1](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/939bcd4d-45ff-48ca-ab87-95abbe6dff02)
 
- This the build pipeline, the agent has performed the following task that are mentioned in the image below.
 
 ![7](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/ba5b0df0-5e3b-440a-9371-484b7c65f5a9)
 
- The artifact is created for the Application.
 
 ![8 1](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/c1d0ef9c-64b3-4eed-9ef8-50ff62c79508)
 
- I can configure the Pipeline in such a way that whenever there are any changes made to the application the Build Pipeline will be automatically triggered and added to the main branch. With this any changes made to the code, the Build Pipeline will be automatically triggered. 
 
 ![8](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/823e524e-2b9f-4495-9e5d-ff97c346b97b)
 
- Now testing, the automated Build Pipeline. Make changes to the code.
 
 ![9](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/3ae59675-e474-4b0a-ae4a-1545c2be6b06)
 
- Commit locally and add to the Azure Repo, as soon as the commit has been made to the Azure Repo, the Build Pipeline will be triggered.
  >D:\Project\HelloworldApp>git add . <br>
  Using this command I have staged the content. <br>
  >D:\Project\HelloworldApp>git commit -m "Title Update" <br>
  Committed the changes that were made <br>
  >D:\Project\HelloworldApp>git push origin master <br>
  Pushed the code to the master branch of the Azure Repository.
 
 - As I commited the changes, the Build Pipeline has been triggered. With the Title Update.
 
 ![10 4](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/2bea6bf1-1958-41d6-861d-fdd33abe6442)
 
 - Now, the artifact that has been saved for this should be available on the WebApp, for this I will create WebApp to host the application.


## Creating a 3 Web Application – DEV, QA and PROD
  - Dev
    - Name - helloworlddemoapp1 <br>
      Resource Group - HelloWorldApp <br>
      Runtime Stack - .NET 7 <br>
      Region – East US <br>
      Pricing Plan for the App Service Plan – Basic B1
 
 ![10](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/771bc4dd-588c-42b1-bcb4-1269b4baab90)
      
  - QA
    - Name - helloworlddemoapp-qa-testing <br>
      Resource Group - HelloWorldApp <br>
      Runtime Stack - .NET 7 <br>
      Region – East US <br>
      Pricing Plan for the App Service Plan – Basic B1
 
 ![10 2](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/e24e5e22-9793-42b3-8845-ec522f8fa000)
   
   - PROD
     - Name - helloworlddemoapp-qa-testing <br>
       Resource Group - HelloWorldApp <br>
       Runtime Stack - .NET 7 <br>
       Region – East US <br>
       Pricing Plan for the App Service Plan – Basic B1
 
 ![10 1](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/51e8ac18-bd81-4a31-a639-fd6869fb5c5e)
       
   - In the Prod WebAPp created a Staging Slot, when using the Staging slot the entire application will be hosted in the staging slot and when we swap the slots, Azure does it in such a way that there will be no downtime in the application, when it is being published.
 
 ![10 3](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/63164235-0b26-41e6-bea5-344fc112b9f3)


## Creating a Release Pipeline
This means that I will be connecting the WebApp with the end of the Release Pipeline to publish the code to the user through WebApp. For this, I need to configure the Azure Subscription to my DevOps Project. 

### To connect the Azure Subscription to DevOps Project to use WebApp, I have done the following steps.
- In the Service Connection of the Projects, I have created an ARM Service Connection and selected Manual Service Principal.
 
- In Azure Portal, In App registrations I have created a New Service Principal called Azure DevOps SP. With the 
 
 ![11](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/9557182b-99fc-4585-a6cd-f1f3edae53ed)
 
- Created a New Secret for the SP
 
 ![12](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/0d115a3b-6dcc-4491-8536-566a8fa92055)
 
- Granted Contributor Role to the SP, so that It can access Azure DevOps.
 
 ![13](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/fff40ef8-3f14-4e2c-9373-029ea49ca31d)
 
- Created the Service Connection with the following. And the validation was successful.
 
 ![14](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/98dc6a75-bdbb-4afc-9758-db54786d278c)
 
 ![15](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/dc05da72-6418-4a35-887c-702271552f33)
 
 
 
### Creating a Task for the helloworldapp1 in the Dev Environment
 
 ![16](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/9e148245-75bd-468f-bfb0-4ba08bd56bee)

### Taking the Aritifact from the Build Pipeline and feeding it to the WebApp

 ![17](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/37bc3b1d-3be2-44dd-b1fd-1156e76e19b4)
 
### Creating 2 more Stages, these are QA and PROD. For QA, I am taking Pre Deployment from 
 
![19](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/5883953f-951f-49d2-b210-ea3b41c75554) 
 
![18](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/c20f13fa-73a0-4818-b22b-211e25931257)
 
### In the PROD first task will be deploy on the staging slot 
 
 ![20](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/a8afbbd8-6bf8-488b-87dc-829f5b474e8d)

### After being deployed to the staging, added a new task (App service Manage) for the agent to deploy the webapp to production	
 
 ![21](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/05ac6e1d-e843-4a35-9f1c-749432b1b08d)

### Before moving into PROD, added a new agentless job, created a action in this for Manual Intervention
 
![22](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/a0db84c1-d8b1-4b39-8050-aa97be976cef) 

### Added a Contineous Deployment Trigger, so that whenever there are changes to the to the code in the Development, CD Pipeline will update automatically.
 
 ![23](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/5348c9d0-5b84-430a-8bf2-fc9ad04b024a)
  
## TESTING THE CI/CD Pipeline
  - Made changes to the code and now pushing the code to the Azure Repo
  
 ![25](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/bbfbcc41-f015-495c-ac0d-9705c8cdb1dc)
 
  - CI Pipeline Triggered
  
 ![24](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/1503b17b-93bc-4725-aa83-6d615da4ba93)
 
  - CD Pipeline Triggered 
 
 ![26](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/37487a44-615e-4766-bb34-77f2f12f156b)
  
  Before approving, checking the application if OK
  
  - Before deploying to PROD, I need to approve
 
 ![28](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/9a3f1130-c8ca-46b3-9bc6-af5101e545b5)
  
  The following job was performed before being deployed to the PROD.
  
  The Application is deployed 
  FQDN - https://real-helloworldapp.azurewebsites.net/
 
 ![29](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/ed0fa07c-6acc-4334-889b-5953935ab309)


   
# Troubleshooting

![troubleshoot](https://github.com/harshitmamgain/CI-CD-Pipeline-with-Azure-DevOps/assets/106948902/c1c5d128-ab2e-4b7f-9d0a-17d64a0c1960)


# Conclusion

