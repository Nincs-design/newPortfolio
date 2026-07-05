# ninoo.art — portafolio 3D

Portafolio web 3D: tú diseñas la escena en Blender, este repositorio le da la interactividad
(cámara 360°, etiquetas, modales, objetos clicables y voladores) y GitHub Pages lo publica gratis.

## Estructura

```
index.html      ← el motor de interactividad (edita solo el bloque CONFIG)
escena.glb      ← tu escena exportada de Blender (ahora mismo, la escena de prueba)
.nojekyll       ← evita que GitHub procese los archivos (no tocar)
videos/         ← (crea esta carpeta cuando añadas tus mp4 para las pantallas)
```

## Publicar en GitHub Pages (sin terminal)

1. Entra en [github.com/new](https://github.com/new) y crea un repositorio **público**
   (por ejemplo `portafolio`). No marques ninguna casilla de inicialización.
2. En la pantalla del repo vacío, pulsa **"uploading an existing file"** y arrastra
   `index.html`, `escena.glb` y `.nojekyll`. Pulsa **Commit changes**.
   - Si tu sistema oculta `.nojekyll` (empieza por punto), no pasa nada: puedes crearlo
     después en GitHub con **Add file → Create new file**, nombre `.nojekyll`, contenido vacío.
3. Ve a **Settings → Pages**. En *Build and deployment*:
   - Source: **Deploy from a branch**
   - Branch: **main** / carpeta **/ (root)** → **Save**
4. Espera 1–2 minutos. Tu web estará en:
   `https://TU-USUARIO.github.io/NOMBRE-DEL-REPO/`
   (la URL exacta aparece arriba en esa misma página de Settings → Pages).

## Actualizar la escena

1. Exporta desde Blender: **File → Export → glTF 2.0**, formato **glTF Binary (.glb)**,
   con *+Y Up* activado y *Animation* marcado si tienes animaciones.
   Antes de exportar: `Ctrl+A → All Transforms` sobre tus objetos.
2. Nombra el archivo `escena.glb`.
3. En GitHub: entra en el repo, pulsa sobre `escena.glb` → icono del lápiz no sirve para
   binarios; usa **Add file → Upload files** y sube el nuevo `escena.glb` (sobrescribe el
   anterior) → **Commit changes**.
4. Espera ~1 minuto y recarga la web con **Ctrl+Shift+R** (recarga forzada, para saltarte
   la caché del navegador).

Lo mismo para editar textos: abre `index.html` en GitHub, pulsa el lápiz, edita el bloque
`CONFIG` y haz commit.

## Añadir vídeos a las pantallas

1. Crea la carpeta subiendo un archivo con ruta: **Add file → Upload files** no permite
   carpetas vacías, así que sube directamente tus mp4 escribiendo `videos/` al arrastrar,
   o usa **Create new file** con nombre `videos/.gitkeep` primero.
2. En `index.html`, dentro de `CONFIG.videos`, descomenta y apunta a tus archivos:
   ```js
   videos: {
     modeling: 'videos/reel-modeling.mp4',
   },
   ```
3. Comprime los vídeos (H.264, 720p suele sobrar para una pantalla dentro de la escena).
   GitHub rechaza archivos de más de 100 MB y penaliza repos pesados: intenta que cada
   mp4 quede por debajo de ~20 MB.

## Convenciones de nombres en Blender

El sufijo `.001`, `.002`… que añade Blender se ignora automáticamente.

| Nombre en Blender        | Comportamiento                                                        |
|--------------------------|-----------------------------------------------------------------------|
| `btn_<id>`               | Clicable: crece al pasar el ratón y abre `CONFIG.modals[<id>]`        |
| `tag_<id>`               | Empty: ancla la etiqueta HTML `CONFIG.tags[<id>]` con línea de cota   |
| `screen_<id>`            | Su material reproduce `CONFIG.videos[<id>]` en loop (silenciado)      |
| `fly_<id>`               | Vuela por la ruta de empties `route_<id>_00`, `_01`, `_02`…           |
| `route_<id>_<nn>`        | Empties que definen la ruta (curva cerrada suave)                     |
| `spin_x_… / spin_y_… / spin_z_…` | Rotación continua en ese eje                                  |
| `float_…`                | Levita arriba y abajo suavemente                                      |
| `cam_center`             | Empty: posición de la cámara                                          |
| `cam_look`               | Empty: hacia dónde mira la cámara al cargar                           |

Las animaciones exportadas desde Blender (keyframes, esqueletos, shape keys) se
reproducen todas automáticamente en bucle.

## Comprobar que todo funciona

Abre la web, pulsa **F12 → Console**. Al cargar verás un resumen del tipo:

```
Escena cargada: 2 clicables, 2 etiquetas, 1 volador, 1 spin, 1 float, 0 animaciones de Blender
```

Si un nombre no casa con las convenciones o falta una clave en `CONFIG`, aparecerá
un aviso ahí mismo indicando cuál.
