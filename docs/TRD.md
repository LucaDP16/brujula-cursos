# TRD — Technical Requirements Document — Brújula

## 1. Stack y versiones exactas

Versiones verificadas el 23/10/2026 con `--version` / `npm view` / PyPI.

| Capa | Elección | Versión exacta | Por qué |
|------|----------|----------------|---------|
| Frontend | React + Vite + pnpm | React 19.3.1 · Vite 8.3.5 · pnpm 12.15.0 | Stack fijado por la SPEC; Vite da un dev server rápido y pnpm instala eficiente |
| Backend | FastAPI + uv | FastAPI 0.162.0 · uv 0.12.1 | Stack fijado por la SPEC; FastAPI valida con tipos y genera docs OpenAPI |
| ORM / driver | SQLAlchemy 2.x + Alembic + psycopg 3 | última estable al momento de instalar (se registra en la Instancia B) | Mapeo de entidades y sesiones con migraciones versionadas |
| BD | PostgreSQL | 18.6 | Fijado por la SPEC |
| Runtime frontend | Node.js | v24.21.0 | Es el instalado en la máquina de desarrollo |
| Infra (dev) | Docker + Compose | Docker 29.5.3 | Stack fijado por la SPEC |

> La SPEC de clase citaba FastAPI `0.158.2` como referencia del 21/10/2026. Se registra
> `0.162.0` porque la SPEC pide "última stable" y la versión exacta que se instala.
> La versión de Python la resuelve `uv` al crear el proyecto; se registra en la Instancia B.

## 2. Decisiones técnicas

| # | Decisión | Alternativas consideradas | Elegida | Por qué |
|---|----------|---------------------------|---------|---------|
| TD-001 | Autenticación | JWT stateless / sesión en servidor / OAuth externo | JWT en `Authorization: Bearer`, contraseñas con bcrypt | Sin estado en el servidor, simple para una SPA. OAuth está fuera de alcance (PRD §8). bcrypt cumple el mínimo de la SPEC |
| TD-002 | Paginación | offset + total / cursor | offset + total (`page`, `page_size`, máximo 50) | La UI necesita mostrar "página X de N"; con volúmenes de cientos o miles de cursos el costo del offset es aceptable |
| TD-003 | Formato de errores | envelope propio / RFC 7807 | `{ "error": { "code", "message" } }` | Más simple de consumir desde el frontend; ya está usado en BackendSchema §3 |
| TD-004 | Modelado del roadmap | tabla intermedia con estado / referencia directa | tabla `roadmap_items` con estado y `UNIQUE (usuario_id, curso_id)` | Permite estado por curso y hace cumplir BR-008 en la base |
| TD-005 | Rate limit del login | contador en memoria / tabla en Postgres / librería externa | contador en memoria del proceso por IP + email: 5 intentos fallidos en 15 minutos → 429 | Una sola instancia en desarrollo; evita dependencias. Limitación registrada en §6 |
| TD-006 | ORM | SQLAlchemy / SQLModel | SQLAlchemy 2.x + Alembic | Mapeo explícito y migraciones versionadas; separa bien entidades de los esquemas de la API |
| TD-007 | Recuperación de contraseña | token guardado en claro / token hasheado | token aleatorio, se guarda su hash, vence a los 30 minutos y se usa una sola vez | Si se filtra la base los tokens no sirven. En desarrollo el enlace se imprime en los logs (no hay servidor de email, PRD §9) |

## 3. Arquitectura

### 3.1 Patrón y capas

Screaming Architecture + arquitectura en capas:

```mermaid
flowchart LR
    HTTP[HTTP / Routers] --> SVC[Services — lógica de negocio]
    SVC --> REPO[Repositories — acceso a datos]
    REPO --> DB[(PostgreSQL)]
    SVC --> MODELS[Models / entidades]
```

- **Routers:** reciben HTTP, validan el input y delegan. Sin lógica de negocio.
- **Services:** reglas de negocio puras y testeables (sin BD ni HTTP). Acá viven BR-003 (solo validador), BR-007 (no validar lo propio) y BR-009 (rechazo con motivo).
- **Repositories:** única puerta a la BD. Sin reglas de negocio.
- **La estructura de carpetas habla del dominio** (cursos, aportes, roadmap), no del framework.

### 3.2 Estructura de carpetas (se crea en la Instancia B)

