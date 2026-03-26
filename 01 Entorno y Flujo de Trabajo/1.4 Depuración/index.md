---
title: "1.4 Depuración"
tags:
  - MATLAB
  - depuración
  - debug
  - índice
  - obsidian
aliases:
  - "Debug en MATLAB"
  - "Diagnóstico de errores MATLAB"
---

# 1.4 Depuración

Depurar no es “arreglar errores al azar”: es inspeccionar estado, flujo y supuestos para localizar causa raíz. Esta sección convierte la depuración en un proceso metódico usando breakpoints y comandos `db*`.

## Regla mental

No preguntes “¿qué línea está mal?” primero. Pregunta: **“¿en qué estado empezó a desviarse el programa?”**.

La depuración efectiva es rastreo de estado + control de ejecución.

## Notas principales

### [[Puntos de interrupción]]

Explica cómo pausar ejecución en lugares estratégicos y analizar contexto local.

### [[Comandos de depuración]]

Cubre comandos de control (`dbstop`, `dbstep`, `dbcont`, `dbclear`, `dbstatus`, `dbquit`, etc.).

## Flujo de depuración recomendado

```matlab
% 1) Activar pausa en error
dbstop if error

% 2) Ejecutar script/función problemática
mi_script

% 3) Ya en pausa, inspeccionar
whos

% 4) Avanzar controladamente
% dbstep
% dbcont
```

Este patrón evita “parches ciegos” y produce diagnósticos repetibles.

## Tabla de diagnóstico rápido

| Situación | Herramienta recomendada |
|---|---|
| Error inmediato y reproducible | `dbstop if error` |
| Lógica compleja paso a paso | `dbstep` + breakpoints |
| Sospecha de valores inesperados | Workspace + inspección en pausa |
| Demasiados breakpoints | `dbstatus` / `dbclear` |

## Comparaciones

### Depuración interactiva vs prints indiscriminados

- Interactiva: observas estado real justo en el punto crítico.
- Prints indiscriminados: ruido y poca precisión temporal.

### Corregir síntoma vs corregir causa

- Síntoma: cambias una línea hasta que “deja de fallar”.
- Causa: identificas supuesto roto (tipo, tamaño, orden, ruta, etc.).

## Buenas prácticas

1. Define hipótesis antes de poner breakpoints.
2. Reduce caso de prueba al mínimo reproducible.
3. Verifica dimensiones/tipos en puntos críticos.
4. Limpia breakpoints al terminar sesión.
5. Registra la causa raíz y la corrección final.

## Errores comunes

- Saltar directo a editar código sin inspeccionar estado.
- Depurar con contexto contaminado por variables viejas.
- Poner demasiados breakpoints sin criterio.
- No validar que el fix realmente cubre el caso raíz.

## Notas relacionadas

- [[Puntos de interrupción]]
- [[Comandos de depuración]]
- [[Workspace]]
- [[Editor]]
- [[01 Entorno y Flujo de Trabajo/1.3 Scripts vs funciones/index|Scripts vs funciones]]

## Escenario de depuración bien ejecutado

Supón que una función de análisis devuelve `NaN` inesperados. En vez de reescribir el algoritmo completo, activas `dbstop if error` o colocas un breakpoint antes del cálculo crítico. Avanzas con `dbstep`, inspeccionas variables y detectas que un vector llega vacío por una condición previa mal formulada. Corriges el origen y no el síntoma.

Este enfoque reduce tiempo de corrección y evita introducir regresiones.


]