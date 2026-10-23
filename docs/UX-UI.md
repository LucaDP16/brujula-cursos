# UX-UI — Guía visual de estilos — Brújula

> Inspiración fijada por la SPEC: apariencia y experiencia similares a Google (estética Material:
> limpieza, espacio en blanco, azul primario, sombras sutiles, radios amplios). Los valores de
> esta guía son concretos y son los que se usan al construir cada pantalla.

## 1. Principios

1. **Mobile primero:** el layout base se diseña para 360 px y se mejora hacia arriba, nunca se repara.
2. **Orientar, no abrumar:** pocas acciones por pantalla; una sola acción principal en azul.
3. **El estado vacío, el de carga y el de error son parte del diseño**, no un agregado final.
4. **El estado de un curso se entiende de un vistazo:** cada estado tiene color, texto e ícono (nunca solo color).

## 2. Design tokens

### 2.1 Paleta de colores

| Token | Valor | Uso | Contraste AA (medido) |
|-------|-------|-----|-----------------------|
| `--color-primary` | `#1a73e8` | botones primarios, links, foco | 4.5:1 sobre blanco ✓ (límite AA) |
| `--color-primary-dark` | `#185abc` | hover y press del primario | — |
| `--color-primary-container` | `#e8f0fe` | chips activos, fila seleccionada | — |
| `--color-on-primary` | `#ffffff` | texto sobre el primario | 4.5:1 sobre `#1a73e8` ✓ (límite AA) |
| `--color-surface` | `#ffffff` | cards, header, menú | — |
| `--color-background` | `#f8f9fa` | fondo de página | — |
| `--color-text` | `#202124` | texto principal | 15.27:1 sobre `#f8f9fa` ✓ |
| `--color-text-muted` | `#5f6368` | metadatos, texto secundario | 6.04:1 sobre blanco ✓ |
| `--color-border` | `#dadce0` | bordes de inputs y divisores | decorativo; el foco entrega el 3:1 |
| `--color-hover-grey` | `#f1f3f4` | hover de filas y botones secundarios | — |
| `--color-success` | `#188038` | curso publicado, completado, toast de éxito | 5.01:1 sobre blanco ✓ |
| `--color-warning` | `#f9ab00` | fondo del badge "en revisión" | usar siempre con texto oscuro `#202124` |
| `--color-error` | `#d93025` | errores, curso rechazado | 4.77:1 sobre blanco ✓ |

> Ratios verificados el 23/10/2026 en https://webaim.org/resources/contrastchecker/.
> El amarillo `#f9ab00` no tiene contraste suficiente como color de texto sobre blanco: solo se
> usa como fondo con texto `#202124`.
> El azul primario cumple AA con el mínimo exacto (4.5:1), sin margen: no se debe aclarar.

### 2.2 Tipografía

| Token | Familia | Tamaño / line-height | Uso |
|-------|---------|----------------------|-----|
| `--font-family-base` | Roboto, system-ui, sans-serif | — | todo el texto |
| `--text-sm` | | 14px / 20px | metadatos, badges, tablas |
| `--text-base` | | 16px / 24px | cuerpo y formularios |
| `--text-lg` | | 18px / 24px | títulos de card |
| `--text-xl` | | 22px / 28px | títulos de pantalla |

Roboto se carga desde Google Fonts con `font-display: swap`, pesos 400 y 500.

### 2.3 Espaciado, radios, sombras y capas

| Token | Valor | Uso |
|-------|-------|-----|
| `--space-1` … `--space-8` | 4, 8, 12, 16, 24, 32, 40, 48 px | ritmo de 4 px en todo el layout |
| `--radius-input` | 8px | inputs y selects |
| `--radius-card` | 12px | cards de curso |
| `--radius-button` | 18px | botones (pill) |
| `--radius-modal` | 28px | modales |
| `--shadow-1` | `0 1px 2px rgba(60,64,67,.1), 0 1px 3px 1px rgba(60,64,67,.15)` | cards en reposo |
| `--shadow-2` | `0 1px 2px rgba(60,64,67,.1), 0 2px 6px 2px rgba(60,64,67,.15)` | hover de card, menú, modal |
| `--z-header` | 100 | header sticky |
| `--z-drawer` | 200 | menú sándwich y su overlay |
| `--z-modal` | 300 | modales |
| `--z-toast` | 400 | notificaciones |

### 2.4 Bloque de variables

