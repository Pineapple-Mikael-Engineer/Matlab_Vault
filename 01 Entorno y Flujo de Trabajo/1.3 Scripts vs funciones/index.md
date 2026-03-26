---
title: "1.3 Scripts vs funciones"
tags:
  - MATLAB
  - scripts
  - funciones
  - índice
  - obsidian
aliases:
  - "Arquitectura de código MATLAB"
  - "Modularidad en MATLAB"
---

# 1.3 Scripts vs funciones

Este índice resume la decisión más importante de diseño en MATLAB básico/intermedio: cuándo usar script y cuándo función. No es un detalle de estilo; impacta mantenibilidad, reutilización, pruebas y depuración.

## Regla mental

- **Script**: orquesta flujo de trabajo.
- **Función**: encapsula lógica reusable con entradas/salidas explícitas.

Cuando todo es script, el proyecto crece desordenado. Cuando todo es función sin criterio, el flujo global se vuelve fragmentado. El equilibrio correcto mejora calidad y velocidad.

## Piezas de esta sección

### [[Scripts]]

Punto de entrada típico, ejecución secuencial en workspace base. Ideal para pipelines, demos y scripts de control.

### [[Live Scripts]]

Formato `.mlx` orientado a análisis narrativo, enseñanza y reporte interactivo.

### [[01 Entorno y Flujo de Trabajo/1.3 Scripts vs funciones/Funciones/index|Funciones en MATLAB]]

Subíndice completo de variantes y patrones de funciones: en archivo, anónimas, locales/privadas, validación, argumentos variables.

### [[Funciones Anidadas]]

Caso especial para encapsulación con acceso a contexto de función externa.

## Ejemplo comparativo

```matlab
% Script (main.m)
datos = rand(1,100);
media = calcularMedia(datos);
fprintf('Media: %.3f\n', media);

% Función (calcularMedia.m)
function m = calcularMedia(x)
    m = mean(x);
end
```

El script coordina; la función calcula.

## Tabla de decisión

| Criterio | Script | Función |
|---|---|---|
| Reutilización | Baja-media | Alta |
| Encapsulación | Baja | Alta |
| Claridad de entradas/salidas | Implícita | Explícita |
| Escalabilidad | Limitada | Mejor |
| Uso típico | Orquestación | Lógica de negocio |

## Comparaciones útiles

### Script puro vs arquitectura mixta

- Script puro: rápido al inicio, frágil al crecer.
- Mixta (script + funciones): mejor mantenimiento y pruebas.

### Función tradicional vs anónima

- Tradicional: lógica compleja o reutilizable.
- Anónima: operaciones simples y localizadas.

## Buenas prácticas

1. Mantén script principal breve y descriptivo.
2. Extrae a funciones toda lógica repetida.
3. Usa nombres de función alineados con acción/resultados.
4. Aplica validación de argumentos cuando proceda.
5. Documenta responsabilidad de cada función.

## Errores comunes

- Scripts gigantes con estado implícito difícil de rastrear.
- Funciones sin contrato claro de entrada/salida.
- Repetición de bloques por no modularizar.
- Mezclar pruebas de consola con código final.

## Notas relacionadas

- [[Scripts]]
- [[Live Scripts]]
- [[01 Entorno y Flujo de Trabajo/1.3 Scripts vs funciones/Funciones/index|Funciones en MATLAB]]
- [[Funciones Anidadas]]
- [[01 Entorno y Flujo de Trabajo/1.4 Depuración/index|Depuración]]

## Escenario de evolución de proyecto

Un proyecto suele empezar con un script corto. Al crecer, aparecen cálculos repetidos, validaciones similares y bloques difíciles de mantener. Ahí conviene extraer funciones para separar responsabilidades. El script queda como “director” del flujo, mientras las funciones ejecutan tareas concretas.

Esta transición no solo mejora orden; también facilita pruebas unitarias, reutilización y depuración localizada.

## Patrón recomendado

- Script principal: configuración + secuencia de pasos.
- Funciones: cálculo, validación, transformación y visualización reusable.
- Documentación: breve encabezado por función y ejemplos de uso.
