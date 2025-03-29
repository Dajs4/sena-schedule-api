# Documentación de la API REST SENA Schedule

## Descripción General

Esta API REST proporciona una interfaz para gestionar los horarios del SENA, permitiendo administrar instructores, fichas (grupos), programas de formación, ambientes, sedes, competencias, resultados de aprendizaje y la programación de clases.

La API está construida con FastAPI, utiliza SQLAlchemy como ORM para interactuar con la base de datos MySQL, y JWT para la autenticación y autorización.

## Base URL

```
http://localhost:8000/api/v1
```

## Documentación Interactiva

La API proporciona documentación interactiva que permite explorar y probar todos los endpoints disponibles:

- **Swagger UI**: Disponible en `http://localhost:8000/docs`
  - Interfaz interactiva que permite probar los endpoints directamente desde el navegador
  - Muestra todos los esquemas, parámetros y respuestas posibles

- **ReDoc**: Disponible en `http://localhost:8000/redoc`
  - Versión más amigable para lectura de la documentación
  - Mejor para consulta y referencia

Ambas interfaces de documentación son accesibles sin necesidad de autenticación y permiten ver la descripción completa de todos los endpoints, incluyendo los esquemas de solicitud y respuesta.

## Autenticación

La API utiliza autenticación JWT (JSON Web Token). Para acceder a los endpoints protegidos, es necesario incluir el token JWT en el encabezado de la solicitud:

```
Authorization: Bearer <token>
```

### Endpoints de Autenticación

#### Registro de Usuario

```
POST /api/v1/auth/register
```

Permite registrar un nuevo usuario en el sistema.

**Cuerpo de la solicitud:**
```json
{
  "email": "usuario@example.com",
  "password": "contraseña",
  "role": "Coordinator" // o "Administrator"
}
```

**Respuesta exitosa (201 Created):**
```json
{
  "success": true,
  "message": "User registered successfully"
}
```

#### Inicio de Sesión

```
POST /api/v1/auth/login
```

Permite iniciar sesión y obtener un token JWT.

**Cuerpo de la solicitud:**
```json
{
  "email": "usuario@example.com",
  "password": "contraseña"
}
```

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Login successful",
  "data": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "token_type": "bearer"
  }
}
```

## Gestión de Instructores

### Obtener todos los instructores

```
GET /api/v1/instructors?skip=0&limit=100
```

Retorna una lista paginada de todos los instructores.

**Parámetros de consulta:**
- `skip`: Número de registros a omitir (default: 0)
- `limit`: Número máximo de registros a devolver (default: 100)

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Instructors retrieved successfully",
  "data": [
    {
      "id": 1,
      "name": "John Smith",
      "contract_type": "Contract",
      "coordination": "Systems",
      "status": "Active",
      "weekly_hours": 32,
      "image": "john.jpg"
    },
    // ...
  ],
  "total": 10,
  "page": 1,
  "size": 100,
  "pages": 1
}
```

### Obtener instructor por ID

```
GET /api/v1/instructors/{instructor_id}
```

