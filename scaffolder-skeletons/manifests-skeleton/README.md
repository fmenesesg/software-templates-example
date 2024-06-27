# Ejemplo de repositorio k8s con Kustomize

A continuación de explica el funcionamiento  de Kustomize, los manifiestos se deben modificar en función de la app que se quiera desplegar (nombres, recursos, etc)

## La siguiente es la estructura de las carpetas de este repositorio de ejemplo:

├── base

  │   ├── deployment.yaml

  │   ├── hpa.yaml

  │   ├── kustomization.yaml

  │   └── service.yaml

  └── overlays

      ├── dev

      │   ├── hpa.yaml

      │   └── kustomization.yaml

      ├── production

      │   ├── hpa.yaml

      │   ├── kustomization.yaml

      │   ├── rollout-replica.yaml

      │   └── service-loadbalancer.yaml

      └── staging

          ├── hpa.yaml

          ├── kustomization.yaml

          └── service-nodeport.yaml

 

BASE:

La carpeta base contiene recursos comunes, como los archivos de configuración de recursos predeterminados deployment.yaml, service.yaml y hpa.yam

 

El archivo kustomization.yaml es el archivo más importante en la carpeta base y describe qué recursos usa.

*

apiVersion: kustomize.config.k8s.io/v1beta1

  kind: Kustomization

  

  resources:

service.yaml

deployment.yaml

hpa.yaml

*

 

OVERLAY:

La carpeta de Overlay alberga superposiciones específicas del entorno. Tiene 3 subcarpetas (una para cada entorno).

e.j:

 

dev/kustomization.yaml

 

apiVersion: kustomize.config.k8s.io/v1beta1

  kind: Kustomization

  bases:

../../base

  patchesStrategicMerge:

hpa.yaml

 

Este archivo define a qué configuración base hacer referencia y aplicar parches mediante patchesStrategicMerge, que permite definir y superponer archivos YAML parciales sobre la base.

 

PATCHES:

Es necesario parchear la aplicación con Kustomize para que sus cambios persistan entre actualizaciones