```css
:root {
  --color-primary: #1a73e8;
  --color-primary-dark: #185abc;
  --color-primary-container: #e8f0fe;
  --color-on-primary: #ffffff;
  --color-surface: #ffffff;
  --color-background: #f8f9fa;
  --color-text: #202124;
  --color-text-muted: #5f6368;
  --color-border: #dadce0;
  --color-hover-grey: #f1f3f4;
  --color-success: #188038;
  --color-warning: #f9ab00;
  --color-error: #d93025;

  --font-family-base: "Roboto", system-ui, sans-serif;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.375rem;

  --space-1: 4px;  --space-2: 8px;  --space-3: 12px; --space-4: 16px;
  --space-5: 24px; --space-6: 32px; --space-7: 40px; --space-8: 48px;

  --radius-input: 8px;
  --radius-card: 12px;
  --radius-button: 18px;
  --radius-modal: 28px;

  --shadow-1: 0 1px 2px rgba(60,64,67,.1), 0 1px 3px 1px rgba(60,64,67,.15);
  --shadow-2: 0 1px 2px rgba(60,64,67,.1), 0 2px 6px 2px rgba(60,64,67,.15);

  --z-header: 100;
  --z-drawer: 200;
  --z-modal: 300;
  --z-toast: 400;
}
```

## 3. Estrategia mobile-first con mejora progresiva

La base no usa media queries. `@media (min-width: …)` solo agrega o mejora; nunca arregla lo base.

| Capa | Dónde aplica | Qué define |
|------|--------------|------------|
| **Base (sin media queries)** | todos los viewports | 1 columna; header con menú sándwich; cards apiladas; listas del roadmap y del panel como cards; tipografía mobile; targets táctiles ≥ 44 px |
| `@media (min-width: 768px)` | tablets | grid de 2 columnas en el catálogo; listados como tabla real; mayor padding de página |
| `@media (min-width: 1024px)` | escritorio | grid de 3 columnas; navegación horizontal en el header y menú sándwich oculto; contenedor con `max-width: 1140px` |

## 4. Componentes (anatomía y estados)

### 4.1 Botón

- **Primario:** fondo `--color-primary`, texto `--color-on-primary`, forma pill (`--radius-button`), padding `11px 23px`, Roboto 500 14px. Hover: `--color-primary-dark` más `--shadow-1`.
- **Secundario:** sin fondo, texto `--color-primary`; hover con fondo `--color-hover-grey`.
- **Peligro:** texto `--color-error`; "Rechazar" y "Quitar" piden confirmación o motivo antes de ejecutar.
- **Estados de todos:** `default`, `hover`, `focus-visible` (anillo de 2px `--color-primary` con offset de 2px), `active`, `disabled` (opacidad .38), `loading` (spinner y deshabilitado).

### 4.2 Campo de formulario (input, select, textarea)

- Borde de 1px `--color-border`, radio `--radius-input`, label siempre visible arriba del campo.
- **Estados:** `default`, `focus` (borde de 2px `--color-primary`), `filled`, `error` (borde y mensaje en `--color-error`, mensaje asociado con `aria-describedby`), `disabled` (fondo `--color-hover-grey`).
- Los errores se muestran junto al campo y nunca borran lo que el usuario escribió.

### 4.3 Card de curso

- Superficie `--color-surface`, radio `--radius-card`, `--shadow-1`, padding `--space-4`.
- Contenido: título (`--text-lg`), categoría como chip (`--color-primary-container`), tema, docentes en `--text-sm` muted, y badges de cuestionarios, examen, certificado y aval.
- **Estados:** `default`, `hover` (`--shadow-2`), `en mi roadmap` (borde `--color-primary` más ícono de check).

### 4.4 Badge de estado

Todo estado se comunica con color, texto e ícono; nunca solo con color.

| Estado | Fondo | Texto | Ícono |
|--------|-------|-------|-------|
| `en_revision` | `--color-warning` | `#202124` | `schedule` |
| `publicado` | `#e6f4ea` | `--color-success` | `check_circle` |
| `rechazado` | `#fce8e6` | `--color-error` | `cancel` |
| `pendiente` (roadmap) | `--color-hover-grey` | `--color-text` | `radio_button_unchecked` |
| `en_curso` (roadmap) | `--color-primary-container` | `--color-primary-dark` | `play_circle` |
| `completado` (roadmap) | `#e6f4ea` | `--color-success` | `task_alt` |