Retorna la información detallada de un instructor específico, incluyendo fichas asignadas, horarios y horas por día.

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Instructor retrieved successfully",
  "data": {
    "id": 1,
    "name": "John Smith",
    "contract_type": "Contract",
    "coordination": "Systems",
    "status": "Active",
    "weekly_hours": 32.0,
    "image": "john.jpg",
    "created_at": "2023-05-15T10:30:00",
    "updated_at": "2023-05-20T14:45:00",
    "groups": [
      {
        "id": 1,
        "group_number": "2345678",
        "schedule": "Morning",
        "program_name": "Systems Technician"
      }
    ],
    "teaching_schedule": [
      {
        "topic_id": 1,
        "topic_name": "Introduction to Programming",
        "day": "Monday",
        "classroom": {
          "id": 1,
          "number": "A101",
          "campus": "Main Campus"
        },
        "timeblock": {
          "id": 1,
          "start_time": "07:00",
          "end_time": "09:00",
          "duration_minutes": 120
        },
        "group": {
          "id": 1,
          "group_number": "2345678"
        }
      }
    ],
    "hours_by_day": {
      "Monday": 4.0,
      "Tuesday": 2.0,
      "Wednesday": 4.0
    }
  }
}
```

### Crear instructor

```
POST /api/v1/instructors
```

Crea un nuevo instructor (solo administradores).

**Cuerpo de la solicitud:**
```json
{
  "name": "Mary Johnson",
  "contract_type": "Staff",
  "coordination": "Multimedia",
  "status": "Active",
  "image": "mary.jpg"
}
```

**Respuesta exitosa (201 Created):**
```json
{
  "success": true,
  "message": "Instructor created successfully",
  "data": {
    "id": 2,
    "name": "Mary Johnson",
    "contract_type": "Staff",
    "coordination": "Multimedia",
    "status": "Active",
    "weekly_hours": 0.0,
    "image": "mary.jpg",
    "created_at": "2023-08-01T09:15:00",
    "updated_at": "2023-08-01T09:15:00",
    "groups": [],
    "teaching_schedule": [],
    "hours_by_day": {}
  }
}
```

### Actualizar instructor

```
PUT /api/v1/instructors/{instructor_id}
```

Actualiza la información de un instructor existente (solo administradores).

**Cuerpo de la solicitud:**
```json
{
  "name": "Mary Smith",
  "contract_type": "Contract",
  "status": "Inactive"
}
```

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Instructor updated successfully",
  "data": {
    "id": 2,
    "name": "Mary Smith",
    "contract_type": "Contract",
    "coordination": "Multimedia",
    "status": "Inactive",
    "weekly_hours": 0.0,
    "image": "mary.jpg",
    "created_at": "2023-08-01T09:15:00",
    "updated_at": "2023-08-02T11:30:00",
    "groups": [],
    "teaching_schedule": [],
    "hours_by_day": {}
  }
}
```

### Eliminar instructor

```
DELETE /api/v1/instructors/{instructor_id}
```

Elimina un instructor (solo administradores). No es posible eliminar instructores con temas asignados.

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Instructor with ID 2 deleted successfully"
}
```

## Gestión de Grupos (Fichas)

### Obtener todos los grupos

```
GET /api/v1/groups?skip=0&limit=100
```

Retorna una lista paginada de todos los grupos (fichas).

**Parámetros de consulta:**
- `skip`: Número de registros a omitir (default: 0)
- `limit`: Número máximo de registros a devolver (default: 100)

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Groups retrieved successfully",
  "data": [
    {
      "id": 1,
      "group_number": "2345678",
      "schedule": "Morning",
      "program_id": 1,
      "start_date": "2023-01-15",
      "created_at": "2023-01-10T08:00:00",
      "updated_at": "2023-01-10T08:00:00"
    },
    // ...
  ],
  "total": 5,
  "page": 1,
  "size": 100,
  "pages": 1
}
```

### Obtener grupo por ID

```
GET /api/v1/groups/{group_id}
```

