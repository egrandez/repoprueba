# Landing Happy Garden

> Documento de la tarea, no del core. Se borra cuando sus pendientes están
> cerrados. No es documentación permanente ni referencia de cómo se hacen las
> cosas.

## Registro

| | |
| --- | --- |
| Repositorio y tipo | `repoprueba` — sitio estático (no es `front-end-core` ni `lp-core`) |
| Página o template | `index.html` (raíz) |
| URLs de los frames de Figma | No hubo. No se entregó enlace ni captura |
| File key | — |
| Idioma y marca | Español; marca «Happy Garden» |
| Viewports diseñados | No hubo diseño. Implementado mobile-first; breakpoints de 48 rem y 56 rem |
| Fecha | 2026-10-13 |

## Qué se implementó

Landing de una sola página con header, hero, sección de servicios, CTA y footer.

| Pieza | Clasificación | Archivos |
| --- | --- | --- |
| Tokens de marca (color, tipo, espaciado, radio, sombra, layout) | Nuevo | `styles/settings/tokens.css` |
| Base / normalización y foco | Nuevo | `styles/elements/base.css` |
| Objetos de maquetación (contenedor, pila, grilla) | Nuevo | `styles/objects/layout.css` |
| Header con marca, navegación y CTA | Nuevo | `styles/components/header.css`, bloque `.c-header` en `index.html` |
| Hero (mensaje, acciones, cifras, visual SVG) | Nuevo | `styles/components/hero.css`, `.c-hero` en `index.html` |
| Sección «Nuestros servicios» (encabezado compartido + tarjetas) | Nuevo | `styles/components/section.css`, `styles/components/services.css`, `.c-section`/`.c-services` |
| Banda CTA final | Nuevo | `styles/components/cta.css`, `.c-cta` en `index.html` |
| Footer (marca, enlaces, contacto, legal) | Nuevo | `styles/components/footer.css`, `.c-footer` en `index.html` |
| Botón / enlace de acción (primary, accent, ghost) | Nuevo | `styles/components/button.css` |
| Utilidades (skip link, visually-hidden) | Nuevo | `styles/utilities/utilities.css` |
| Punto de entrada CSS con las capas | Nuevo | `styles/style.css` |

Todas las piezas son nuevas porque el repositorio no traía ningún core,
componente, token ni utilidad previos: solo un `index.html` de prueba.

## Decisiones

Tomadas por el agente (revisables):

- **No hay core, así que los lineamientos se aplican adaptados.** El repo no es
  `front-end-core` ni `lp-core`: no hay `package.json`, ni Pug, ni SCSS compilado,
  ni script de assets. Se replicó la arquitectura de capas con CSS plano
  (`settings` → `elements` → `objects` → `components` → `utilities`), conectado
  con `@import` desde `styles/style.css`.
- **Se limitó el lenguaje a HTML + CSS.** No hay comportamiento que lo justifique
  (header sticky, hover y foco se resuelven con CSS), así que no se escribió
  JavaScript. Tampoco se usó `details`/`summary` porque no hay acordeón.
- **La ilustración del hero es un SVG inline y el logo/iconos son SVG**, no
  imágenes rasterizadas: no hay assets entregados y el propio lineamiento prohíbe
  sustituir un asset ausente por un placeholder remoto. Así no hay WebP que
  generar ni `pnpm optimize` que ejecutar.
- **Tipografía con pila del sistema** (`system-ui`, Segoe UI, Roboto…). Sin
  diseño no se puede declarar y servir una familia concreta, y no se inventó una
  fuente de Google (dependencia de red no pedida).
- **Se corrigieron dos defectos propios detectados en verificación**: el texto de
  la banda CTA pasó de blanco al 85 % (contraste 4.15:1) a blanco pleno, y el
  color del anillo de foco pasó de `--color-brand` (invisible sobre la banda
  verde) a `--color-ink`.

Tomadas por la persona responsable: ninguna registrada (no hubo diseño ni brief
con contenido, marca o enlaces).

## Supuestos

Cada uno con su motivo y con qué lo confirmaría.

