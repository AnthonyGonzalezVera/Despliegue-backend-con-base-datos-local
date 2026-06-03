# Práctica Semana 8 — Automatización de despliegue Backend con Docker Compose, PostgreSQL y pgAdmin

## Duración

120 minutos

## Fundamentos

Docker permite crear, desplegar y administrar aplicaciones mediante contenedores, proporcionando entornos consistentes y reproducibles. Docker Compose facilita la orquestación de múltiples servicios mediante un único archivo de configuración, permitiendo desplegar aplicaciones compuestas por varios contenedores interconectados.

En esta práctica se automatizó el despliegue de una aplicación backend basada en Spring Boot, junto con una base de datos PostgreSQL y su panel de administración pgAdmin, utilizando Docker Compose. Además, se implementó una estrategia Multi-Stage Build para optimizar la construcción de la imagen Docker de la aplicación.

Se utilizaron los siguientes componentes:

* Docker — Plataforma de contenedorización.
* Docker Compose — Herramienta para definir y ejecutar aplicaciones multicontenedor.
* PostgreSQL — Sistema gestor de bases de datos relacional.
* pgAdmin — Herramienta gráfica para administración de PostgreSQL.
* Spring Boot — Framework utilizado para el desarrollo del backend.
* Maven — Herramienta de construcción y gestión de dependencias.
* Java 21 — Entorno de ejecución utilizado por la aplicación.
* Dockerfile Multi-Stage — Técnica para optimizar imágenes Docker.

---

## Conocimientos previos requeridos

* Comandos básicos de Linux.
* Uso básico de Docker y Docker Compose.
* Manejo de Git y GitHub.
* Conocimientos básicos de Spring Boot.
* Uso de terminal y navegador web.

---

## Objetivos

* Clonar el proyecto backend.
* Configurar variables de entorno mediante archivo .env.
* Crear un Dockerfile Multi-Stage para la aplicación.
* Configurar PostgreSQL y pgAdmin mediante Docker Compose.
* Crear redes y volúmenes para persistencia y conectividad.
* Construir la imagen Docker del backend.
* Desplegar todos los servicios mediante Docker Compose.
* Verificar la conectividad entre backend y PostgreSQL.
* Comprobar el funcionamiento de pgAdmin.

---

## Equipo necesario

| Recurso           | Detalle                   |
| ----------------- | ------------------------- |
| Sistema operativo | Kali Linux                |
| Software          | Docker y Docker Compose   |
| Herramientas      | Git, Nano y navegador web |
| Lenguaje          | Java 21                   |
| Base de datos     | PostgreSQL 16             |
| Conectividad      | Acceso a Internet         |

---

## Material de apoyo

* Docker Documentation
* Docker Compose Documentation
* PostgreSQL Documentation
* pgAdmin Documentation
* Spring Boot Documentation
* Guía de la asignatura

---

# Procedimiento

## Paso 1 — Clonación del proyecto

Se clonó el repositorio proporcionado por el docente y posteriormente se ingresó al directorio principal del proyecto.

```bash
git clone https://github.com/maguaman2/tendencias-mar22-security.git

cd tendencias-mar22-security
```

Figura 1-1. Clonación del proyecto backend

**Figura 1-1**

---

## Paso 2 — Configuración del archivo .env

Se creó un archivo `.env` para almacenar las variables de entorno utilizadas por PostgreSQL, pgAdmin y la aplicación backend.

```bash
nano .env
```

Posteriormente se verificó su contenido.

```bash
cat .env
```

Figura 1-2. Configuración del archivo .env

**Figura 1-2**

---

## Paso 3 — Creación del Dockerfile Multi-Stage

Se creó el archivo Dockerfile para empaquetar la aplicación backend utilizando una estrategia Multi-Stage Build.

```bash
nano Dockerfile
```

Figura 1-3. Creación del Dockerfile

**Figura 1-3**

---

## Paso 4 — Configuración del Dockerfile

Se configuró el Dockerfile utilizando Maven para compilar la aplicación y Java 21 para su ejecución.

Figura 1-4. Configuración del Dockerfile Multi-Stage

**Figura 1-4**

### Explicación del Dockerfile

| Instrucción                         | Función                           |
| ----------------------------------- | --------------------------------- |
| FROM maven:3.9.6-eclipse-temurin-21 | Entorno de compilación            |
| WORKDIR /app                        | Directorio de trabajo             |
| COPY                                | Copia archivos del proyecto       |
| RUN mvn clean package               | Compila la aplicación             |
| FROM eclipse-temurin:21-jre         | Imagen ligera para ejecución      |
| COPY --from=builder                 | Copia el archivo JAR generado     |
| EXPOSE 8080                         | Expone el puerto de la aplicación |
| ENTRYPOINT                          | Ejecuta la aplicación             |

