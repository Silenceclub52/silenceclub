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
- Rama de trabajo actual: `claude/determined-gauss-cmhm1g` →
  https://claude-determined-gauss-cmhm1g-silenceclub.floral-snowflake-c31a.workers.dev/
- Método: cambios en la rama de pruebas → Jorge revisa → si aprueba, se pasa a `main`.
- Volver atrás: Cloudflare → silenceclub → Implementaciones → ⋯ → Revertir.

## Estructura del repositorio
```
wrangler.jsonc     # name silenceclub, assets ./public, "previews": {} (obligatorio para las ramas)
public/
  index.html       # toda la web en un solo archivo (CSS y JS en línea)
  _headers         # X-Robots-Tag: noindex en *.workers.dev
  favicon.svg, favicon.ico, apple-touch-icon.png, icon-192.png, icon-512.png, site.webmanifest   # favicon: la línea de sonido (ruido violeta → calma) y el punto a medias, sobre negro #0B0A0C
  robots.txt, sitemap.xml
  img/             # jorge.webp, jorge-alt.webp, marina-3d.webp, amok-mapa.webp, og-silenceclub.jpg (1200x630, imagen para redes)
```
Solo se publica `public/`. No meter nada sensible ahí.

## La web (public/index.html)
- SEO y redes: título "Silence Club · Dirección creativa y estratégica", descripción de ~155 caracteres, Open Graph y Twitter con `og-silenceclub.jpg` (fondo negro, logo, "Primero, silencio.", la línea de sonido de violeta a calma) y JSON-LD `ProfessionalService`. Si se cambia la imagen, cambiarle el nombre de archivo para que las redes no sirvan la antigua de caché.
- Secciones: hero · pausa "Las mejores decisiones…" · Cómo trabajo (`#como-trabajo`) · Proyectos (`#proyectos`) · Así será trabajar juntos (`#juntos`, oscura) · Detalle/retícula · Cosas que no voy a hacer · Hola, soy Jorge (`#sobre-mi`) · Contacto.
- Menú: Cómo trabajo · Proyectos · Sobre mí · Hablemos.
- **Hero**: H1 "Primero, silencio." con "Dirección creativa y estratégica" firmado a su derecha (debajo en móvil). Fila 2: H2 `.say` (a quién acompaño) a la izquierda y, a la derecha, el párrafo (qué hago, en las 3 patas) que remata en `.punch` "este es tu sitio.". CTA principal (sólido, blanco en oscuro) "Ver cómo trabajo" → `#como-trabajo`; secundario "Cuéntame tu negocio" → `#contacto`.
- **Cómo trabajo** ("Trabajo por proyectos."): 4 fases `.ph` (01 Inicio · 02 Desarrollo · 03 Desenlace · 04 Y después). A la izquierda, fijo (`.proc-side`, sticky), un visor con la línea de sonido en canvas `#procLine` que cambia según la fase que se lee: ruido con cabezal de escucha y marcas de oportunidades → se separa en tres pistas (una por pata; se ilumina la que se lee) → una línea que se mide y sube → calma con pulso y cola discontinua ("sigue"). Debajo, índice de fases clicable y barra de progreso. En móvil el visor es una franja fija bajo el menú y el índice se oculta. El progreso se mide cuando el título `.ph-n` de cada fase cruza el 55 % de la pantalla.
  - Inicio: tarjeta `.score` "Scoring de tu negocio" con puntos (lleno/medio/vacío) **marcada como Ejemplo** (no son datos de clientes), "Alcance y presupuesto" ✓ y "Soluciones" tachado (`.redact`): no se desvelan, se aplican.
  - Desarrollo: las 3 patas `.pil` con frase de dolor del cliente («…»), título, frase de solución y entregables. Cada una con su línea SVG (pico / onda / pulso), dibujada por JS con las mismas formas que el visor.
