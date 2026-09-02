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
`Tipo` (`tabla` · `contactabilidad` · `mapa` · `bloques` · `subportada` · `tablamanual` · `grafico` · `mapadatos` · `campo` para portada · `hoja` para definir un caso),
`Activa` (SI/NO), `Fuente` (para tipo tabla: nombre de la hoja de datos; para tipo bloques: enlace opcional a la
fuente de esos datos, igual en todas las filas de un mismo bloque), y para tipo `bloques` una fila por celda con:
`Bloque` (1-4), `Celda` (1-4), `Contenido` (`texto` · `imagen` · `imagen+texto`), `Titulo`, `Detalle`,
`Cifra`, `Fondo` (`blanco` · `azul` · `dorado` · `rojo` · `gris`), `Imagen` (nombre de archivo en `fotos/` o `imagenes/`)
y `Ajuste` (`recortar` · `completa`).

- **Portada**: filas con `Hoja=Portada`, `Tipo=campo`; el estilo del campo va en `Contenido`
  (`eyebrow`, `titulo`, `subtitulo`, `etiqueta`, `convenio`, `normal`) y el texto en `Detalle`.
- **Enlace a la fuente en bloques**: cada bloque (no cada celda) puede tener un enlace opcional a la fuente de
  esos datos, que aparece como un pequeño pie de página dentro del bloque ("Ver fuente ↗"). Se deja vacío en los
  bloques que no tengan una fuente externa que enlazar. Se edita en **Diseño de hojas**, justo debajo del selector
  de número de celdas de cada bloque.
- **Subportada**: sección de tipo `Tipo=subportada` dentro de `Informe` o de un `Caso N`, para introducir un subtema
  con la misma configuración visual que la Portada. Una fila por campo, usando `Bloque` como número de campo (orden),
  `Contenido` para el estilo (mismos valores que la Portada), `Detalle` para el texto y `Cifra` (SI/NO) para
  mostrar u ocultar ese campo puntual. Se crea desde **Diseño de hojas**, eligiendo "Subportada" en el
  desplegable junto al botón "+ Agregar sección".
- **Tabla manual**: sección de tipo `Tipo=tablamanual`, para una tabla pequeña escrita directamente en el editor,
  sin vincularla a ninguna hoja del Excel de datos. Se define el número de filas y de columnas al crearla (máximo
  50 filas y 12 columnas) y luego se pueden seguir agregando o quitando filas y columnas desde el editor, así como
  elegir el color del encabezado (blanco, azul, dorado, rojo o gris, los mismos usados en el fondo de los bloques).
  En el Excel, la fila con `Bloque=0` define los encabezados de columna (`Celda`=número de columna, `Titulo`=texto
  del encabezado, `Fondo`=color del encabezado); las filas con `Bloque=1` en adelante son los datos (`Bloque`=número
  de fila, `Celda`=número de columna, `Detalle`=valor de esa celda).
- **Gráfico**: sección de tipo `Tipo=grafico`, que dibuja un gráfico de barras o de torta a partir de una hoja
  del Excel de datos, o a partir de una tabla manual ya creada en la misma hoja del Informe o del Caso. Una sola
  fila define la sección: `Contenido`=`barras` o `torta`, `Titulo`=nombre de la columna que se usa como etiqueta,
  `Detalle`=nombre de la columna numérica que se grafica, `Cifra`=`excel` o `tablamanual` (el origen de los datos).
  Si el origen es `excel`, `Fuente`=nombre de la hoja de datos. Si el origen es `tablamanual`, `Fondo`=título de
  la tabla manual de referencia (debe existir una tabla manual con ese título exacto en la misma hoja). Si una
  etiqueta aparece en varias filas, sus valores se suman. Se crea y edita desde **Diseño de hojas**, donde el
  origen, la hoja o tabla, y las columnas se eligen de listas desplegables en vez de escribirse a mano.
- **Mapa de datos**: sección de tipo `Tipo=mapadatos`, distinta del mapa fijo original (`Tipo=mapa`), que colorea
  el mismo contorno de Colombia por departamento a partir de una hoja del Excel o de una tabla manual, y se puede
  repetir varias veces en un mismo informe (a diferencia del mapa fijo, que es único). Una sola fila define la
  sección, con las mismas columnas y el mismo significado de `Cifra`/`Fondo`/`Fuente` que la sección `grafico`,
  cambiando `Titulo` por el nombre de la columna de departamento. El nombre del departamento se compara sin
  distinguir mayúsculas ni tildes; los que no se reconozcan se listan debajo del mapa para poder corregirlos.
- **Tabla resumen (gráfico y mapa)**: ambos tipos pueden mostrar, opcionalmente, una tabla con los mismos datos
  agregados a la izquierda o a la derecha del gráfico o del mapa, controlado por la columna `Imagen` de esa fila
  (`ninguna` · `izquierda` · `derecha`). Se elige desde el mismo editor, en el desplegable "Tabla resumen".
- **Casos Emblemáticos**: cada caso se define con una fila `Tipo=hoja` (nombre en `Seccion`, estado en `Activa`)
  y sus secciones (bloques, subportadas, tablas manuales o gráficos) con `Hoja=Caso N`. Los casos inactivos no
  aparecen en el menú.

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

## Copias de seguridad y cómo revertir un error

Con la casilla "Crear copia de seguridad" activada (por defecto), cada guardado desde la app copia
automáticamente el Excel anterior a `backups/informe_AAAA-MM-DD_HHMM.xlsx` **antes** de sobrescribirlo.
Para revertir: descarga la copia que necesites desde esa carpeta en GitHub y súbela reemplazando
`data/informe.xlsx`.

Independientemente de esa casilla, todo el repositorio tiene el historial completo de Git: en la página
de cualquier archivo en GitHub hay un botón **"History"** con todas las versiones anteriores, cada una
descargable.

## Hojas nuevas de datos (tablas genéricas)

Puedes agregar cualquier hoja de datos nueva al Excel (por ejemplo `Indicadores_2026`) y mostrarla como
tabla en el Informe o en un Caso: agrega una fila en `Diapositivas` con `Tipo=tabla` y `Fuente` igual al
**nombre exacto** de la nueva hoja. Se muestra automáticamente con sus columnas y filas tal cual estén en
Excel (no requiere que el HTML tenga una tabla preconstruida para ese caso).

## Las imágenes no aparecen

Si subiste fotos a `fotos/` o `imagenes/` directamente en GitHub y no aparecen en la galería o el selector
del editor, seguramente falta agregarlas al `manifest.json` correspondiente. La forma más confiable de
evitar ese paso manual es usar **Configuración → 8 · Imágenes disponibles en el repositorio → "Actualizar
lista de imágenes desde GitHub"** (con el token puesto): detecta automáticamente todas las imágenes reales
de esas carpetas sin depender de los manifest.json.

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
