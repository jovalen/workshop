# AZURE ARC: Alta de un cluster k8s
En este ejercicio vamos a ver cómo podemos operacionalizar de forma centralizada, cualquier cluster de k8s, independientemente de dónde se esté ejecutando (On prem, AWS, ...)

## Instalación de prerequisitos:
```
az extension add --name connectedk8s
az extension add --name k8sconfiguration
```
## Creación de nuevo RG
```
az group create --name arcsamplerg --location eastus
```

## Ahora añadimos el cluster
```
az connectedk8s connect --name 'micluster' --resource-group 'arcsamplerg' --location 'eastus'
```
