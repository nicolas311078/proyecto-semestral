# Innovatech Chile - Etapa 3: Orquestación y Automatización en AWS

Este repositorio contiene la solución de infraestructura, orquestación y automatización CI/CD para la Evaluación Parcial N°3.

## Estructura del Proyecto
* **Frontend**: Repositorio de la interfaz de usuario desarrollada en React/Vite.
* **Backend**: Repositorio de la lógica de negocio, compuesto por los microservicios de Ventas y Despachos en Spring Boot.
* **Infraestructura**: Archivo unificado de manifiestos de Kubernetes (`k8s-master.yaml`) para el despliegue en AWS EKS.

## Requisitos Técnicos Implementados
* **Orquestación en la Nube**: Despliegue completo gestionado mediante un clúster de AWS EKS (Elastic Kubernetes Service).
* **Automatización CI/CD**: Pipeline integrado con GitHub Actions que ejecuta de forma automática el Build, Push de imágenes a Amazon ECR y Deploy al clúster tras cada commit.
* **Escalabilidad Dinámica**: Implementación de Horizontal Pod Autoscaler (HPA) con un umbral del 50% de CPU, permitiendo escalar los servicios de 1 a 3 réplicas automáticamente.
* **Seguridad y Redes**: Gestión de credenciales sensibles mediante Kubernetes Secrets y exposición del servicio Frontend al exterior utilizando una arquitectura NodePort (Puerto 32303).
* **Persistencia Temporal**: Base de datos MySQL 8.0 configurada con volúmenes `emptyDir` para el entorno de pruebas del laboratorio.
