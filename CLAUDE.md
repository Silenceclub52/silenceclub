# Silence Club · Web de Jorge

Web personal de dirección creativa y estratégica de Jorge, bajo la marca **Silence Club**.
Concepto de marca: *"Primero, silencio."* Escuchar antes de proponer. La **línea de sonido**
(que vibra con ruido y vuelve a la calma) es el hilo visual de toda la web.

## Cómo trabajar con Jorge
- Responder en español, de forma contundente, crítica y enfocada al trabajo.
- Jorge no es técnico: dar pasos concretos, uno a uno, con los nombres exactos de los botones.
- No inventar datos ni cifras de clientes. Los textos con resultados (punto pendiente "3") esperan datos reales de Jorge.
- Nunca tocar `main` sin que Jorge lo pida ("pásalo a main").

## Infraestructura
| Pieza | Dónde |
|---|---|
| Código | GitHub `Silenceclub52/silenceclub` |
| Hosting | Cloudflare Workers (static assets), proyecto `silenceclub`, cuenta info@silenceclub.es |
| Dominio | `silenceclub.es` registrado en **DonDominio** (`.es` no se puede trasladar a Cloudflare). Nameservers ya apuntan a Cloudflare: `jonah.ns.cloudflare.com`, `monroe.ns.cloudflare.com` |
| DNS | Gestionado en Cloudflare. Dominios personalizados del Worker: `silenceclub.es` y `www.silenceclub.es` |
| Correo | Google Workspace (`info@silenceclub.es`). **No tocar** los registros MX, TXT (SPF, DKIM `google._domainkey`, `google-site-verification`) |
| Netlify | Plan antiguo de 9 €/mes, a bajar a Free. **No borrar la cuenta**: aloja `bela-vlc.netlify.app` (web de la clienta Bela) y `silenceclub-propuestas.netlify.app` (solo logo, sin uso) |

## Flujo de publicación
- Rama `main` → producción (`silenceclub.es`). Comando: `npx wrangler deploy`.
- Cualquier otra rama → versión de prueba automática (`npx wrangler preview`):
  `https://<rama-con-guiones>-silenceclub.floral-snowflake-c31a.workers.dev/`
- Rama de trabajo actual (candidata a `main`): `claude/intelligent-cannon-dipviu` →
  https://claude-intelligent-cannon-dipviu-silenceclub.floral-snowflake-c31a.workers.dev/
  Sale de `main` y trae de `claude/nueva-estetica-contraste` solo lo que Jorge eligió (ver abajo).
  Las ramas `claude/nueva-estetica` y `claude/nueva-estetica-contraste` (versión única sin modo oscuro) quedan como referencia; Jorge las descartó como base. Antes de empezar una rama nueva, partir de la última rama de trabajo
  si aún no está en `main`, no de `main`: así se perdió el menú una vez.
  (`claude/gallant-thompson-hyejgu` ya está entera dentro de `main`; no se usa.)
- Pasar a `main`: avance rápido (`git push origin <rama>:main`), sin merge ni rebase. Antes, `npx wrangler@4 deploy --dry-run`.
  Después, Jorge comprueba en Cloudflare → Implementaciones que la publicación salió en verde (desde aquí no se puede ver).
- Método: cambios en la rama de pruebas → Jorge revisa → si aprueba, se pasa a `main`.
- Volver atrás: Cloudflare → silenceclub → Implementaciones → ⋯ → Revertir.

