# Quepa — Landings

Landings web de Quepa: agente conversacional de IA que vive en WhatsApp y recomienda lugares.

## Estructura

```
quepa-web/
├── index.html             ← Landing B2C (quepa.co)
├── comercios/
│   └── index.html         ← Landing B2B (comercios.quepa.co)
├── vercel.json            ← Config de routing y dominios para Vercel
├── netlify.toml           ← Config equivalente para Netlify
├── .gitignore
└── README.md              ← este archivo
```

Cada HTML es **autocontenido**: SVGs inlineados, fuentes desde Google Fonts CDN, JS embebido. No hay build step. No hay dependencias externas.

## Antes de publicar

Hay tres placeholders que hay que reemplazar:

### 1. Waitlist endpoint (`index.html`, B2C)

Busca esta línea (cerca del final del archivo, dentro del `<script>`):

```js
const WAITLIST_ENDPOINT = ""; // ej: "https://formspree.io/f/abcd1234"
```

Reemplaza la cadena vacía por tu endpoint real. Opciones:

- **Formspree** → `https://formspree.io/f/<ID>` (gratis hasta 50 envíos/mes)
- **Getform** → `https://getform.io/f/<ID>`
- **Netlify Forms** → si despliegas en Netlify, agrega `data-netlify="true"` al `<form>` y omite el endpoint
- **Tally / Notion** → endpoint propio
- **Backend propio** → tu URL POST que recibe `{ email, city?, source }`

### 2. Cal.com / Calendly URL (`comercios/index.html`, B2B)

Busca esta línea:

```js
const DEMO_URL = "https://cal.com/quepa/demo"; // <-- reemplazar cuando tengamos el link real
```

Cambia por tu link real de agenda.

### 3. Cross-links B2C ↔ B2B

Si vas a usar dominios distintos a `quepa.co` y `comercios.quepa.co`, busca y reemplaza esas dos cadenas en ambos HTMLs (varios `<a href>` y enlaces del footer).

## Deploy — opciones

### Opción A · Vercel (recomendada)

1. Crea cuenta en [vercel.com](https://vercel.com) y conecta tu GitHub.
2. **Import Project** → selecciona el repo.
3. Vercel detecta automáticamente el sitio estático. **Deploy**.
4. Para tener `quepa.co` y `comercios.quepa.co` como subdominio real:
   - **Project Settings → Domains → Add** `quepa.co`
   - El B2B en `/comercios` quedará accesible en `quepa.co/comercios` por defecto.
   - Para el subdominio aparte (`comercios.quepa.co`) necesitas un **segundo proyecto en Vercel** apuntando al mismo repo pero con **Root Directory = `comercios`**.

### Opción B · Netlify

1. Crea cuenta en [netlify.com](https://netlify.com) y conecta GitHub.
2. **Add new site → Import from Git**.
3. Build command: vacío. Publish directory: `.` (raíz).
4. Para subdominios distintos, mismo patrón que Vercel: dos sitios apuntando a la misma repo, uno con base en `/`, otro con base en `/comercios`.

### Opción C · GitHub Pages (más simple, sin subdominios)

1. En tu repo, **Settings → Pages → Source: main / root**.
2. Tu landing B2C queda en `https://<usuario>.github.io/<repo>/` y el B2B en `https://<usuario>.github.io/<repo>/comercios/`.
3. Para dominio propio, configura un `CNAME` en el repo y en tu DNS.

## Subir el repo a GitHub — paso a paso

Asumiendo que ya estás en esta carpeta (`quepa-web/`):

```bash
# 1. Inicializar repo local
git init
git add .
git commit -m "Quepa — landings B2C + B2B v1.0"

# 2. Renombrar rama a main si fuera necesario
git branch -M main

# 3. Crear repo en github.com (sin README ni .gitignore — los tienes ya)
#    Después copia la URL que GitHub te da, ej: https://github.com/<usuario>/quepa-web.git

# 4. Conectar y empujar
git remote add origin https://github.com/<usuario>/quepa-web.git
git push -u origin main
```

Eso es todo. A partir de ahí cada vez que cambies algo:

```bash
git add .
git commit -m "describe el cambio"
git push
```

Y si tienes Vercel/Netlify conectado, el sitio se redeploya automáticamente en ~30 segundos.

## Cosas que valdría la pena agregar más adelante

- **Open Graph image (`og:image`)** — un PNG 1200×630 con el lockup Quepa para previews en WhatsApp/Twitter/LinkedIn. Falta en ambos HTMLs.
- **Google Tag Manager / Plausible** para analítica.
- **Sitemap.xml** y **robots.txt** para SEO.
- **Hosteo de fuentes propio** (descargar Hanken Grotesk + JetBrains Mono y servirlas con `@font-face`) para no depender de Google Fonts CDN.
- **Compresión de imágenes** cuando agreguen fotos reales de lugares al catálogo.

## Versión

v1.0 · Mayo 2026
