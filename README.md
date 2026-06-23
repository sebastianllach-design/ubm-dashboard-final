# UBM Dashboard Reset Final

Esta es la versión limpia para arrancar de cero.

## Qué contiene
- `index.html`: dashboard estático listo para publicar en Render.

## Fuente de datos ya configurada
El dashboard ya apunta a estos CSV públicos publicados en Google Sheets:
- `01_CRM_Operativo`
- `02_Maestros`

## GitHub
Crear un repo nuevo y subir **solo** el contenido de esta carpeta.
En la raíz del repo tiene que quedar directamente `index.html`.

## Render
Crear un **Static Site** nuevo con:
- Build Command: vacío
- Publish Directory: `.`

## Uso
Al abrir el dashboard:
- carga automático
- el botón **Sincronizar** vuelve a leer Google Sheets
