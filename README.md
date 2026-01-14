# BackJuice Portal - Landing Page

Landing page promocional para BackJuice Portal, la aplicacion de gestion de cuotas para tesoreros y apoderados.

## Tecnologias

- HTML5
- Tailwind CSS (CDN)
- Google Fonts (Inter)

## Estructura

```
backjuice-landing/
├── index.html      # Pagina principal
├── css/            # Estilos adicionales (si se necesitan)
├── js/             # Scripts adicionales (si se necesitan)
├── img/            # Imagenes y screenshots
└── README.md
```

## Secciones

1. **Hero** - Propuesta de valor principal
2. **Problema** - Dolor del tesorero tradicional
3. **Features** - Funcionalidades principales
4. **Para Tesoreros** - Beneficios especificos
5. **Para Apoderados** - Beneficios especificos
6. **Beneficios** - Estadisticas de impacto
7. **Download** - CTA de descarga
8. **Footer** - Links y contacto

## Desarrollo Local

Simplemente abre `index.html` en tu navegador o usa un servidor local:

```bash
# Con Python
python -m http.server 8000

# Con Node.js (npx)
npx serve .
```

## Deploy

### GitHub Pages

1. Crear repositorio en GitHub
2. Push del codigo
3. Settings > Pages > Source: main branch
4. La pagina estara en: `https://tu-usuario.github.io/backjuice-landing`

### Netlify

1. Conectar repositorio de GitHub
2. Deploy automatico en cada push
3. URL personalizada disponible

## Personalizacion

### Colores

Los colores se configuran en el `<script>` de Tailwind:

```javascript
tailwind.config = {
    theme: {
        extend: {
            colors: {
                primary: { ... },
                accent: { ... }
            }
        }
    }
}
```

### Contenido

Editar directamente el HTML. Las secciones estan claramente marcadas con comentarios.

## Screenshots

Para agregar screenshots de la app:

1. Capturar pantallas de la app
2. Guardar en `/img/`
3. Reemplazar los mockups de ejemplo en el HTML

## Licencia

MIT
