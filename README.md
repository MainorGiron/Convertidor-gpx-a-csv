# Convertidor GPX a CSV

Herramienta local para convertir archivos `.gpx` con datos de clientes a `.csv`, conservando tildes y ñ, validando campos obligatorios y aplicando formatos automáticos.

## Uso

1. Abre `convertidor-gpx-csv.html` con doble clic (Chrome o Edge).
2. Carga uno o varios archivos `.gpx` (botón o arrastrar y soltar).
3. En **Columnas y formatos** elige qué columnas van al CSV, su nombre, formato y si son obligatorias.
4. Corrige las celdas en rojo haciendo clic sobre ellas.
5. Pulsa **Descargar CSV**.

Todo se procesa en el navegador; no se envía ningún dato a internet.

## Qué hace

- **Codificación**: lee el GPX respetando su codificación, repara textos dañados (`MarÃ­a` → `María`) y exporta en UTF-8 (con BOM opcional para Excel).
- **Validación**: los campos obligatorios vacíos o con formato inválido se marcan en rojo, con un resumen por columna.
- **Formatos**:
  | Formato | Entrada | Salida |
  |---|---|---|
  | Teléfono | `98001100`, `+504 9800 1100` | `9800-1100` |
  | Identidad | `0801199012345` | `0801-1990-12345` |
  | Coordenada | `14.0818` | `14.081800` |
  | Nombre Propio | `MARIA DE LA PAZ` | `Maria de la Paz` |
  | MAYÚSCULAS, Solo dígitos, Correo | | |
- **Descripción en columnas**: separa líneas tipo `Teléfono: 9800…` o tablas HTML dentro de `<desc>`.
- La configuración de columnas se guarda en el navegador y se reutiliza en cada archivo.
