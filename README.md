# 🛠️ MegaTienda: laboratorio de mejora web

> **Curso:** Calidad de Software
> **Tema:** Corregir y mejorar una web real: rendimiento (Core Web Vitals), accesibilidad (WCAG 2.2) y usabilidad (heurísticas de Nielsen)
> **Duración sugerida:** 3 a 4 horas (una o dos sesiones)
> **Modalidad:** individual o en grupos de 3 a 5 personas
> **Requisito previo:** haber hecho la auditoría del laboratorio anterior ([megatienda-auditoria-web](https://github.com/jesusguzman28/megatienda-auditoria-web))

---

## ⚠️ Antes de empezar: lee esto

En el laboratorio anterior **encontraste** los problemas de `index.html`. Ahora te toca **arreglarlos**.

El `index.html` de este repositorio es **la misma página con errores**. Tu trabajo es convertirla en una tienda rápida, accesible y fácil de usar, **paso a paso**, midiendo el antes y el después.

> 💡 **Regla de oro:** un paso = un commit. Así tu docente (y tú) pueden ver qué cambió en cada mejora y si mejoró la métrica.

Al final vas a entregar:

1. Tu repositorio con **un commit por paso**.
2. Tu página publicada en **GitHub Pages**.
3. El archivo `ENTREGA.md` completo, con las métricas **antes y después**.

---

## 🎯 Objetivo de aprendizaje

Al terminar podrás:

- Corregir problemas de **LCP, CLS e INP** y explicar por qué mejoran.
- Aplicar criterios **WCAG 2.2 (nivel A y AA)** en HTML, CSS y JavaScript.
- Rediseñar textos e interacciones con las **10 heurísticas de Nielsen**.
- **Medir** el impacto de cada cambio con Lighthouse y PageSpeed Insights.

---

## 📦 Paso 0: preparar tu copia del proyecto

### 0.1 Crear tu repositorio

1. Entra a este repositorio en GitHub y pulsa el botón verde **Use this template → Create a new repository**.
   (Si no aparece, usa **Fork**.)
2. Ponle de nombre `megatienda-mejorada-<tu-apellido>` y márcalo como **Public**.
3. Clónalo en tu computadora:

```bash
git clone https://github.com/<tu-usuario>/megatienda-mejorada-<tu-apellido>.git
cd megatienda-mejorada-<tu-apellido>
```

### 0.2 Publicarlo en GitHub Pages (antes de tocar nada)

1. En tu repositorio: **Settings → Pages**.
2. En *Source* elige **Deploy from a branch**, rama **main**, carpeta **/ (root)** y pulsa **Save**.
3. En 1 o 2 minutos tu página estará en `https://<tu-usuario>.github.io/megatienda-mejorada-<tu-apellido>/`.

> ✅ Cada vez que hagas `git push`, la página publicada se actualiza sola. **No hace falta crear otra carpeta ni subir archivos a mano.**

### 0.3 Abrirlo en local

- Abre la carpeta en **VS Code** e instala la extensión **Live Server**.
- Clic derecho en `index.html` → **Open with Live Server**.

### 0.4 Medir el ANTES (¡no te lo saltes!)

1. Abre tu página publicada en **Chrome en modo incógnito**.
2. `F12` → pestaña **Lighthouse** → modo **Navigation**, dispositivo **Mobile**, marca todas las categorías → **Analyze page load**.
3. Anota en `ENTREGA.md` (tabla "Antes"): Performance, Accessibility, Best Practices, LCP, CLS y TBT.
4. Haz también la prueba en **PageSpeed Insights**: `https://pagespeed.web.dev/` con la URL de tu página.
5. Pulsa el botón **Comprar** del primer producto con la pestaña **Performance** grabando y anota cuánto tarda en responder (**INP**).

```bash
git add ENTREGA.md
git commit -m "Paso 0: métricas iniciales"
git push
```

---

## 🗺️ Mapa de la mejora

Haz los pasos **en orden**: los primeros son los que más impacto tienen con menos esfuerzo.

| Paso | Tema | Qué mejora | Tiempo aprox. |
|---|---|---|---|
| 1 | Cabecera del documento | Accesibilidad, SEO | 10 min |
| 2 | Quitar lo que bloquea la carga | LCP, FCP, TBT | 20 min |
| 3 | Imágenes | LCP, peso, CLS | 25 min |
| 4 | Contenido que "salta" | CLS | 20 min |
| 5 | Estructura semántica | Accesibilidad, lector de pantalla | 25 min |
| 6 | Teclado y foco | Accesibilidad | 25 min |
| 7 | Contraste, tamaño y diseño responsive | Accesibilidad, móvil | 25 min |
| 8 | Formulario de registro | Accesibilidad, usabilidad | 40 min |
| 9 | Interacciones rápidas | INP | 25 min |
| 10 | Textos, ayuda y patrones oscuros | Heurísticas de Nielsen | 20 min |
| 11 | Medir el DESPUÉS y entregar | Todo | 20 min |

Cada paso tiene la misma estructura:

- 🔎 **Busca:** qué parte del código revisar.
- 🛠️ **Haz:** qué cambiar (con una pista de código, no la solución completa).
- ✅ **Comprueba:** cómo verificar que funcionó.
- 💾 **Guarda:** el commit.

---

## Paso 1: cabecera del documento

🔎 **Busca:** las primeras 6 líneas de `index.html`.

🛠️ **Haz:**

1. Declara el idioma de la página en la etiqueta `<html>` (WCAG 3.1.1).
2. Cambia el `<title>` "Inicio" por uno que describa la página (WCAG 2.4.2).
3. **Permite el zoom:** en el `<meta name="viewport">` borra `maximum-scale=1` y `user-scalable=no` (WCAG 1.4.4).
4. Agrega una `<meta name="description">`.

```html
<!-- Pista -->
<html lang="??">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>MegaTienda | ...</title>
```

✅ **Comprueba:** en el móvil (o con `F12 → modo dispositivo`) ya puedes hacer zoom con dos dedos. Lighthouse ya no muestra "`<html>` element does not have a `[lang]` attribute".

💾 **Guarda:** `git commit -am "Paso 1: idioma, título y zoom"`

---

## Paso 2: quitar lo que bloquea la carga

> Todo lo que está en el `<head>` **sin `defer`** detiene el dibujo de la página hasta que termina de descargarse y ejecutarse.

🔎 **Busca:** el `<head>` y el primer `<script>` ("Inicializando configuración...").

🛠️ **Haz:**

1. **Borra** el script que hace un bucle `while` de 2.5 segundos. No hace nada útil.
2. **Borra** jQuery y Lodash. Búscalo tú mismo: ¿se usa `$(` o `_.` en algún lugar del archivo? (`Ctrl+F`)
3. **Borra** Font Awesome y Animate.css. ¿Hay alguna clase `fa-` o `animate__`?
4. **Fuentes:** quédate con **una sola familia** (o ninguna, usando fuentes del sistema) y cambia `display=block` por `display=swap`.

```css
/* Pista: fuentes del sistema, 0 descargas */
body { font-family: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif; }
```

5. **Pantalla de precarga:** elimina el `<div id="precarga">` y el código que lo quita en `window.addEventListener('load', ...)`. El contenido debe verse apenas llega, no esconderse.
6. **Bucle de "Analítica":** busca `setInterval(function () { trabajar(150); }, 700);` y bórralo.
7. **"Productos relacionados":** el bucle que crea 2500 `<div>` ocultos en `#relleno` no se ve en ninguna parte. Bórralo junto con el `<div id="relleno">`.
8. Cambia `document.write(...)` por HTML normal escrito directamente en la página.

✅ **Comprueba:** Lighthouse → **Total Blocking Time** debe bajar muchísimo y desaparecer "Eliminate render-blocking resources". En la pestaña **Network**, el número de peticiones debe bajar.

💾 **Guarda:** `git commit -am "Paso 2: quitar recursos bloqueantes y tareas largas"`

---

## Paso 3: imágenes

> La imagen grande de bienvenida es el **elemento LCP**: lo más grande que ve el usuario. Hoy se inserta con JavaScript **a los 3.5 s** y pesa varios MB.

🔎 **Busca:** el `setTimeout` que llena `#hero` y todas las etiquetas `<img>` de la grilla.

🛠️ **Haz:**

1. Mueve la imagen principal **al HTML** (dentro de `#hero`) y borra el `setTimeout`.
2. Pide imágenes del **tamaño en que se muestran**. Picsum permite elegir tamaño y formato WebP:
   - Principal: `https://picsum.photos/id/1015/1200/500.webp` (en lugar de 4000×2600).
   - Productos: `https://picsum.photos/id/26/600/400.webp` (en lugar de 2400×1600).
3. Agrega **siempre** `width` y `height` (evita que la página salte).
4. A la imagen principal ponle `fetchpriority="high"`; a las de producto, `loading="lazy"`.
5. Escribe un **`alt` útil** en cada imagen (WCAG 1.1.1). Describe lo que el cliente necesita saber, no "imagen1".

```html
<!-- Pista -->
<img src="https://picsum.photos/id/26/600/400.webp"
     srcset="https://picsum.photos/id/26/400/267.webp 400w,
             https://picsum.photos/id/26/600/400.webp 600w"
     sizes="(max-width: 600px) 100vw, 33vw"
     width="600" height="400" loading="lazy"
     alt="...">
```

6. **Bonus:** agrega en el `<head>` `<link rel="preconnect" href="https://picsum.photos">`.

✅ **Comprueba:** en **Network → Img** el peso total de imágenes debe bajar de varios MB a unos cientos de KB. En Lighthouse, el **LCP** debe bajar y desaparecer "Image elements do not have explicit width and height".

💾 **Guarda:** `git commit -am "Paso 3: imágenes con tamaño, formato, alt y prioridad"`

---

## Paso 4: contenido que "salta" (CLS)

> CLS mide cuánto se mueve la página mientras la lees. Cada vez que algo aparece **encima** de lo que ya se ve, todo se desplaza y el usuario hace clic donde no quería.

🔎 **Busca:** los `setTimeout` de `#zona-cookies`, `#zona-promo` y `#zona-anuncio`.

🛠️ **Haz:**

1. **Banner promocional:** escríbelo directamente en el HTML (sin `setTimeout`).
2. **Anuncio:** ¿hace falta? Si lo mantienes, **reserva su espacio** desde el inicio:

```css
#zona-anuncio { min-height: 250px; }
```

3. **Aviso de cookies:** que aparezca como una **capa fija en la parte inferior** (no empuja nada):

```css
.cookies { position: fixed; left: 0; right: 0; bottom: 0; }
```

   y que tenga dos botones de **igual peso visual**: "Aceptar" y "Rechazar" (nada de "al seguir navegando aceptas todo").

4. **Modal de newsletter:** borra `setTimeout(abrirModal, 8000)`. El modal no debe abrirse solo.

✅ **Comprueba:** `F12 → Performance → ⚙️ → marca "Screenshots"` y graba la carga: la página ya no debe moverse. **CLS < 0.1** en Lighthouse.

💾 **Guarda:** `git commit -am "Paso 4: eliminar desplazamientos de contenido (CLS)"`

---

## Paso 5: estructura semántica

> Un lector de pantalla "navega" por encabezados y regiones. Si todo son `<div>`, la página es una pared sin puertas.

🛠️ **Haz:**

1. Usa **regiones (landmarks)**:

```html
<header> ... <nav aria-label="Principal"> ... </nav> ... </header>
<main id="contenido"> ... </main>
<footer> ... </footer>
```

2. Agrega como **primer elemento del `<body>`** un enlace "Saltar al contenido" (WCAG 2.4.1):

```html
<a class="saltar" href="#contenido">Saltar al contenido</a>
```

```css
.saltar { position: absolute; left: -9999px; }
.saltar:focus { left: 1rem; top: 1rem; }
```

3. **Encabezados en orden:** un solo `<h1>` en la página; secciones con `<h2>`; productos con `<h3>`. Revisa el `<h1>` del modal, los `<h5>` de los productos y el `<h4>` suelto de "Opiniones".
4. **Tabla de envíos:** agrega `<caption>` y convierte la primera fila en encabezados `<th scope="col">`.
5. **`aria-hidden`:** quita `aria-hidden="true"` del `<div>` que contiene el enlace "Escríbenos aquí" (oculta un enlace que el teclado sí alcanza).
6. **`<iframe>`:** agrégale un `title` (ej. `title="Mapa de tiendas"`) o reemplázalo por una lista con las direcciones.
7. Textos en otro idioma: tradúcelos al español o márcalos con `lang="en"`.

✅ **Comprueba:** instala la extensión **HeadingsMap** o usa **axe DevTools**: los encabezados deben verse en orden (h1 → h2 → h3) sin saltos. Con el lector de pantalla (**NVDA** en Windows o **VoiceOver** en Mac), pulsa `D` (NVDA) para saltar entre regiones.

💾 **Guarda:** `git commit -am "Paso 5: estructura semántica, encabezados y landmarks"`

---

## Paso 6: teclado y foco

> Prueba ahora: **desconecta el mouse** e intenta comprar algo solo con `Tab`, `Shift+Tab`, `Enter` y `Espacio`. No se puede.

🛠️ **Haz:**

1. **Borra** la regla `*:focus { outline: none; }` y agrega un foco visible (WCAG 2.4.7):

```css
:focus-visible { outline: 3px solid #1d4ed8; outline-offset: 2px; }
```

2. **Regla simple:** si **lleva a otra parte** → `<a href>`. Si **hace algo** → `<button type="button">`.
   - El menú y el logo (`<div onclick="location.href=...">`) → `<a href="#productos">`.
   - Los botones de compra `<div class="b2/b3/b4">` → `<button>`.
   - Los botones del formulario (`.botones div`) → `<button>`.
3. **Quita todos los `tabindex` positivos** (`tabindex="1"`, `"2"`, `"3"`). El orden del foco debe seguir el orden visual (WCAG 2.4.3).
4. **Modal de newsletter:** cámbialo por un `<dialog>` nativo. El navegador mueve el foco dentro, lo cierra con `Esc` y lo atrapa por ti:

```html
<dialog id="modal" aria-labelledby="modal-titulo">
  <h2 id="modal-titulo">...</h2>
  ...
  <button type="button" onclick="this.closest('dialog').close()" aria-label="Cerrar">×</button>
</dialog>
```

```js
document.getElementById('modal').showModal(); // para abrirlo
```

5. El botón de cerrar debe medir **al menos 24×24 px** (WCAG 2.5.8).

✅ **Comprueba:** con **solo el teclado** puedes: saltar al contenido, usar el menú, agregar un producto, llenar el formulario, abrir y cerrar el newsletter con `Esc`. **Siempre** ves dónde está el foco.

💾 **Guarda:** `git commit -am "Paso 6: navegación por teclado y foco visible"`

---

## Paso 7: contraste, tamaño y diseño responsive

🛠️ **Haz:**

1. **Contraste mínimo 4.5:1** para texto normal y 3:1 para texto grande (WCAG 1.4.3). Revisa con el *picker* de color de DevTools (muestra el ratio) o con https://webaim.org/resources/contrastchecker/
   Colores a revisar: `body` (#a0a0a0), `.menu`, `.precio` (#c8e6c9), `.precio-viejo`, los cuatro botones, `.testimonios` (#555 sobre #333), `.pie`, `marquee`, `.promo`.

```css
/* Pista de paleta con buen contraste sobre blanco */
:root {
  --texto: #1f2937;      /* 14.7:1 */
  --texto-suave: #4b5563;/*  7.5:1 */
  --primario: #1d4ed8;   /*  6.7:1, texto blanco encima */
  --precio: #15803d;     /*  5.0:1 */
  --peligro: #b91c1c;    /*  6.5:1 */
}
```

2. **Tamaño de letra:** ningún texto por debajo de **14 px**; el cuerpo en **16 px**. Usa `rem`.
3. **Responsive** (WCAG 1.4.10): cambia `width: 1200px` por:

```css
body { max-width: 1200px; margin: 0 auto; padding: 0 1rem; }
.grilla { grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); }
```

   Revisa también los anchos fijos de `.buscador input` (400px), `.registro` (600px) y `#modal .caja` (520px): usa `width: 100%` + `max-width`.
4. **Movimiento** (WCAG 2.2.2): elimina el `<marquee>`, la animación `.parpadeo` y el carrusel de testimonios (o agrégale un botón de pausa). Si dejas alguna animación, respeta:

```css
@media (prefers-reduced-motion: reduce) {
  * { animation: none !important; transition: none !important; }
}
```

5. **Un solo estilo** para el botón de compra (WCAG 3.2.4 / Nielsen H4): los seis productos deben usar el mismo botón con el mismo texto: **"Agregar al carrito"**.

✅ **Comprueba:** Lighthouse ya no reporta "Background and foreground colors do not have a sufficient contrast ratio". En `F12 → modo dispositivo → 320 px` no hay scroll horizontal. Con `Ctrl +` al **400 %** todo sigue legible.

💾 **Guarda:** `git commit -am "Paso 7: contraste, tipografía, responsive y movimiento"`

---

## Paso 8: formulario de registro

> Este es el paso más largo. Hoy nadie puede registrarse: no hay etiquetas, el error no dice nada, las reglas de la contraseña son secretas, hay un captcha imposible y un botón que borra la cuenta sin preguntar.

🛠️ **Haz:**

1. **Etiqueta visible para cada campo** (WCAG 1.3.1, 3.3.2). El `placeholder` **no** sustituye a la etiqueta:

```html
<label for="f-correo">Correo electrónico</label>
<input type="email" id="f-correo" name="correo" autocomplete="email" required>
```

2. **Tipos de campo correctos:**
   - Correo → `type="email"` + `autocomplete="email"`.
   - Nombre → `autocomplete="name"`.
   - Contraseña → `autocomplete="new-password"`.
   - Fecha → `type="date"` + `autocomplete="bday"` (adiós al "dd/mm/aaaa **o** mm-dd-aa").
3. **Elimina** el campo "Código del producto que vio en la sección anterior" (Nielsen H6: los productos no muestran ningún código).
4. **Elimina el captcha** de imagen (WCAG 1.1.1 y 3.3.8). Si necesitas frenar robots, usa un campo *honeypot* oculto.
5. **Muestra las reglas de la contraseña antes de escribir** y conéctalas al campo:

```html
<label for="f-clave">Contraseña</label>
<input type="password" id="f-clave" aria-describedby="clave-ayuda">
<p id="clave-ayuda">Mínimo 8 caracteres, con al menos una letra y un número.</p>
```

   (Y ajusta la expresión regular de `enviar()` a esas mismas reglas.)
6. **Errores claros, junto al campo y no solo en rojo** (WCAG 1.4.1, 3.3.1, 3.3.3):

```html
<input id="f-correo" aria-invalid="true" aria-describedby="correo-error">
<p id="correo-error" class="error-texto">⚠ Escribe un correo con el formato nombre@dominio.com</p>
```

   En `enviar()`: por cada campo con error pon `aria-invalid="true"`, muestra **qué falló y cómo arreglarlo** y lleva el foco al primer campo con error (`el.focus()`). Cambia "ERR_VALIDATION_0x8F" y "HTTP 200 OK - payload persistido" por mensajes humanos.
7. **Mensajes que el lector de pantalla anuncie** (WCAG 4.1.3): `<div id="mensaje" role="status">`.
8. **"Borrar todo":** quítalo o pide confirmación.
9. **"Eliminar mi cuenta"** (WCAG 3.3.4 / Nielsen H5):
   - Sácalo del formulario a una sección aparte ("Zona de cuenta").
   - Usa color de peligro y un texto claro.
   - Pide confirmación con un `<dialog>` donde el foco empiece en **"Cancelar"**.
10. **Sesión de 90 s que borra el formulario sin avisar** (WCAG 2.2.1): elimina ese `setInterval`. Si de verdad hace falta un tiempo límite, avisa antes y ofrece "Necesito más tiempo".
11. Usa un `<button type="submit">` y `onsubmit` con `event.preventDefault()` en lugar de `return false` + `<div onclick>`.

✅ **Comprueba:**
- Envía el formulario vacío: cada campo dice **qué** falta, el foco va al primero y el lector de pantalla lo lee.
- Haz clic en una etiqueta: el cursor se coloca en su campo.
- En el móvil, el campo de correo muestra el teclado con `@`.
- "Eliminar mi cuenta" **nunca** borra nada sin confirmar.

💾 **Guarda:** `git commit -am "Paso 8: formulario accesible con validación clara"`

---

## Paso 9: interacciones rápidas (INP)

> **INP** mide cuánto tarda la página en reaccionar a un clic o una tecla. Lo bueno es **menos de 200 ms**. Hoy "Comprar" tarda **1500 ms** y cada tecla del buscador, **450 ms**.

🔎 **Busca:** la función `trabajar(ms)` y **todos** los lugares donde se llama.

🛠️ **Haz:**

1. **Borra** las llamadas a `trabajar()` en `agregar()`, `buscar()` y `enviar()`. Luego borra la función.
2. **Dale feedback inmediato al usuario** al agregar al carrito (Nielsen H1):

```html
<p id="aviso-carrito" role="status"></p>
```

```js
function agregar(nombre) {
  carrito++;
  document.getElementById('cnt').textContent = carrito;
  document.getElementById('aviso-carrito').textContent = '✔ ' + nombre + ' agregado al carrito';
}
```

3. **Buscador útil:** usa `<input type="search">` con `<label>`, espera a que el usuario deje de escribir (*debounce*) y **filtra los productos de verdad**:

```js
var espera;
function buscar(texto) {
  clearTimeout(espera);
  espera = setTimeout(function () {
    // 1. recorre los .producto
    // 2. oculta (atributo hidden) los que no contienen el texto
    // 3. escribe en #resultado (role="status") cuántos encontraste
  }, 250);
}
```

4. **Envío del formulario:** simula la espera con `setTimeout` (no con un bucle), desactiva el botón y muestra "Enviando…" mientras tanto (Nielsen H1).
5. **Bonus (Nielsen H3):** permite **ver y quitar** productos del carrito.

✅ **Comprueba:** `F12 → Performance → graba → clic en "Agregar al carrito"`. No debe aparecer ninguna tarea roja (**Long task** > 50 ms). La extensión **Web Vitals** de Chrome muestra el INP en vivo: debe salir **verde**.

💾 **Guarda:** `git commit -am "Paso 9: interacciones sin bloqueo (INP) y feedback inmediato"`

---

## Paso 10: textos, ayuda y patrones oscuros

> Ahora la página es rápida y accesible. Falta que **hable como el cliente** y que **no lo engañe**.

🛠️ **Haz:**

| Antes | Problema | Cámbialo por (ejemplo) |
|---|---|---|
| "Ejecutar transacción SKU", "Adquirir", "añadir" | Jerga y falta de consistencia (H2, H4) | "Agregar al carrito" |
| "Account", "Shipping", "Items", "Password" | Idioma mezclado (H2) | "Mi cuenta", "Envíos", "Carrito", "Contraseña" |
| "Click aquí", "haga click aquí", "aquí" | Enlaces sin propósito (WCAG 2.4.4) | "Ver términos y condiciones" |
| "ENVÍO GRATIS*" (asterisco sin explicar) | Información oculta (H8) | "Envío gratis en Lima por compras desde S/ 150" |
| `alert('ERR_HELP_404...')` en "Ayuda" | Sin ayuda real (H10) | Sección `#ayuda` con preguntas frecuentes (`<details>`) |
| "No gracias, prefiero pagar precio completo" | *Confirmshaming* | "No, gracias" |
| "Hasta 12 correos diarios" | Abuso | "Un correo por semana. Puedes darte de baja cuando quieras." |
| "Al seguir navegando aceptas todo" | Consentimiento forzado | Botones "Aceptar" y "Rechazar" del mismo tamaño |
| `target="_blank"` sin aviso | Cambio de contexto inesperado | Agrega "(se abre en otra pestaña)" |
| "Query ejecutada: … registros indexados (0 match)" | Jerga técnica (H2) | "Encontramos 2 productos para «mochila»" |

Para la ayuda:

```html
<section id="ayuda">
  <h2>Preguntas frecuentes</h2>
  <details>
    <summary>¿Cuánto demora mi pedido?</summary>
    <p>En Lima de 24 a 48 horas; en provincias de 3 a 7 días.</p>
  </details>
</section>
```

✅ **Comprueba:** pídele a alguien que **no sea de tu grupo** que haga tres tareas sin ayuda: (1) agregar la cámara al carrito, (2) saber cuánto cuesta el envío a provincias y (3) registrarse. Anota en `ENTREGA.md` dónde dudó.

💾 **Guarda:** `git commit -am "Paso 10: lenguaje claro, ayuda y sin patrones oscuros"`

---

## Paso 11: medir el DESPUÉS y entregar

1. Haz `git push` y espera 1 o 2 minutos a que se actualice GitHub Pages.
2. Repite **exactamente** las mismas mediciones del Paso 0 (incógnito, Mobile).
3. Completa la tabla "Después" de `ENTREGA.md` y responde las preguntas de reflexión.
4. Pasa una revisión automática extra con **axe DevTools** o **WAVE** (https://wave.webaim.org/) y anota los errores que queden.

```bash
git add ENTREGA.md
git commit -m "Paso 11: métricas finales y reflexión"
git push
```

5. Entrega en el aula virtual: **link del repositorio** + **link de GitHub Pages**.

### 🎯 Metas mínimas

| Métrica | Meta |
|---|---|
| Lighthouse Performance (Mobile) | ≥ 90 |
| Lighthouse Accessibility | ≥ 95 (ideal 100) |
| LCP | < 2.5 s |
| CLS | < 0.1 |
| TBT | < 200 ms |
| INP al agregar al carrito | < 200 ms |
| Prueba solo con teclado | Se puede completar la compra y el registro |

> ⚠️ **Un 100 en Lighthouse no significa que la página sea accesible.** Las herramientas automáticas detectan entre el 30 % y el 40 % de los problemas. Las pruebas con teclado, lector de pantalla y otra persona (Pasos 6, 8 y 10) **son obligatorias**.

---

## 📋 Rúbrica de evaluación (20 puntos)

| Criterio | Puntos | Excelente | En proceso |
|---|---|---|---|
| Rendimiento (Pasos 2, 3, 4, 9) | 5 | Cumple las metas de LCP, CLS, TBT e INP | Mejora, pero alguna métrica sigue en rojo |
| Accesibilidad (Pasos 1, 5, 6, 7, 8) | 6 | Se usa solo con teclado y lector; contraste correcto; formulario accesible | Lighthouse alto, pero falla la prueba manual |
| Usabilidad (Pasos 8, 10) | 4 | Textos claros, ayuda real, sin patrones oscuros, prueba con usuario documentada | Cambios parciales |
| Proceso (commits y medición) | 3 | Un commit por paso con mensaje claro; antes y después medidos igual | Pocos commits o mediciones incompletas |
| Reflexión (`ENTREGA.md`) | 2 | Explica qué cambio tuvo más impacto y por qué | Respuestas genéricas |

---

## 🧰 Herramientas

- **Lighthouse:** integrado en Chrome (`F12 → Lighthouse`).
- **PageSpeed Insights:** https://pagespeed.web.dev
- **Extensión Web Vitals:** Chrome Web Store → "Web Vitals".
- **axe DevTools:** Chrome Web Store → "axe DevTools".
- **WAVE:** https://wave.webaim.org
- **Contraste:** https://webaim.org/resources/contrastchecker/
- **Lector de pantalla:** NVDA (Windows, gratis: https://www.nvaccess.org) o VoiceOver (Mac: `Cmd + F5`).
- **WCAG 2.2 en español:** https://www.w3.org/WAI/standards-guidelines/wcag/es
- **Guía de Core Web Vitals:** https://web.dev/articles/vitals

---

## ❓ Problemas frecuentes

- **"Hice push y la página publicada no cambia".** Espera 1 o 2 minutos y recarga con `Ctrl + Shift + R`. Revisa la pestaña **Actions** de tu repositorio: el despliegue debe estar en verde.
- **"Lighthouse me da números distintos cada vez".** Es normal. Mide siempre en incógnito, con las demás pestañas cerradas, y si puedes, saca el promedio de 3 ejecuciones.
- **"Rompí algo y no sé qué".** Mira el último cambio con `git diff` o vuelve al commit anterior con `git checkout -- index.html`.
- **"¿Puedo separar el CSS y el JS en otros archivos?"** Sí, y es buena práctica. Si lo haces, carga el JS con `<script src="app.js" defer></script>`.
