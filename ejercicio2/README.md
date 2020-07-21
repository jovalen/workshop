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
## Para ver las versiones disponibles:
```azurecli
az aks get-versions -l eastus -o table
```

## Ahora desplegamos el cluster
```
az aks create \
--resource-group aksworkshoprg \
--name aksworkshop1111 \
--vm-set-type VirtualMachineScaleSets \
--load-balancer-sku standard \
--location eastus \
--kubernetes-version 1.18.4 \
--network-plugin azure \
--vnet-subnet-id $subnetid \
--service-cidr 10.2.0.0/24 \
--dns-service-ip 10.2.0.10 \
--docker-bridge-address 172.17.0.1/16 \
--generate-ssh-keys
```
### Una vez que el cluster está desplegado, obtenemos los credenciales para poder conectarnos
```
az aks get-credentials \
    --resource-group aksworkshoprg \
    --name aksworkshop1111
```
## Ahora creamos un Namespace (podemos utilizar el fichero workshopns.yml de este repositorio)
```
kubectl apply -f workshopns.yml
```
## El siguiente paso que vamos realizar en este ejercicio del workshop, es el despliegue de una aplicación, utilizando el container registry creado en el ejercicio 1 para realizar el build de las imágenes docker
### Clonamos el repo dónde está el Dockerfile y el código fuente
```
git clone https://github.com/MicrosoftDocs/mslearn-aks-workshop-ratings-api.git
cd mslearn-aks-workshop-ratings-api
````
### Ahora utilizando una task de ACR, vamos a realizar el build de la imagen
```
az acr build \
    --resource-group workshoprg \
    --registry myacrworkshop001 \
    --image ratings-api:v1 .
```
