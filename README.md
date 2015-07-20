## Introduction

This Connector API App sample provides connectivity to Azure Storage Queue.  It provides the following functionalities:
1. Get Message 
2. Delete Message 
3. Send Message 
4. Trigger on Message Available 

This Connector sample illustrates the following:
1. Developing a basic API App 
2. Adding summary and other annotations from XML Comments 
3. Dynamically generate swagger based on configuration 
4. Implementing a basic polling based Trigger 

NOTE: This is a sample Connector API App.  The purpose of the API App is to illustrate how a Connector can be developed.  The codebase, therefore, has bee kept really simple and does not include error handling, validation logic, diagnostic logging, optimizations, etc.


## Building the Sample

### Pre-Requisites:

You will need the following to use the sample:
1. An Azure Storage account with a couple of Queues 
2. Visual Studio 2013 
3. Azure SDK 2.5.1 or above 

### Instructions:
1. Open web.config and add the necessary Storage Connection string in the following sections:

    ```xml
     <appSettings>
         <add key="StorageConnectionString" value="{add your Storage Connection String here}"/>
     </appSettings>
    ```
    
2. Open the project in Visual Studio and Build it. 

#### Local Testing
1. Run the project (F5 - Start Debugging) 
2. Navigate to http://localhost:24128/swagger 
3. Use the Swagger UI to test the operations 

#### Publishing to Azure

Follow the steps in the following tutorial: http://azure.microsoft.com/en-us/documentation/articles/app-service-dotnet-deploy-api-app/

#### Testing with Logic Apps
1. Create a Logic App in the same Resource Group where you have published this Connector API App. 
2. Follow the steps here:  http://azure.microsoft.com/en-us/documentation/articles/app-service-logic-create-a-logic-app/ 

HINT: You can configure the Logic App to run manually.  Click 'Run Now' to run the Logic App.

 
## Description


The Connector API methods are implemented in the MessagesController class (in the MessagesController.cs file).

It provides the following 3 actions:
* GetMessage: Reads a message from a Queue. The message is not deleted. 
* DeleteMessage: Deletes a message that is previously read from a Queue. 
* SendMessage: Sends a message to a Queue. 

In addition, it implements a trigger:
* NewMessageTrigger: Returns a message from a Queue. Previous message is deleted. 

SwaggerConfig class in (SwaggerConfig.cs) defines how the swagger is geenrated.  The project is configred to generate XML Comments, and Swagger configuration is configured to use that.  It also adds 3 operation filters:
* AddDefaultResponseFilter: Adds a "default" response object 
* DiscoverQueueFilter: adds dynamically an enum that lists all the queues in the configured storage account 
* TriggerStateFilter: adds extra vendor extension fields for the triggerState parameter 
