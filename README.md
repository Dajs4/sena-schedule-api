# SENA Schedule API
API para gestión de horarios del SENA desarrollada con FastAPI y SQLAlchemy.

## Requisitos

- Python 3.9+
- PostgreSQL (o la base de datos que estés usando)
- Docker (opcional)

## Instalación

1. Clonar el repositorio:
git clone https://github.com/Dajs4/sena-schedule-api.git
cd sena-schedule-api
Copiar
2. Crear entorno virtual:
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
Copiar
3. Instalar dependencias:
pip install -r requirements.txt
pip install -r requirements-dev.txt  # Para desarrollo
Copiar
4. Configurar variables de entorno:
cp .env.example .env
Edita el archivo .env con tus configuraciones
Copiar
5. Inicializar la base de datos:
python scripts/init_db.py
Copiar
6. Ejecutar migraciones:
alembic upgrade head
Copiar
7. Iniciar servidor de desarrollo:
uvicorn app.main --reload
Copiar
## Desarrollo

### Instalación de herramientas de desarrollo
pre-commit install
Copiar
### Ejecutar pruebas
pytest
Copiar
### Convenciones de código

Este proyecto utiliza:
- Black para formateo de código
- Flake8 para linting
- isort para ordenar imports
- mypy para verificación de tipos

## Estructura del Proyecto

sena-schedule-api/
├── app/
│   ├── __init__.py
│   ├── main.py                # Punto de entrada de la aplicación
│   ├── core/                  # Configuraciones centrales
│   │   ├── __init__.py
│   │   ├── config.py          # Configuraciones de la aplicación
│   │   ├── database.py        # Configuración de la base de datos
│   │   ├── security.py        # Funciones de seguridad (JWT)
│   │   └── exceptions.py      # Manejo de excepciones personalizado
│   ├── models/                # Modelos SQLAlchemy
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── campus.py
│   │   ├── classroom.py
│   │   ├── instructor.py
│   │   ├── timeblock.py
│   │   ├── training_program.py
│   │   ├── group.py
│   │   ├── competency.py
│   │   ├── learning_outcome.py
│   │   ├── quarter.py
│   │   └── topic.py
│   ├── schemas/               # Esquemas Pydantic
│   │   ├── __init__.py
│   │   ├── common.py
│   │   ├── user.py
│   │   ├── campus.py
│   │   ├── classroom.py
│   │   ├── instructor.py
│   │   ├── timeblock.py
│   │   ├── training_program.py
│   │   ├── group.py
│   │   ├── competency.py
│   │   ├── learning_outcome.py
│   │   ├── quarter.py
│   │   └── topic.py
│   ├── controllers/           # Lógica de negocio
│   │   ├── __init__.py
│   │   ├── auth.py
│   │   ├── campus.py
│   │   ├── classroom.py
│   │   ├── instructor.py
│   │   ├── timeblock.py
│   │   ├── training_program.py
│   │   ├── group.py
│   │   ├── competency.py
│   │   ├── learning_outcome.py
│   │   ├── quarter.py
│   │   └── topic.py
│   ├── operations/            # Operaciones de la base de datos
│   │   ├── __init__.py
│   │   ├── user.py
│   │   ├── campus.py
│   │   ├── classroom.py
│   │   ├── instructor.py
│   │   ├── timeblock.py
│   │   ├── training_program.py
│   │   ├── group.py
│   │   ├── competency.py
│   │   ├── learning_outcome.py
│   │   ├── quarter.py
│   │   └── topic.py
│   └── routers/               # Rutas de FastAPI
│       ├── __init__.py
│       ├── auth.py
│       ├── campus.py
│       ├── classroom.py
│       ├── instructor.py
│       ├── timeblock.py
│       ├── training_program.py
│       ├── group.py
│       ├── competency.py
│       ├── learning_outcome.py
│       ├── quarter.py
│       └── topic.py
├── tests/                     # Pruebas automatizadas
│   ├── __init__.py
│   ├── conftest.py            # Configuración para pruebas
│   ├── test_auth.py
│   ├── test_instructor.py
│   └── test_group.py
├── scripts/                   # Scripts útiles
│   └── init_db.py             # Script para inicializar la base de datos
├── .env                       # Variables de entorno (no incluido en control de versiones)
├── .env.example               # Ejemplo de variables de entorno
├── .gitignore                 # Archivos a ignorar en Git
├── Dockerfile                 # Definición para contenerizar la aplicación
├── docker-compose.yml         # Configuración de Docker Compose
├── requirements.txt           # Dependencias del proyecto
├── requirements-dev.txt       # Dependencias de desarrollo (linters, etc.)
├── pyproject.toml             # Configuración de herramientas (Black, isort)
├── setup.cfg                  # Configuración de Flake8
├── .pre-commit-config.yaml    # Configuración de pre-commit hooks
├── mypy.ini                   # Configuración de MyPy
├── .github/                   # Configuración de GitHub
│   └── workflows/
│       ├── ci.yml             # Workflow de Integración Continua
│       └── release.yml        # Workflow de Releases
├── alembic/                   # Migraciones de base de datos
└── README.md                  # Documentación del proyecto
