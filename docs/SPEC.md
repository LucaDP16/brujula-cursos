# SPEC — Brújula

> Versión refinada de la SPEC de clase (20/10/2026). La SPEC original se versionó
> sin cambios en el primer commit del repositorio. Este documento conserva todos
> sus requisitos y resuelve sus ambigüedades.

## 1. Identidad del producto

- **Nombre propio:** Brújula
- **Una línea:** para estudiantes que quieren aprender gratis · permite armar un catálogo comunitario de cursos y un roadmap personal · porque orienta entre cientos de opciones dispersas.

## 2. Problema

Los cursos gratuitos de calidad están repartidos en decenas de plataformas y proveedores. Quien quiere aprender un tema tiene que buscar, comparar y decidir solo, sin saber si un curso es bueno, está vigente o realmente es gratuito.

Las listas existentes suelen ser estáticas, las arma una sola persona y quedan desactualizadas. Tampoco ayudan a ordenar el aprendizaje: sirven para descubrir cursos, no para planificar un camino.

Una comunidad cambia la ecuación: muchas personas aportan cursos que conocen, y otras personas los validan antes de publicarlos. El catálogo crece más rápido y con más variedad que si lo mantuviera una sola persona, y cada integrante arma su propio roadmap a partir de lo que la comunidad ya revisó.

## 3. Visión

En su mejor versión, Brújula es el lugar de referencia de la comunidad para encontrar cursos gratuitos confiables, con cientos de cursos validados de múltiples plataformas, categorías bien cubiertas y roadmaps personales que ayudan a cada estudiante a sostener su aprendizaje hasta completarlo.

## 4. Alcance

### Dentro

- Registro, login y recuperación de contraseña.
- Aporte de cursos gratuitos con sus metadatos y declaración de toma.
- Validación de aportes por usuarios con rol validador.
- Catálogo comunitario paginado, con búsqueda por texto y filtro por categoría.
- Roadmap personal con estado por curso.

### Fuera (out of scope explícito)

- Cursos de pago.
- Certificados emitidos por la plataforma.
- Aplicación móvil nativa.
- Votos, puntuaciones, comentarios o reseñas de cursos.
- Notificaciones por email sobre el estado de un aporte.
- Roadmaps compartidos o públicos entre usuarios.
- Login con cuentas externas (Google, GitHub).

## 5. Reglas de producto clave

| # | Regla | Origen |
|---|-------|--------|
| BR-001 | Un curso aportado entra "en revisión" y solo llega al catálogo comunitario tras ser validado | SPEC clase ("cursos cargados y validados pasan al catálogo") |
| BR-002 | Quien aporta un curso declara si lo tomó o no | SPEC clase |
| BR-003 | Solo un usuario con rol validador puede aprobar o rechazar aportes | decisión propia |
| BR-004 | Cada usuario ve y edita solo su propio roadmap | decisión propia |
| BR-005 | Cada curso del roadmap tiene un estado: pendiente, en curso o completado | decisión propia |

> El detalle completo de reglas vive en el PRD §7.

## 6. Stack registrado

| Capa | Tecnología | Versión exacta instalada |
|------|------------|--------------------------|
| Frontend | React + Vite + pnpm | [completar con pnpm -v] |
| Backend | FastAPI + uv | [completar con uv --version] |
| Base de datos | PostgreSQL | 18.6 |
| Infra (dev) | Docker + Docker Compose | [completar con docker --version] |

> Las versiones exactas se completan antes de entregar (ver TRD §1).

## 7. Restricciones técnicas heredadas de la SPEC de clase

| # | Restricción | Cómo la vamos a cumplir |
|---|-------------|-------------------------|
| 1 | Mobile-first con mejora progresiva | Estilos base sin media queries; `@media (min-width: …)` solo para agregar o mejorar (ver UX-UI §3) |
| 2 | Paginación de todos los listados | Paginación por offset (`page`, `page_size`) con total (ver TRD TD-002) |
| 3 | Menú sándwich en mobile | Componente de header con botón hamburguesa y panel lateral accesible (ver UX-UI §4.5) |
| 4 | Screaming Architecture | Carpetas por dominio del negocio, no por framework (ver TRD §3.2) |
| 5 | Apariencia inspirada en Google | Estética Material con tokens concretos (ver UX-UI §2) |

## 8. Vacíos de la SPEC de clase y decisiones tomadas

| # | Vacío detectado | Opciones consideradas | Decisión | Dónde se desarrolla |
|---|-----------------|-----------------------|----------|---------------------|
| 1 | ¿Quién valida los cursos aportados? | Rol validador / votos de la comunidad | Rol validador: revisa y aprueba o rechaza | PRD BR-003 |
| 2 | ¿Hay roles? | Dos roles / un solo rol | Dos roles: usuario y validador | PRD BR-003, TRD TD-001 |
| 3 | ¿Qué es "incluir en mi catálogo"? | Lista con estado / lista simple | Roadmap ordenado con estado por curso | PRD BR-005, BackendSchema §3.4 |
| 4 | ¿Cómo se busca en el catálogo? | Texto y categoría / solo categoría | Texto libre más filtro por categoría | BackendSchema §3.2 |

## 9. Historia de la SPEC

| Fecha | Cambio | Por qué |
|-------|--------|---------|
| 20/10/2026 | SPEC de clase recibida en el aula | — |
| 21/10/2026 | Primer commit: SPEC de clase tal cual | regla de la consigna |
| 23/10/2026 | Refinamiento: identidad, alcance, reglas y decisiones sobre los vacíos | resolver las ambigüedades de la SPEC de clase |
