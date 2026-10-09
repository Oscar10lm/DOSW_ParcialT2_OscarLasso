# DOSW_ParcialT2_OscarLasso

## Índice
1. [Datos del estudiante](#1-datos-del-estudiante)
2. [Descripción del proyecto](#2-descripción-del-proyecto)
3. [Despliegue](#3-despliegue)
4. [Estructura del proyecto](#4-estructura-del-proyecto)
5. [Tecnologías y dependencias](#5-tecnologías-y-dependencias)
6. [Configuración y variables de entorno](#6-configuración-y-variables-de-entorno)
7. [Cómo ejecutar](#7-cómo-ejecutar)
8. [Pruebas](#8-pruebas)
9. [Docker](#9-docker)
10. [CI/CD](#10-cicd)
11. [Herramientas de modelado y diseño (evidencias)](#11-herramientas-de-modelado-y-diseño-evidencias)

## 1. Datos del estudiante
- **Nombre completo:** Oscar David Lasso Martinez
- **Grupo:** DOSW - 01RE
- **Profesor agregado al repositorio:** *(Añadir check o estado)*

## 2. Descripción del proyecto
Este proyecto es un servicio backend estructurado utilizando el patrón MVC y Spring Boot. Su propósito es proveer una API RESTful base y escalable para las siguientes fases del desarrollo, integrando validaciones, seguridad básica con JWT y conectividad con MongoDB.

## 3. Despliegue
- **URL del servicio en Azure:** [https://<nombre>.azurewebsites.net](https://<nombre>.azurewebsites.net)
- **URL del health:** [https://<nombre>.azurewebsites.net/api/v1/health](https://<nombre>.azurewebsites.net/api/v1/health)
- **URL de Swagger:** [https://<nombre>.azurewebsites.net/swagger-ui/index.html](https://<nombre>.azurewebsites.net/swagger-ui/index.html)

## 4. Estructura del proyecto
```text
src/main/java/edu/dosw/parcial/
├── controller/    # Expone los endpoints REST, DTOs y manejadores de excepciones
│   ├── dtos/      # Objetos de transferencia de datos (Request y Response)
│   ├── mappers/   # Mapper IN: convierte de DTO a Dominio y viceversa
│   └── handlers/  # Captura y maneja excepciones globales (ej. GlobalExceptionHandler)
├── core/          # Lógica de negocio de la aplicación
│   ├── services/  # Implementación de los casos de uso y lógica de negocio
│   ├── models/    # Entidades del dominio puro
│   ├── validators/# Validaciones de negocio complejas
│   └── utils/     # Clases utilitarias y transversales
├── persistence/   # Comunicación con la base de datos (MongoDB)
│   ├── entities/  # Modelos de base de datos (@Document)
│   ├── repositories/# Interfaces de acceso a datos (MongoRepository)
│   └── mappers/   # Mapper OUT: convierte de Dominio a Entity/Document y viceversa
├── config/        # Configuraciones globales de Spring (Seguridad, Swagger, etc.)
└── main/          # Clase principal que levanta el contexto (@SpringBootApplication)
```

## 5. Tecnologías y dependencias

| Tecnología | Versión | Para qué se usa |
| :--- | :--- | :--- |
| Java | 21 | Lenguaje de programación principal |
| Spring Boot | 3.3.4 | Framework base para el desarrollo rápido del backend |
| Spring Web | 3.3.4 | Creación de endpoints RESTful |
| Spring Data MongoDB | 3.3.4 | ORM para interactuar con bases de datos NoSQL (MongoDB) |
| Spring Security | 3.3.4 | Autenticación y protección de rutas |
| JWT (JJWT) | 0.12.3 | Generación y validación de tokens de seguridad |
| SpringDoc OpenAPI | 2.5.0 | Generación de la documentación de la API e interfaz Swagger |
| Lombok | 1.18.34 | Reducción de código repetitivo (getters, setters, constructores) |
| MapStruct | 1.5.5.Final | Mapeo automático entre objetos (DTOs, Dominio y Entidades) |
| JUnit 5 & Mockito | 3.3.4 | Pruebas unitarias y de integración |
| Jacoco | 0.8.12 | Reporte de cobertura de pruebas |
| SonarQube Scanner | 3.10.0.2594 | Análisis estático de código |

## 6. Configuración y variables de entorno

| Variable | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `MONGODB_URI` | Cadena de conexión hacia el clúster de MongoDB | `mongodb+srv://user:pass@cluster.mongodb.net/db_name` |
| `JWT_SECRET` | Clave secreta (Base64 de al menos 44 caracteres) para firmar los JWT | `dGhpc2lzYXNlY3JldGtleXRoYXRtdXN0YmVhdGxlYXN0NDRjaGFyc2xvbmc=` |
| `PORT` | Puerto en el cual arranca el servicio internamente | `8080` |

## 7. Cómo ejecutar
### Local con Maven
```bash
export MONGODB_URI=mongodb://localhost:27017/dosw_parcial
export JWT_SECRET=unaClaveSecretaMuyLargaDeMasDe44CaracteresBase64==
mvn spring-boot:run
```

### Mongo local con Docker
```bash
docker run -d -p 27017:27017 --name mongo-parcial mongo:7
```

### Local con Docker (Imagen del proyecto)
```bash
docker run -p 8080:8080 -e MONGODB_URI=mongodb://host.docker.internal:27017/dosw_parcial -e JWT_SECRET=unaClaveSecretaMuyLargaDeMasDe44CaracteresBase64== dosw-parcial:local
```

## 8. Pruebas
Ejecutar las pruebas unitarias y generar el reporte:
```bash
mvn test
```
**Captura del resultado en verde:**
*(Agrega aquí tu captura: `![Test en verde](docs/images/test-verde.png)`)*

**Captura del reporte Jacoco:**
*(Agrega aquí tu captura: `![Jacoco Report](docs/images/jacoco-report.png)`)*

## 9. Docker
Comandos para construir y ejecutar la imagen:
```bash
docker build -t dosw-parcial:local .
docker run -p 8080:8080 -e JWT_SECRET=tuSecretBase64 dosw-parcial:local
```
**Captura del contenedor corriendo:**
*(Agrega aquí tu captura: `![Docker run](docs/images/docker-run.png)`)*

## 10. CI/CD
El proyecto cuenta con un pipeline configurado en GitHub Actions (`.github/workflows/ci-cd.yml`). El pipeline automatiza:
1. **Compilación y Pruebas:** Verifica que el código compile y los tests pasen correctamente.
2. **Construcción y Publicación de Imagen Docker:** Compila la imagen y hace push a Docker Hub.
3. **Despliegue Automático en Azure:** Actualiza el App Service para que corra la última versión de la imagen.

**Secrets configurados en el repositorio:**
- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `AZURE_CREDENTIALS`
- `AZURE_WEBAPP_NAME`
- `JWT_SECRET`

**Captura del workflow en verde:**
*(Agrega aquí tu captura: `![Workflow success](docs/images/workflow-success.png)`)*

## 11. Herramientas de modelado y diseño (evidencias)
**Cuenta de modelado (Draw.io/Lucidchart/Miro):**
*(Agrega aquí tu captura: `![Diagramas](docs/images/modelado.png)`)*

**Cuenta en Figma:**
*(Agrega aquí tu captura: `![Figma](docs/images/figma.png)`)*