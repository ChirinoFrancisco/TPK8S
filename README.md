Proyecto Kubernetes – Implementación de Sitio Web Estático

Asegurate de contar con lo siguiente instalado en la maquina:
Git: Para clonar y versionar los repos
Minikube: Para levantar el cluster que va a tener kubernetes local
Kubectl: Para interactuar con el cluster
Docker: Necesario para la construccion y ejecucion de contenedores

Paso a paso
1) Página Web Estática
Clona el repositorio este tiene la web estatica y los manifiestos de configuración para desplegar la página en el cluster:
git clone https://github.com/ChirinoFrancisco/TPK8S

2) Arranca Minikube usando el sigueinte comando:
minikube start

3) Montar la carpeta que contiene la página al pod.
Ejecuta el siguiente comando en una terminal aparte (no la cierres hasta dejar de usar minikube) para copiar los archivos de la carpte donde tienes los archivos de la página web al directorio del pod:
minikube mount "Directorio de la página web donde hiciste el clone":/mnt/web

4) Aplicar los manifiestos.
Dirigete con la consola a la carpeta donde hayas guardado los manifiestos y ejecuta los siguientes comandos en orden:

kubectl apply -f volumen/pv.yaml
kubectl apply -f volumen/pvc.yaml
kubectl apply -f desplegar/web-deployment.yaml
kubectl apply -f desplegar/web-deployment-direct.yaml
kubectl apply -f servicio/web-service.yaml

5) Obtener el nombre del pod.
Se debe obetener el nombre del pod para verificar que los archivos se hayan copiado correctamente y la página se pueda ejecutar, para ello usa el siguiente comando:
kubectl get pods

6) Verificar los archivos en el pod.
Usa los siguientes comandos para verificar los archivos:
kubectl exec -it "nombre del pod completo" -- /bin/sh

Ej. En mi caso fue:
kubectl exec -it web-deployment-669985dbf7-4tlfj -- /bin/sh

Luego entraras a la consola del pod, donde deberas ingresar el siguiente comando:
ls -l /usr/share/ingnx/html

Este comando debería mostrarte todos los archivos de la página web, es decir, el index.html, el style.css y la carpeta assets. 

Por último ejecuta el comando exit para salir de la consola del pod.

7) Ingresar a la página web.
Para esto haremos uso del siguiente comando:
minikube service web-service

Una vez ejecutado se abrira el navegador con la página web cargada.
