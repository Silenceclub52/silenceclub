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
- Rama de trabajo actual: `claude/great-euler-ytewtv` (parte de `claude/gallant-thompson-hyejgu`) →
  https://claude-great-euler-ytewtv-silenceclub.floral-snowflake-c31a.workers.dev/
- Método: cambios en la rama de pruebas → Jorge revisa → si aprueba, se pasa a `main`.
- Volver atrás: Cloudflare → silenceclub → Implementaciones → ⋯ → Revertir.

## Estructura del repositorio
```
wrangler.jsonc     # name silenceclub, assets ./public, "previews": {} (obligatorio para las ramas)
public/
  index.html       # toda la web en un solo archivo (CSS y JS en línea)
  _headers         # X-Robots-Tag: noindex en *.workers.dev
  favicon.svg, apple-touch-icon.png
  img/             # jorge.webp, jorge-alt.webp, marina-3d.webp, amok-mapa.webp, og.jpg (1200x630)
```
Solo se publica `public/`. No meter nada sensible ahí.

## La web (public/index.html)
- Secciones: hero "Primero, silencio." · Qué hago · Trabajos (Bela Brow Studio, Bela Marchica, Marina Beach Club, AMOK) · Cómo trabajo · Detalle/retícula · Cosas que no voy a hacer · Hola, soy Jorge · Contacto.
- Retícula de 12 columnas (`.grid`); tecla G o botón "Mostrar retícula" la muestra.
- Tipografías Google Fonts: Fraunces (serif) y Figtree (sans).
- Colores: tokens en `:root` (`--niebla` fondo, `--papel`, `--tinta`, `--grafito`, `--linea`, `--violeta`, `--melocoton`).
  Modo oscuro sigue al sistema, con negros profundos: fondo `#0B0A0C`, superficies `#141317`, secciones oscuras `#040405`.
- **Logo Silence Club**: vectorizado letra a letra en un `<svg class="sc-defs">` (ids `sc-s0..6` = SILENCE, `sc-c0..3` = CLUB), reutilizado con `<use>`. viewBox `0 0 362 97`: línea en y=48 de x=0 a 358, punto (círculo medio relleno) en 351,86.5 r9.
  En el menú sustituye a "Jorge"; al pasar el ratón la línea vibra y vuelve al silencio. El pie aún dice "Jorge" y usa el logo antiguo en máscaras PNG (`.fsc`).
- **Intro de marca** (~1,8 s, solo dentro del logo del menú; sin capa a pantalla completa, sin bloquear scroll ni ocultar el hero):
  la línea se dibuja desde el centro con un poco de ruido y, a la vez, 15 barras verticales tenues (`g.bars`, opacidad .16, creadas por JS y retiradas al terminar) nacen de ella como ecualizador; todo se calma → SILENCE sube y CLUB baja desde la línea (recortados con `clipPath` `sc-up`/`sc-dn` del propio SVG del menú) → aparece el punto.
  No sale con "reducir movimiento" ni con la pestaña oculta. El estado inicial se pinta en el mismo script que sigue al `<nav>`, así que no hay parpadeo. Durante la intro se ignora el hover del logo. La onda del hero (bajo "Ver cómo trabajo") suena a la vez y con el mismo ritmo, pero mucho más baja (16 px frente a 74 sin intro; `introSoft`), sincronizada con el reloj del logo vía `window.__scIntro` (= `performance.now()` al arrancar la intro).
  (La versión anterior, a pantalla completa con columnas y el logo viajando al menú, está en el commit `a22760a`.)
- Retrato sin filtros ni capa melocotón (se quitó a petición de Jorge).

## Pendientes conocidos
1. Botón "Explorar en 3D" de Marina: faltan `marina.glb` y `model-viewer.min.js` en `public/`.
2. Enlace e iframe de Bela apuntan a `bela-vlc.netlify.app`; cambiar a `belabrowstudio.com` cuando ese dominio funcione.
3. Textos (descartado por ahora): resultados con cifras reales, frase de entrada más clara, quitar "Soy perfeccionista. Mucho.".
4. Valorar si el pie debe pasar también al logo nuevo.
5. Si se usa `workers.dev` de producción, apagarlo en Cloudflare → Dominios, para no duplicar la web.
6. Bajar Netlify a Free.

## Pruebas locales
- Servir `public/` con `python3 -m http.server` y usar Playwright con `executablePath: '/opt/pw-browsers/chromium'`.
- Bloquear `fonts.googleapis`/`netlify` en las pruebas (la red del entorno no llega y bloquea el renderizado).
- Para animaciones: falsear `performance.now` y `requestAnimationFrame` con un script de inicio y avanzar el tiempo a mano (`page.clock` no funciona aquí).
- Comprobar `wrangler` con `npx wrangler@4 deploy --dry-run` (debe leer solo los archivos de `public/`).
