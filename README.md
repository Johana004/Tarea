☁️ Proyecto Clima PHP: Docker + Kubernetes (Open-Meteo API)

Este documento es una guía para la construcción y el despliegue de una aplicación web básica en PHP. Esta aplicación consume la API pública de clima de Open-Meteo, está contenida en un contenedor Docker y se despliega en un clúster local de Kubernetes (k8s) utilizando Docker Desktop en Windows.

1. ⚙️ Requisitos Previos

Asegúrese de que su entorno cumple con los siguientes requisitos:

Sistema Operativo: Windows 10/11 Professional o Enterprise.

Docker Desktop: Instalado y en ejecución (el ícono de la ballena debe estar estable).

Kubernetes: Habilitado y en estado Running dentro de Docker Desktop (sección Settings → Kubernetes).

Terminal: PowerShell (recomendado) o Bash.

2. 📁 Clonación e Inicialización del Proyecto

Para obtener los archivos necesarios (index.php, Dockerfile, y k8s-deployment.yaml), clone el repositorio o copie el contenido en una carpeta local de su preferencia.

Abra su terminal (PowerShell).

Navegue a la ubicación donde desea almacenar el proyecto.

Utilice el comando de clonación (o simplemente pegue la carpeta si ya tiene los archivos):

# Ejemplo de clonación (si aplica):
git clone [URL_DE_SU_REPOSITORIO]
cd php-clima-k8s # o el nombre de la carpeta del proyecto


3. 📦 Construcción de la Imagen Docker

Desde la carpeta raíz del proyecto (donde se encuentra el Dockerfile), ejecute el siguiente comando para construir la imagen de la aplicación. Esta imagen se etiquetará como php-clima-api:v1.

docker build -t php-clima-api:v1 .


Importante: El punto (.) al final indica a Docker que el contexto de construcción es el directorio actual.

4. 🚀 Despliegue en Kubernetes

Una vez que la imagen php-clima-api:v1 ha sido construida y etiquetada localmente, aplique el manifiesto de Kubernetes. Este archivo crea un Deployment y un Service de tipo NodePort.

kubectl apply -f k8s-deployment.yaml


5. 🔍 Verificación del Despliegue

Verifique el estado de los componentes desplegados en Kubernetes para asegurarse de que la aplicación se haya iniciado correctamente.

Verificar el Pod (debe estar en estado Running):

kubectl get pods


Verificar el Servicio (obtener el puerto NodePort asignado):

kubectl get service php-clima-service


En la columna PORTS, busque el mapeo. Generalmente será 80:30080/TCP. El puerto 30080 es el puerto de acceso externo.

6. 🌐 Acceso a la Aplicación

Con el Pod en estado Running y el Service configurado, puede acceder a la aplicación web de clima desde su navegador en Windows:

http://localhost:30080


Si el NodePort asignado es diferente a 30080, utilice el puerto correspondiente que obtuvo en el paso anterior.

7. 🧹 Limpieza de Recursos (Opcional)

Para eliminar los recursos creados en Kubernetes y la imagen Docker de su sistema:

# Eliminar Deployment y Service
kubectl delete -f k8s-deployment.yaml

# Eliminar la imagen Docker (use con cuidado)
docker rmi php-clima-api:v1
