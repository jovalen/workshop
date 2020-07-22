# En este ejercicio vamos a crear un cluster de Openshift gestionado
## Instalamos la extendion de ARO para el AZ CLI
```
az extension add -n aro --index https://az.aroapp.io/stable
```

## Ahora creamos una VNET que contenga 2 subnets vacías
Nos creamos estas variables
```
LOCATION=eastus                 # the location of your cluster
RESOURCEGROUP=workshoparo-rg          # the name of the resource group where you want to create your cluster
CLUSTER=cluster                 # the name of your cluster
```
Y ahora ejecutamos la creación del RG y de la vnet
```
az group create --name $RESOURCEGROUP --location $LOCATION

az network vnet create \
--resource-group $RESOURCEGROUP \
--name aro-vnet \
--address-prefixes 10.0.0.0/22

az network vnet subnet create \
--resource-group $RESOURCEGROUP \
--vnet-name aro-vnet \
--name master-subnet \
--address-prefixes 10.0.0.0/23 \
--service-endpoints Microsoft.ContainerRegistry

az network vnet subnet create \
--resource-group $RESOURCEGROUP \
--vnet-name aro-vnet \
--name worker-subnet \
--address-prefixes 10.0.2.0/23 \
--service-endpoints Microsoft.ContainerRegistry

az network vnet subnet update \
--name master-subnet \
--resource-group $RESOURCEGROUP \
--vnet-name aro-vnet \
--disable-private-link-service-network-policies true
```
## Creamos el cluster
```
az aro create \
  --resource-group $RESOURCEGROUP \
  --name $CLUSTER \
  --vnet aro-vnet \
  --master-subnet master-subnet \
  --worker-subnet worker-subnet
```