---

## Paso 5 — Creación del archivo docker-compose.yml

Se creó el archivo de configuración Docker Compose para administrar PostgreSQL, pgAdmin y la aplicación backend.

```bash
nano docker-compose.yml
```

Figura 1-5. Creación del archivo docker-compose.yml

**Figura 1-5**

---

## Paso 6 — Configuración de Docker Compose

Se configuraron los servicios PostgreSQL, pgAdmin y backend, incluyendo redes y volúmenes para persistencia de datos.

Figura 1-6. Configuración de servicios Docker Compose

**Figura 1-6**

---

## Paso 7 — Construcción de la imagen Docker

Se construyó la imagen de la aplicación backend.

```bash
docker compose build
```

Posteriormente se verificaron las imágenes disponibles.

```bash
docker images
```

Figura 1-7. Construcción de la imagen Docker

**Figura 1-7**

---

## Paso 8 — Despliegue de contenedores

Se levantaron todos los servicios definidos en Docker Compose.

```bash
docker compose up -d
```

Posteriormente se verificó el estado de los contenedores.

```bash
docker ps
```

Figura 1-8. Despliegue de contenedores

**Figura 1-8**

---

## Paso 9 — Verificación de volúmenes Docker

Se verificó la creación correcta de los volúmenes para persistencia de datos.

```bash
docker volume ls
```

Figura 1-9. Verificación de volúmenes

**Figura 1-9**

---

## Paso 10 — Verificación de redes Docker

Se verificó la creación de la red utilizada para la comunicación entre servicios.

```bash
docker network ls
```

Figura 1-10. Verificación de redes

**Figura 1-10**

---

## Paso 11 — Verificación de conexión Backend y PostgreSQL

Se revisaron los registros del contenedor backend para comprobar la conexión exitosa hacia PostgreSQL y la ejecución de migraciones.

```bash
docker logs backend_app
```

Figura 1-11. Verificación de conexión con PostgreSQL

**Figura 1-11**

---

## Paso 12 — Acceso a pgAdmin

Se accedió al panel de administración pgAdmin mediante navegador web.

```text
http://localhost:5050
```

Credenciales utilizadas:

```text
Usuario: admin@admin.com
Contraseña: admin123
```

Figura 1-12. Acceso a pgAdmin

**Figura 1-12**

---

# Diagrama de arquitectura

```text
┌──────────────────────────────────────────────┐
│                 Docker Engine                │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │            PostgreSQL                │    │
│  │              :5432                   │    │
│  └───────────────┬──────────────────────┘    │
│                  │                           │
│                  ▼                           │
│  ┌──────────────────────────────────────┐    │
│  │         Backend Spring Boot          │    │
│  │              :8080                   │    │
│  └──────────────────────────────────────┘    │
│                                              │
│  ┌──────────────────────────────────────┐    │
│  │             pgAdmin                  │    │
│  │              :5050                   │    │
│  └──────────────────────────────────────┘    │
└──────────────────────────────────────────────┘
```

---

# Resultados esperados

Se automatizó correctamente el despliegue de una aplicación backend utilizando Docker Compose. Se configuró una base de datos PostgreSQL con persistencia mediante volúmenes Docker, así como un panel de administración pgAdmin para su gestión. Se construyó una imagen optimizada utilizando Multi-Stage Build, permitiendo reducir el tamaño final de la imagen y mejorar el proceso de despliegue. Finalmente, se verificó la conectividad entre la aplicación backend y PostgreSQL mediante los registros de ejecución.

---

# Bibliografía

Docker Inc. (2026). Docker Documentation.

https://docs.docker.com/

Docker Inc. (2026). Docker Compose Documentation.

https://docs.docker.com/compose/

The PostgreSQL Global Development Group. (2026). PostgreSQL Documentation.

https://www.postgresql.org/docs/

pgAdmin Development Team. (2026). pgAdmin 4 Documentation.

https://www.pgadmin.org/docs/

VMware Tanzu. (2026). Spring Boot Documentation.

https://docs.spring.io/spring-boot/docs/current/reference/html/

Apache Software Foundation. (2026). Apache Maven Documentation.

https://maven.apache.org/guides/

## Repositorio utilizado

https://github.com/maguaman2/tendencias-mar22-security
