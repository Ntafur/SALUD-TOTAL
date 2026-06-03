# Archivos de prueba — Nota crédito "NA" y selección de archivos

Estos JSON sirven para validar las nuevas funciones del **Editor RIPS · SALUD TOTAL**:

1. Casilla de selección por archivo enlazada con **Descargar todos (7z)**.
2. Filtros **Seleccionar todos** / **Seleccionar modificados**.
3. Botón **NA** (al lado de *Autocompletar nulls*): genera la nota crédito
   `numNota` = valor de `numFactura` y `tipoNota` = `"NA"`, **solo en archivos
   con estado *Modificado***.

## Archivos incluidos

| Archivo | numFactura | Estado al cargar | Motivo |
|---|---|---|---|
| `PRUEBA_NA_modificado_11323405.json` | `11323405` | **Modificado** | Trae un medicamento con `codTecnologiaSalud` `...-08`, que el editor normaliza a `...-8` al cargar y lo marca como modificado. |
| `PRUEBA_NA_modificado_11323406.json` | `11323406` | **Modificado** | Igual que el anterior (`...-04` → `...-4`). Además incluye una consulta con `numAutorizacion: null` para probar *Autocompletar nulls*. |
| `PRUEBA_SIN_CAMBIOS_11323407.json` | `11323407` | **Sin cambios** | Ya viene normalizado y sin nulls; el botón **NA** debe **omitirlo**. |

En los tres archivos `numNota` y `tipoNota` empiezan en `null`.

## Pasos de prueba

1. Abrir `Editor_RIPS_10_1.html` y cargar los 3 archivos de esta carpeta.
2. En la barra lateral, cada archivo aparece con una **casilla** marcada.
   - **Seleccionar modificados** deja marcados solo los dos archivos
     `...11323405` y `...11323406`.
   - **Seleccionar todos** vuelve a marcar los tres.
3. Pulsar el botón **NA** (cabecera).
   - Resultado esperado:
     - `PRUEBA_NA_modificado_11323405.json` → `numNota: "11323405"`, `tipoNota: "NA"`
     - `PRUEBA_NA_modificado_11323406.json` → `numNota: "11323406"`, `tipoNota: "NA"`
     - `PRUEBA_SIN_CAMBIOS_11323407.json` → **sin cambios** (`numNota: null`, `tipoNota: null`)
4. Pulsar **Descargar todos (7z)**: el `.7z` contiene únicamente los archivos
   cuya casilla esté marcada (el contador junto al botón indica cuántos se
   incluirán).

> Nota: el botón **NA** solo reemplaza `numNota`/`tipoNota` cuando están en
> `null`/vacío; nunca pisa un valor ya existente.
