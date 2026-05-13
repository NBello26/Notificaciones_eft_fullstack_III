# 🔔 Microservicio de Notificaciones - Sistema de Gestión de Mascotas

Este repositorio contiene el microservicio de **Notificaciones**, desarrollado con Spring Boot. Su rol principal dentro del ecosistema es gestionar y despachar alertas, avisos y correos electrónicos a los usuarios (por ejemplo, cuando hay una posible coincidencia de su mascota perdida o cuando un reporte es actualizado).

## 🚀 Arquitectura y Prácticas DevOps

Al estar desacoplado del resto de la lógica de negocio, este servicio puede procesar colas de mensajes o peticiones HTTP de forma asíncrona sin afectar el rendimiento de los demás módulos. 

El ciclo de vida y despliegue está totalmente automatizado mediante un pipeline de **CI/CD con GitHub Actions**:
1. Compila el código fuente en Java y verifica su integridad.
2. Empaqueta el artefacto en una imagen de Docker.
3. Sube la imagen a **Amazon Elastic Container Registry (ECR)**.
4. Ejecuta un despliegue remoto y seguro en la instancia **EC2** correspondiente (Nodo Back 3) mediante **AWS Systems Manager (SSM)**.
5. Inyecta variables de entorno esenciales (configuraciones de servidor de correo, Service Discovery y credenciales) directamente en el contenedor durante el tiempo de ejecución.

### 🌐 Ecosistema de Infraestructura en AWS
Este microservicio opera en el tercer nodo de backend, optimizando costos y recursos al compartir instancia con el servicio de Geolocalización:

* **Nodo Web:** ApiGateway y Frontend
* **Nodo Back 1:** Eureka Server y BFF
* **Nodo Back 2:** Microservicios de Mascotas y Usuarios
* **Nodo Back 3:** Microservicio de Geolocalización y **Microservicio de Notificaciones** (Este repositorio) 📍
* **Nodo Back 4:** Motor de Coincidencias
* **Base de Datos:** SanosDB (PostgreSQL)

## 🛠️ Tecnologías Principales

* **Framework:** Java 17 / Spring Boot 3
* **Mensajería/Correo:** Java Mail Sender (u otra integración de notificaciones)
* **Cloud Native:** Spring Cloud Netflix Eureka Client
* **Contenedores:** Docker
* **CI/CD:** GitHub Actions
* **Infraestructura AWS:** EC2, ECR, SSM, IAM

## ⚙️ Descubrimiento y Redes

* **Aislamiento Interno:** Aunque se encarga de enviar comunicaciones al exterior (ej. correos), recibe sus instrucciones (triggers) estrictamente desde la red privada de la VPC de AWS, protegiéndolo de peticiones públicas no autorizadas.
* **Auto-Registro (Eureka):** Al iniciar, el contenedor adquiere la IP privada de su EC2 y se registra en el servidor Eureka (bajo el identificador `MS-NOTIFICACIONES`). El BFF y otros servicios utilizan este nombre para solicitar el envío de alertas sin depender de configuraciones estáticas.

## 📦 Repositorios del Proyecto

Explora el resto de la infraestructura y microservicios de este ecosistema:

**Frontend y Puertas de Enlace**
* 🌐 [Frontend_eft_fullstack_III](https://github.com/NBello26/Frontend_eft_fullstack_III)
* 🚪 [ApiGateway_eft_fullstack_III](https://github.com/NBello26/ApiGateway_eft_fullstack_III)
* 🌉 [BFF_eft_fullstack_III](https://github.com/NBello26/BFF_eft_fullstack_III)

**Descubrimiento y Base de Datos**
* 🧭 [Eureka_eft_fullstack_III](https://github.com/NBello26/Eureka_eft_fullstack_III)
* 🗄️ [BD_eft_fullstack_III](https://github.com/NBello26/BD_eft_fullstack_III)

**Microservicios de Negocio**
* 🐾 [Reporte_Mascota_eft_fullstack_III](https://github.com/NBello26/Reporte_Mascota_eft_fullstack_III)
* 👤 [Usuarios_eft_fullstack_III](https://github.com/NBello26/Usuarios_eft_fullstack_III)
* 📍 [Geolocalizacion_eft_fullstack_III](https://github.com/NBello26/Geolocalizacion_eft_fullstack_III)
* 🔔 [Notificaciones_eft_fullstack_III](https://github.com/NBello26/Notificaciones_eft_fullstack_III) *(Estás aquí)*
* 🧩 [Coincidencias_eft_fullstack_III](https://github.com/NBello26/Coincidencias_eft_fullstack_III)

---
*Desarrollado como parte del proyecto final de integración de arquitectura DevOps.*