1. **Marca «Happy Garden»**, tomada del nombre del directorio de trabajo
   (`happy-garden`). Se confirmaría con el brief real de marca.
2. **Contenido y copys en español, redactados como presentación.** El encargo se
   hizo en español y no se entregó texto final. Se confirmaría con los copys
   definitivos.
3. **Navegación de tres destinos**: Servicios, Contacto y el ancla de marca. Se
   confirmaría con el mapa de navegación real.
4. **Datos de contacto de ejemplo** (`hola@happygarden.example`,
   `+34 600 000 000`) y **una sola sección de contenido** (Servicios). Se
   confirmarían con los datos e información reales del negocio.

## Defectos del diseño

No aplica: no se recibió ningún frame de Figma ni captura que pudiera presentar
inconsistencias.

## Qué se verificó

No hay scripts de repo (`pnpm optimize`, `lint`, `validate`, `build`,
`verify:render`, `preview`): el repositorio no trae `package.json`. Se usó un
canal equivalente, sirviendo la página con `python -m http.server` y auditándola
en un navegador real.

| Comando / canal | Resultado |
| --- | --- |
| `python -m http.server 8099` (sirve la raíz) | OK — la página carga |
| Consola del navegador | Sin errores ni advertencias |
| CSS cargado | OK — `@import` resueltos; `body` y botón toman sus tokens |
| Lighthouse — Accessibility | 1.00 (100) |
| Lighthouse — Best Practices | 1.00 (100) |
| Lighthouse — SEO | 1.00 (100) |
| Lighthouse — fallos | Ninguno |

Comprobado a mano / por medición:

- **Breakpoints reales** midiendo la página en iframes de 375, 768 y 1440 px (el
  viewport del navegador disponible era 800 px): a 375 px todo apila en una
  columna y el CTA del header se oculta; a 1440 px el hero va a dos columnas y
  servicios y footer a tres. Sin desbordamiento horizontal en ninguno.
- **Jerarquía y presencia de contenido**: un `h1`, dos `h2`, cinco SVG, anclas con
  destino real (`#main`, `#servicios`, `#contacto`).
- **Tokens aplicados**: `body` con la pila del sistema; botón primario en
  `rgb(47,125,79)` = `--color-brand`.
- **Estructura de capas**: el stylesheet de entrada resuelve sus 11 `@import`.

## Qué NO se verificó

Explícito, no por omisión:

- **Figma**: no existió diseño; nada se comparó contra una fuente visual.
- **Foco visible real**: la ventana del navegador no obtuvo foco de documento
  (`document.hasFocus()` = `false`), así que `:focus-visible` no llegó a
  activarse y no se observó el anillo de foco. La regla existe
  (`outline: 3px solid var(--color-ink)` en `elements/base.css`) y su contraste
  se dedujo, pero no se vio.
- **Navegación por teclado end-to-end** (Tab/Shift+Tab, orden de foco, retorno de
  foco): no comprobada con foco real de ventana.
- **Lectores de pantalla**: no se ejecutó ninguno.
- **PHP, rater, CMS y cualquier integración**: no existen en este repo.
- **Rendimiento/LCP**: sin imágenes rasterizadas, no aplica la estrategia de
  `preload`/`imagesrcset`.

## Pendientes

1. **Enlaces y datos de contacto reales** (email, teléfono, formulario o canal de
   presupuesto). Depende de: negocio. Los botones de CTA hoy apuntan al bloque de
   contacto del footer como presentación. Bloquea la publicación.
2. **Copys definitivos y datos de negocio** (cifras del hero, servicios, textos
   legales). Depende de: marketing / negocio.
3. **Identidad visual real** (logotipo, familia tipográfica, paleta de marca).
   Depende de: diseño. Hoy hay una paleta y un logo de presentación.
4. **Diseño/validación de accesibilidad de estados** (hover, foco observado,
   teclado). Depende de: QA / front-end.
5. **Integración de analítica y consentimiento**, si aplica. Depende de:
   marketing / legal. No implementado.

## Coste y uso

Se completa cuando la persona responsable dé la tarea por concluida, con el
comando de medición del repo. Este repositorio no trae `pnpm medir:coste`, así
que queda sin completar.
