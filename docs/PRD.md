# PRD — Product Requirements Document — Brújula

## 1. Resumen ejecutivo

Brújula es una webapp donde una comunidad de estudiantes construye en conjunto un catálogo de
cursos gratuitos. Cualquier usuario registrado aporta cursos; un validador los aprueba y recién
entonces pasan al catálogo comunitario. Desde el catálogo, cada usuario arma su roadmap
personal: una lista de cursos con estado (pendiente, en curso, completado) para sostener su
aprendizaje.

## 2. Objetivos y métricas de éxito

| Objetivo | Cómo sabemos que lo logramos |
|----------|------------------------------|
| Que el catálogo tenga cursos validados de varias plataformas y categorías | Cantidad de cursos publicados; cantidad de categorías con al menos un curso |
| Que los aportes se revisen rápido | Porcentaje de aportes revisados (aprobados o rechazados) en menos de 48 h |
| Que el roadmap se use para planificar | Cantidad de cursos por roadmap; porcentaje de cursos que pasan a "completado" |

## 3. Personas

### Persona 1 — Mariana, estudiante autodidacta

- **Contexto:** estudia programación por su cuenta, de noche, desde el celular o la notebook.
- **Motivaciones:** aprender gratis con cursos de calidad y tener un orden claro de qué estudiar.
- **Frustraciones hoy:** hay demasiados cursos dispersos; no sabe cuáles están vigentes ni si
  realmente son gratis.
- **Historias tipo:** "Quiero ver qué hay sobre bases de datos sin tener que registrarme."
  "Quiero guardar los cursos que me interesan y marcar cuáles ya terminé."

### Persona 2 — Diego, estudiante avanzado y colaborador

- **Contexto:** ya hizo varios cursos gratuitos y quiere ayudar a que otros los encuentren.
- **Motivaciones:** compartir lo que le sirvió y que la comunidad lo reconozca como fuente confiable.
- **Frustraciones hoy:** no tiene dónde dejar sus recomendaciones de forma ordenada y verificable.
- **Historias tipo:** "Quiero aportar un curso indicando que yo lo tomé."
  "Quiero saber si mi aporte fue aprobado o rechazado."

### Persona 3 — Lucía, validadora de la comunidad

- **Contexto:** integrante de confianza de la comunidad con tiempo para revisar aportes.
- **Motivaciones:** que el catálogo mantenga calidad y no tenga enlaces rotos o cursos de pago.
- **Frustraciones hoy:** revisar aportes a mano por mensajes es lento y se pierde información.
- **Historias tipo:** "Quiero ver todos los aportes pendientes en un solo lugar."

## 4. User stories

| ID | Prioridad | Historia | Criterios de aceptación |
|----|-----------|----------|-------------------------|
| US-001 | P0 | Como visitante, quiero ver el catálogo comunitario paginado y buscar por texto y categoría, para descubrir cursos sin registrarme | - [ ] Dado que visito `/catalogo`, veo la primera página (20 cursos)<br>- [ ] Dado que hay más de 20 cursos, aparece el paginador<br>- [ ] Dado que busco un texto, solo veo cursos publicados que coinciden<br>- [ ] Dado que filtro por categoría, solo veo cursos de esa categoría<br>- [ ] Dado que no hay resultados, veo un empty state |
| US-002 | P0 | Como persona nueva, quiero registrarme e iniciar sesión, para usar las funciones de comunidad | - [ ] Dado un email nuevo y una contraseña válida, el registro crea mi cuenta<br>- [ ] Dado un email ya registrado, veo un error claro<br>- [ ] Dado credenciales correctas, inicio sesión<br>- [ ] Dado credenciales incorrectas, veo un error sin revelar cuál dato falló<br>- [ ] Dado demasiados intentos fallidos, el login se bloquea temporalmente |
| US-003 | P0 | Como usuario, quiero aportar un curso gratuito con sus datos, para enriquecer el catálogo | - [ ] Dado que completo todos los campos obligatorios, el aporte se crea en estado "en revisión"<br>- [ ] Dado que declaro si tomé el curso o no, esa declaración queda guardada<br>- [ ] Dado una URL ya aportada, veo un error de duplicado<br>- [ ] Dado un campo inválido, veo el error junto al campo |
| US-004 | P0 | Como validador, quiero revisar los aportes pendientes y aprobarlos o rechazarlos, para mantener la calidad del catálogo | - [ ] Dado que soy validador, veo la lista paginada de aportes en revisión<br>- [ ] Dado que apruebo un aporte, pasa a "publicado" y aparece en el catálogo<br>- [ ] Dado que rechazo un aporte, indico un motivo y pasa a "rechazado"<br>- [ ] Dado que no soy validador, no puedo acceder a esta pantalla |
| US-005 | P0 | Como usuario, quiero agregar un curso del catálogo a mi roadmap, para planificar mi aprendizaje | - [ ] Dado un curso publicado, puedo agregarlo a mi roadmap con estado "pendiente"<br>- [ ] Dado un curso que ya tengo, veo que ya está incluido<br>- [ ] Dado que no tengo sesión, se me pide iniciar sesión |
| US-006 | P0 | Como usuario, quiero ver mi roadmap y cambiar el estado de cada curso, para seguir mi avance | - [ ] Dado que abro `/roadmap`, veo mis cursos paginados con su estado<br>- [ ] Dado que cambio el estado de un curso, el cambio se guarda<br>- [ ] Dado que quito un curso, desaparece de mi roadmap<br>- [ ] Dado que no tengo cursos, veo un empty state con enlace al catálogo |
| US-007 | P1 | Como usuario, quiero recuperar mi contraseña, para volver a entrar si la olvido | - [ ] Dado que ingreso mi email, se genera un token de un solo uso con vencimiento<br>- [ ] Dado un token válido, puedo definir una contraseña nueva<br>- [ ] Dado un token vencido o ya usado, veo un error |
| US-008 | P1 | Como usuario, quiero ver el estado de mis aportes, para saber si fueron aprobados | - [ ] Dado que abro "mis aportes", veo la lista paginada con su estado<br>- [ ] Dado un aporte rechazado, veo el motivo |
| US-009 | P1 | Como visitante, quiero ver el detalle de un curso, para decidir si me sirve | - [ ] Dado un curso publicado, veo todos sus datos<br>- [ ] Dado un curso inexistente o no publicado, veo un error 404 |

