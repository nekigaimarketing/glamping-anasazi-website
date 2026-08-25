# Glamping Anasazi — Sitio Web Oficial & Integración KepLead CRM

Sitio web oficial de alta conversión para **Glamping Anasazi** (Samalayuca, Chihuahua), con soporte bilingüe (ES/EN), sistema de reservaciones conectado a WhatsApp y webhook directo a KepLead CRM.

---

## 📁 Estructura del Proyecto

```
glamping-anasazi-website/
├── index.html                  # Código principal HTML5 + CSS + JS (Optimizado)
├── assets/
│   ├── images/
│   │   └── logo.png            # Logotipo oficial Anasazi Glamping
│   └── videos/
│       └── hero.mp4            # Video de fondo para el Hero (Colocar aquí)
└── README.md                   # Esta guía
```

---

## 🎥 Guía de Video para el Hero (`hero.mp4`)

Para que el video de fondo cargue al instante en computadoras y celulares sin ralentizar el sitio:

### 1. Especificaciones Técnicas Recomendadas
- **Formato**: `MP4` (Contenedor estándar web).
- **Códec de Video**: `H.264` (AVC) — Máxima compatibilidad con Safari iOS, Chrome y Firefox.
- **Resolución**: `1920x1080` (Full HD) o `1280x720` (720p).
- **Cuadros por segundo (FPS)**: `24 fps` o `30 fps`.
- **Audio**: **ELIMINAR LA PISTA DE AUDIO (MUTE COMPLETO)**. Al no tener audio, ahorras un 30% del peso del archivo y aseguras el `autoplay` en dispositivos móviles.
- **Tasa de bits (Bitrate)**: `1.5 Mbps - 2.5 Mbps` (Bitrate constante o variable en 2 pasadas).
- **Peso objetivo del archivo**: Entre **3 MB y 8 MB** (máximo 15 MB).
- **Duración ideal**: Entre `10 y 20 segundos` en bucle suave (seamless loop).

### 2. Cómo Comprimirlo Fácilmente (Herramientas Gratis)
*   **Opción A: HandBrake (Recomendado)**:
    1. Descarga [HandBrake](https://handbrake.fr/).
    2. Arrastra tu video original.
    3. Preset: Selecciona `Web -> Discord / Web 1080p30`.
    4. En la pestaña **Audio**: Elimina todas las pistas de audio (`Tracks -> Clear`).
    5. En la pestaña **Video**: RF (Calidad constante) entre `24` y `28`.
    6. Guarda como `hero.mp4` y muévelo a `assets/videos/hero.mp4`.
*   **Opción B: Con FFmpeg (Línea de comando)**:
    ```bash
    ffmpeg -i video_original.mov -vcodec h264 -an -crf 26 -preset slow -movflags +faststart assets/videos/hero.mp4
    ```

---

## 🚀 Cómo Subir a GitHub y Desplegar en Cloudflare Pages

### Paso 1: Inicializar Git y subir a GitHub
Abre tu terminal en la carpeta del proyecto:
```bash
cd "/Users/nateramacair/Documents/N8N Software Creator/glamping-anasazi-website"
git init
git add .
git commit -m "Initial commit - Glamping Anasazi Website"
git branch -M main
```
1. Ve a [GitHub.com](https://github.com/) → **New repository**.
2. Nómbralo `glamping-anasazi-website` (selecciona **Private**).
3. Copia los comandos para subir tu repo existente:
```bash
git remote add origin https://github.com/TU_USUARIO/glamping-anasazi-website.git
git push -u origin main
```

---

### Paso 2: Desplegar en Cloudflare Pages
1. Inicia sesión en [Cloudflare Dashboard](https://dash.cloudflare.com/).
2. En el menú lateral izquierdo, ve a **Workers & Pages** → **Create application** → Pestaña **Pages**.
3. Selecciona **Connect to Git** → Elige tu repositorio `glamping-anasazi-website`.
4. En la configuración de build:
   - **Framework preset**: `None`
   - **Build command**: *(Dejar en blanco)*
   - **Build output directory**: `.` (o dejar en blanco)
5. Haz clic en **Save and Deploy**.
6. ¡En 30 segundos tu sitio estará en vivo con HTTPS gratuito! (ej: `glamping-anasazi.pages.dev`).

---

### Paso 3: Conectar el Dominio de Hostinger a Cloudflare
1. En tu proyecto de Cloudflare Pages, ve a **Custom domains** → **Set up a custom domain**.
2. Escribe el dominio del cliente (ej: `glampinganasazi.com` o `www.glampinganasazi.com`).
3. Ve al panel de Hostinger (**hPanel**) → **DNS Zone Editor**:
   - Agrega un registro **CNAME**:
     - **Name / Host**: `@` (o `www`)
     - **Target / Points to**: `glamping-anasazi.pages.dev`
     - **TTL**: `Default` o `300`

---

## 🤖 Integración con KepLead CRM

El formulario de reservación (`#bookingForm`) captura:
- Nombre completo
- Teléfono / WhatsApp (10 dígitos)
- Tipo de Alojamiento
- Fecha de Check-in y Check-out
- Número de Huéspedes
- Peticiones especiales

Al hacer clic en **Enviar Solicitud**:
1. Envía un `POST` con formato JSON al webhook de n8n:
   ```json
   {
     "name": "Juan Pérez",
     "phone": "6561234567",
     "accommodation": "Suite con Rooftop",
     "checkin": "2026-09-10",
     "checkout": "2026-09-12",
     "guests": "2",
     "comments": "Llegamos en la tarde",
     "source": "website",
     "tenant_id": "glamping-anasazi"
   }
   ```
2. Redirige automáticamente al usuario a WhatsApp con el mensaje ya estructurado para asegurar un 100% de conversión inmediata.
