# Flujo de Git — Brújula

## Modelo elegido: GitHub Flow

`main` siempre contiene el estado estable y revisable del repositorio.
Todo cambio se desarrolla en una rama corta que sale de `main` y vuelve a `main` mediante merge.

**Por qué GitHub Flow:** es un trabajo individual, de plazos cortos y con un solo flujo
(documentos primero, código después). Git Flow agrega ramas `develop` y `release` que no
aportan nada en este contexto; GitHub Flow mantiene lo mínimo necesario y deja la historia legible.

## Ramas

| Rama | Propósito | Ejemplo |
|------|-----------|---------|
| `main` | Estado estable. Nunca se commitea directo. | — |
| `docs/<documento>` | Un documento de la Instancia A | `docs/prd`, `docs/trd` |
| `chore/<tarea>` | Configuración y mantenimiento | `chore/gitignore` |
| `feat/<tarea>` | Funcionalidad nueva (Instancia B) | `feat/auth-login` |
| `fix/<tarea>` | Corrección de errores | `fix/pagination-offset` |

Regla: una tarea = una rama. Nombres en minúsculas, con guiones, formato `tipo/descripcion`.

## Cómo se mergea

1. Crear la rama desde `main` actualizado.
2. Commitear en la rama con mensajes convencionales.
3. Volver a `main` y mergear con `git merge --no-ff`, que fuerza un commit de merge
   y deja visible en el historial que el trabajo pasó por una rama.
4. Subir `main` y la rama con `git push`.

Las ramas mergeadas no se borran, para que la historia quede auditable.

## Convención de commits

Conventional Commits en el 100% de la historia: `tipo: descripción en presente`.

Tipos usados: `docs`, `feat`, `fix`, `chore`, `refactor`, `test`, `docker`.

Los mensajes son específicos:
- Correcto: `docs: add PRD user stories`
- Incorrecto: `docs: cambios`, `wip`

## Gate de aprobación

No se versiona código fuente de la aplicación hasta recibir el correo de aprobación
de la Instancia A. Hasta entonces el repositorio contiene solo documentación,
`README.md` y `.gitignore`.