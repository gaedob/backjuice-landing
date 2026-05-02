# Revisión de la Landing — Kuota

**Fecha:** 1 de mayo de 2026
**Archivo revisado:** `index.html` (1.527 líneas)
**Foco:** mobile-first
**Frentes:** Diseño visual / UX · Código HTML/CSS · SEO

---

## Resumen ejecutivo

La landing tiene una base sólida: estética dark/coral consistente, jerarquía tipográfica fuerte, copy con voz definida ("Cuotas sin perseguir. Pagos sin Excel."), y una estructura de secciones lógica. El código está limpio, sin frameworks pesados, todo en un solo archivo.

Los tres problemas más urgentes:

1. **El mockup del dashboard se oculta en mobile** — y mobile es tu prioridad. El hero pierde su mejor activo visual justo donde más usuarios entrarán.
2. **La sección "Ingeniería que no improvisa" habla a desarrolladores, no a tesoreros.** Tu público objetivo (centros de padres, juntas de vecinos, clubes) no sabe qué es Supabase, Flet, ni le importan 1.013 tests. Esa sección desinfla la promesa "sin Excel, simple".
3. **Faltan screenshots reales de la app.** Todos los "mockups" son divs estilizados. Para una landing de producto esto es crítico — la gente compra lo que ve, no lo que imagina.

Detalle abajo, organizado por prioridad.

---

## 🔴 Críticos (arreglar primero)

### 1. Mockup oculto en mobile (UX)

**Dónde:** línea 466 — `.hero-mockup { display: none; }` en `@media (max-width: 768px)`

**Problema:** En desktop el hero es texto + dashboard mockup. En mobile solo queda el texto. Pierdes el "wow visual" justo en el dispositivo prioritario.

**Qué hacer:**
- Mostrar una versión simplificada del mockup debajo del CTA en mobile (no al lado).
- O reemplazarlo por un screenshot real de la app móvil (más impacto que un mockup en HTML).

### 2. Links a la app van a `http://` (Seguridad + SEO)

**Dónde:** todos los `href="http://myapp-flet.onrender.com/"` (líneas 500, 517, 548, 1007, 1026, 1046, 1105, 1135).

**Problema:** Mixed-content. Si la landing está en HTTPS, los navegadores bloquean los enlaces o muestran advertencias. Google también penaliza.

**Qué hacer:** cambiar todos a `https://myapp-flet.onrender.com/`. Idealmente, un dominio propio (ej: `app.kuota.app`).

### 3. Falta `og:image` (SEO + redes sociales)

**Dónde:** las meta tags Open Graph (líneas 12-15).

**Problema:** Cuando alguien comparte el link en WhatsApp, Slack o Twitter, no aparece imagen — solo texto. Para una landing de producto, esto reduce drásticamente el CTR.

**Qué hacer:** agregar:

```html
<meta property="og:image" content="https://kuota.app/og-image.png">
<meta property="og:image:width" content="1200">
<meta property="og:image:height" content="630">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:image" content="https://kuota.app/og-image.png">
```

Crear un PNG de 1200×630px con el logo, headline y un mockup. Guardarlo en la raíz como `og-image.png`.

### 4. Sección "Ingeniería" desconecta del público objetivo (UX/Copy)

**Dónde:** líneas 839-936 — toda la sección "Construido para durar" + stack técnico.

**Problema:** Tu propuesta de valor es "sin Excel, sin caos, simple para tesoreros". Pero le hablas a un tesorero de un curso de colegio sobre "JWT cifrado con Fernet, RLS en Supabase, exponential backoff". Es ruido para ese público.

**Qué hacer:** dos opciones:
- **A (recomendado):** quitar la sección. Mover el contenido técnico a una página `/tech` o `/seguridad` enlazada desde el footer.
- **B:** reescribir con beneficios, no specs. Ejemplo: en vez de "1.013 tests automatizados" → "Probado en miles de escenarios reales antes de cada actualización". En vez de "Supabase + SQLite" → "Tus datos siempre disponibles, incluso sin internet".

