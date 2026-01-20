# Simulador de Inversiones 💼

Proyecto desarrollado como parte del challenge técnico.</br>
API REST construida con **Spring Boot**, **Java 21** y **PostgreSQL**, que permite gestionar usuarios, portafolios y posiciones de inversión.

Esta API permite:
- Registrar usuarios e instrumentos financieros (assets).
- Crear portafolios de inversión asociados a cada usuario.
- Consultar el valor total del portafolio según los precios actuales de los activos.

---

## 🚀 Tecnologías utilizadas
- Java **21**
- Spring Boot 3.x
- Spring Data JPA / Hibernate
- PostgreSQL (base de datos utilizada en este proyecto)
- Swagger
- Maven
- JUnit y Mockito (para tests unitarios)
- Postman

---
## ⚙️ Configuración y ejecución

### 1️⃣ Requisitos previos
- Tener instalado:
  - **IntelliJ IDEA**
  - **Java 21+**
  - **JDK 21** (probado con versión 21.0.5)
  - **Maven 3.9+**
  - **PostgreSQL 18+**
  - **pgAdmin (herramienta visual para administrar PostgreSQL)**
  - **Postman**
> ⚠️ Importante: el proyecto fue desarrollado con **Java 21**.  
> Si tenés una versión anterior (por ejemplo Java 17 o 8), puede fallar la compilación o la ejecución.

### 2️⃣ Crear la base de datos en PostgreSQL

Antes de ejecutar el proyecto, asegurate de crear la base de datos en **PostgreSQL**.  
Podés hacerlo fácilmente desde **pgAdmin** siguiendo estos pasos:

1. Abrí **pgAdmin** y conectate a tu servidor
2. En el panel izquierdo, expandí el servidor y hacé **clic derecho** sobre **Databases** → **Create** → **Database**
3. En el campo **Database name**, escribí:`simulador_inversiones`
4. En el campo **Owner**, dejá: `postgres`

5. Hacé clic en **Save** ✅

Esto creará la base vacía en tu servidor de PostgreSQL.

---

### 🚀 Inicialización automática

Al iniciar la aplicación por primera vez:

- **Spring Boot** crea automáticamente todas las **tablas** según las entidades definidas en el código.
- Se ejecuta un **script de inicialización** (`data.postgresql.sql`), que inserta automáticamente algunos **datos de ejemplo** en la base.

> ⚡️ No es necesario ejecutar ningún script manualmente.  
> El proceso de creación e inserción de datos se realiza de forma automática al correr la aplicación.

### ▶️ Ejecutar la aplicación

1️⃣ **Editar la configuración de la base de datos**

Abrir el archivo:  
**`src/main/resources/application.properties`**

reemplazar con tu usuario y contraseña de PostgreSQL donde corresponda:

### Configuración de base de datos: PostgreSQL
spring.datasource.url=jdbc:postgresql://localhost:5432/simulador_inversiones
spring.datasource.username=TU_USUARIO </br>
spring.datasource.password=TU_CONTRASEÑA </br>

💡 Las tablas y los datos iniciales se crearán automáticamente al iniciar la aplicación.
No es necesario ejecutar ningún script manualmente.

2️⃣ **Ejecutar la aplicación**

Desde la raíz del proyecto, abrí la ruta:
**`src/main/java/com.challenge.simuladorinversiones`**

Hacé doble clic en el archivo **`SimuladorinversionesApplication.java`** </br>
luego presioná el botón ▶️ Run (Play) para iniciar la aplicación.

3️⃣ **Acceder a la documentación Swagger**

Una vez iniciada la aplicación, ingresar a:
👉 http://localhost:8080/swagger-ui.html

Desde esta interfaz podés visualizar y probar todos los endpoints de la API directamente desde el navegador

---

### 📦 Colección de Postman

Podés importar la colección de endpoints incluida en el proyecto para probar la API desde **Postman**
**`src/main/resources/api-docs.json`**

Este archivo fue exportado desde **Swagger**, por lo tanto contiene **todas las rutas y ejemplos de requests** listos para usar.

#### 🧭 Instrucciones para importar en Postman:
1. Abrir **Postman**.
2. Hacer clic en **Import**.
3. Seleccionar la pestaña **Files**.
4. Buscar el archivo: `src/main/resources/api-docs.json`.
5. Hacer clic en **Import**.
6. Se importarán automáticamente todas las rutas y ejemplos de requests.

### 📤 Ejemplos de Requests (via Swagger o Postman)

