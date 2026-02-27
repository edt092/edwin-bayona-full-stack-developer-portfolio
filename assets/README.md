# assets/

Esta carpeta contiene los GIFs de demostración de cada proyecto para el portfolio.

## Archivos esperados

| Archivo                  | Proyecto              | Flujo a capturar                                      |
|--------------------------|-----------------------|-------------------------------------------------------|
| `gif-pawart.gif`         | PawArtStudio          | subir foto mascota → generar arte IA → vista 3D       |
| `gif-promogimmicks.gif`  | PromoGimmicks         | navegar catálogo → filtrar categoría → contacto       |
| `gif-ksp.gif`            | KS Promocionales      | abrir editor canvas → subir logo → ajustar → WhatsApp |
| `gif-elfogon.gif`        | El Fogón Gourmet      | hero → scroll menú → reseñas → mapa → WhatsApp        |
| `gif-parasoles.gif`      | Parasoles San Andrés  | hero → sección productos → CTA WhatsApp               |
| `gif-frases.gif`         | FrasesRandom          | obtener frase → copiar → guardar favorito → compartir |

## Cómo grabar los GIFs

### Herramientas recomendadas
- **ScreenToGif** (Windows) — https://www.screentogif.com/
  Gratis, liviano, permite recortar área y optimizar frames
- **GIPHY Capture** (Mac) — https://giphy.com/apps/giphycapture
- **LICEcap** (Windows/Mac) — https://www.cockos.com/licecap/

### Configuración recomendada
- **Área de captura**: 800 × 500 px (o 1280 × 720 para mayor calidad)
- **Duración**: 10–15 segundos máximo
- **FPS**: 12–15 fps (balance entre fluidez y tamaño de archivo)
- **Tamaño máximo**: < 5 MB por GIF para carga rápida

### Pasos en ScreenToGif
1. Abre el proyecto localmente (`npm run dev`)
2. Abre ScreenToGif → "Recorder"
3. Ajusta el marco al navegador
4. Graba el flujo principal del proyecto
5. En el editor: File → Save As → GIF
6. Guarda con el nombre exacto de la tabla de arriba

### Agregar el GIF al portfolio
En `index.html`, busca el comentario `GIF_PLACEHOLDER: gif-NOMBRE.gif`
y reemplaza el div placeholder con:
```html
<img src="assets/gif-NOMBRE.gif"
     alt="Descripción del proyecto"
     class="w-full h-full object-cover object-top group-hover:scale-105 transition-transform duration-700">
```
