
# Container on Azure

A brief description of project 'Capture-database-changes' In this project we create docker container to listen to our database changes.
The container i run on azure container instances.




![Running Container](https://i.ibb.co/d2KBykS/Screenshot-2022-04-28-at-11-20-02-AM.png)

## Azure Container Registory

1) To run private container you have to push container to container Registory in my case it was azure container registory.
Go to [Container Registory](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.ContainerRegistry%2Fregistries) and create your.

2) After Setting up container registory next step is to push your container to registory.
3) Use CLI to login to docker registory
- docker login <registary-name>.azurecr.io
- prompt for username / password to login here
- get login credentials from registary access key Setting
![](https://i.ibb.co/w62Y0zJ/Screenshot-2022-04-28-at-11-27-03-AM.png) 

## Pushing Image to Registory
To push to azure registary image name should include registary as follow
```
<registary-name>.azurecr.io/<image-name>:latest
```

### Example docker-compose.yaml
```
version: '3'
services:
  pg-cdc-container:
    build: 
      context: .
      args:
        - DB_CONNECTION_STRING=${DB_CONNECTION_STRING}
        - SERVICE_BUS_CONNECTION_STRING=${SERVICE_BUS_CONNECTION_STRING}
        - SERVICE_BUS_QUEUE=${SERVICE_BUS_QUEUE}
        - REPLICATION_SLOT_NAME=${REPLICATION_SLOT_NAME}
    image: myregistory.azurecr.io/cdc-container:0.4
```

### Push Steps
1) Build your image
- ``` docker compose build ```
2) Push to registary
- ``` docker push <image-name>```
- ``` e.g docker push myregistory.azurecr.io/cdc-container:0.4```


## Check Azure Container Registory to verify push
![](https://i.ibb.co/h1gZ8yB/Screenshot-2022-04-28-at-11-38-12-AM.png)


## Deploying image using Azure Container Instances
Go to [Container Instances](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.ContainerInstance%2FcontainerGroups) and select your resource group and select image do config and thats all 
container is now running.

### Viewing logs
We can use azure cli tools to as well to see logs
Download az tools ```brew update && brew install azure-cli```
#### Login & View logs
- ``` az login ``` It will open web page and ask you to login. (It also return all you active directories tenant id, which you can use to switch directories)
- Switch directories ```az login --tenant <tenant_id>```
- View log of container ```az container logs --resource-group myResourceGroup --name mycontainer```