### Cambios de la rama candidata (sobre `main`)
- Se mantiene modo claro y oscuro. Nada de losas redondeadas a sangre en secciones grandes.
- Modo oscuro: hero y "Te toca a ti" en negro con un toque muy ligero de púrpura `#0E0B14` (`theme-color` oscuro igual). El hero sube bajo el menú (`margin-top:-72px`) para que ese fondo llegue arriba.
- Servicio con la estructura, textos y funcionamiento de la rama de contraste (colores de `main`): títulos "01 · INICIO"; scoring en tarjeta con medios puntos que se rellenan despacio (.22 s por punto, con la tarjeta bien a la vista) y debajo dos cajas; el candado se sacude una vez (`.nudge`) cuando las soluciones cruzan la mitad de la pantalla y se abre al llegar a Desarrollo; patas con "Responde a", título algo más grande, frase en primera persona y entregables plegados ("Ver entregables"; su línea `.pil-ln` vibra al abrir y cerrar, como en "Cosas que no voy a hacer"); faldón móvil solo cuando el título de la fase ya no se ve. Intro de Desarrollo: "Con todo lo analizado, paso a la acción. Trabajo sobre tres áreas:". Seguimiento anual: "Seguimos trabajando juntos todo el año, con seguimiento de todo lo que hemos puesto en marcha."
- Seguimiento anual: en lugar del filete sobre el precio, el año como línea de sonido (`.yr`): al verla suena con ondas (~2,6 s) y, al calmarse, se queda en reposo con una montañita fija sobre cada mes (gaussianas estrechas, `YS=11` en un viewBox de 1200); entonces aparecen, suaves y escalonadas, las iniciales ENE…DIC. Con "reducir movimiento", forma final directa. (Antes: punto recorriendo una línea, y doce puntos llenándose; descartados.)
- Proyectos: tarjetas `.card` sobre el fondo (sin el papel punteado); la línea de sonido es el borde superior; filtro en una cápsula.
- Filtro de proyectos con icono a línea por sector (`.pfi`): Todos = medio punto que gira medio giro; Nightlife = bola de discoteca (rejilla de facetas, cuelga de un hilo) que se balancea con un brillo y tres destellos; Beauty = barra de labios con un reflejo que la cruza y un destello. Solo al pulsar (`.pf.play`).
- "Cosas que no voy a hacer": lista editorial, título fijo a la izquierda, sin numeración ni medio punto.
- Sobre mí: el de `main`, con la sombra del retrato en gris corporativo (`--grafito` al 22 %, antes melocotón) y la frase de Silence Club en serif recta.
- "Te toca a ti": centrado, con el medio punto encima. Detalle: texto de Jorge ("Intento cuidar siempre hasta el último detalle…"). Desenlace: "El proyecto no acaba al publicarlo. Vemos qué ajustar."

### Estado (actualizar en cada paso a `main`)
- En `main` (producción) desde el último "pásalo a main": todo lo descrito abajo (hero, pausa, Servicio por fases, Proyectos con filtro y logos, marcas, menú esmerilado, pie con logo), con precios "··· €" por definir.
- Rama de pruebas = `main` en ese momento; los cambios nuevos se hacen en la rama y se vuelven a pasar.

## Estructura del repositorio
```
wrangler.jsonc     # name silenceclub, assets ./public, "previews": {} (obligatorio para las ramas)
public/
  index.html       # toda la web en un solo archivo (CSS y JS en línea)
  _headers         # X-Robots-Tag: noindex en *.workers.dev
  favicon.svg, favicon.ico, apple-touch-icon.png, icon-192.png, icon-512.png, site.webmanifest   # favicon: la línea de sonido (ruido violeta → calma) y el punto a medias, sobre negro #0B0A0C
  robots.txt, sitemap.xml
  img/logos/       # siluetas de marcas (sounds, condado, marina, amok, sonora, marchica, bigroom, aramon, bela)
  img/             # jorge.webp, jorge-alt.webp, marina-3d.webp, amok-mapa.webp, og-silenceclub.jpg (1200x630, imagen para redes)
```
Solo se publica `public/`. No meter nada sensible ahí.