## 5. Requisitos funcionales

| ID | Requisito | Historia relacionada |
|----|-----------|----------------------|
| RF-001 | Registro de usuarios con email, nombre y contraseña | US-002 |
| RF-002 | Login con rate limiting y contraseñas cifradas con bcrypt | US-002 |
| RF-003 | Recuperación de contraseña: solicitud, token y nueva contraseña | US-007 |
| RF-004 | Alta de aportes con todos los metadatos y declaración de toma | US-003 |
| RF-005 | Validación de aportes por usuarios con rol validador | US-004 |
| RF-006 | Catálogo público paginado con búsqueda por texto y filtro por categoría | US-001 |
| RF-007 | Detalle de curso publicado | US-009 |
| RF-008 | Roadmap personal: agregar, listar, cambiar estado y quitar cursos | US-005, US-006 |
| RF-009 | Listado de mis aportes con su estado | US-008 |

## 6. Requisitos no funcionales

| ID | Requisito | Notas |
|----|-----------|-------|
| RNF-001 | Todos los listados paginados | heredado de la SPEC de clase |
| RNF-002 | Mobile-first con mejora progresiva | heredado de la SPEC de clase |
| RNF-003 | Menú sándwich en mobile | heredado de la SPEC de clase |
| RNF-004 | Screaming Architecture | heredado de la SPEC de clase |
| RNF-005 | El catálogo responde en menos de 2 s con 1.000 cursos | decisión propia |
| RNF-006 | Contraseñas nunca almacenadas ni registradas en claro | decisión propia |

## 7. Reglas de negocio

| ID | Regla | Fuente |
|----|-------|--------|
| BR-001 | Un curso aportado no aparece en el catálogo hasta ser validado | SPEC clase |
| BR-002 | Quien aporta un curso declara si lo tomó o no | SPEC clase |
| BR-003 | Solo un usuario con rol validador puede aprobar o rechazar aportes | decisión propia |
| BR-004 | Cada usuario ve y edita solo su propio roadmap | decisión propia |
| BR-005 | Cada curso del roadmap tiene un estado: pendiente, en curso o completado | decisión propia |
| BR-006 | No se puede aportar dos veces la misma URL de curso | decisión propia |
| BR-007 | Un validador no puede validar sus propios aportes | decisión propia |
| BR-008 | Un curso solo puede estar una vez en un mismo roadmap | decisión propia |
| BR-009 | Un aporte rechazado debe tener un motivo | decisión propia |

## 8. Fuera de alcance (out of scope)

- Cursos de pago.
- Certificados emitidos por la plataforma.
- Aplicación móvil nativa.
- Votos, puntuaciones, comentarios y reseñas de cursos.
- Notificaciones por email sobre el estado de un aporte.
- Roadmaps compartidos o públicos.
- Login con cuentas externas (Google, GitHub).
- Edición de un aporte después de enviado.

## 9. Supuestos y decisiones sobre la SPEC de clase

Las decisiones sobre quién valida, los roles, el significado de "incluir en mi catálogo" y la
búsqueda están justificadas en `docs/SPEC.md` §8. Supuestos adicionales:

- El primer validador se crea por seed (datos iniciales), no desde la interfaz.
- El envío de email de recuperación se simula en desarrollo: el enlace con el token se muestra
  en los logs del backend.

## 10. Preguntas abiertas

| Pregunta | Impacto | Estado |
|----------|---------|--------|
| ¿Los aportes sin revisar vencen después de un tiempo? | bajo | abierta: se evalúa tras el MVP |
| ¿Se puede reenviar un aporte rechazado corregido? | medio | decidida: fuera de alcance de esta versión (se aporta uno nuevo) |