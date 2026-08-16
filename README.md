# Agua Purificada CÉLICA — Catálogo Vending (Página web)

Este repositorio contiene una página estática (index.html) que sirve como catálogo digital de una máquina vending ubicada en Célica, Loja. A continuación se documentan los usos, funciones y cómo operar o mantener la página.

## Descripción rápida
La página muestra en tiempo real el inventario disponible en la máquina vending obteniendo datos publicados desde una hoja de cálculo de Google Sheets (CSV publicado). Incluye búsqueda, filtrado por categorías, contador de visitas local, y accesos rápidos para soporte y sugerencias vía WhatsApp.

## Usos para los visitantes
- Ver catálogo digital de productos (snacks, chocolates, galletas, bebidas, aseo y cuidado).
- Buscar productos por nombre o código (campo de búsqueda en la parte superior).
- Filtrar por categoría usando los botones "Todas", "Snacks Salados", "Chocolates y Dulces", "Galletas", "Bebidas", "Aseo y Cuidado".
- Ver precio, disponibilidad (stock) y selección (código) de cada producto.
- Abrir la ubicación de la máquina en Google Maps mediante el botón "Ver en Google Maps".
- Reportar un problema con una selección (abre modal para enviar reporte por WhatsApp).
- Sugerir productos (modal que envía sugerencia por WhatsApp).

## Usos para administradores
- Actualizar la lista de productos abriendo la hoja de cálculo de administración (requiere contraseña).
  - Botón "Actualizar lista" solicita una clave y, si es correcta, abre Google Sheets en modo edición.
  - La clave administrador está definida en index.html en la constante `ADMIN_PASSWORD`.
- El inventario se recarga automáticamente cada 30 segundos desde la publicación CSV.

## Cómo actualizar el inventario
1. Abrir la hoja de cálculo de administración (enlace en la constante `SPREADSHEET_EDIT_URL`).
2. Editar las columnas de la hoja. El script detecta automáticamente las columnas más comunes por nombre (código, producto, cantidad/stock, imagen/logo, pvp).
3. Publicar como CSV (la URL publicada está en `PUBLISHED_CSV_URL`).
4. La página usa la publicación CSV para cargar y formatear los datos automáticamente.

## Detalles técnicos (cómo funciona)
- La página es un HTML estático con Tailwind CSS y FontAwesome incluidos vía CDN.
- JavaScript en el archivo `index.html` realiza:
  - Fetch al CSV publicado de Google Sheets y parseo de líneas CSV (función `parseCSVLine`).
  - Detección automática de índices de columna (código, producto, cantidad/stock, imagen/logo, pvp) y ficheros fallback si faltan encabezados.
  - Conversión de enlaces de imagen (Google Drive / Google Photos / URLs directas) mediante `convertImageUrl`.
  - Inferencia de categoría e icono a partir del nombre del producto (funciones `inferCategory` e `inferIcon`).
  - Renderizado del grid de productos, conteo de selecciones totales y disponibles, búsqueda y filtrado.
  - Modal de soporte y sugerencias que abren WhatsApp con el mensaje preformateado.
  - Contador de visitas almacenado en `localStorage`.

## Campos esperados en la hoja de cálculo
Recomendado (pero el parser es flexible):
- Código / Código (columna con identificador de selección, ej. "001").
- Producto / Productos (nombre del producto).
- Cantidad / Stock (número entero con unidades disponibles).
- Imagen / Logo / Foto (URL o enlace de Google Drive compartido).
- PVP (precio de venta; acepta formatos como "0.30", "$0.30", "0,30").

Si alguno de estos encabezados no existe, el script usa índices por defecto o intenta adivinar la columna correcta.

## Cómo reportar problemas y sugerencias
- Botón "Soporte" abre un modal para enviar un reporte por WhatsApp al número configurado en el script.
- Botón "Pedir Producto" abre un modal para sugerencias por WhatsApp.
- Desde cada tarjeta de producto hay un botón "Reportar problema" que abre el modal con la selección prellenada.

## Seguridad y consideraciones
- La contraseña administrativa (`ADMIN_PASSWORD`) está embebida en `index.html` en texto claro. Para mayor seguridad se recomienda:
  - Mover la administración a un endpoint autenticado o a una cuenta privada de Google con permisos.
  - No publicar la contraseña en repositorios públicos.
- El CSV publicado en Google Sheets debe contener solo datos no sensibles, ya que la publicación es pública.

## Desarrollo y despliegue
- Es una página estática: se puede servir desde GitHub Pages, Netlify, Vercel o cualquier hosting estático.
- Requisitos: conexión a internet para CDNs (Tailwind, FontAwesome) y acceso público a la publicación CSV en Google Sheets.
- Para desarrollo local, clonar el repo y abrir `index.html` en un navegador, o usar un servidor estático para evitar restricciones de CORS en algunos navegadores.

## Variables importantes en index.html
- PUBLISHED_CSV_URL: URL pública del CSV (Google Sheets published CSV).
- SPREADSHEET_EDIT_URL: URL de edición de la hoja de cálculo.
- ADMIN_PASSWORD: clave para abrir la hoja de cálculo desde el botón "Actualizar lista".

## Notas finales
Este README resume los usos principales de la página y cómo mantener actualizada la información del inventario. Si necesitas que añada instrucciones de despliegue concretas (por ejemplo configuración para GitHub Pages o Netlify) o que mueva la contraseña a una variable de entorno, puedo crear los archivos y pasos necesarios.