---

## 🟡 Importantes (mejoran mucho la conversión)

### 5. Falta social proof / screenshots reales

No hay testimonios, logos de instituciones que ya usan Kuota, ni capturas reales de la app. La gente compra confianza, y eso se construye con prueba social.

**Qué agregar:**
- 1 sección con 2-3 testimonios cortos de tesoreros reales (con foto, nombre, curso/institución).
- Logos en grayscale de colegios/clubes que usen Kuota.
- 3-4 screenshots reales de la app móvil en lugar de los mockups dibujados.
- Si tienes números: "Usado por X centros de padres", "Y participantes activos".

### 6. CTAs inconsistentes

A lo largo de la página alternas: "Empezar gratis", "Comenzar", "Comenzar Pro", "Crear cuenta gratis", "Abrir app". Esto fragmenta el mensaje.

**Recomendación:** definir 1 CTA primario ("Empezar gratis") y 1 secundario ("Hablar con el equipo") y repetirlos consistentemente.

### 7. Stats que no comunican (sección de números)

`1.013+ Tests automatizados` no significa nada para un tesorero. Reemplazar por métricas que importan al usuario:

- "Grupos activos" / "Centros de padres usándolo"
- "Pagos procesados al mes"
- "Promedio de tiempo ahorrado por tesorero/mes"
- "Tasa de morosidad reducida en grupos que usan Kuota"

Si aún no tienes esos números, prefiere cualidades concretas: "Setup en 5 minutos", "0 instalación para apoderados", "Datos cifrados de extremo a extremo".

### 8. 4 planes es demasiado

Free / Starter / Pro / Enterprise — en mobile se ve como una lista larga y diluye la decisión.

**Recomendación:** quitar "Starter" o fusionarlo. Tres planes (Free / Pro / Enterprise) es el patrón estándar y reduce la fricción de elección.

### 9. Falta scroll-padding-top

**Dónde:** los anchors (`#funcionalidades`, `#planes`, etc.) hacen scroll, pero el nav fijo (64px) tapa el inicio de la sección.

**Fix:** agregar al CSS:
```css
html { scroll-padding-top: 80px; }
```

---

## 🟢 Recomendaciones de calidad de código

### 10. Tailwind CDN en producción

`<script src="https://cdn.tailwindcss.com"></script>` (línea 19) — el propio Tailwind advierte que no se use en producción. Es lento (parsea CSS en el cliente), no se purga, y aumenta el peso de la página.

Además, casi no usas Tailwind: la mayoría es CSS custom. **Recomendación:** quitar el CDN completo. Tu CSS ya cubre todo.

### 11. Inline styles excesivos

Hay cientos de `style="..."` inline. Hace el HTML difícil de mantener y ensucia el DOM.

**Recomendación:** mover a clases en el `<style>`. Por ejemplo, en vez de `style="font-size:18px;font-weight:700;margin:10px 0 8px;"` repetido en cada problema, crear `.problem-title`.

### 12. Layout responsive en JavaScript

`updateGrids()` (línea 1463) cambia `gridTemplateColumns` desde JS escuchando `resize`. Esto:
- Causa flash al cargar (FOUC)
- Es frágil (si JS falla, layout se rompe)
- Se podría hacer 100% con media queries CSS

**Recomendación:** mover esa lógica a `@media` queries puras.

### 13. Iconos: mezcla de SVG y emoji

En la sección de feature tabs (`📊`, `📱`, `🎓`, `⚽`, etc.) y en otras secciones usas emojis. Pero el resto son SVG estilizados. Resultado: los emojis se ven distinto en iOS, Android, Windows y Linux.

**Recomendación:** reemplazar emojis por SVG inline o usar Lucide/Heroicons. Mantiene look consistente.

