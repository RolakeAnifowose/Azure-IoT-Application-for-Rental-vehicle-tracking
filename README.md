# Azure-IoT-Application-for-Rental-vehicle-tracking

The idea behind the project is to design, develop and deploy an IoT application to track rental vehicles collecting data on things like the longitude, latitude, distance covered, speed and time.
The application is developed using Azure services: functions, storage accounts, web application, map, CDN, pipeline, and third-party services like GitHub, Slack and Grafana.

![image](https://github.com/RolakeAnifowose/Azure-IoT-Application-for-Rental-vehicle-tracking/assets/31238382/9d1cf058-0b74-4052-92a7-9e8c3cd3c7fd)

## Development Process
Two simulators were created by modifying the simulated device application used in the 
workshops to begin development. The first simulator was for geolocation data (longitude and 
latitude), while the second was for speed (distance, time, and speed). After modifying the 
simulators, I moved to Azure. To kick off development on the cloud, I created a resource group 
which acted as a container for my Azure resources.

Following that, an IoT hub was created. It acted as a gateway between the simulators and the rest of the Azure services. The simulators created in the previous step connected to the IoT hub using an MQTT protocol. Within the IoT hub, two IoT devices were created for each simulator. The connection string of each IoT device was added to the simulated device applications, replacing the existing ones. Doing this enabled a connection between the simulated devices and the IoT hub, making it possible for the simulators to send and receive telemetry data.

The next step in the development process involved setting up a Cosmos DB database. I created 
a Cosmos DB account, a new database, and two containers to hold sensor data sent from my 
two simulators. The Geolocation container held location sensor data in the form of longitude 
and latitude data. In contrast, the Speed container held distance, time and speed data. The sensor 
data were stored in the form of JSON documents.

After that, Azure functions were created. For the first function, I used the IoT hub template.
The Azure function was created alongside the source files needed. I created an advanced
storage account and copied the connection string. I added the connection strings for my 
advanced storage account, Cosmos DB database, and Azure IoT event hub to my 
local.settings.json file.I then added an HTTP trigger function to connect to the Cosmos DB 
database and return the stored sensor data. I ran my function application locally and tested the 
returned endpoint using Postman. Following my local testing and verification, I deployed my 
functions to an Azure function application. This process was repeated for my second simulator, 
leaving me with four Azure functions and two function applications.

After testing, deploying and ensuring that things worked correctly, an Azure web application 
was created using the ‘dotnet new webapp -f net6.0’ command. This created the web 
application with all the source files needed for development. I modified the index file of the 
web application to display data from the previously created HTTP functions by fetching data 
from the function URLs. I ran the web application locally, verified that the data was displayed 
correctly, and then deployed it to Azure.

I created an Azure DevOps pipeline to automate the web application building and deployment. 
The web application code was pushed to a GitHub repository, and a yaml file was created 
containing steps to automate the building and deployment of the web app. To take it a step 
further, I added a Slack integration on the pipeline to send a notification whenever a build 
succeeds.
![image](https://github.com/RolakeAnifowose/Azure-IoT-Application-for-Rental-vehicle-tracking/assets/31238382/a1b065ae-a3cf-4336-a2cf-fceabe7695bc)
![image](https://github.com/RolakeAnifowose/Azure-IoT-Application-for-Rental-vehicle-tracking/assets/31238382/e769a067-4bd5-4dea-b5dd-d8fd6682b7da)

As part of my DevOps implementation, I created an Azure Managed Grafana dashboard to 
display metrics from the function applications, Cosmos DB database, Azure Maps, and web 
application. I used the Azure content delivery network to improve my web application’s 
reliability. I created a CDN profile on Azure and then created an endpoint, with the origin being 
the Azure web app created previously.

![image](https://github.com/RolakeAnifowose/Azure-IoT-Application-for-Rental-vehicle-tracking/assets/31238382/9a0ed521-59b8-411a-8f63-8b9194a3b060)
