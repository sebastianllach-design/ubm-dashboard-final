# Dashboard UBM — conexión directa al CRM

Esta versión NO utiliza `05_Dashboard_Public` ni hojas auxiliares con fórmulas.

El dashboard consulta directamente:

- `01_CRM_Viajes`
- `02_CRM_Asistencias`

La consulta solicita únicamente las columnas necesarias para visualizar el dashboard.
No solicita teléfono, email ni notas.

## Importante sobre privacidad

Esta arquitectura evita que el dashboard reciba o muestre teléfono, email y notas.

Sin embargo, para que un Static Site público de Render pueda consultar Google Sheets sin autenticación,
el Google Sheet debe estar accesible mediante enlace/consulta pública. Eso significa que proteger la
visualización NO equivale a proteger el archivo fuente de alguien que tenga acceso a su URL.

Si se requiere confidencialidad estricta del archivo fuente, hace falta un backend autenticado o un
endpoint intermedio que entregue solo columnas permitidas.

## Configuración

1. Subir `CRM_UBM_Viajes_Final_Simplificado.xlsx` a Google Drive.
2. Abrirlo con Google Sheets.
3. Guardarlo como una hoja nativa de Google Sheets.
4. Copiar el ID del archivo desde la URL:
   `https://docs.google.com/spreadsheets/d/ID_DEL_ARCHIVO/edit`
5. Abrir `config.js`.
6. Reemplazar `PEGAR_ID_GOOGLE_SHEET` por ese ID.
7. Mantener los nombres de hojas:
   - `01_CRM_Viajes`
   - `02_CRM_Asistencias`
8. Para el modo simple sin backend, configurar el Google Sheet como:
   - Compartir
   - Acceso general
   - Cualquier persona con el enlace
   - Lector
9. Subir `index.html`, `config.js` y `README.md` al repositorio GitHub.
10. Render:
    - Static Site
    - Build Command: vacío
    - Publish Directory: `.`
11. Deploy / Manual Deploy → Deploy latest commit.

## Columnas que recibe el dashboard desde Viajes

- ID
- Fecha de ingreso
- Cliente
- Ciudad
- Responsable
- Segmento
- Origen
- Campaña
- Destino
- Etapa
- Prioridad
- Próxima acción
- Estado de seguimiento
- Motivo de pérdida
- Venta
- Costo
- Margen
- Días vencida
- Semáforo

No solicita:
- Teléfono
- Email
- Detalle de origen
- Notas
- Moneda original / valor original
- Tipo de cambio

## Columnas que recibe desde Asistencias

- ID
- Fecha
- Cliente
- Cantidad de personas
- Ciudad
- Responsable
- Producto
- Tipo de producto
- Destino
- Aseguradora
- Origen
- Etapa
- Venta
- Costo
- Margen

No solicita teléfono, detalle de origen ni notas.

## Definiciones comerciales

- Ventas totales = suma de `Valor_Vendido_USD` de oportunidades con `Etapa = Ganado`.
- Conversión = Oportunidades Ganadas / Total de Oportunidades del universo filtrado.
- No se utiliza “Conversión decisiones”.

## Filtro temporal

- Todo
- Semana actual
- Mes actual
- Año actual
- Rango personalizado

Todos los KPIs, listados y gráficos usan el mismo período.

## Ajustes de fechas

- `Rango personalizado (Desde / Hasta)` aparece de forma explícita en el selector de período.
- Al seleccionarlo se muestran los campos `Desde` y `Hasta`.
- Todas las fechas visibles como texto en el dashboard se muestran en formato `dd/mm/aaaa`.

## Revisión v8

Correcciones aplicadas:
- Parser numérico corregido: los decimales con punto ya no se interpretan como miles.
- `Ventas totales` suma solo `Valor_Vendido_USD` de oportunidades con `Etapa = Ganado` y valor numérico cargado.
- `Conversión` = Ganadas / Total de oportunidades válidas del universo filtrado.
- Se excluyen filas de prueba, filas sin cliente y filas con errores.
- Ticket promedio de asistencias usa solo operaciones con importe cargado.
- Desde/Hasta quedan siempre visibles.
- Los filtros Semana/Mes/Año completan automáticamente Desde/Hasta.
- Editar Desde o Hasta cambia automáticamente a Rango personalizado.
- Todas las fechas visibles se presentan como dd/mm/aaaa.
- Config.js ya contiene el ID correcto del Google Sheet.

## Revisión v9 — todas las oportunidades

- El dashboard inicia siempre con `Todas las etapas`.
- No se filtran oportunidades por etapa durante la carga.
- Se puede filtrar por Cotizado, Seguimiento, Ganado, Perdido y Sin etapa.
- Se agregó `Limpiar filtros` para volver al universo completo.
- El filtro de etapa ya no puede conservar accidentalmente `Ganado` al cargar una nueva versión.
- La pestaña Oportunidades muestra un resumen por etapa para verificar rápidamente el universo visible.

## Corrección v10 - filtro de fechas

Google Visualization puede entregar fechas como `Date(año,mes,día)`.
El mes en ese formato es base cero: julio = 6.

La versión v10 reconoce:
- Google Visualization `Date(2026,6,15)`
- `dd/mm/aaaa`
- `aaaa-mm-dd`
- números seriales de Excel/Google Sheets
- objetos Date de JavaScript

Esto corrige el caso en que un rango de fechas válido devolvía cero oportunidades.

## v11 - identidad visual

Se agregó el logo oficial de UBM como favicon del dashboard:
- favicon.png (64x64)
- apple-touch-icon.png (180x180)
- ubm-icon-512.png (respaldo de alta resolución)

No se modificó la lógica de datos, filtros, KPIs ni conexión con Google Sheets.


## v13 - generación de demanda

- El gráfico `Origen de oportunidades` ahora ocupa todo el ancho disponible.
- Se convirtió en barras horizontales con mejor altura, etiquetas visibles y cantidad al final de cada barra.
- Debajo del gráfico aparece el detalle por origen con cantidad y porcentaje.
- El bloque `Campañas` quedó debajo del gráfico.
- Se agregó `Monto por campaña (oportunidades ganadas)` usando únicamente oportunidades con `Etapa = Ganado`.


## v14 - actualización de columnas CRM Viajes
Ajustado para la estructura de `01_CRM_Viajes` después de eliminar:
- `Detalle_Origen`
- `Valor_Original`
- `Tipo_Cambio`

La consulta ahora usa `A4:AA` y las nuevas posiciones de:
Campaña, Destino, Etapa, Prioridad, Seguimiento, Valor_Vendido_USD, Costo_USD, Margen_USD, Dias_Vencida y Semaforo_Seguimiento.
