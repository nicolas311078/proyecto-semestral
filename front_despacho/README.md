# Innovatech Chile - Etapa 3: Capa Frontend

Este repositorio contiene la interfaz de usuario desarrollada en React/Vite, contenerizada y configurada para su orquestación en la Evaluación Parcial N°3.

## Estructura del Proyecto
* **Código Fuente**: Aplicación React/Vite que consume las APIs de Ventas y Despachos.
* **Dockerfile**: Instrucciones de construcción de la imagen optimizada para producción.
* **.github/workflows/deploy.yml**: Pipeline de GitHub Actions para el flujo CI/CD.

## Requisitos Técnicos Implementados
* **Almacenamiento en la Nube**: La imagen Docker generada se almacena automáticamente en Amazon ECR.
* **Despliegue Orquestado**: Configurado para ejecutarse dentro de un clúster AWS EKS.
* **Redes y Accesibilidad**: Expuesto mediante un servicio de tipo `NodePort` (Puerto 32303) para asegurar el acceso público y sortear las limitaciones de red del entorno académico.
* **Tolerancia a Fallos**: Implementación de Horizontal Pod Autoscaler (HPA) al 50% de CPU para escalar de 1 a 3 réplicas.

## Instrucciones de Ejecución
1. Actualice la configuración de su clúster local:
   `aws eks update-kubeconfig --name innovatech-cluster`
2. Asegúrese de haber aplicado el manifiesto de infraestructura principal ubicado en el repositorio central:
   `kubectl apply -f k8s-master.yaml`
3. Monitoree el estado del servicio y los pods del frontend:
   `kubectl get pods -l app=frontend`
4. Acceda a la aplicación mediante el navegador ingresando a la IP pública de su nodo EC2 por el puerto **32303** (previa apertura en el Security Group de AWS).