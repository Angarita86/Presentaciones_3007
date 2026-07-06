# Informe de Seguimiento Interactivo — Convenio 3007 de 2025

Informe web interactivo para el seguimiento de la implementación de Planes Integrales
de Reparación Colectiva (Unidad para las Víctimas · CISP).

## Estructura del repositorio

```
├── index.html          → El informe (no requiere edición para actualizaciones de rutina)
├── config.json         → Diseño: fecha de corte, nombres de hojas, logo
├── data/
│   └── informe.xlsx    → Datos del corte (tablas del informe + hoja Insumo_Sujetos para el mapa)
├── fotos/
│   ├── manifest.json   → Índice de fotos por sujeto (Registro Fotográfico)
│   └── *.jpeg          → Fotografías del registro
├── imagenes/
│   ├── manifest.json   → Índice de imágenes para las diapositivas del Informe
│   └── *.jpg           → Imágenes de uso general (no aparecen en el Registro Fotográfico)
├── assets/
│   ├── footer.png      → Cinta institucional (pie de página de cada sección)
│   └── logo.png        → (Opcional) Logo personalizado
└── docs/
    └── propuesta-tipos-diapositiva.html → Catálogo de los 6 tipos de diapositiva
                                           definidos para el futuro editor de diseño
```

## Publicar en GitHub Pages (una sola vez)

1. Crea un repositorio nuevo en tu cuenta de GitHub (por ejemplo `informe-cisp`).
2. Sube **todo el contenido de esta carpeta** al repositorio (arrastra los archivos en
   la página del repo → "Add file" → "Upload files", o usa Git desde tu computador).
3. En el repositorio ve a **Settings → Pages**.
4. En "Source" selecciona **Deploy from a branch**, rama **main**, carpeta **/ (root)** y guarda.
5. En 1-2 minutos tu informe quedará publicado en:
   `https://TU_USUARIO.github.io/informe-cisp/`

> ⚠️ **Confidencialidad:** en una cuenta gratuita, GitHub Pages es público — cualquiera con
> la URL puede ver el informe. Si el contenido es sensible, usa esta publicación solo de
> forma temporal mientras se habilita el servidor institucional, y evita divulgar la URL.

## Control de acceso

Al abrir la página aparece una pantalla de usuario y contraseña. Las credenciales están
definidas dentro de `index.html` como un hash SHA-256 (no en texto plano). Para cambiarlas,
genera el nuevo hash con: `sha256("cisp3007:USUARIO:CONTRASEÑA")` y reemplaza el valor de la
constante `EXPECTED` en `index.html`.

> ⚠️ **Alcance real de esta protección:** es una barrera de privacidad razonable para uso
> temporal — evita que alguien con el enlace vea los datos casualmente. **No es seguridad
> robusta**: al ser un sitio estático, los archivos de datos (`data/informe.xlsx`,
> `fotos/*.jpeg`, `config.json`) siguen siendo accesibles por URL directa para alguien con
> conocimientos técnicos que conozca o adivine las rutas. La solución definitiva es el
> servidor institucional con autenticación real.

## Actualizar el informe (rutina de cada corte)

1. **Datos:** reemplaza `data/informe.xlsx` con el Excel del nuevo corte
   (mismos nombres de hoja y columnas — la estructura está en el Manual de usuario dentro del informe).
2. **Fotos:** sube las imágenes nuevas a la carpeta `fotos/` y actualiza `fotos/manifest.json`
   agregando el archivo al sujeto correspondiente (o un sujeto nuevo con `titulo`, `detalle` y `archivos`).
3. **Diseño de las diapositivas:** el orden, la visibilidad y el contenido de las secciones vive en la hoja
   `Diapositivas` del mismo Excel. Puedes editarla a mano, o usar el editor visual del informe
   (**Configuración → Diseño de hojas**): activar/desactivar secciones, reordenarlas y crear secciones de
   "bloques" (celdas de texto, imagen o imagen+texto, con título, detalle, cifra, fondo y ajuste de imagen).
   Al terminar, botón **"Descargar Excel actualizado"** → sube ese archivo como `data/informe.xlsx`.