## La web (public/index.html)
- SEO y redes: título "Silence Club · Dirección creativa y estratégica", descripción (la misma en meta, Open Graph, Twitter y JSON-LD: "Acompaño a personas con un negocio que todavía no ven todo lo que pueden construir con él. Estrategia de marca, identidad, web y captación."), Open Graph y Twitter con `og-silenceclub.jpg` (fondo negro, logo, "Primero, silencio.", la línea de sonido de violeta a calma) y JSON-LD `ProfessionalService`. Si se cambia la imagen, cambiarle el nombre de archivo para que las redes no sirvan la antigua de caché.
- Secciones: hero · pausa "Las mejores decisiones…" · Servicio (`#servicio`) · Proyectos (`#proyectos`) · Detalle/retícula · Cosas que no voy a hacer · Hola, soy Jorge (`#sobre-mi`) · Contacto. "Así será trabajar juntos" se quitó (repetía "Cómo trabajo").
- Menú: Servicio · Proyectos · Sobre mí · Hablemos. (El kicker violeta de la sección también dice "Servicio"; el botón del hero sigue siendo "Ver cómo trabajo".)
- **Hero** (jerarquía original, a petición de Jorge): H1 "Primero, silencio."; debajo, a la izquierda, H2 `.role` "Dirección creativa y estratégica" grande (muy importante para Jorge); a la derecha `.pitch`: H3 `.hook` (sans, la tipografía de antes) "Acompaño a personas con un negocio que todavía no ven todo lo que pueden construir con él.", párrafo `.lead` y remate `.punch` "este es tu sitio." en serif **recta (sin cursiva), subrayado**, a 19–22 px. CTA principal (sólido, blanco en oscuro) "Ver cómo trabajo" → `#servicio`; secundario "Cuéntame tu negocio" → `#contacto`.
- **Pausa** (`section.story`, sin `.block`): franja corta con fondo `--papel` (~370 px en escritorio): medio punto, la frase en serif recta de 28–46 px centrada y una línea pequeña (`.hush`, 240 px) que suena al verla y se calla. Ni la versión gigante de antes ni la línea mínima (Jorge: "se ha quedado en nada").
- **Servicio** ("Trabajo por proyectos."): 3 fases `.ph` (01 Inicio · 02 Desarrollo · 03 Desenlace). Sin animación de líneas (a Jorge no le gustó: demasiado ruido).
  - Escritorio: índice fijo `.proc-idx` a la izquierda (cols 1–3) con marcas vacío/medio/lleno según la fase, y el estado ("Escuchando", "Dando forma", "Midiendo") bajo la fase activa. Contenido en cols 5–12.
  - Móvil: solo el faldón `.viz-bar` fijo bajo el menú (top 60 px, mismo cristal que la barra): "02 Desarrollo · ● Dando forma". Es lo que Jorge quiere conservar.
  - La fase activa es la última cuyo título `.ph-n` ha cruzado el 55 % de la pantalla.
  - Inicio: tarjeta `.score` "Scoring de tu negocio" (**Ejemplo**, no datos reales). Debajo, separado en `.score-out` ("Al cerrar el scoring"): "Alcance y presupuesto" ✓ y "Soluciones" con candado (`.unlock`, enlaza a Desarrollo) y texto desenfocado; al llegar a Desarrollo el candado se abre (`.score.open`). "Alcance y presupuesto" lleva un icono € (`.eur`). Sin nota debajo (Jorge la quitó).
  - Desarrollo ("Ahora, manos a la obra."): las 3 patas como cajas `.pil` (opción B elegida por Jorge entre 5: la cinta de sonido hace de borde superior de la caja; canvas `.pil-line` absoluto, 44 px, top −22 px; caja con borde fino sin borde superior y sombra suave). Cada caja: cinta de sonido en canvas (estilo de las líneas antiguas, más fina: 8 px; se agita al verla y se queda respirando con su forma: pico, onda, pulso; sin violeta), etiqueta "Lo que te preocupa" (`.pil-k`, para que se entienda que son dolores del cliente) + frase en sans legible con filete, título, solución, entregables tangibles con medio punto (no repetir el título: p. ej. "Mapas 3D de reservas") y pie de precio `.pil-price` en una franja con tinte violeta muy suave para que destaque el €: ✓ "Si tu proyecto lo necesita · desde ··· €". Captación: mapas de reservas en 2D y 3D, estrategia de publicidad en RRSS y posicionamiento SEO.
  - Desenlace: pastilla ✓ "Incluido en el precio de tu proyecto" (`.ph-incl`).
  - Aparte de las fases, tarjeta `.beyond` "Más allá del proyecto · ¿Estamos cómodos juntos?" (seguimiento anual, tinte violeta, icono ↻, pie con "··· € al año") y el CTA "Mira cómo ha ido en negocios reales" (no tocar).
