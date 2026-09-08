# Miis Cosmetics — landing page

Landing de una sola página para agendar citas de maquillaje.

## Estado del proyecto

Última actualización: 6 de septiembre de 2026.

- **Sitio publicado:** https://miis-cosmetics-guybt97-4822.vercel.app
- **Repositorio:** https://github.com/guybrutus/Landing-page-Miis-Cosmetics
- **Vercel:** cuenta `guybt97-4822`, proyecto `miis-cosmetics`.

La página está terminada de diseño y funcionamiento. Lo que falta son los datos reales.

### Ojo: GitHub y Vercel NO están conectados

El sitio se desplegó subiendo los archivos directamente, no desde el repositorio.
**Hacer `push` a GitHub no actualiza el sitio publicado.** Son dos pasos separados.

Conectar el repo a Vercel sigue pendiente. Al importarlo probablemente se cree un
proyecto y una URL nuevos en vez de reutilizar `miis-cosmetics`, así que habría que
decidir cuál se queda y borrar el otro.

### Pendiente antes de darla por terminada

Todos los datos visibles son de ejemplo:

- [ ] **WhatsApp real.** Hoy dice `5210000000000`, así que ninguna solicitud de cita llega a nadie.
- [ ] **Correo real.** Hoy dice `hola@miiscosmetics.com`.
- [ ] **Precios reales.** Los cinco servicios tienen tarifas inventadas.
- [ ] **Fotos del portafolio.** Los cuatro marcos están vacíos.
- [ ] **Instagram** del pie de página.

Dónde cambiar cada cosa está explicado en la sección siguiente.

### Decisiones ya tomadas (para no rediscutirlas)

- Sin frameworks, sin dependencias y sin paso de build: un solo archivo HTML.
- Sin backend. El contacto va por WhatsApp a propósito, en vez de montar un servidor.
- `page.html` es la fuente; `index.html` se genera a partir de ella. **No editar `index.html` a mano.**
- La raíz del repositorio es la carpeta de la landing, para que `index.html` quede en la raíz
  y Vercel o GitHub Pages lo encuentren sin configurar nada.

### Si trabajas en la copia local de Windows

La carpeta es `D:\GUY CLAUDE CODE\PRIMERA LANDING PAGE`. Esa unidad no registra ownership,
así que git falla con *dubious ownership*. Se arregla una sola vez:

```bash
git config --global --add safe.directory "D:/GUY CLAUDE CODE/PRIMERA LANDING PAGE"
```

Trabajando desde la nube esto no aplica.

## Archivos

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La página completa. Ábrela con doble clic o súbela a cualquier hosting. |
| `page.html` | La misma página sin `<html>/<head>/<body>`. Es la fuente del Artifact publicado. |

Si editas `page.html`, regenera `index.html` con:

```bash
cd "D:/GUY CLAUDE CODE/PRIMERA LANDING PAGE" && { printf '%s\n' '<!doctype html>' '<html lang="es">' '<head>' '<meta charset="utf-8">' '<meta name="viewport" content="width=device-width, initial-scale=1">' '<meta name="description" content="Miis Cosmetics: maquillaje profesional para novia, editorial y evento social. Agenda tu cita en linea.">'; sed 's|<!--BODY-->|</head>\n<body>|' page.html; printf '%s\n' '</body>' '</html>'; } > index.html
```

## Qué tienes que cambiar antes de publicarla

1. **Teléfono y correo.** Al inicio del `<script>`, en el bloque `CONFIGURA AQUI TUS DATOS`:
   - `WHATSAPP` — clave de país + número, sin `+` ni espacios (ej. `521555123456`).
   - `EMAIL` — correo de contacto.
   - `CIERRA` — días cerrados (`0` domingo, `1` lunes).
   Cambia también los mismos datos en el footer (`wa.me/...`, `mailto:...`, `instagram.com/...`).

2. **Precios y tiempos.** Están en la sección `SERVICIOS` de `page.html`, en `.svc-price` y `.svc-meta`. Los valores actuales son de ejemplo. Si cambias los nombres de servicio, actualiza también el `<select id="servicio">` y el objeto `NOMBRES` del script.

3. **Fotos del portafolio.** Cada marco es un `<figure class="shot">`. Para poner una foto real:

   ```html
   <figure class="shot"><img src="fotos/novia-01.jpg" alt="Maquillaje de novia en luz natural"><span>Novia · luz natural</span></figure>
   ```

   Formato vertical 3:4, y comprímelas (WebP o JPG < 300 KB) para que la página cargue rápido.

4. **Horario y datos del estudio.** En el footer y en el riel de la sección `04 / Agenda`.

## Cómo funciona el formulario

No hay servidor. Al enviar, la página valida los datos, arma un resumen y genera dos enlaces: uno a WhatsApp con el mensaje ya escrito y otro `mailto:`. Reglas de validación:

- WhatsApp de al menos 8 dígitos.
- Fecha de hoy en adelante, con **48 horas** mínimo de anticipación.
- Domingos y lunes rechazados (configurable en `CIERRA`).

Si más adelante quieres que las citas lleguen a una hoja de cálculo o a un calendario, el punto de enganche es el `form.addEventListener("submit", ...)`: ahí ya tienes el objeto con todos los datos listo para mandarlo a Formspree, Google Forms o tu propio endpoint.

## Detalles técnicos

- Sin frameworks ni dependencias: un solo archivo HTML.
- Tipografías desde Google Fonts: Bodoni Moda, Archivo, IBM Plex Mono.
- Tema claro y oscuro automáticos según la preferencia del sistema.
- La escalera de tonos del hero se dibuja en `<canvas>`; los colores están en el array `TONOS`.