4. **Configuración global:** fecha de corte, nombres de hojas y logo → hoja **Configuración** →
   **"Descargar config.json"** → súbelo reemplazando el existente.
5. Los cambios quedan publicados automáticamente ~1 minuto después de subirlos.

### Hoja `Diapositivas` (estructura)

Columnas: `Hoja` (`Portada` · `Informe` · `Caso 1` a `Caso 5`), `Seccion`, `Orden`,
`Tipo` (`tabla` · `contactabilidad` · `mapa` · `bloques` · `campo` para portada · `hoja` para definir un caso),
`Activa` (SI/NO), `Fuente` (para tipo tabla: nombre de la hoja de datos), y para tipo `bloques` una fila por celda con:
`Bloque` (1-4), `Celda` (1-4), `Contenido` (`texto` · `imagen` · `imagen+texto`), `Titulo`, `Detalle`,
`Cifra`, `Fondo` (`blanco` · `azul` · `dorado` · `rojo` · `gris`), `Imagen` (nombre de archivo en `fotos/` o `imagenes/`)
y `Ajuste` (`recortar` · `completa`).

- **Portada**: filas con `Hoja=Portada`, `Tipo=campo`; el estilo del campo va en `Contenido`
  (`eyebrow`, `titulo`, `subtitulo`, `etiqueta`, `convenio`, `normal`) y el texto en `Detalle`.
- **Casos Emblemáticos**: cada caso se define con una fila `Tipo=hoja` (nombre en `Seccion`, estado en `Activa`)
  y sus secciones de bloques con `Hoja=Caso N`. Los casos inactivos no aparecen en el menú.

## Guardar directo desde el informe (token de GitHub)

En **Configuración → 7 · Guardar directo en el repositorio** puedes publicar el Excel de diseño y el
config.json sin descargar archivos. Requiere un token de GitHub:

1. GitHub → tu perfil → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.
2. Repository access: **Only select repositories** → este repositorio.
3. Permissions → Repository permissions → **Contents: Read and write**. Lo demás en "No access".
4. Define una fecha de expiración corta (30-90 días) y genera el token.
5. Pégalo en el informe y usa "Guardar Excel y config en el repositorio".

> El token es una llave de escritura sobre el repositorio: no lo compartas ni lo publiques.
> El informe lo recuerda en el equipo donde se pegó (queda en el navegador hasta usar el botón
> "Olvidar token" o hasta que expire). En computadores compartidos, usar "Olvidar token" al terminar.

## Probar en el computador antes de subir

El informe lee archivos externos, así que abrirlo con doble clic no carga los datos
del repositorio (limitación de seguridad de los navegadores). Para probar localmente:

```
# Dentro de la carpeta del proyecto (requiere Python, ya incluido en Windows/Mac/Linux):
python -m http.server 8000
```

Luego abre `http://localhost:8000` en el navegador.

> Nota: aunque se abra con doble clic sin servidor, el informe **no se rompe** —
> muestra los datos de ejemplo embebidos y permite cargar el Excel y las fotos
> manualmente desde la hoja de Configuración, como siempre.

## Formato de `fotos/manifest.json`

```json
{
  "Flamenco": {
    "titulo": "501 - Consejo Comunitario Flamenco",
    "detalle": "María La Baja, Bolívar",
    "archivos": ["Flamenco_1.jpeg", "Flamenco_2.jpeg"]
  }
}
```

## Formato de `config.json`

```json
{
  "fechaCorte": "2026-06-16",
  "nombreHojaInforme": "Informe",
  "nombreHojaFotos": "Registro Fotográfico",
  "logo": null
}
```

- `fechaCorte`: formato AAAA-MM-DD.
- `logo`: ruta a la imagen (por ejemplo `"assets/logo.png"`) o `null` para usar los logos de texto.