Retorna la información detallada de un grupo específico, incluyendo programa de formación, trimestre actual, competencias y resultados de aprendizaje programados para el trimestre actual, e instructores asignados.

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Group retrieved successfully",
  "data": {
    "id": 1,
    "group_number": "2345678",
    "schedule": "Morning",
    "program_id": 1,
    "start_date": "2023-01-15",
    "created_at": "2023-01-10T08:00:00",
    "updated_at": "2023-01-10T08:00:00",
    "program": {
      "id": 1,
      "name": "Systems Technician",
      "type": "trainingChain",
      "duration": 4,
      "status": "Active"
    },
    "current_quarter": {
      "id": 2,
      "quarter_number": 2,
      "start_date": "2023-04-01",
      "end_date": "2023-06-30",
      "status": "Active"
    },
    "competencies": [
      {
        "id": 1,
        "name": "Software Development",
        "learning_outcomes": [
          {
            "id": 1,
            "name": "Create algorithms according to specifications"
          },
          {
            "id": 3,
            "name": "Implement information systems"
          }
        ],
        "instructors": [
          {
            "id": 1,
            "name": "John Smith",
            "contract_type": "Contract",
            "status": "Active",
            "coordination": "Systems",
            "weekly_hours": 32.0,
            "image": "john.jpg"
          }
        ]
      }
    ],
    "end_date": "2024-01-15"
  }
}
```

### Crear grupo

```
POST /api/v1/groups
```

Crea un nuevo grupo (ficha) (solo administradores).

**Cuerpo de la solicitud:**
```json
{
  "group_number": "3456789",
  "schedule": "Afternoon",
  "program_id": 2,
  "start_date": "2023-02-01"
}
```

**Respuesta exitosa (201 Created):**
```json
{
  "success": true,
  "message": "Group created successfully",
  "data": {
    "id": 2,
    "group_number": "3456789",
    "schedule": "Afternoon",
    "program_id": 2,
    "start_date": "2023-02-01",
    "created_at": "2023-01-25T10:30:00",
    "updated_at": "2023-01-25T10:30:00",
    "program": {
      "id": 2,
      "name": "Software Development Technologist",
      "type": "openChain",
      "duration": 7,
      "status": "Active"
    },
    "current_quarter": {
      "id": 2,
      "quarter_number": 2,
      "start_date": "2023-04-01",
      "end_date": "2023-06-30",
      "status": "Active"
    },
    "competencies": [],
    "end_date": "2024-11-01"
  }
}
```

### Actualizar grupo

```
PUT /api/v1/groups/{group_id}
```

Actualiza la información de un grupo existente (solo administradores).

**Cuerpo de la solicitud:**
```json
{
  "schedule": "Evening",
  "start_date": "2023-02-15"
}
```

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Group updated successfully",
  "data": {
    "id": 2,
    "group_number": "3456789",
    "schedule": "Evening",
    "program_id": 2,
    "start_date": "2023-02-15",
    "created_at": "2023-01-25T10:30:00",
    "updated_at": "2023-02-01T14:45:00",
    "program": {
      "id": 2,
      "name": "Software Development Technologist",
      "type": "openChain",
      "duration": 7,
      "status": "Active"
    },
    "current_quarter": {
      "id": 2,
      "quarter_number": 2,
      "start_date": "2023-04-01",
      "end_date": "2023-06-30",
      "status": "Active"
    },
    "competencies": [],
    "end_date": "2024-11-15"
  }
}
```

### Eliminar grupo

```
DELETE /api/v1/groups/{group_id}
```

Elimina un grupo (solo administradores). No es posible eliminar grupos con temas asignados.

**Respuesta exitosa (200 OK):**
```json
{
  "success": true,
  "message": "Group with ID 2 deleted successfully"
}
```

## Códigos de Estado

La API utiliza los siguientes códigos de estado HTTP:

- `200 OK`: La solicitud se ha completado correctamente
- `201 Created`: El recurso se ha creado correctamente
- `400 Bad Request`: La solicitud contiene datos inválidos o falta información
- `401 Unauthorized`: No se ha proporcionado autenticación o es inválida
- `403 Forbidden`: El usuario no tiene permisos para realizar la acción
- `404 Not Found`: El recurso solicitado no existe
- `409 Conflict`: Conflicto al crear o actualizar un recurso (por ejemplo, un email duplicado)
- `500 Internal Server Error`: Error interno del servidor

## Manejo de Errores

Todas las respuestas de error siguen el siguiente formato:

```json
{
  "success": false,
  "message": "Descripción del error",
  "error": {
    "type": "tipo_de_error"
  }
}
```

## Notas de Despliegue

La API puede desplegarse utilizando Docker y Docker Compose. Para iniciar la aplicación:

```bash
docker-compose up -d
```

Esto iniciará tanto el servidor de base de datos MySQL como la API. La API estará disponible en http://localhost:8000.

### Importante para el Uso con Git Bash en Windows

Si estás accediendo a la API desde Git Bash en Windows, debes usar los endpoints completos con `/api/v1/` al inicio. La aplicación incluye un middleware que corrige automáticamente las rutas que pudieran incluir el prefijo de sistema de archivos Windows (`/C:/Program Files/Git/`).

Para un funcionamiento óptimo, se recomienda:

1. Ejecutar la aplicación desde PowerShell o CMD en lugar de Git Bash.
2. Al usar herramientas como Bruno o Postman, asegúrate de usar las rutas correctas (por ejemplo, `http://localhost:8000/api/v1/auth/login`).

**Nota sobre la documentación interactiva**: Cuando accedas a la documentación interactiva desde Swagger UI (`/docs`) o ReDoc (`/redoc`), los endpoints se mostrarán correctamente. Sin embargo, si usas los botones "Try it out" en Swagger UI mientras ejecutas la aplicación desde Git Bash, es posible que los endpoints generados incluyan el prefijo de Git Bash. En este caso, el middleware corregirá automáticamente las rutas.
