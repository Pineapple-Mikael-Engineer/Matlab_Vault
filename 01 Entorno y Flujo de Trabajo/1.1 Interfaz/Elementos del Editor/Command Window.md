---
title: "Command Window"
tags:
  - MATLAB
  - interfaz
  - command-window
  - entorno
  - flujo-de-trabajo
aliases:
  - "Command Window MATLAB"
---

# Command Window

Es el terminal interactivo de MATLAB donde ejecutas comandos en tiempo real y ves los resultados inmediatamente.

## ¿Qué puedes hacer aquí?

- **Cálculos rápidos**:
  ```matlab
  >> 2 + 2
  ans = 4
  
  >> sqrt(16) * pi
  ans = 12.5664
  ```

- **Probar funciones sueltas** sin necesidad de crear un archivo:
  ```matlab
  >> sin(pi/2)
  ans = 1
  ```

- **Llamar [[Scripts|scripts y funciones]]** que hayas guardado:
  ```matlab
  >> mi_script
  >> resultado = mi_funcion(5, 10)
  ```

- **Inspeccionar y modificar variables** del [[Workspace]] en tiempo real:
  ```matlab
  >> x = [1 2 3; 4 5 6];
  >> x(2,3)
  ans = 6
  >> x(1,:) = [10 20 30]
  ```

- **[[Comandos de depuración|Depurar]]**: cuando el código se pausa en un breakpoint, el Command Window te permite inspeccionar variables y ejecutar comandos en ese contexto.

## Características clave

| Característica | Uso |
|----------------|-----|
| **Historial** | Flechas arriba/abajo para recuperar comandos anteriores. `↑` varias veces para navegar. Ver [[Atajos de teclado esenciales en MATLAB|Atajos de teclado esenciales]] |
| **Autocompletado** | `Tab` para completar variables, funciones o nombres de archivos. |
| **Limpiar pantalla** | `clc` (clear command window) — no borra variables, solo la pantalla. |
| **Ayuda rápida** | `doc comando` abre documentación completa. `help comando` muestra ayuda en el Command Window. |
| **Múltiples líneas** | `...` (tres puntos) para continuar un comando en la línea siguiente. |

## Atajos útiles

| Atajo | Acción |
|-------|--------|
| `↑` / `↓` | Navegar por historial de comandos |
| `Tab` | Autocompletar |
| `Ctrl+C` | Cancelar ejecución (útil si el código se queda en bucle infinito) |
| `clc` | Limpiar pantalla |
| `clear` | Limpiar variables del workspace |
| `clear all` | Limpiar todo (variables, funciones compiladas, etc.) |

## Casos de uso típicos

1. **Exploración interactiva**: Pruebas rápidas antes de escribir un script.
2. **Depuración**: Inspeccionar valores cuando el código se detiene en un breakpoint.
3. **Aprendizaje**: Probar funciones nuevas y ver su comportamiento inmediato.
4. **Control del entorno**: Añadir rutas (`addpath`), cargar datos (`load`), gestionar workspace. Ver [[addpath y savepath|Configuración de la ruta de búsqueda]].

## Analogía

El Command Window es la **"calculadora inteligente + consola"** — pero con superpoderes: no solo calcula, sino que también controla todo el entorno MATLAB, inspecciona variables y ejecuta cualquier comando del sistema.

---

### Relación con otros elementos

- [[Editor]]: Escribes código que luego ejecutas desde el Command Window o directamente desde el Editor.
- [[Workspace]]: El Command Window te permite ver y modificar las variables que aparecen allí.
- [[Current Folder]]: Puedes navegar por directorios con `cd` y ejecutar scripts ubicados en la carpeta actual.

---
