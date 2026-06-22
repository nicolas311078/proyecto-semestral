# Innovatech Chile - Etapa 3: Capa de Microservicios Backend

Este repositorio contiene la lógica de negocio de Innovatech, compuesta por los microservicios de Ventas y Despachos construidos en Spring Boot, listos para su despliegue en AWS EKS.

## Estructura del Proyecto
* **Microservicio Despachos**: API REST para la gestión de envíos.
* **Microservicio Ventas**: API REST para el control de transacciones.
* **Dockerfiles**: Archivos de construcción independiente para cada microservicio.
* **.github/workflows/deploy.yml**: Automatización del flujo CI/CD para compilar y subir las imágenes a Amazon ECR.

## Requisitos Técnicos Implementados
* **Comunicación Segura**: Los servicios operan mediante `ClusterIP` (puertos 8080 y 8081), manteniéndose aislados del internet público y comunicándose internamente con el Frontend y la base de datos.
* **Gestión de Secrets**: Las contraseñas de conexión a MySQL se inyectan en tiempo de ejecución a través de variables de entorno protegidas por Kubernetes Secrets.
* **Autoscaling Independiente**: Cada microservicio cuenta con su propia política HPA que reacciona al 50% de carga de CPU, escalando de 1 a 3 réplicas para mantener la operatividad del sistema.

## Instrucciones de Ejecución
1. Enlace su entorno con el clúster EKS:
   `aws eks update-kubeconfig --name innovatech-cluster`
2. **Importante:** Cree el secreto de la base de datos antes del despliegue:
   `kubectl create secret generic db-secrets --from-literal=password=rootpassword`
3. Despliegue los servicios aplicando el manifiesto de infraestructura central:
   `kubectl apply -f k8s-master.yaml`
4. Verifique la correcta comunicación interna revisando los registros de ejecución:
   `kubectl logs deployment/innovatech-back-despachos`
   `kubectl logs deployment/innovatech-back-ventas`