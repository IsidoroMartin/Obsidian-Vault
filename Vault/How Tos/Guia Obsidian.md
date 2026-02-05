Table of Contents

1. [1. Configuración de los Links (Enlaces)](#1-configuraci%C3%B3n-de-los-links-enlaces)
2. [2. Configuración de Imágenes y Excalidraw](#2-configuraci%C3%B3n-de-im%C3%A1genes-y-excalidraw)
	1. [Gestión de Adjuntos (Imágenes)](#gesti%C3%B3n-de-adjuntos-im%C3%A1genes)
	2. [Excalidraw y Auto-exportación](#excalidraw-y-auto-exportaci%C3%B3n)
	3. [Importar Colecciones en Excalidraw](#importar-colecciones-en-excalidraw)
3. [3. Gestión de Rutas y Jerarquía (Ejemplos Prácticos)](#3-gesti%C3%B3n-de-rutas-y-jerarqu%C3%ADa-ejemplos-pr%C3%A1cticos)
	1. [Ejemplo de Jerarquía de Carpetas](#ejemplo-de-jerarqu%C3%ADa-de-carpetas)
	2. [Rutas Relativas para Imágenes](#rutas-relativas-para-im%C3%A1genes)
	3. [Links entre Notas (Markdown Links)](#links-entre-notas-markdown-links)
	4. [Links de Ancla (Dentro de la misma página)](#links-de-ancla-dentro-de-la-misma-p%C3%A1gina)
4. [4. Plugin: Advanced Tables](#4-plugin-advanced-tables)
5. [5. Visualización de Código (Plugins)](#5-visualizaci%C3%B3n-de-c%C3%B3digo-plugins)
6. [6. Table of Contents (Compatible con GitHub)](#6-table-of-contents-compatible-con-github)
7. [7. Sintaxis Básica de Markdown y Callouts](#7-sintaxis-b%C3%A1sica-de-markdown-y-callouts)
	1. [Formato de Texto Básico](#formato-de-texto-b%C3%A1sico)
	2. [Bloques de Código](#bloques-de-c%C3%B3digo)
	3. [Callouts (Avisos / Toasts)](#callouts-avisos--toasts)


# Guía de Configuración de Obsidian para GitHub

Esta guía te ayudará a configurar Obsidian para que tus notas sean totalmente compatibles con GitHub, incluyendo el manejo de enlaces, imágenes, tablas y diagramas.

## 1. Configuración de los Links (Enlaces)

Para que los enlaces funcionen correctamente en GitHub (que usa Markdown estándar), debemos desactivar los "WikiLinks" nativos de Obsidian.

1. Ve a **Settings** (Configuración) > **Files & Links** (Archivos y Enlaces).
2. Desactiva la opción **Use Wiki links**.
   - *Efecto:* Ahora los enlaces se crearán como `[Nombre](ruta/al/archivo.md)` en lugar de `[[Archivo]]`.
   - Esto asegura que al hacer clic en un enlace dentro de GitHub, te lleve al archivo correcto.

## 2. Configuración de Imágenes y Excalidraw

### Gestión de Adjuntos (Imágenes)
Es recomendable guardar todas las imágenes en una carpeta específica o junto a la nota para mantener el orden.

1. Ve a **Settings** > **Files & Links**.
2. En **Default location for new attachments**, selecciona **In subfolder under current folder** (En subcarpeta bajo la carpeta actual) y nómbrala `assets` o `images`. O bien, selecciona una carpeta global si prefieres.
3. Asegúrate de que los enlaces a imágenes también sean estándar (esto se cubre con el paso 1).

### Excalidraw y Auto-exportación
Para ver tus diagramas en GitHub sin necesidad de abrir Obsidian, necesitas exportarlos automáticamente a PNG o SVG.

1. Instala el plugin **Excalidraw**.
2. Ve a la configuración del plugin Excalidraw.
3. Busca la sección **Embed & Export**.
4. Activa **Auto-export PNG** (o SVG).
5. Configura el **Auto-export filename suffix** si lo deseas (por defecto suele estar bien).
6. **Importante:** Asegúrate de incrustar la versión PNG en tu nota Markdown en lugar del archivo `.excalidraw`.
   - Cuando insertes el dibujo en tu nota, asegúrate de que el enlace apunte al archivo `.png` generado, por ejemplo: `![](assets/dibujo.excalidraw.png)`.
   - *Tip:* Puedes configurar el plugin para que al incrustar un dibujo (`Ctrl/Cmd + click` en el botón de enlace) use automáticamente la imagen exportada.

### Importar Colecciones en Excalidraw
Para usar iconos y formas personalizadas (AWS, Azure, etc.), sigue estos pasos:

1. En la barra de la derecha, pulsa en Browse Libraries, esto te abrirá  [libraries.excalidraw.com](https://libraries.excalidraw.com/) desde tu navegador.
2. Busca la colección que desees y haz clic en el botón **Download** para descargar el archivo `.excalidrawlib` a tu carpeta de descargas.
3. Una vez descargado, haz clic en el icono de **Library** (la estantería) en la barra de herramientas lateral del lienzo.
4. Haz clic en el icono de **Importar** (icono de carpeta/abrir) que aparece en el panel de la librería.
5. Selecciona el archivo `.excalidrawlib` que acabas de descargar en tu carpeta de descargas.
6. La colección se añadirá a tu librería de Excalidraw y estará lista para usarse.

## 3. Gestión de Rutas y Jerarquía (Ejemplos Prácticos)

Para que las imágenes y enlaces funcionen en GitHub, es vital entender las **rutas relativas**. GitHub no sabe dónde está tu disco duro (`C:/Users/...` o `/home/...`), solo conoce la estructura dentro del repositorio.

### Ejemplo de Jerarquía de Carpetas
Supongamos que tienes esta estructura en tu proyecto:

```text
Mi-Proyecto-Obsidian/
├── README.md
├── imagenes-globales/
│   └── logo.png
├── notas/
│   ├── Nota-A.md
│   ├── Nota-B.md
│   └── assets/           <-- Carpeta de adjuntos específica para 'notas'
│       ├── diagrama.png
│       └── foto.jpg
└── diario/
    └── 2023-10-27.md
```

### Rutas Relativas para Imágenes
Cuando insertas una imagen, la ruta debe ser relativa al archivo `.md` donde estás escribiendo.

**Caso 1: Imagen en subcarpeta `assets` (junto a la nota)**
- Archivo editando: `notas/Nota-A.md`
- Imagen objetivo: `notas/assets/foto.jpg`
- **Link correcto:** `![](assets/foto.jpg)`
- *Explicación:* Obsidian busca en la carpeta `assets` que está en el mismo nivel que `Nota-A.md`.

**Caso 2: Imagen en carpeta superior o paralela**
- Archivo editando: `notas/Nota-A.md`
- Imagen objetivo: `imagenes-globales/logo.png`
- **Link correcto:** `![](../imagenes-globales/logo.png)`
- *Explicación:* `../` significa "subir un nivel" (salir de `notas/` a la raíz) y luego entrar a `imagenes-globales/`.

### Links entre Notas (Markdown Links)
**Caso 3: Link de `Nota-A` a `Nota-B` (Misma carpeta)**
- `[Ir a Nota B](Nota-B.md)`

**Caso 4: Link de `Nota-A` a `README.md` (Carpeta superior)**
- `[Volver al Inicio](../README.md)`

**Caso 5: Link de `README.md` a `Nota-A` (Hacia una subcarpeta)**
- `[Leer Nota A](notas/Nota-A.md)`

### Links de Ancla (Dentro de la misma página)
Los links de ancla ("Anchor links") sirven para navegar a una sección específica (un encabezado) dentro del mismo documento.

1.  Asegúrate de tener un encabezado, por ejemplo: `## Mi Sección Importante`.
2.  Para crear el link, usa el formato `[Texto del link](#título-del-encabezado-en-minúsculas-y-con-guiones)`.

**Reglas de conversión para GitHub:**
- Todo en minúsculas.
- Los espacios se reemplazan por guiones `-`.
- Se eliminan la mayoría de signos de puntuación (puntos, comas, paréntesis).

**Ejemplos:**
- Encabezado: `## 1. Introducción` -> Link: `[Ir a Intro](#1-introducción)`
- Encabezado: `### ¿Cómo instalar?` -> Link: `[Ver instalación](#cómo-instalar)`
- Encabezado: `## Configuración de Imágenes` -> Link: `[Configurar](#configuración- de-imágenes)`

## 4. Plugin: Advanced Tables

Este plugin facilita enormemente la creación y manipulación de tablas en Markdown, que suelen ser tediosas.

1. Instala **Advanced Tables** desde la tienda de plugins de la comunidad.
2. **Uso básico:**
   - Escribe `|` y presiona `Tab`.
   - Escribe el encabezado, presiona `Tab` para ir a la siguiente columna.
   - Presiona `Enter` para pasar a la siguiente fila. El plugin formateará automáticamente la tabla para que sea legible en el editor.
3. **Barra de herramientas:** Abre la barra lateral derecha (o el comando) para ver botones para insertar filas, columnas, moverlas o alinear texto.

## 5. Visualización de Código (Plugins)

Obsidian soporta resaltado de sintaxis nativamente, pero puedes mejorarlo con plugins.

**Plugins recomendados:**
*   **Editor Syntax Highlight:** Mejora el resaltado de sintaxis en el modo edición (Live Preview), haciendo que se vea más parecido al modo lectura.
*   **Copy Button for Code Blocks:** (Ahora nativo en versiones recientes de Obsidian) Añade un botón para copiar el código fácilmente.

## 6. Table of Contents (Compatible con GitHub)

GitHub genera automáticamente un índice si usas la vista previa, pero si quieres un índice explícito dentro del documento, necesitas un plugin que genere enlaces Markdown estándar (`[Sección](#sección)`).

**Plugin recomendado:** **"Table of Contents"** (por *hipstersmoothie* u otros similares que generen texto estático).

1. Instala el plugin.
2. Configura el plugin para que el estilo de lista sea **Markdown**.
3. Usa el comando (normalmente `Create Table of Contents`) para insertar el índice en tu nota.
4. **Compatibilidad:** Asegúrate de que los enlaces generados se vean así: ` - [Título](#título)`.

## 7. Sintaxis Básica de Markdown y Callouts

Aquí tienes una referencia rápida de los elementos más útiles para documentar.

### Formato de Texto Básico
- **Negrita:** `**Texto en negrita**`
- *Cursiva:* `*Texto en cursiva*`
- ~~Tachado~~: `~~Texto tachado~~`
- `Código en línea`: Usar backticks `texto`.

### Bloques de Código
Para insertar código de programación, usa tres comillas invertidas (backticks) seguidas del nombre del lenguaje.

**Sintaxis (Lo que escribes):**
````markdown
```javascript
function saludar() {
  console.log("Hola mundo");
}
```
````

**Resultado (Lo que ves):**
```javascript
function saludar() {
  console.log("Hola mundo");
}
```

### Callouts (Avisos / Toasts)
Obsidian soporta "Callouts" (o Admonitions) que se renderizan como bloques de colores con iconos. GitHub también soporta la mayoría de estos nativamente.

**Tipos comunes compatibles con GitHub:**

**Nota Informativa (Azul):**
*Sintaxis:*
```markdown
> [!NOTE]
> Este es un aviso de información general. Útil para notas adicionales.
```
*Resultado:*
> [!NOTE]
> Este es un aviso de información general. Útil para notas adicionales.

**Consejo (Verde):**
*Sintaxis:*
```markdown
> [!TIP]
> Un consejo útil o sugerencia para mejorar el flujo de trabajo.
```
*Resultado:*
> [!TIP]
> Un consejo útil o sugerencia para mejorar el flujo de trabajo.

**Importante (Morado):**
*Sintaxis:*
```markdown
> [!IMPORTANT]
> Información crucial que el usuario debe leer.
```
*Resultado:*
> [!IMPORTANT]
> Información crucial que el usuario debe leer.

**Advertencia (Amarillo):**
*Sintaxis:*
```markdown
> [!WARNING]
> Ten cuidado con esta configuración, podría tener efectos secundarios.
```
*Resultado:*
> [!WARNING]
> Ten cuidado con esta configuración, podría tener efectos secundarios.

**Error o Precaución (Rojo):**
*Sintaxis:*
```markdown
> [!CAUTION]
> No borres este archivo o el sistema dejará de funcionar.
```
*Resultado:*
> [!CAUTION]
> No borres este archivo o el sistema dejará de funcionar.

*(Nota: En Obsidian puedes usar `[!ERROR]` o `[!DANGER]`, pero en GitHub se usa `[!CAUTION]` para el rojo).*
