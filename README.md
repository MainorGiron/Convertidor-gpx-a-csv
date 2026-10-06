# Convertidor GPX a CSV

Herramienta local para convertir archivos `.gpx` con datos de clientes al formato de la plantilla de socios de negocio de SAP Business One (`FORMATO PARA INGRESAR CLIENTES.xlsx`), conservando tildes y ñ y validando cada dato.

## Uso

1. Abre `convertidor-gpx-csv.html` con doble clic (Chrome o Edge).
2. Carga uno o varios archivos `.gpx` (botón o arrastrar y soltar).
3. Revisa **Ruta** y **Día de visita**: se toman del nombre del archivo (`1.- CD4R1 - LUNES ...gpx` → ruta `CD4R1`, día `1`; `3.- RUTA 29 - MARTES ...gpx` → ruta `RUTA 29`, día `2`).
4. Corrige las celdas en rojo haciendo clic sobre ellas y usa **Excluir** en los puntos que no son clientes (por ejemplo `IMP 01`).
5. **Descargar CSV**: archivo para importar, con las 22 columnas y los dos encabezados de la plantilla.
6. **Descargar Excel (revisión)**: el mismo contenido en `.xlsx`, con filtros y los errores en rojo.

Todo se procesa en el navegador; no se envía ningún dato a internet.

## De dónde sale cada columna

| Columna | Origen por defecto | Obligatoria |
|---|---|---|
| CardCode | Vacío: el código lo asigna SAP manualmente | No |
| CardName | Negocio | Sí |
| U_Ruta | Ruta del nombre del archivo | Sí |
| U_Latitud / U_Longitud | Coordenadas del punto (6 decimales) | Sí |
| Address | Dirección | Sí |
| Phone1 | Teléfono (`9800-1100`) | Sí |
| Notes | Propietario | Sí |
| City | Municipio | Sí |
| County | Departamento | Sí |
| U_Dia_Visita | Día del nombre del archivo como número: 1 lunes, 2 martes, 3 miércoles, 4 jueves, 5 viernes, 6 sábado, 7 domingo | Sí |
| CardType, GroupCode, PayTermsGrpCode, Country, DebitorAccount, Properties1, U_TaxCode | Valores fijos de la plantilla (`cCustomer`, `104`, `-1`, `HN`, `_SYS00000001984`, `tYES`, `EXE`) | — |
| PriceListNum, SalesPersonCode, Territory, RTN | Vacíos; se llenan solos si el GPX trae ese dato | No |

Todo se puede cambiar en el panel izquierdo y la configuración se recuerda.

## Validaciones

- **Nombre separado por "/"**: `NEGOCIO / PROPIETARIO / TELÉFONO / DIRECCIÓN / MUNICIPIO / DEPARTAMENTO`; detecta el teléfono aunque venga pegado al nombre. Si trae RTN (`RTN03018010326279`, `RTN: 0301-...` o 14 dígitos sueltos) en cualquier posición, lo pasa a la columna RTN.
- **Municipio y departamento**: catálogo oficial de Honduras (18 departamentos, 298 municipios). Corrige errores de escritura y abreviaturas (`DIATRITO CENTRAL`, `FCO MORAZAN`, `TEGUCIGALPA` → `DISTRITO CENTRAL`) y marca en rojo cuando el municipio no pertenece al departamento.
- **Largo máximo** de cada campo según la plantilla (por ejemplo Phone1: 20, Address: 100).
- **Codificación**: repara textos dañados (`MarÃ­a` → `María`). Exporta en CSV UTF-8 (con BOM opcional) o en Texto Unicode con tabulaciones.
