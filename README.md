# Kubernetes Manifests
Requisitos previos
Antes de comenzar, asegúrate de tener instalados y configurados los siguientes requisitos:

Minikube: Para crear el clúster de Kubernetes local.

kubectl: La herramienta de línea de comandos para interactuar con Kubernetes.

Git: Para gestionar los repositorios y la integración con GitHub.

Instalación de Minikube y kubectl:
Instalar Minikube: Sigue las instrucciones en la documentación oficial de Minikube.

Instalar kubectl: Sigue las instrucciones en la documentación oficial de kubectl.

Iniciar Minikube

Inicia Minikube ejecutando el siguiente comando:

minikube start

Este comando configurará y levantará un clúster de Kubernetes local.

Desplegar el entorno:

Clona este repositorio en tu máquina local:


git clone https://github.com/AndresFarmache/k8s-manifests

cd minikube-devops/k8s-manifests

Aplica los manifiestos para crear los recursos necesarios en Kubernetes:


kubectl apply -f deployment/web-deployment.yaml
kubectl apply -f persistentvolumeclaim/pvc-web-content.yaml
kubectl apply -f persistentvolume/pv-web-content.yaml
kubectl apply -f service/web-service.yaml
Estos comandos crean un Deployment para el contenedor de Nginx, un PersistentVolumeClaim para el almacenamiento persistente, y un Service para exponer la aplicación.

Verifica que los recursos estén desplegados correctamente:


kubectl get pods
kubectl get services
Los pods deben estar en estado Running, y el servicio debe estar expuesto correctamente.

Acceder a la aplicación
Para acceder a la aplicación desde tu navegador, utiliza el siguiente comando para obtener la URL:

minikube service web-service --url
Esto te proporcionará una URL que podrás abrir en tu navegador. La aplicación servirá el contenido estático desde el volumen persistente.

Personalización del contenido
El contenido de la página web estático está basado en un repositorio de GitHub. Puedes personalizarlo siguiendo estos pasos:

Realiza un fork del repositorio static-website en tu cuenta de GitHub.

Clónalo en tu máquina local:


Información adicional
El entorno usa Nginx como servidor web para servir el contenido estático.

El PersistentVolume se monta desde un directorio local y es usado para almacenar el contenido de la página web de forma persistente incluso si los pods son reiniciados.

El entorno fue diseñado para ser accesible localmente a través de Minikube, lo que permite a los desarrolladores trabajar en un entorno de pruebas realista.


