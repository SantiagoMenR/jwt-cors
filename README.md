# 📘 User Management & Authentication System  
### Spring Boot + JWT + Protected Routes + CORS + Frontend

Este proyecto implementa un sistema completo de **registro e inicio de sesión**, usando **Spring Boot**, **JWT**, filtros personalizados, **rutas protegidas**, **políticas CORS**, y un **frontend simple** en HTML/JavaScript para visualizar datos y consumir la API.

Incluye:

- ✅ Registro de usuarios  
- ✅ Login con JWT  
- ✅ Generación y validación de tokens  
- ✅ Rutas protegidas según rol  
- ✅ Filtro personalizado para autenticación  
- ✅ Comunicación segura mediante JSON  
- ✅ Política CORS configurada  
- ✅ Frontend sencillo con Fetch API  
- ✅ Almacenamiento de token en localStorage  

---

## 📁 Estructura del Proyecto

/backend
├── controller
├── service
├── filter
├── util (JwtUtil)
├── model (User, Role)
└── resources
└── application.properties

/frontend
├── index.html
├── panel.html
└── script.js


---

## 🚀 Tecnologías

### Backend
- Java 17+
- Spring Boot
- Spring Security
- JWT (JJWT)
- Lombok
- Maven

### Frontend
- HTML
- JavaScript (Fetch API)
- LocalStorage

---

# 🔐 Funcionalidades del Backend

## ➤ Registro de usuarios  
**POST `/auth/register`**

Body (JSON):
```json
{
  "username": "test",
  "password": "1234",
  "role": "ADMIN"
}

➤ Inicio de sesión

POST /auth/login

Body (JSON):

{
  "username": "test",
  "password": "1234"
}

Retorna un token JWT válido por 1 hora.
➤ Ruta protegida

GET /admin/panel

Requiere header:

Authorization: Bearer <token>

El JwtFilter se encarga de validar el token y cargar el usuario en el contexto de Spring Security.
🔧 Seguridad con JWT

El JWT contiene:

    sub → username

    role → rol del usuario

    exp → expiración

    Firma HS256

El filtro personalizado:

    Lee el header Authorization

    Extrae el token

    Valida firma y expiración

    Obtiene username y rol

    Inserta Authentication en Spring Security

🌐 Configuración de CORS

@Bean
public WebMvcConfigurer corsConfigurer() {
    return new WebMvcConfigurer() {
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**")
                    .allowedOrigins("http://localhost:5500")
                    .allowedMethods("GET","POST","PUT","DELETE")
                    .allowedHeaders("*")
                    .allowCredentials(true);
        }
    };
}

Permite que el frontend (en otro puerto) llame a la API sin bloqueos de CORS.
🧩 Frontend

Incluye:

    ✔ Formulario de registro

    ✔ Formulario de login

    ✔ Almacenamiento del token JWT en localStorage

    ✔ Llamadas a rutas protegidas

    ✔ Redirección al panel si el login es exitoso

📄 Ejemplo de envío seguro en JSON
Registro

fetch("http://localhost:8081/auth/register", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username, password, role })
});

Login

const res = await fetch("http://localhost:8081/auth/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username, password })
});

Ruta protegida

fetch("http://localhost:8081/admin/panel", {
    headers: {
        "Authorization": "Bearer " + localStorage.getItem("jwt")
    }
});

🧱 Seguridad

Este proyecto evita:

    ❌ Enviar usuario y contraseña en la URL

    ❌ Cache de credenciales

    ❌ Tokens sin firma

    ❌ Acceso a rutas protegidas sin autenticación

E implementa:

    ✔ JWT firmados

    ✔ Filtro de autenticación

    ✔ Roles

    ✔ Validación del token

    ✔ CORS configurado

▶️ Cómo Ejecutar
1️⃣ Backend

mvn spring-boot:run

API Disponible en:
➡ http://localhost:8081
2️⃣ Frontend

Usar Live Server, o:

npx serve frontend

Frontend en:
➡ http://localhost:5500
🧪 Pruebas con Postman / Insomnia
Registro

POST /auth/register

{
  "username": "admin",
  "password": "12345",
  "role": "ADMIN"
}

Login

POST /auth/login

{
  "username": "admin",
  "password": "12345"
}

Ruta protegida

GET /admin/panel

Header:

Authorization: Bearer <token>