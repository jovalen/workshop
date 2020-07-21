# Azure Container Registry

En este ejercicio vamos a crear un registro de imágenes de contenedores a través del AZ CLI

## Create a resource group

Creamos un resource group con el comando  [az group create].


```azurecli
az group create --name workshoprg --location eastus
```

## Create a container registry

In this quickstart you create a *Basic* registry, which is a cost-optimized option for developers learning about Azure Container Registry. For details on available service tiers, see [Container registry service tiers][container-registry-skus].

Create an ACR instance using the [az acr create] command. The registry name must be unique within Azure, and contain 5-50 alphanumeric characters. In the following example, *myacrworkshop001* is used. Update this to a unique value.

```azurecli
az acr create --resource-group myResourceGroup \
  --name myacrworkshop001 --sku Basic
```

When the registry is created, the output is similar to the following:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2019-01-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myacrworkshop001",
  "location": "eastus",
  "loginServer": "myacrworkshop001.azurecr.io",
  "name": "myacrworkshop001",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Take note of `loginServer` in the output, which is the fully qualified registry name (all lowercase). 

## Log in to registry

Before pushing and pulling container images, you must log in to the registry. To do so, use the [az acr login][az-acr-login] command.

```azurecli
az acr login --name <registry-name>
```

The command returns a `Login Succeeded` message once completed.

## List container images

The following example lists the repositories in your registry:

```azurecli
az acr repository list --name <registry-name> --output table

