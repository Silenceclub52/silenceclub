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
- Rama de trabajo actual: `claude/great-euler-ytewtv` →
  https://claude-great-euler-ytewtv-silenceclub.floral-snowflake-c31a.workers.dev/
  (`claude/gallant-thompson-hyejgu` ya está entera dentro de `main`; no se usa.)
- Pasar a `main`: avance rápido (`git push origin <rama>:main`), sin merge ni rebase. Antes, `npx wrangler@4 deploy --dry-run`.
  Después, Jorge comprueba en Cloudflare → Implementaciones que la publicación salió en verde (desde aquí no se puede ver).
- Método: cambios en la rama de pruebas → Jorge revisa → si aprueba, se pasa a `main`.
- Volver atrás: Cloudflare → silenceclub → Implementaciones → ⋯ → Revertir.

### Estado (actualizar en cada paso a `main`)
- En `main` (producción): intro de marca en el logo con barras, onda del hero sincronizada y reactiva al dedo, favicon nuevo, SEO e imagen para redes.
- Solo en la rama de pruebas, pendiente de "pásalo a main": menú esmerilado sobrio (barra de lado a lado en móvil), hamburguesa recta, onda del hero que suena al aparecer en iPhone y vibración del logo al rebotar.

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
- Secciones: hero "Primero, silencio." · Qué hago · Trabajos (Bela Brow Studio, Bela Marchica, Marina Beach Club, AMOK) · Cómo trabajo · Detalle/retícula · Cosas que no voy a hacer · Hola, soy Jorge · Contacto.
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
- Retrato sin filtros ni capa melocotón (se quitó a petición de Jorge).

## Pendientes conocidos
0. Tras publicar en `main` cambios de SEO o imagen para redes: forzar la relectura en developers.facebook.com/tools/debug (Volver a extraer) y linkedin.com/post-inspector; enviar `sitemap.xml` en Search Console.
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
- iPhone 15 Pro Max (el de Jorge): 430 px de ancho; en Safari la altura útil al cargar ronda los 760 px por las barras del navegador, así que la onda del hero (top ≈ 797 px) queda bajo el pliegue. Simular con `viewport {width:430,height:760}, isMobile, hasTouch`.
- Gestos táctiles reales (scroll con el dedo, tirar del borde): `Input.dispatchTouchEvent` vía CDP.
- Al detectar el final de la página, dejar margen: `scrollY` puede quedarse unos píxeles por debajo de `scrollHeight - innerHeight` (se usa `m-6`).
- Imágenes generadas (favicon, `og-silenceclub.jpg`, iconos): se dibujan en HTML/SVG y se capturan con Playwright. Fraunces y Figtree se obtienen de npm (`@fontsource-variable/fraunces`, `@fontsource-variable/figtree`), porque Google Fonts no es accesible desde el entorno.
