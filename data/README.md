# Respaldos de datos — MP 2026

Copia de seguridad versionada de los registros reales, para que **no se pierdan**
aunque se borre el navegador. Estado al **18-06-2026**.

## Archivos

| Archivo | Qué es | Uso |
|---|---|---|
| **`Respaldo_GMP2026_20260618.json`** | **Respaldo real de la app anterior** (`tipo: respaldo-gmp2026`, v2): **79 mantenciones, 63 pendientes, 14 correctivos, 5 empresas, 1 ingeniero**. | **Fuente reimportable.** En la app: «Importar respaldo anterior». |
| `Registros_GMP2026_20260618.xlsx` | Exportación legible de esos mismos datos (hojas Equipos, Programación, Mantenciones, Pendientes, Correctivos). | Referencia / lectura. Redundante con el JSON. |
| `SIGEM_V2.xlsx` | Planilla SIGEM (Inventario, Pendientes, Tareas, Bitácora, Registro…). Estructura distinta, no es respaldo de la app. | Archivo histórico / consulta. |
| `Sistema_Gestion_MP2026_Integrado_VF.xlsx` | Planilla integrada (Panel, Órdenes de Trabajo, Línea de Compra, Repuestos, Visitas, Envíos, Bajas…). | Archivo histórico / consulta. |

## Cómo recuperar los registros en la app

1. Abre `../index.html`.
2. Carga primero `../sample-data/ProgramaciónMP2026.xlsm` (o tu `.xlsm` oficial actual).
3. Menú lateral → **«Importar respaldo anterior»** → elige
   `Respaldo_GMP2026_20260618.json`.
4. Verás las 79 mantenciones, 63 pendientes y 14 correctivos. Usa **«Respaldar
   ahora»** para generar tu copia JSON + Excel actualizada.

> Importado verificado: 79 / 63 / 14 (+23 avances), sin tipos sin mapear ni
> registros huérfanos. Los `equipos` y `registros` del respaldo **no** se importan:
> se re-leen del `.xlsm` oficial (la fuente de verdad).
