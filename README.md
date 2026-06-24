# 🚀 Innovatech Chile - Microservicio de Ventas y Despachos

Este repositorio forma parte de la Evaluación Parcial N° 3 (EP3) de la asignatura **Introducción a Herramientas DevOps** de Duoc UC. Contiene el código fuente y la infraestructura como código (Dockerfile/Pipeline) para el backend de Ventas y Despachos de Innovatech Chile.

## 📋 Descripción del Proyecto
El microservicio de ventas gestiona la lógica de órdenes de compra y se integra con la base de datos de ventas. Está diseñado para operar detrás de un Application Load Balancer (ALB) utilizando enrutamiento basado en rutas `/api/v1/ventas`.
El microservicio de despachos administra la lógica de envíos e intentos de entrega. Funciona de manera independiente al módulo de ventas y es ruteado a través del Application Load Balancer (ALB) de AWS bajo la ruta `/api/v1/despachos`.

### 🛠️ Tecnologías Utilizadas
* **Backend:** Java 17 con Spring Boot.
* **Contenedorización:** Docker.
* **Infraestructura Cloud (AWS):** ECR, ECS (Fargate) y ALB.
* **CI/CD:** GitHub Actions.

---

## ⚙️ Cómo ejecutar el proyecto localmente

### Mediante Maven/Spring Boot
1. Clona este repositorio.
2. Configura las variables de entorno para la base de datos (DB_ENDPOINT, DB_USERNAME, DB_PASSWORD, etc.).
3. Ejecuta `mvn spring-boot:run`.

### Mediante Docker
1. Construye las imagenes localmente ejecutando:
`docker build -t backend-ventas .`
`docker build -t backend-despachos .`

3. Ejecuta los contenedores mapeando el puerto y pasando las variables de entorno:
`docker run -p 8080:8080 -e DB_ENDPOINT=localhost -e DB_NAME=ventas_db backend-ventas`
`docker run -p 8081:8081 -e DB_ENDPOINT=localhost -e DB_NAME=despachos_db backend-despachos`
---

## 🔄 Pipeline CI/CD y Despliegue Automatizado
Al realizar un `push` a la rama `deploy`, GitHub Actions se encarga de:
1. **Build:** Compilar el proyecto y construir la imagen Docker.
2. **Push:** Etiquetar y enviar la imagen a AWS ECR.
3. **Deploy:** Forzar una nueva implementación (`force-new-deployment`) en el servicio de Ventas dentro del clúster de AWS ECS.

## 📈 Escalabilidad
Estos microservicios cuentan con una política de **Target Tracking Autoscaling** en ECS, que aumenta el número de tareas (contenedores) si el uso de CPU excede el 50%, manteniendo la disponibilidad ante alta demanda.

---