- **Proyectos** ("Negocios a los que he acompañado."): filtro por sector `.pf` (Todos marcado · Nightlife · Belleza; contadores por JS desde `data-sector`). Orden: Bela (abierta al cargar), Marina, AMOK, Marchica. Etiquetas `.pj-tags` como ítems con medio punto, idénticos a los de las patas (también en móvil). Ojo: `.pj-head` es un `<button>` y necesita `font:inherit`, si no sale la fuente del sistema. Cada proyecto: titular = resultado · "Qué necesitaba" · "Qué hicimos" (viñetas vacío → medio → lleno) · "Resultado".
- **Logo por proyecto** (`.pj-lg`): encima del nombre del cliente en cada proyecto, pequeño (alto `30/r^0.55`), gris al 50 %, más visible al pasar o abrir. En escritorio `flex:none` (la columna de `.pj-meta` estiraría el alto); en móvil `flex-basis:100%` para que vaya en su propia línea.
- **Marcas** (`.brands`, al final de Proyectos): "Negocios que he acompañado" + 9 logos como siluetas de un color (`public/img/logos/*.png`, blanco + alfa, 160 px de alto, usados como `mask` con `background:currentColor`): gris `--grafito` al 55 %, se encienden al pasar. Alto compensado a ojo por proporción (`--h = 50/r^0.55`). En móvil, cuadrícula 3×3. Origen: las propuestas de Jorge (index_17: Grupo Sounds, Condado, Marina, Sala Sonora, AMOK; index_18: Aramón, Marchica, The Big Room Formigal) y la imagen de Bela que envió (260 px; su web no es accesible desde el entorno).
- Desplegables (Proyectos y "Cosas que no voy a hacer") con chevron violeta (como en la web de Bela), ya no con el medio punto. Se mantiene la línea que vibra al pasar y al abrir.
- **Precio** (Jorge quiere que se intuya que todo tiene coste): por pata "desde ··· €", Desenlace "incluido en el precio", seguimiento "··· € al año". Se publican con el marcador **"··· €"** (Jorge quiere que se vea que hay precio aunque aún no esté definido; no quitarlos). Cuando dé importes, sustituir cada `··· €` en `.amt`.
- Detalle: "Tengo debilidad por el detalle." (antes "Soy perfeccionista. Mucho."). Cosas que no voy a hacer: "Diseñar para que te guste únicamente a ti."
- Contacto: "Te toca a ti."; el botón principal muestra el email `info@silenceclub.es`.
- Pie: logo vectorial (`svg.logo`) + "Dirección creativa y estratégica" a la izquierda; "Mostrar retícula" a la derecha. Ya no existe el logo PNG en máscaras (`.fsc`).
- Retícula de 12 columnas (`.grid`); tecla G o botón "Mostrar retícula" la muestra.
- Tipografías Google Fonts: Fraunces (serif) y Figtree (sans).
- Colores: tokens en `:root` (`--niebla` fondo, `--papel`, `--tinta`, `--grafito`, `--linea`, `--violeta`, `--melocoton`).
  Modo oscuro sigue al sistema, con negros profundos: fondo `#0B0A0C`, superficies `#141317`, secciones oscuras `#040405`.
- **Logo Silence Club**: vectorizado letra a letra en un `<svg class="sc-defs">` (ids `sc-s0..6` = SILENCE, `sc-c0..3` = CLUB), reutilizado con `<use>`. viewBox `0 0 362 97`: línea en y=48 de x=0 a 358, punto (círculo medio relleno) en 351,86.5 r9.
  En el menú sustituye a "Jorge"; al pasar el ratón la línea vibra y vuelve al silencio. Lo mismo (función `ring`), más amplio (×2), al rebotar en los extremos de la página: al llegar arriba o abajo con scroll, y al tirar más allá del borde con el dedo (>36 px) estando ya en el extremo. Pensado para móvil, donde no hay cursor. El pie aún dice "Jorge" y usa el logo antiguo en máscaras PNG (`.fsc`).
- **Intro de marca** (~1,8 s, solo dentro del logo del menú; sin capa a pantalla completa, sin bloquear scroll ni ocultar el hero):
  la línea se dibuja desde el centro con un poco de ruido y, a la vez, 15 barras verticales tenues (`g.bars`, opacidad .16, creadas por JS y retiradas al terminar) nacen de ella como ecualizador; todo se calma → SILENCE sube y CLUB baja desde la línea (recortados con `clipPath` `sc-up`/`sc-dn` del propio SVG del menú) → aparece el punto.
  No sale con "reducir movimiento" ni con la pestaña oculta. El estado inicial se pinta en el mismo script que sigue al `<nav>`, así que no hay parpadeo. Durante la intro se ignora el hover del logo. La onda del hero (bajo "Ver cómo trabajo") suena a la vez y con el mismo ritmo, pero más baja (28 px frente a 74 sin intro; `introSoft`) y concentrada en el centro (campana gaussiana, ancho ~11 % de la línea) con el mismo ruido que la línea del logo: montañas y valles en una zona, no repartidos por toda la línea, sincronizada con el reloj del logo vía `window.__scIntro` (= `performance.now()` al arrancar la intro). Si la línea no está a la vista durante la intro (en iPhone queda bajo el pliegue por las barras de Safari), su onda suena al entrar en pantalla (se reinicia `__scIntro` en el primer `IntersectionObserver`, umbral .35).
  (La versión anterior, a pantalla completa con columnas y el logo viajando al menú, está en el commit `a22760a`.)
