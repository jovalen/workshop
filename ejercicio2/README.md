# Despliegue de K8S con AKS
En este ejercicio vamos a crear un cluster de AKS

## Creamos un grupo de recursos

Creamos un resource group con el comando  [az group create].


```azurecli
az group create --name aksworkshoprg --location eastus
```

## Como vamos a utilizar el plugin de red Azure Container Network Interface (CNI, como hemos visto en el workshop), creamos una vnet y una subnet
```azurecli
az network vnet create \
    --resource-group aksworkshoprg \
    --location eastus \
    --name aks-vnet \
    --address-prefixes 10.0.0.0/8 \
    --subnet-name aks-subnet \
    --subnet-prefix 10.240.0.0/16
```
## Ahora nos guardamos en una variable el ID de la subnet para utilizarlo más adelante en la creación del cluster de AKS
```azurecli
subnetid=`az network vnet subnet show -g aksworkshoprg --vnet-name aks-vnet --name aks-subnet --query id -o tsv`
```
