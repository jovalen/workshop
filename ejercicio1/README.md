# Azure Container Registry

En este ejercicio vamos a crear un registro de imágenes de contenedores a través del AZ CLI

### Creación del ACR


Click [here](https://docs.microsoft.com/en-us/azure/dev-spaces/quickstart-team-development) for more details and to try it out.

## Push Images to Azure Container Registry (ACR)

This section grabs the container images from Docker Hub and then pushes them to the Azure Container Registry that was created.

```bash
# Pull Images from Docker Hub to Local Workstation
docker pull kevingbb/imageclassifierweb:v1
docker pull kevingbb/imageclassifierworker:v1

# Authenticate to ACR
az acr list -o table
az acr login -g $RG -n ${PREFIX}acr

# Push Images to ACR
docker tag kevingbb/imageclassifierweb:v1 ${PREFIX}acr.azurecr.io/imageclassifierweb:v1
docker tag kevingbb/imageclassifierworker:v1 ${PREFIX}acr.azurecr.io/imageclassifierworker:v1
docker push ${PREFIX}acr.azurecr.io/imageclassifierweb:v1
docker push ${PREFIX}acr.azurecr.io/imageclassifierworker:v1
```


