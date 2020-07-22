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
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password Y0urpassw0rd \
    --size Standard_D2_v3 
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
#### Probamos que se ha instalado correctamente, listando imagenes por ejemplo
```
sudo docker image list
```

#### Ahora damos permisos a los usuarios sin necesidad de usar sudo ( Para este workshop nos va a resultar más sencillo)
```
sudo groupadd docker
sudo gpasswd -a $USER docker
newgrp docker
```
### Instalamos AZ CLI
```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```
### Instalamos kubectl para poder administrar los clusters
```
sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```
### Instalamos el cliente helm en nuestra dev VM
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