- **Proyectos** ("Negocios a los que he acompañado."): filtro por sector `.pf` (Todos · Nightlife · Belleza; contadores calculados por JS desde `data-sector`). Orden: Marina, AMOK, Marchica (Nightlife) y Bela (Belleza); Marina abierto al cargar. Cada proyecto: titular = resultado · "Qué necesitaba" · "Qué hicimos" (viñetas vacío → medio → lleno) · "Resultado".
- Desplegables (Proyectos y "Cosas que no voy a hacer") con chevron violeta (como en la web de Bela), ya no con el medio punto. Se mantiene la línea que vibra al pasar y al abrir.
- Retícula de 12 columnas (`.grid`); tecla G o botón "Mostrar retícula" la muestra.
- Tipografías Google Fonts: Fraunces (serif) y Figtree (sans).
- Colores: tokens en `:root` (`--niebla` fondo, `--papel`, `--tinta`, `--grafito`, `--linea`, `--violeta`, `--melocoton`).
  Modo oscuro sigue al sistema, con negros profundos: fondo `#0B0A0C`, superficies `#141317`, secciones oscuras `#040405`.
- **Logo Silence Club**: vectorizado letra a letra en un `<svg class="sc-defs">` (ids `sc-s0..6` = SILENCE, `sc-c0..3` = CLUB), reutilizado con `<use>`. viewBox `0 0 362 97`: línea en y=48 de x=0 a 358, punto (círculo medio relleno) en 351,86.5 r9.
  En el menú sustituye a "Jorge"; al pasar el ratón la línea vibra y vuelve al silencio. El pie aún dice "Jorge" y usa el logo antiguo en máscaras PNG (`.fsc`).
- **Intro de marca** (~1,8 s, solo dentro del logo del menú; sin capa a pantalla completa, sin bloquear scroll ni ocultar el hero):
  la línea se dibuja desde el centro con un poco de ruido y, a la vez, 15 barras verticales tenues (`g.bars`, opacidad .16, creadas por JS y retiradas al terminar) nacen de ella como ecualizador; todo se calma → SILENCE sube y CLUB baja desde la línea (recortados con `clipPath` `sc-up`/`sc-dn` del propio SVG del menú) → aparece el punto.
  No sale con "reducir movimiento" ni con la pestaña oculta. El estado inicial se pinta en el mismo script que sigue al `<nav>`, así que no hay parpadeo. Durante la intro se ignora el hover del logo. La onda del hero (bajo "Ver cómo trabajo") suena a la vez y con el mismo ritmo, pero más baja (28 px frente a 74 sin intro; `introSoft`) y concentrada en el centro (campana gaussiana, ancho ~11 % de la línea) con el mismo ruido que la línea del logo: montañas y valles en una zona, no repartidos por toda la línea, sincronizada con el reloj del logo vía `window.__scIntro` (= `performance.now()` al arrancar la intro).
  (La versión anterior, a pantalla completa con columnas y el logo viajando al menú, está en el commit `a22760a`.)
- **Onda del hero** (canvas `#wave`): en desktop reacciona al cursor (`pointermove`). En móvil, al hacer scroll con la línea en pantalla, suena en la vertical del dedo (`touchmove`, y con la inercia hasta 1,5 s después de soltar), con energía limitada a .6 para que sea suave. No actúa con "reducir movimiento".
- Retrato sin filtros ni capa melocotón (se quitó a petición de Jorge).

## Pendientes conocidos
1. Botón "Explorar en 3D" de Marina: faltan `marina.glb` y `model-viewer.min.js` en `public/`.
2. Enlace e iframe de Bela apuntan a `bela-vlc.netlify.app`; cambiar a `belabrowstudio.com` cuando ese dominio funcione.
3. Textos (descartado por ahora): resultados con cifras reales, frase de entrada más clara, quitar "Soy perfeccionista. Mucho.".
4. Valorar si el pie debe pasar también al logo nuevo.
5. Si se usa `workers.dev` de producción, apagarlo en Cloudflare → Dominios, para no duplicar la web.
6. Bajar Netlify a Free.
7. "Así será trabajar juntos" (`#juntos`) repite ideas de la nueva "Cómo trabajo" (escuchar, decir lo que veo, hacerlo). Decidir con Jorge si se fusiona o se quita.
8. Confirmar con Jorge que Marchica es Nightlife (se dedujo por entradas, reservas y temporada).

## Pruebas locales
- Servir `public/` con `python3 -m http.server` y usar Playwright con `executablePath: '/opt/pw-browsers/chromium'`.
- Bloquear `fonts.googleapis`/`netlify` en las pruebas (la red del entorno no llega y bloquea el renderizado).
- Para animaciones: falsear `performance.now` y `requestAnimationFrame` con un script de inicio y avanzar el tiempo a mano (`page.clock` no funciona aquí).
- Comprobar `wrangler` con `npx wrangler@4 deploy --dry-run` (debe leer solo los archivos de `public/`).