### 14. Accesibilidad

- Los botones de FAQ usan `<button>` pero el patrón nativo es `<details><summary>`. Mejor para SEO y screen readers.
- Los SVG inline no tienen `<title>` ni `aria-label`. Screen readers no los anuncian.
- Los `<button>` sin `type="button"` pueden submitear forms si quedan dentro de uno.
- Contraste: `--text-3: #525248` sobre `--bg: #0b0b0a` da ratio bajo (~4.5:1, justo en el límite WCAG AA). Considera subirlo a `#6e6d62` para cumplir AA holgado.

### 15. Favicon insuficiente

Solo tienes `<link rel="icon">` SVG inline. Falta:

```html
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon.png">
<meta name="theme-color" content="#f97316">
<meta name="apple-mobile-web-app-capable" content="yes">
```

`theme-color` cambia el color de la barra del navegador en mobile — pequeño detalle que se nota mucho.

---

## 🔵 SEO — checklist completo

### Lo que está bien ✅
- `<title>` descriptivo con marca y propuesta
- `meta description` clara y con keywords
- `lang="es"` correcto
- Canonical link presente
- Estructura de headings correcta (1× H1, varios H2, H3 anidados)
- Open Graph básico

### Lo que falta ❌
- `og:image`, `twitter:card`, `twitter:image` (crítico — ya mencionado en #3)
- JSON-LD Schema.org. Para una app deberías tener:
  - `SoftwareApplication` schema
  - `Organization` schema
  - `FAQPage` schema (tienes una sección de FAQ, aprovéchala)
- `robots.txt` y `sitemap.xml` en la raíz del sitio
- `meta name="robots" content="index,follow"` explícito
- `theme-color` para mobile

### Ejemplo de FAQPage Schema

Agregar al `<head>`:

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Kuota es gratuito de verdad?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí. El plan Free no tiene fecha de vencimiento..."
      }
    }
    /* ...resto de FAQs */
  ]
}
</script>
```

Esto puede hacer que tus FAQs aparezcan directamente en Google con formato expandido (rich snippets).

---

## ⚙️ Lista de "quick wins" (≤ 30 min cada uno)

1. Cambiar todos `http://` por `https://`
2. Agregar `og:image` y `twitter:card` (con imagen 1200×630)
3. Agregar `<meta name="theme-color" content="#f97316">`
4. Agregar `html { scroll-padding-top: 80px; }`
5. Quitar el `<script src="https://cdn.tailwindcss.com">` (no lo usas casi)
6. Cambiar emojis de feature tabs por SVG
7. Subir contraste de `--text-3`
8. Agregar JSON-LD para FAQPage

## 🏗️ Cambios mayores (≥ 2 horas cada uno)

1. Repensar / quitar sección "Ingeniería"
2. Crear screenshots reales de la app y reemplazar mockups en HTML
3. Agregar sección de social proof (testimonios + logos)
4. Mostrar versión móvil del dashboard mockup en mobile
5. Reducir a 3 planes
6. Mover layout responsive de JS a CSS

---

## Herramientas que recomiendo para el siguiente paso

Si quieres avanzar en **diseño visual** (rehacer mockups, screenshots, OG image):
- **Figma** (gratis, lo ideal para mockups y exportar a PNG)
- **Excalidraw** o **tldraw** si quieres algo rápido y a mano alzada

Si quieres avanzar en **código**:
- **Tailwind CSS compilado** con CLI (no CDN) si decides quedarte con utility-first
- O quedarte con CSS custom (lo que ya tienes funciona bien)

Si quieres validar **SEO**:
- **Google PageSpeed Insights** (mide Core Web Vitals)
- **Lighthouse** (en Chrome DevTools, tab "Lighthouse") — auditoría completa
- **Schema.org Validator** para revisar el JSON-LD

---

*Cualquier punto que quieras profundizar o aplicar — solo dime cuál y lo implementamos.*
