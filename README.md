# Innovatech Chile - Implementación de Arquitectura Contenerizada en AWS EKS

Este repositorio documenta el código fuente y el paso a paso de la modernización de la infraestructura tecnológica para Innovatech Chile. El proyecto orquesta un Frontend y dos servicios Backend independientes en Amazon EKS, integrando un flujo CI/CD automatizado con GitHub Actions, seguridad nativa y autoescalado.

## Equipo del Proyecto
* **Integrantes:** Nicolas Maureira y Esteban Silva
* **Docente:** Eric Ramirez S.
* **Sección:** 001D

---

## Requisitos Previos
Para replicar este entorno, es necesario contar con:
* Cuenta de AWS (entorno AWS Academy con `LabRole` disponible).
* `aws-cli` instalado y autenticado en la terminal.
* `kubectl` instalado para interactuar con el clúster.
* Repositorio en GitHub configurado con los secretos necesarios para Actions (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`).

---

## Fase 1: Configuración de Red y Seguridad (AWS VPC)
La seguridad se planificó desde el día uno mediante una estricta segmentación de red. 

1. **Creación de la VPC:** Se implementa una red virtual privada llamada `DevOpsVPC`.
2. **Alta Disponibilidad:** La red se divide en dos Zonas de Disponibilidad para evitar caídas del sistema.
3. **Capa Pública:** Se crean subredes públicas con conexión a internet mediante un Internet Gateway, destinadas exclusivamente al Frontend.
4. **Capa Privada:** Se configuran subredes privadas que salen a internet de forma segura mediante un NAT Gateway, protegiendo los microservicios y la base de datos del tráfico externo.
5. **Security Groups:** Se habilita la regla de entrada TCP 32303 y 0.0.0.0/0 para permitir el tráfico HTTP al balanceador de carga web.

---

## Fase 2: Creación del Clúster (Amazon EKS)
1. **Despliegue del Clúster:** Se aprovisiona el clúster `innovatech-cluster` en la capa de red configurada previamente.
2. **Asignación de Roles:** Se asigna el `LabRole` (Node Role / Execution Role) a los Worker Nodes (instancias EC2) para otorgar permisos sin exponer credenciales.
3. **Conexión Local:** Vinculamos el clúster con la terminal ejecutando:
   ```bash
   aws eks update-kubeconfig --region us-east-1 --name innovatech-cluster
## Fase 3: Gestión de Imágenes y SecretosAmazon ECR: Se crean los repositorios privados en Amazon Elastic Container Registry (ECR) 
para almacenar las imágenes de React/Vite (Frontend) y Spring Boot (Backend).  Kubernetes Secrets (Base de Datos): 
Se inyectan las credenciales de MySQL en tiempo de ejecución para evitar contraseñas quemadas en el código: 
## Fase 4: Despliegue de la Arquitectura en KubernetesLa orquestación de la plataforma se realiza mediante manifiestos YAML nativos aplicados al clúster.
Capa de Datos: Se despliega el servicio de MySQL (Tipo ClusterIP, Puerto 3306) utilizando los secretos previamente creados.  Capa de Microservicios (Backend): Se despliegan los contenedores de Spring Boot ubicados en los directorios back-Ventas_SpringBoot y back-Despachos_SpringBoot.  Servicio Ventas (Tipo ClusterIP, Puerto 8081)[cite: 1, 2].Servicio Despachos (Tipo ClusterIP, Puerto 8080)[cite: 1, 2].Capa de Presentación (Frontend): Se despliega el contenedor del directorio front_despacho con un LoadBalancer (NodePort 32303) para exponerlo al usuario final[cite: 1, 2, 3].Nota: La infraestructura completa se puede levantar utilizando el archivo consolidado k8s-master.yaml. 
## Fase 5: Monitoreo y AutoescaladoPara garantizar la resiliencia y el uso eficiente de recursos:Horizontal Pod Autoscaler (HPA):
Se implementa el escalado nativo de Kubernetes para reaccionar a los picos de demanda[cite: 1, 2].Métrica de Reacción: Escalado automático al alcanzar el 70% de uso de CPU[cite: 1, 2].Rango Dinámico: Creación dinámica entre 1 y 4 réplicas para los microservicios[cite: 1, 2].Monitoreo con CloudWatch:
Se habilita Amazon CloudWatch para la recolección de telemetría, métricas y el análisis de logs en tiempo real[cite: 1, 2].
## Fase 6: Integración y Despliegue Continuo (CI/CD)El proyecto cuenta con una entrega continua real que elimina la necesidad de realizar despliegues manuales propensos a errores[cite: 1, 2].
Toda la automatización está orquestada mediante GitHub Actions.La configuración del pipeline se encuentra en el archivo .github/workflows/main.yml. Este flujo se activa automáticamente ante cada push de código a la rama principal[cite: 1, 2].  Flujo Exacto del Pipeline (Workflow)El pipeline ejecuta las siguientes etapas secuenciales[cite: 1, 2]:Checkout del Código: Extrae la última versión del repositorio, incluyendo los microservicios front_despacho, back-Despachos_SpringBoot y back-Ventas_SpringBoot.  Configurar Credenciales de AWS: Se autentica de manera segura utilizando los secretos guardados en GitHub para interactuar con Amazon Web Services[cite: 1, 2].Login a Amazon ECR: Establece conexión con el Elastic Container Registry para preparar la subida de los artefactos[cite: 1, 2].Construir y Subir a ECR (Build & Push):Ejecuta el comando docker build para cada microservicio utilizando su respectivo dockerfile.  Realiza el docker push de las imágenes inmutables hacia los repositorios de Amazon ECR[cite: 1, 2].Actualizar Kubeconfig para EKS: Conecta el entorno de ejecución de GitHub con nuestro clúster innovatech-cluster en AWS[cite: 1, 2].Desplegar en Amazon EKS (Deploy): Ejecuta la orden para que Kubernetes actualice los contenedores con las nuevas imágenes.Verificar Estado del Despliegue (Rollout Status): Confirma que los nuevos pods estén en estado Running exitosamente antes de dar por finalizado el proceso CI/CD[cite: 1, 2].
