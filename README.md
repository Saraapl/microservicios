# Reto 4 – Opción 2: Despliegue de Microservicios en Amazon EKS

## Descripción general
En este trabajo se implementó la aplicación Sock Shop, una arquitectura basada en microservicios, desplegada sobre un clúster Amazon EKS (Elastic Kubernetes Service) con nodos EC2 en la región us-east-1.  
El objetivo fue demostrar la capacidad de desplegar, administrar y exponer una aplicación distribuida en Kubernetes dentro de AWS.

## Justificación de la elección de la aplicación
La aplicación Sock Shop fue seleccionada porque es una aplicación de referencia ampliamente utilizada para demostrar arquitecturas de microservicios y despliegues en entornos de Kubernetes y nubes públicas como Amazon EKS.  

Sock Shop fue desarrollada por Weaveworks y Container Solutions con el propósito de servir como un ejemplo educativo y práctico de cómo estructurar, desplegar y escalar sistemas distribuidos en contenedores.  
La aplicación simula una tienda en línea con funcionalidades reales, como catálogo de productos, autenticación de usuarios, carrito de compras, pedidos y procesamiento de pagos. Cada una de estas funcionalidades está implementada como un microservicio independiente.

Esta aplicación resulta ideal para el proyecto porque permite:
- Analizar la comunicación entre servicios mediante APIs internas.
- Comprobar el funcionamiento del balanceo de carga, la escalabilidad y la tolerancia a fallos en Kubernetes.
- Visualizar cómo Kubernetes gestiona pods, servicios, réplicas y Load Balancers.
- Practicar la automatización del despliegue en un entorno realista, similar a sistemas empresariales.

Por estas razones, Sock Shop constituye un caso de estudio adecuado para demostrar el despliegue de microservicios en la nube utilizando las herramientas de AWS (EKS, EC2 y ELB), validando conceptos de orquestación, resiliencia y administración de infraestructura basada en contenedores.

---

## Pasos realizados

### 1. Creación del clúster EKS
- Se creó el clúster bookstore-cluster en la consola de AWS EKS.
- Se seleccionó la configuración rápida con el modo automático habilitado.
- Se asignaron subredes públicas y el rol IAM correspondiente.

### 2. Creación del grupo de nodos (EC2)
- Se configuró el grupo de nodos bookstore-nodes.
- Tipo de instancia: t3.medium
- Tamaño de disco: 20 GiB
- Tamaño deseado: 2 nodos
- Clave de acceso: eks-keypair
- Se configuraron grupos de seguridad para permitir acceso SSH y HTTP.

### 3. Configuración local
- Instalación y configuración de AWS CLI:
  ```bash
  aws configure


Se ingresaron las credenciales del laboratorio y la región us-east-1.

Conexión con el clúster:

aws eks update-kubeconfig --region us-east-1 --name bookstore-cluster

4. Despliegue de la aplicación Sock Shop

Se aplicaron los manifiestos de Kubernetes desde la carpeta del proyecto:

kubectl apply -f complete-demo.yaml

5. Verificación del despliegue

Se verificaron los pods y servicios:

kubectl get pods -n sock-shop
kubectl get svc -n sock-shop

6. Exposición del frontend mediante LoadBalancer

Inicialmente, el servicio front-end estaba configurado como NodePort.

Se editó para exponerlo públicamente:

kubectl edit svc front-end -n sock-shop


Se cambió:

type: NodePort


por:

type: LoadBalancer

7. Acceso a la aplicación

AWS generó automáticamente un Elastic Load Balancer (ELB) con el siguiente dominio público:

http://a8f516e41a68744099fd775f94b74956-261273995.us-east-1.elb.amazonaws.com


Desde este dominio se pudo acceder a la aplicación Sock Shop de manera pública."# microservicios" 
