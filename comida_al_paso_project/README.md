# 🍽️ Comida al Paso - API REST

API REST desarrollada con Django REST Framework, dockerizada y con autenticación JWT.
La idea fue crear una API REST simple y funcional para gestionar productos y categorías de un negocio gastronómico.

## 📋 Características

Esta API permite realizar operaciones CRUD (crear, leer, actualizar y eliminar) sobre productos y categorías, con diferentes niveles de acceso según el tipo de usuario.
El proyecto está completamente dockerizado, con un contenedor para la base de datos y otro para el backend de Django.

## 🧰 Tecnologías utilizadas

Python 3.12

Django 5.x

Django REST Framework

PostgreSQL

Docker / Docker Compose

JWT (SimpleJWT)

## 🚀 Instalación y Ejecución

### Requisitos previos

- Docker Desktop instalado
- Tener Git configurado

### Paso 1: Clonar el repositorio
```bash
git clone 
cd comida_al_paso_project
```

### Paso 2: Configurar variables de entorno

El archivo `.env` ya está configurado. Si necesitás modificarlo, editalo según tus necesidades.

### Paso 3: Levantar los contenedores
```bash
docker-compose up --build
```

Esto va a:
- Crear la base de datos PostgreSQL
- Crear la imagen de Django
- Ejecutar las migraciones
- Cargar los datos iniciales
- Iniciar el servidor en http://localhost:8000

### Paso 4: Crear un superusuario (opcional)

En otra terminal:
```bash
docker-compose exec web python manage.py createsuperuser
```

## 📡 Endpoints Principales

### Públicos (sin autenticación)

- `GET /api/` - Información de la API
- `GET /api/test/` - Test de conexión
- `GET /api/categorias/` - Listar categorías
- `GET /api/categorias/{id}/` - Detalle de categoría
- `GET /api/productos/` - Listar productos
- `GET /api/productos/{id}/` - Detalle de producto

### Protegidos (requieren JWT)

- `POST /api/categorias/` - Crear categoría
- `PUT/PATCH /api/categorias/{id}/` - Actualizar categoría
- `DELETE /api/categorias/{id}/` - Eliminar categoría
- `POST /api/productos/` - Crear producto
- `PUT/PATCH /api/productos/{id}/` - Actualizar producto
- `DELETE /api/productos/{id}/` - Eliminar producto

### Autenticación JWT

- `POST /api/token/` - Obtener token (enviar username y password)
- `POST /api/token/refresh/` - Refrescar token

## 🔑 Ejemplo de Autenticación

### 1. Obtener token
```bash
curl -X POST http://localhost:8000/api/token/ \
  -H "Content-Type: application/json" \
  -d '{"username": "admin", "password": "tu_password"}'
```

Respuesta:
```json
{
  "access": "eyJ0eXAiOiJKV1QiLCJhbGc...",
  "refresh": "eyJ0eXAiOiJKV1QiLCJhbGc..."
}
```

### 2. Usar el token en requests
```bash
curl -X POST http://localhost:8000/api/productos/ \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGc..." \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Nuevo Producto",
    "descripcion": "Descripción",
    "precio": 1500,
    "categoria": 1,
    "stock": 10,
    "disponible": true
  }'
```

## 🛠️ Comandos Útiles
```bash
# Ver logs en tiempo real
docker-compose logs -f web

# Detener los contenedores
docker-compose down

# Detener y eliminar volúmenes (borra la BD)
docker-compose down -v

# Entrar al contenedor de Django
docker-compose exec web bash

# Ejecutar migraciones manualmente
docker-compose exec web python manage.py migrate

# Crear superusuario
docker-compose exec web python manage.py createsuperuser

# Cargar datos iniciales
docker-compose exec web python manage.py loaddata fixtures/initial_data.json
```

## 📁 Estructura del Proyecto
```
comida_al_paso_project/
├── api/                      # App principal
│   ├── migrations/          # Migraciones de BD
│   ├── admin.py            # Configuración del admin
│   ├── models.py           # Modelos (Categoria, Producto)
│   ├── serializers.py      # Serializers con validaciones
│   ├── views.py            # ViewSets y endpoints
│   └── urls.py             # URLs de la API
├── comida_al_paso/          # Configuración del proyecto
│   ├── settings.py         # Settings con seguridad y logging
│   ├── urls.py             # URLs principales
│   └── wsgi.py             # WSGI para producción
├── fixtures/                # Datos iniciales
│   └── initial_data.json   # Categorías y productos
├── logs/                    # Logs de la aplicación
├── .env                     # Variables de entorno (NO subir a Git)
├── .env.example            # Plantilla de variables
├── docker-compose.yml      # Orquestación de contenedores
├── Dockerfile              # Imagen de Django
├── manage.py               # CLI de Django
├── requirements.txt        # Dependencias Python
└── README.md              # Este archivo
```

## 🔒 Seguridad Implementada

- ✅ Variables sensibles en archivo `.env`
- ✅ `DEBUG=False` en producción
- ✅ `ALLOWED_HOSTS` configurado
- ✅ Uso del ORM (sin SQL raw)
- ✅ Validaciones en serializers
- ✅ JWT con rotación de tokens
- ✅ CORS configurado
- ✅ Headers de seguridad (XSS, Content-Type, etc.)
- ✅ Logging completo

## 📊 Acceso al Admin

http://localhost:8000/admin

Usuario: (el que creaste con createsuperuser)

## 👨‍💻 Desarrollo

Este proyecto cumple con los requisitos de la primera entrega:
- Modelo y endpoints con CRUD
- Autenticación por JWT
- Configuración segura
- Mitigaciones OWASP
- Logging configurado
- Dockerfile con Python slim
- docker-compose con servicios web + db

## 🧾 Autor

Erica Ansaloni
Proyecto académico desarrollado como parte del curso de Desarrollo Web con Django y Docker.

## 📝 Notas

- El puerto 8000 es para Django
- El puerto 5432 es para PostgreSQL
- Los logs se guardan en la carpeta `logs/`