- **Onda del hero** (canvas `#wave`): en desktop reacciona al cursor (`pointermove`). En móvil, al hacer scroll con la línea en pantalla, suena en la vertical del dedo (`touchmove`, y con la inercia hasta 1,5 s después de soltar), con energía limitada a .6 para que sea suave. No actúa con "reducir movimiento".
- **Menú** (aparece como barra al bajar >8 px, clase `glass`; tono oscuro sobre `.how`, clase `dark`): cristal esmerilado sobrio, no "liquid glass" literal (Jorge: "no somos Apple").
  - Barra: opacidad .8 en claro / .78 en oscuro, `blur(24px) saturate(160%)`. Sin degradados, relieves ni reflejo arcoíris.
  - Desktop: píldora flotante con borde fino, luz mínima arriba y sombra suave.
  - Móvil (≤900 px): barra de lado a lado, sin esquinas, 60 px de alto, solo una línea fina abajo. El panel de la hamburguesa se abre pegado debajo como una sola lámina, casi opaco (.98, antes .93), con separadores finos. Las reglas móviles llevan `html` delante para ganar en especificidad al bloque de modo oscuro, y hay un bloque `prefers-color-scheme:dark and max-width:900px` propio.
  - Hamburguesa: tres líneas rectas (`M3 4H25`, `M3 10H25`, `M3 16H25`), extremos rectos; al abrir forman la X.
  - Pendiente de decisión de Jorge: si la barra debe ser más transparente (65–70 %) para que se note más el cristal.
- Retrato sin filtros ni capa melocotón (se quitó a petición de Jorge). La "J" sobre la foto lleva el medio punto del mismo color que la letra (antes melocotón), como en el logo.

## Pendientes conocidos
0. WhatsApp: si no sale vista previa o sale una antigua, es la caché o Cloudflare bloqueando al lector (Security → Bots). Comprobar con developers.facebook.com/tools/debug. Tras publicar en `main` cambios de SEO o imagen para redes: forzar la relectura en developers.facebook.com/tools/debug (Volver a extraer) y linkedin.com/post-inspector; enviar `sitemap.xml` en Search Console.
1. Botón "Explorar en 3D" de Marina: faltan `marina.glb` y `model-viewer.min.js` en `public/`.
2. Enlace e iframe de Bela apuntan a `bela-vlc.netlify.app`; cambiar a `belabrowstudio.com` cuando ese dominio funcione.
3. Textos (descartado por ahora): resultados con cifras reales, frase de entrada más clara, quitar "Soy perfeccionista. Mucho.".
5. Si se usa `workers.dev` de producción, apagarlo en Cloudflare → Dominios, para no duplicar la web.
6. Bajar Netlify a Free.
4. Cuando Jorge tenga importes: sustituir los "··· €" de cada pata y del seguimiento anual.

## Pruebas locales
- **Compatibilidad (iPhone con Safari antiguo)**: no depender de `aspect-ratio` (Safari ≥15) ni de `color-mix` (Safari ≥16.2) sin alternativa. Los logos `.lg` llevan ancho explícito `calc(var(--h)*var(--r))` (antes, con `aspect-ratio`, salían con ancho 0 en algunos iPhone). Cada `color-mix` va precedido de un `rgba` equivalente, y hay un bloque `@supports not (aspect-ratio:1)` para los marcos y el retrato. Nada de `max-width:100%` en hijos de celdas de grid que se ajustan al contenido.
- Antes de pasar a main: auditoría en 8 anchos (320–1440), claro/oscuro y con Safari antiguo simulado (quitando `aspect-ratio` y `color-mix` de las hojas de estilo): sin errores JS, sin scroll horizontal, logos con tamaño, imágenes cargadas y marcos con alto.
- Servir `public/` con `python3 -m http.server` y usar Playwright con `executablePath: '/opt/pw-browsers/chromium'`.
- Bloquear `fonts.googleapis`/`netlify` en las pruebas (la red del entorno no llega y bloquea el renderizado).
- Para animaciones: falsear `performance.now` y `requestAnimationFrame` con un script de inicio y avanzar el tiempo a mano (`page.clock` no funciona aquí).
- Comprobar `wrangler` con `npx wrangler@4 deploy --dry-run` (debe leer solo los archivos de `public/`).
- iPhone 15 Pro Max (el de Jorge): 430 px de ancho; en Safari la altura útil al cargar ronda los 760 px por las barras del navegador, así que la onda del hero (top ≈ 797 px) queda bajo el pliegue. Simular con `viewport {width:430,height:760}, isMobile, hasTouch`.
- Gestos táctiles reales (scroll con el dedo, tirar del borde): `Input.dispatchTouchEvent` vía CDP.
- Al detectar el final de la página, dejar margen: `scrollY` puede quedarse unos píxeles por debajo de `scrollHeight - innerHeight` (se usa `m-6`).
- Imágenes generadas (favicon, `og-silenceclub.jpg`, iconos): se dibujan en HTML/SVG y se capturan con Playwright. Fraunces y Figtree se obtienen de npm (`@fontsource-variable/fraunces`, `@fontsource-variable/figtree`), porque Google Fonts no es accesible desde el entorno.
