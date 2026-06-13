# Backend Despachos (Spring Boot) - Innovatech Chile ⚙️

Este repositorio contiene el código fuente, la configuración de contenedorización y el pipeline de despliegue automatizado (CI/CD) para la API Backend de la marca "Despacho" de Innovatech Chile.

## 📋 Descripción General
El proyecto está desarrollado en **Spring Boot**. Como parte integral de la arquitectura en AWS, este microservicio se ejecuta en un contenedor Docker alojado en una subred privada de EC2, garantizando la seguridad y comunicación exclusiva con el Frontend y la base de datos.

## 🐳 Arquitectura de Contenedorización

### Dockerfile
La imagen del Backend fue optimizada aplicando las siguientes prácticas DevOps:
- **Multi-stage build:** Se utiliza una etapa inicial para compilar el proyecto (ej. con Maven/Gradle) y una etapa final más liviana (JRE) solo para ejecutar el `.jar` compilado, optimizando el rendimiento.
- **Seguridad:** El servicio se ejecuta con un usuario no root para minimizar riesgos de vulnerabilidades.

### Docker Compose
El entorno se levanta localmente mediante un archivo `docker-compose.yml`, que incluye:
- **Servicio:** `backend-despachos`
- **Puertos expuestos:** `8082:8080`
- **Redes:** Conectado a la misma red interna que el Frontend.

---

## 💾 Persistencia de Datos
Para garantizar que la información crítica no se pierda al reiniciar o actualizar los contenedores, se implementaron volúmenes de Docker:
- **Tipo de Volumen:** `Named Volume`
- **Justificación:** `Se configuró un Named Volume llamado innovatech_db_volume para asegurar la persistencia de la base de datos MySQL, permitiendo que Docker gestione el almacenamiento de forma segura e independiente del ciclo de vida del contenedor`.

---

## ⚙️ Pipeline CI/CD (GitHub Actions)
El despliegue está automatizado. El flujo se activa con un `push` en la rama `deploy` y realiza lo siguiente:
1. **Build:** Construcción de la imagen Docker del Backend.
2. **Push:** Envío de la imagen al registro Docker Hub.
3. **Deploy:** Actualización automática del contenedor en la instancia EC2 correspondiente.
*(Las credenciales de AWS y variables de entorno se gestionan de forma segura mediante GitHub Secrets).*

---

## 🚀 Instrucciones de Ejecución Local

1. Clonar el repositorio:
```bash
   git clone https://github.com/bast0rd/back-despacho.git