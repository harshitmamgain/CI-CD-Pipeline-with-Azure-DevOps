# CI/CD Pipeline with Azure DevOps for .NET Application

In this Project, I have created an automated build and release pipeline for .NET application with Azure DevOps. The Application is deployed on the Azure WebApp.
Complete Pipeline
<Explain the Pipeline>
Added approval before each and every component, explain
Contineous Integraton and Contineous Deployment will auto trigger once the code is being pushed to the Repo


# Overview
 - The services that were used during the course of this Projects are as follows:
     - Azure DevOps
     - Azure Repos
     - Azure Pipelines 
     - App Service
     - Deployment Slots
   
# Steps
 ## Creating the .NET Application on Local Machine
  The following commands, I have run to create to create the application are as follows 
   
    >dotnet new sln -o HelloworldApp <br>
    >cd HelloworldApp <br>
    >dotnet new mvc -n HelloWorldApp.Web <br>
    >dotnet sln HelloworldApp.sln add HelloWorldApp.Web\HelloworldApp.Web.csproj <br>
    >dotnet restore <br>
    >dotnet build --no-restore --configuration release <br>
    >dotnet D:\Project\HelloworldApp\HelloWorldApp.Web\bin\Release\net7.0\HelloworldApp.Web.dll
  
  The webpage is available on the Local host - http://localhost:5000/
  
- Now initialising GIT on the local machine to push the application on the Azure Repository.
    
    >D:\Project\HelloworldApp>git init <br>
    >D:\Project\HelloworldApp>git status <br>
    >D:\Project\HelloworldApp>git add . <br>
    >D:\Project\HelloworldApp>git commit -m "Intial Status" <br>
    >D:\Project\HelloworldApp>git remote add origin https://hmamgain79@dev.azure.com/hmamgain79/CI%20CD%20Pipeline%20for%20.NET%20Application/_git/CI%20CD%20Pipeline%20for%20.NET%20Application <br>
    >D:\Project\HelloworldApp>git push -u origin --all
    
 The application has now been pushed to the Azure DevOps Repos.

 ## Using the classic editor to create a Continuous Integration Pipeline. Picking the project from the Azure DevOps Repos.
- Build Pipeline Specifications
  >Name - HellowebApp Build Pipeline <br>
  >Agent Pool – Azure Pipelines <br>
  >Agent Specification - ubuntu-20.04 <br>
  >Artifact name – drop <br>
  >Path to publish - $(build.artifactstagingdirectory)

I have disabled the Test Project, as this is not a part of the project. 

- This the build pipeline, the agent has performed the following task that are mentioned in the image below.
- The artifact is created for the Application.
- I can configure the Pipeline in such a way that whenever there are any changes made to the application the Build Pipeline will be automatically triggered and added to the main branch. With this any changes made to the code, the Build Pipeline will be automatically triggered. 
- Now testing, the automated Build Pipeline. Make changes to the code.
- Commit locally and add to the Azure Repo, as soon as the commit has been made to the Azure Repo, the Build Pipeline will be triggered.
  >D:\Project\HelloworldApp>git add . <br>
  Using this command I have staged the content. <br>
  >D:\Project\HelloworldApp>git commit -m "Title Update" <br>
  Committed the changes that were made <br>
  >D:\Project\HelloworldApp>git push origin master <br>
  Pushed the code to the master branch of the Azure Repository.
 
 - As I commited the changes, the Build Pipeline has been triggered. With the Title Update. 
 - Now, the artifact that has been saved for this should be available on the WebApp, for this I will create WebApp to host the application.


## Creating a 3 Web Application – DEV, QA and PROD
  - Dev
    - Name - helloworlddemoapp1 <br>
      Resource Group - HelloWorldApp <br>
      Runtime Stack - .NET 7 <br>
      Region – East US <br>
      Pricing Plan for the App Service Plan – Basic B1
      
  - QA
    - Name - helloworlddemoapp-qa-testing <br>
      Resource Group - HelloWorldApp <br>
      Runtime Stack - .NET 7 <br>
      Region – East US <br>
      Pricing Plan for the App Service Plan – Basic B1
   
   - PROD
     - Name - helloworlddemoapp-qa-testing <br>
       Resource Group - HelloWorldApp <br>
       Runtime Stack - .NET 7 <br>
       Region – East US <br>
       Pricing Plan for the App Service Plan – Basic B1
       
   - In the Prod WebAPp created a Staging Slot, when using the Staging slot the entire application will be hosted in the staging slot and when we swap the slots, Azure does it in such a way that there will be no downtime in the application, when it is being published.


## Creating a Release Pipeline
This means that I will be connecting the WebApp with the end of the Release Pipeline to publish the code to the user through WebApp. For this, I need to configure the Azure Subscription to my DevOps Project. 

To connect the Azure Subscription to DevOps Project to use WebApp, I have done the following steps.
- In the Service Connection of the Projects, I have created an ARM Service Connection and selected Manual Service Principal.
- In Azure Portal, In App registrations I have created a New Service Principal called Azure DevOps SP. With the 
- Created a New Secret for the SP
- Granted Contributor Role to the SP, so that It can access Azure DevOps.
- Created the Service Connection with the following. And the validation was successful.

Creating a Task for the helloworldapp1 in the Dev Environment

Taking the Aritifact from the Build Pipeline and feeding it to the WebApp

Creating 2 more Stages, these are QA and PROD. For QA, I am taking Pre Deployment from 

In the PROD first task will be deploy on the staging slot 

After being deployed to the staging, added a new task (App service Manage) for the agent to deploy the webapp to production	

Before moving into PROD, added a new agentless job, created a action in this for Manual Intervention 

Added a Contineous Deployment Trigger, so that whenever there are changes to the to the code in the Development, CD Pipeline will update automatically.
  
## TESTING THE CI/CD Pipeline
  Made changes to the code and now pushing the code to the Azure Repo
  
  CI Pipeline Triggered
  
  CD Pipeline Triggered 
  
  Before approving, checking the application if OK
  
  Before deploying to PROD, I need to approve
  
  The following job was performed before being deployed to the PROD.
  
  The Application is deployed 
  FQDN - https://real-helloworldapp.azurewebsites.net/


   
# Troubleshooting




# Conclusion