```
brujula-cursos/
├── backend/
│   ├── pyproject.toml
│   ├── Dockerfile
│   ├── app/
│   │   ├── main.py
│   │   ├── config.py
│   │   ├── identidad/          # registro, login, recuperación, roles
│   │   │   ├── router.py
│   │   │   ├── service.py
│   │   │   ├── repository.py
│   │   │   └── models.py
│   │   ├── catalogo/           # cursos publicados y categorías
│   │   ├── aportes/            # aportar, mis aportes, validación
│   │   └── roadmap/            # roadmap personal
│   ├── migrations/             # Alembic
│   └── tests/
├── frontend/
│   ├── package.json
│   ├── Dockerfile
│   └── src/
│       ├── main.jsx
│       ├── app/                # shell, router, layout global
│       ├── features/
│       │   ├── identidad/      # registro, login, recuperación
│       │   ├── catalogo/       # listado, detalle
│       │   ├── aportes/        # formulario, mis aportes, panel de validación
│       │   └── roadmap/
│       │       ├── components/
│       │       ├── hooks/
│       │       └── api.js
│       └── styles/             # tokens globales (de UX-UI)
├── docker-compose.yml
└── docs/
```

### 3.3 Servicios Docker (entorno de desarrollo)

| Servicio | Imagen/base | Puerto | Rol |
|----------|-------------|--------|-----|
| postgres | postgres:18.6 | 5432 | base de datos |
| backend | python:slim + uv | 8000 | API FastAPI (dev) |
| frontend | node:24-alpine + pnpm | 5173 | SPA Vite (dev server) |

```mermaid
flowchart LR
    BROWSER[Navegador :5173] --> FE[frontend Vite]
    FE -- "/api/*" --> BE[backend FastAPI :8000]
    BE --> PG[(postgres :5432)]
```

> En la clase del 27/10 estos Dockerfiles evolucionan a multi-stage. Por ahora, lo mínimo que levanta.

## 4. Convenciones

- **Commits:** Conventional Commits — `feat:`, `fix:`, `docs:`, `test:`, `refactor:`, `chore:`, `docker:`
- **Flujo de Git:** GitHub Flow con una rama por tarea y merge `--no-ff` a `main`. Detalle completo en [`docs/GIT_WORKFLOW.md`](./GIT_WORKFLOW.md)
- **Errores de API:** envelope propio (TD-003). Ejemplo:

  ```json
  { "error": { "code": "url_duplicada", "message": "Ya existe un curso con esa URL" } }
  ```

- **Variables de entorno:** nunca se versionan; el repo incluye un `.env.example` sin secretos.

  | Variable | Ejemplo | Obligatoria | Descripción |
  |----------|---------|-------------|-------------|
  | DATABASE_URL | postgresql+psycopg://brujula:CAMBIAR@postgres:5432/brujula | sí | conexión a la base |
  | JWT_SECRET | (valor largo y aleatorio) | sí | firma de los tokens; nunca en el repo |
  | JWT_EXPIRE_MINUTES | 60 | no | vigencia del token de acceso |
  | LOGIN_MAX_ATTEMPTS | 5 | no | intentos fallidos antes de bloquear |
  | LOGIN_WINDOW_MINUTES | 15 | no | ventana del rate limit |
  | VITE_API_URL | http://localhost:8000 | sí | URL de la API para el frontend |

- **Logging y excepciones:** se registran las peticiones y los errores 5xx. Las excepciones de negocio se lanzan en los services y un manejador global las convierte al envelope de errores. Nunca se loguean contraseñas ni tokens de acceso.

## 5. Requisitos no funcionales técnicos

| Restricción (SPEC) | Cómo se cumple | Dónde se verifica |
|--------------------|----------------|-------------------|
| Paginación de listados | TD-002: offset + total en todos los listados | `GET /api/v1/cursos`, `/aportes/mios`, `/aportes/pendientes`, `/roadmap` |
| Mobile-first con mejora progresiva | base sin media queries y `min-width` solo para mejorar (UX-UI §3) | componentes de UX-UI §4 |
| Menú sándwich | componente de header con ARIA (UX-UI §4.5) | APPFlow, pantallas con header |
| Screaming Architecture | árbol de §3.2 | revisión del repo |
| Login con rate limiting y bcrypt | TD-001 y TD-005 | `POST /api/v1/auth/login` (429 al exceder el límite) |

## 6. Riesgos técnicos

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|--------------|---------|------------|
| El rate limit en memoria se pierde al reiniciar el backend y no funciona con varias instancias | media | bajo | Aceptado para desarrollo con una instancia; migrar a Postgres o Redis si se escala |
| El formulario de aporte tiene muchos campos y su validación lleva más tiempo del estimado | media | medio | Un único esquema de validación en el backend y mensajes de error uniformes |
| Las versiones de React 19 / Vite 8 son muy recientes y pueden traer cambios | baja | medio | Fijar versiones exactas en `package.json` y en `pyproject.toml` y no actualizar durante el MVP |
| Un secreto (`JWT_SECRET`, contraseñas) termina en el repo | baja | alto | `.env` en `.gitignore`, solo `.env.example` versionado, revisar `git status` antes de cada commit |