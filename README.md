

# RabbitMQ-AzureServiceFabric

This repository contains a Azure Service Fabric Applikation Package for a RabbitMQ Cluster.
It's als includes a documantation of how the package was created.

## Introduction 
Using RabbitMQ as an Message Queuing Event Bus in a OnPremise Environment has serveral advantages.
Our Environment is restricted to be OnPremise. But the Software should be created Cloud-Ready. 

## Preq... and Preparation 

### Perapare the Example Environment 
Before creating the Application Package an Example Virtual Maschine has been prepared with an Example installation of RabbitMQ.

1) Create Windows Server 2016 Virtual Maschine A.
2) Install Erlang on Virtual Maschine A
3) Install RabittMQ in Virtual Maschine A
4) Configure RabbitMQ for StandAlone usage .
5) Create a second Virtual Maschine B like Virtual Maschine B. 
6) Configure RabbitMQ Cluster for both Maschines 
7) Test the Enviromment by Sample Applications 
   - Create an Exchange, 
   - Create a queue
   - Send Message zu Exchange, 
   - Read Message vom Queue
   
### Azure Service Fabric SDK 
Install the Azure Service SDK.  

### Microft Visual C++ Redistributable (x64) 
Erlang needs the The C++ Redistributable (x64). On Windows 10 and Windows Server 2016 it should already existing. Otherwise it needs to be installed.

### Perpare Folder Structure 
Create as Folder C:\Samples\RabbitMQ-AzSF-image\Code
Create an Empty TextFile/Batchfile "start_RabbitMQ.bat" in C:\Samples\RabbitMQ-AzSF-image\Code


## Creating the Azure Service Appliaction Package 
The Application Server Package i been Build by Microsoft Visual Studio 2017 

### Create Visual Studio Application Pakage Project 
<<image of visual Studio, net Service Fabric Application>>

The Template "Ausführbare Gastdate" is selected. 
- Codepaket-Folder: "C:\samples\RabbiteMQ..\code
- Programm: start_rabbitMQ.bat
- ProjectName RabbitMQService

<<image of Template Properties>>
   
### Copy the Erlang and RabbitMQ files to the Code-folder 

#### Erlang Files 
The installation of Erlang created a Erl10.1 Folder under Programm-Files. 
Copy the Files to the "\code\Erl" Directory Previous Prepared.  

(Caution: Copy these Files from an Installed instance of erlang, the Files/folder-Structure in the installation Zip/Exe differ from the installed ones)  

#### RabbitMQ Files 

RabbitMQ 3.7.9
Copy the files of the RabittMQ installation to /code/rabbitMQ_Service 


### Configure Erlang and RabbitMQ Environment 
During the installation Process of the Azure Service Fabirc Application Package on a Node. 
server Instance-folder are created in the Data-Fodler of the Fabric. 
/Fabric/work/applications/<Application Package Name>_<"App" +Installation_instance> 
   
The content files are copied there. 
To start the rabbitMQ Service the Applicaton needs to be configured on these Folders. 
Therefore a PowerShell Script "SetupEnvironment.ps1" is created in "/?"

PowerShell Scripts can't be executed directly from the AzureServiceFabric als StartUp-Objects
the prepared Batchfile is calling out PowerShell Script.

<code>
   powershell.exe -ExecutionPolicy Bypass -Command ".\SetupEnvironment.ps1"
</code>

The SetupEnvironment Script sets serveral Variables for the Erlang and RabbitMQ Runtime

<code> 
   Start-Transscript .\rabbitemq-setup-environment.log

   Write-Host "Envoronment variables" 
</code>



<b>Erl.ini</b>


### Powershell Script to Deploy-Package on a Node 

During Deplopment phase of the Package the package 

Deploy a Package

<code>
   Connect-ServiceFabricCluster -ConnectionEndpoint localhost:19000
   
   $parameters = @{ ENVIRONMENT="Stage";ClusterInstall="false";InstanceCount="-1";ErlangCockie="ErlangCookie" }
   
   Copy-ServiceFabricApplicationPackage -ApplicationPackagePath Debug\ -ApplicatinPackagePathInImageStore Arvato.Infrastructure.RabittMQ
   Register-ServiceFabricApplicationType -ApplicatinPackagePathInImageStore Arvato.Infrastructure.RabittMQ
   New-ServiceFabricApplication -Application fabric:/Stage/Arvato.Infrastructure.RabittMQ `
                                -ApplicationTypeName Arvato.Infrastructure.RabittMQ `
                                -ApplicationTypeVersion 3.7.9 `
                                -ApplicationParameter $parameters
</code>


# Distribute the Application Package 

The Application Package can be packed and published by nuget.

https://github.com/Azure/SFNuGet



# Deploy Application Package on a Development Maschine
