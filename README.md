# Brújula

> WebApp de gestión de comunidad de cursos: catálogo comunitario de cursos gratuitos
> + roadmaps personales de aprendizaje.
>
> **Estado:** 📚 Documentación (Instancia A) — desarrollo pendiente de aprobación docente.

**Stack:** React 19.3.1 + Vite 8.3.5 + pnpm 12.15.0 · FastAPI 0.162.0 + uv 0.12.1 · PostgreSQL 18.6 · Docker Compose (dev)

---

## Qué hace

Brújula es para estudiantes que quieren aprender gratis sin perderse entre cientos de plataformas.
Cualquier persona registrada puede aportar un curso gratuito de cualquier proveedor, indicando si
ella misma lo tomó. Un usuario con rol validador revisa el aporte y, si lo aprueba, el curso entra
al catálogo comunitario. Desde ahí, cada usuario arma su roadmap personal y marca el avance de
cada curso (pendiente, en curso, completado).

## Documentación

| Documento | Responde la pregunta | Link |
|-----------|----------------------|------|
| SPEC | ¿Qué nos pidieron construir y qué decidimos nosotros? | [docs/SPEC.md](docs/SPEC.md) |
| PRD | ¿Para quién, qué problema resuelve, qué hace y qué NO hace? | [docs/PRD.md](docs/PRD.md) |
| TRD | ¿Cómo lo construimos técnicamente y por qué? | [docs/TRD.md](docs/TRD.md) |
| UX-UI | ¿Cómo se ve, se siente y se comporta? | [docs/UX-UI.md](docs/UX-UI.md) |
| APPFlow | ¿Qué pantallas hay y cómo se navega? | [docs/APPFlow.md](docs/APPFlow.md) |
| BackendSchema | ¿Qué datos persistimos y qué API expone? | [docs/BackendSchema.md](docs/BackendSchema.md) |
| Implementation Plan | ¿En qué orden construimos y qué significa "hecho"? | [docs/IMPLEMENTATION_PLAN.md](docs/IMPLEMENTATION_PLAN.md) |
| Flujo de Git | ¿Cómo se organizan ramas, merges y commits? | [docs/GIT_WORKFLOW.md](docs/GIT_WORKFLOW.md) |

## Cómo revisar este repo

- Los `.md` se renderizan solos en GitHub.
- Los diagramas Mermaid (`erDiagram`, `flowchart`, `stateDiagram`) también se renderizan en GitHub.
- El historial muestra el flujo: la SPEC original es el primer commit, y cada documento entra por
  su propia rama (`docs/...`) mergeada a `main` con `--no-ff`.

## Estado del trabajo

| Instancia | Contenido | Estado |
|-----------|-----------|--------|
| A — Documentación | repo + 7 documentos | ✅ enviada a aprobación el 23/10/2026 |
| B — Desarrollo del MVP | ejecución del Implementation Plan | ⏳ pendiente de aprobación |
| C — Clase 27/10 | testing, Docker multi-stage, gestión del cambio | ⏳ |

## Versionado

- **Flujo de Git:** GitHub Flow. `main` siempre estable y nunca se commitea directo; una rama por
  tarea (`docs/<documento>`, `chore/<tarea>`, `feat/<tarea>`) que se mergea con `git merge --no-ff`.
  Detalle en [docs/GIT_WORKFLOW.md](docs/GIT_WORKFLOW.md).
- **Convención de commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `test:`, `docker:`, `chore:`).

---

Trabajo Práctico — Desarrollo de Software, UTN FR La Plata, 2026.