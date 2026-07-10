# Merlin Arcana — Sitio Fase 1 (informativo)

Sitio estático autocontenido (un solo `index.html`, sin dependencias externas).

## Estructura

- `index.html` — el sitio completo (HTML + CSS + JS). GitHub Pages exige este nombre en la raíz.
- El archivo `.env` / `.env.example` **ya no es necesario**: GitHub Pages es hosting estático y no ejecuta backend ni lee variables de entorno. El envío del formulario se hace vía Formspree, cuyo endpoint es público por diseño.

## Paso 1 — Activar el formulario (Formspree)

1. Crear cuenta gratis en https://formspree.io (registrarse con `merlinarcana@merlinarcana.com`).
2. Crear un nuevo Form. Como correo de destino, dejar `merlinarcana@merlinarcana.com` (habrá que confirmar un correo de verificación que llega a Zoho).
3. Formspree da un endpoint tipo `https://formspree.io/f/abcdwxyz`.
4. En `index.html`, buscar `CONTACT_CONFIG` y pegar el endpoint:

```js
const CONTACT_CONFIG = {
  endpoint: "https://formspree.io/f/abcdwxyz"
};
```

El formulario ya incluye: estados del botón (Enviando…), mensaje de éxito/error inline, y honeypot anti-spam.

## Paso 2 — Publicar en GitHub Pages

1. Crear repo en GitHub (p. ej. `merlin-arcana`), subir `index.html` a la raíz de la rama `main`.
2. En el repo: **Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save**.
3. En 1–2 minutos el sitio queda en `https://<usuario>.github.io/merlin-arcana/`.

## Paso 3 — Conectar el dominio merlinarcana.com

1. En **Settings → Pages → Custom domain**, escribir `merlinarcana.com` y guardar (esto crea un archivo `CNAME` en el repo).
2. En **Porkbun → merlinarcana.com → DNS**:
   - **Eliminar** el registro `ALIAS merlinarcana.com → uixie.porkbun.com` y el `CNAME *.merlinarcana.com → uixie.porkbun.com` (son del parking de Porkbun; al usar GitHub Pages ya no aplican).
   - **Agregar** 4 registros `A` (host vacío) con las IPs de GitHub Pages:
     - `185.199.108.153`
     - `185.199.109.153`
     - `185.199.110.153`
     - `185.199.111.153`
   - **Agregar** un `CNAME` con host `www` → `<usuario>.github.io`
   - ⚠️ **NO tocar**: los MX de Zoho, el SPF, el DKIM (`zmail._domainkey`) ni el TXT `zoho-verification`. El correo es independiente del hosting web y debe seguir intacto.
3. De vuelta en GitHub Pages, esperar a que valide el dominio y marcar **Enforce HTTPS**.

## Verificación final

- `https://merlinarcana.com` carga el sitio con candado (HTTPS).
- El formulario envía y el registro llega a la bandeja de Zoho.
- Enviar/recibir correo sigue funcionando (los cambios de DNS del sitio no tocan los MX).
