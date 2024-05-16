# ST0263 Tópicos Especiales en Telemática

Estudiantes:
- Neller Pellegrino Baquero - npellegrib@eafit.edu.co
- Pablo Micolta Lopez - pmicoltal@eafit.edu.co
- Valeria Guerra Zapata - vguerraz@eafit.edu.co

## Reto 4 - Kubernetes
Video sustentación: https://eafit-my.sharepoint.com/:v:/g/personal/vguerraz_eafit_edu_co/EZUylTEiMadIsb6W9thQ8tQB2BWwbFEFHrODZoJpuWA54Q?e=iSeqo3

### Descripción del reto
Despliegue de WebApp Monolítica en un clúster de alta disponibilidad en Kubernetes.

####  Aspectos de la actividad cumplidos o desarrollados
- Uso del servicio adminstrado EKS de AWS.
- Requisitos del reto 3 como nombre de dominio, balanceador de carga, base de datos externa (usando el servicio RDS de AWS), sistema de archivos externo a la capa de servicios (usando el servicio EFS de AWS).

####  Aspectos de la actividad no cumplidos o desarrollados
- Certificado SSL para el dominio.

## Arquitectura
![Arquitectura](https://github.com/vguerraz/ST0263-reto4/assets/84991036/fa6126cd-5ff9-4307-af8d-acf981e41b13)


## Descripción del ambiente de desarrollo y técnico
Para ejecutar y desplegar los recursos, ejectutamos:

```bash
kubectl apply -f cluster-issuer.yaml
kubectl apply -f cert.yaml
kubectl apply -f efs-pv.yaml
kubectl apply -f efs-pvc.yaml
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f wordpress-service.yaml
kubectl apply -f wordpress-ingress.yaml
```

Para comprobar el despliegue se ejecuta: 

```bash
kubectl get all
```

Se trabajó desde `AWS CloudShell`, allí se instaló `kubectl` con el siguiente comando:

```bash
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.3/2024-04-19/bin/linux/amd64/kubectl
```

También, para la instalación de `helm` (y el despliegue del controlador de NGINX Ingress se ejecutó:

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh
helm upgrade --install ingress-nginx ingress-nginx \
--repo https://kubernetes.github.io/ingress-nginx \
--namespace ingress-nginx --create-namespace
```

Instalamos cert-managet con el comando:

```bash
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.14.5/cert-manager.yaml
```

Por, último, pra la base de datos se instaló el cliente mysql en la instancia ec2 que se conectara a la base de datos mysql creada en RDS:
```bash
sudo apt-get install mariadb-client
```

## Componentes
Como se explica en el video, encontramos varios servicios usados en la consola de AWS como EFS, RDS, EC2 y EKS, dentro del clúster encontramos varios archivos de configuración o manifiestos como: 
- cert.yaml: En este manifiesto se define un recurso de tipo Certificate para la encriptación TLS.
- cluster-issuer.yaml: Se configura un recurso tipo ClusterIssuer para el servidor ACME para el certificado.
- efs-pv.yaml: Este manifiesto define un PersistentVolume para representar el recurso de almacenamiento compartido con EFS.
- efs-pv.yaml: Este manifiesto define un PersistentVolumeClaim para representar la capa de abstracción entre el pod y el PersistentVolume.
- wordpress-deployment.yaml: Aqui se define un recurso Deployment para desplegar WordPress, se definieron 2 replicas, y se consume la base de datos creada en el servicio RDS de AWS.
- wordpress-ingress.yaml:En este manifiesto se define un recurso tipo Ingress que tendrá como controlador a NGINX.
- wordpress-service.yaml: Se deine un recurso tipo Service, que expone la app de wordpress como un servicio a internet.


## Video
https://eafit-my.sharepoint.com/:v:/g/personal/vguerraz_eafit_edu_co/EZUylTEiMadIsb6W9thQ8tQB2BWwbFEFHrODZoJpuWA54Q?e=iSeqo3
