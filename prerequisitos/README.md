# INSTALAR PREREQUISITOS
En este capítulo vamos a rear en Azure una VM con SO Ubuntu a la que instalaremos docker (versión community) y az CLI. Utilizaremos esta VM como nuestra dev machine.

## Crear una VM en Azure y configurar utilidades

### Creamos un resource group
```azcli
az group create --name workshoprg --location eastus
```
### Creamos la VM
```azcli
az vm create \
    --resource-group workshoprg \
    --name myvm001 \
    --image 18.04-LTS \
    --admin-username azureuser \
    --admin-password $AdminPassword \
    --size Standard_D2_v3 \
    --use-managed-disk \
    --storage-sku Standard_LRS
```
### Instalar docker ce
```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
```
