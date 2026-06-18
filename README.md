# Sistema de Gestión MP 2026 — Equipamiento Clínico HHHA

Aplicación de **centro de control operacional** para la gestión del Mantenimiento
Preventivo (MP), correctivo, pendientes y reprogramaciones de ~1.000 equipos clínicos.

Reconstrucción desde cero de la app anterior: **misma lógica de negocio, interfaz
mucho más simple y directa**. Un único archivo `index.html` (HTML + CSS + JavaScript
*vanilla* + [SheetJS](https://sheetjs.com/) vía CDN). Sin paso de build. Funciona
offline tras la primera carga.

## Cómo usar

1. Abre **`index.html`** en un navegador moderno (Chrome/Edge recomendado para el
   auto-guardado a carpeta). Funciona también como archivo local (`file://`).
2. Pulsa **«Cargar .xlsm»** y selecciona `ProgramaciónMP2026.xlsm`
   (incluido en `sample-data/`). La app **solo lee** la planilla, nunca la escribe.
3. Trabaja desde el **Tablero**: KPIs, equipos que necesitan atención y el panel
   «Por volcar al .xlsm».

### Migrar datos de la app anterior

**«Importar respaldo anterior»** (en el menú lateral o en «Más») lee un respaldo
de la app antigua (`tipo: "respaldo-gmp2026"`) y migra mantenciones, pendientes y
correctivos a la estructura nueva. Equipos y registros **no** se importan: se
re-leen del `.xlsm` oficial. Archivo de prueba: `sample-data/ejemplo_respaldo_anterior.json`.

**«Importar planilla integrada»** lee `Sistema_Gestion_MP2026_Integrado.xlsx`: los
expedientes **correctivos** detallados (OT + envíos + visitas + línea de compra +
reparación en terreno) se **fusionan por Folio SIGEM** con lo ya importado, y los
**pendientes** se añaden marcados con fuente «integrado» (filtrable en la vista
Pendientes). Los respaldos reales y esta planilla se conservan en `data/`.

## Funcionalidades

- **Lectura del `.xlsm`** (`PMP_2026` + `Registro_MP-2026`): encabezados en la fila 7,
  datos desde la fila 8, identificación de columnas por encabezado, exclusión de
  `Q`/`S`, columnas auxiliares tras `AR` ignoradas, N° Inventario y Serie como
  **texto** (conservan ceros a la izquierda). Datos sucios en celdas de mes → ignorados.
- **Persistencia** IndexedDB + localStorage (3 claves: preferencias, planilla cruda,
  datos de usuario). **Reset total**, aviso de respaldo al salir y **auto-guardado a
  carpeta** (File System Access API) donde haya soporte.
- **Respaldo doble**: «Respaldar ahora» genera el **JSON** reimportable *y* un libro
  **Excel** (`.xlsx`) con una hoja por colección (equipos, mantenciones, pendientes,
  correctivos, avances y resumen mes×código). También disponibles por separado en «Más».
- **Tablero / centro de control**: banda de KPIs reactivos, tablero de operatividad
  y panel «Por volcar al `.xlsm`» (celda exacta hoja/fila/columna, copiar/exportar).
- **Equipos**: tabla densa (~1.000 filas, sin virtualización), búsqueda global con
  *debounce*, **filtro por encabezado tipo Excel** (multi-selección), selector de
  columnas recordado, orden por columna, filtros rápidos combinables, exportación a Excel.
- **Ficha de equipo**: historial cronológico unificado (MP + pendientes + correctivos).
- **Mantenimiento Preventivo**: registro del detalle, ciclo Borrador→Oficial,
  pendiente automático con causal o gestión pendiente.
- **Mantenimiento Correctivo**: expediente flexible por **Folio SIGEM** (sub-eventos
  en cualquier orden, OT por completar).
- **Pendientes**: 5 tipos en 3 orígenes, con **bitácora `seguimiento[]`** (agregar en
  un clic) y `enEsperaDe`.
- **Reprogramación**: ciclo C1/C5/C6/C7/C8 (generar → imprimir → 2 firmas → oficializar).
- **Resumen mes × código** con conteos y *drill-down* a la vista de Equipos.

## Lógica de negocio

Documentada en `docs/` (fuente de verdad del comportamiento):

- `docs/Especificacion_MP2026.md` — especificación de implementación.
- `docs/ArbolDecision_MP2026.md` — modelo de datos y reglas de decisión (Parte A/B).
- `docs/Diagramas_MP2026_ajustado.md` — proceso de mantenimiento (auditado).
- `docs/arbol-pendientes.html` — modelo del módulo de pendientes (5 tipos / 3 orígenes).

Puntos clave: clave del equipo = serie si existe, si no inventario · la planilla
manda sobre la app · Borrador nace y se promueve a Oficial al recargar · cumplimiento
solo cuenta `Si`/`Si-RA` · `C1/C5/C6/C7/C8` → reprogramación ≤30 días, `C2/C3/C4` sin fecha.

## Estructura del repositorio

```
index.html                 La aplicación (HTML + JS vanilla + SheetJS por CDN)
docs/                       Especificación y diagramas (lógica de negocio)
sample-data/
  ProgramaciónMP2026.xlsm   Planilla oficial de ejemplo (fuente de datos)
  ejemplo_respaldo_anterior.json  Respaldo viejo de prueba para la migración
```