### 4.5 Lista paginada

- Desde 768 px: tabla real con divisores `--color-border` y hover `--color-hover-grey`.
- Menos de 768 px: las mismas filas como cards apiladas (mismos datos, otro layout; sin scroll horizontal).
- Paginador "‹ 1 2 3 … ›" centrado; la página actual en `--color-primary-container`.
- Se usa en catálogo, mis aportes, roadmap y panel de validación (todas las consultas de listado están paginadas).
- Vacía: muestra el empty state de §4.9.

### 4.6 Menú sándwich

- Botón hamburguesa (ícono `menu`) a la izquierda del header, visible por debajo de 1024 px.
- Panel de 280 px de ancho con `--shadow-2`, sobre un overlay `rgba(32,33,34,.6)`.
- Ítems de 48 px de alto; el activo con fondo `--color-primary-container`.
- **Estados:** cerrado y abierto. **Cierre:** tocar un link, botón X, tecla `ESC` o clic en el overlay.
- ARIA: `aria-expanded` y `aria-controls` en el botón; el foco queda dentro del panel mientras está abierto y vuelve al botón al cerrar.

### 4.7 Modal

Se usa para el motivo de rechazo en el panel de validación.

- Superficie blanca, radio `--radius-modal`, `--shadow-2`, overlay igual al del menú.
- Foco atrapado dentro del modal; cierre con `ESC` y botón X.
- Acciones a la derecha: "Cancelar" (secundario) y "Rechazar" (primario).

### 4.8 Toast

- Fondo `#323232`, texto blanco, radio 8px, ancho mínimo 344 px, `role="status"`.
- Tipos: éxito, error e info. Duración de 4 a 6 segundos.
- Posición: inferior centrado en mobile e inferior izquierda en escritorio.

### 4.9 Skeleton y empty state

- **Skeleton:** bloques `--color-hover-grey` con efecto shimmer mientras cargan listados y detalle.
- **Empty state:** ícono grande muted, título `--text-lg`, una línea de explicación y una acción. Textos concretos:
  - Catálogo sin resultados: "No encontramos cursos con esos filtros" y botón "Limpiar filtros".
  - Roadmap vacío: "Todavía no agregaste cursos" y botón "Explorar el catálogo".
  - Mis aportes vacío: "Todavía no aportaste cursos" y botón "Aportar el primero".
  - Panel de validación vacío: "No hay aportes pendientes".

## 5. Layout y navegación

- **Header:** sticky (`--z-header`), fondo blanco, alto 64 px. En mobile: hamburguesa e ícono `explore` con el nombre "Brújula". En escritorio: logo, navegación horizontal (Catálogo, Mi roadmap, Aportar) y a la derecha "Ingresar" o el nombre del usuario.
- **Opción "Validar":** solo aparece en la navegación si el usuario tiene rol validador.
- **Contenedor:** `max-width: 1140px`, padding lateral `--space-4` en mobile y `--space-5` en escritorio.
- **Pantallas:** el detalle de cada una vive en `docs/APPFlow.md`; acá está el sistema visual con el que se construyen.

## 6. Accesibilidad mínima

- [x] Texto con contraste AA (≥ 4.5:1); ratios verificados en WebAIM el 23/10/2026 (§2.1).
- [ ] `:focus-visible` visible en todo elemento interactivo (anillo azul con offset).
- [ ] Menú sándwich con `aria-expanded`, `aria-controls` y foco gestionado.
- [ ] `prefers-reduced-motion: reduce` desactiva el shimmer y las transiciones.
- [ ] Iconos decorativos con `aria-hidden="true"`; iconos de acción con `aria-label`.
- [ ] Errores de formulario asociados al campo con `aria-describedby`.
- [ ] Los estados no dependen solo del color (badge con texto e ícono, §4.4).
- [ ] Targets táctiles de al menos 44 × 44 px.

> Los ítems sin tildar son requisitos de implementación: se verifican en la Instancia B.

## 7. Iconografía y assets

- **Set:** Material Symbols (https://fonts.google.com/icons), el set de Google, coherente con la inspiración de la SPEC.
- **Logo:** ícono `explore` (brújula) en `--color-primary` junto al nombre "Brújula".
- **Tamaños:** 20 px en botones y 24 px en listas; el color hereda del texto.
- **Imágenes:** sin imágenes decorativas; los empty states usan íconos.