> ⚠️ **Importante:** Asegurate de que el proyecto esté corriendo antes de probar los endpoints en Postman.  
> Si no está corriendo, las requests no funcionarán.

**Crear usuario**

```http
**Endpoint:** `POST {{baseUrl}}/simulador-inversiones/usuario`  
**Content-Type:** `application/json

{
  "email": "juan.perez@testing.com",
  "name": "Juan Pérez"
}
```
1. Abrí Postman.
2. Seleccioná la carpeta Usuario ->`POST`registrar usuario
3. En la URL, aparece: `{{baseUrl}}/simulador-inversiones/usuario`
4. En la pestaña **Body**, elegí **raw** y **JSON**.
5. Pegá el contenido JSON de arriba.
6. Hacé clic en **Send** 🚀

### 📝 Respuesta esperada

- `201 Created`
- O un JSON con los datos del usuario creado.


**Crear asset**

```http
**Endpoint:** `POST {{baseUrl}}/simulador-inversiones/assets`  
**Content-Type:** `application/json`

{
  "name": "Tesla",
  "price": 175.25,
  "symbol": "TSLA"
}

```
### 💡 Cómo probarlo en Postman

1. Abrí Postman.
2. Seleccioná la carpeta **Asset** -> `POST` registrar asset.
3. En la URL, aparece: `{{baseUrl}}/simulador-inversiones/asset`
4. En la pestaña **Body**, elegí **raw** y **JSON**.
5. Pegá el contenido JSON de arriba.
6. Hacé clic en **Send** 🚀

### 📝 Respuesta esperada

- `201 Created`
- O un JSON con los datos del asset creado

---
## 🧪 Tests

El proyecto incluye **tests unitarios de servicios** desarrollados con **JUnit 5** y **Mockito**,  
ubicados en:  
**`src/test/java/com/challenge/simuladorinversiones/`**

### 🧱 Servicios testeados:
- `UsuarioServiceImplTest` → valida la creación de usuarios, detección de emails duplicados y formato inválido.
- `AssetServiceImplTest` → verifica la búsqueda y validaciones de activos (assets).
- `PortafolioServiceImplTest` → prueba la gestión de portafolios y el manejo de excepciones cuando no existen recursos.

### 🧩 Validaciones verificadas:
- Lógica interna de negocio (sin conexión a base de datos real).
- Manejo de excepciones personalizadas:
  `BadRequestException`
  `ResourceNotFoundException`
- Uso de **mocks** para simular dependencias (`UsuarioRepository`, `AssetRepository`, etc.).

### ▶️ Ejecutar los tests:

Podés ejecutar los tests de dos formas:

#### 🔹 Desde el IDE:

Ir a `src/test/java/com.challenge.simuladorinversiones/...`

Hacer clic derecho sobre cualquiera de las clases de test</br>
(`UsuarioServiceImplTest`, `AssetServiceImplTest`, o `PortafolioServiceImplTest`)  
y seleccionar:  
**Run 'NombreDelTest'**

👉 Esto ejecuta solo esa clase de test.

2️⃣  Desde el panel de Maven (IDE)

En **IntelliJ IDEA**, también podés correr los tests desde el panel lateral **Maven**:

1. Abrí el panel **Maven** (pestaña lateral derecha del IDE).
2. Expandí el perfil del proyecto (lado derecho) click en:
   `m > simuladorinversiones > Lifecycle`
3. Hacé doble clic en la tarea **test**.
4. Maven ejecutará todas las pruebas y mostrará el resultado en la consola inferior del IDE.

🧩 Al finalizar, deberías ver un mensaje similar a:

[INFO] BUILD SUCCESS </br>
[INFO] Tests run: 30, Failures: 0, Errors: 0, Skipped: 0


💡 Los tests se ejecutan **en memoria** usando **Mockito**, por lo que **no requieren una base de datos activa**.

---

## 🧠 Supuestos

- Los precios de los assets se mantienen fijos (no hay integración con datos en tiempo real).
- Cada usuario puede tener múltiples portafolios.
- Si un asset ya existe, no se puede volver a registrar con el mismo symbol / name.
- El cálculo de valor total de portafolio se hace al momento de la consultar por un portafolio
- Se asume que cada usuario tiene un email único, por lo que se puede buscar un usuario directamente por su email
- Al agregar un asset a un portafolio: si ya existe, se suma la cantidad; si no existe, se crea una nueva posición solo si el asset existe en la base de datos
