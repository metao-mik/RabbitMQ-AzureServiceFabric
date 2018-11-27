# RabbitMQ-AzureServiceFabric

This repository contains a Azure Service Fabric Applikation Package for a RabbitMQ Cluster.
It's als includes a documantation of how the package was created.

## Introduction 
Using RabbitMQ as an Message Queuing Event Bus in a OnPremise Environment has serveral advantages.
Our Environment is restricted to be OnPremise. But the Software should be created Cloud-Ready. 


## Perapare the Example Environment 
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
   
## Creating the Azure Service Appliaction Package 
