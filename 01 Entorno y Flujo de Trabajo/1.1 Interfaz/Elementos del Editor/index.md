---
title: "Elementos del Editor"
tags:
  - índice
  - interfaz
  - editor
---

# Elementos del Editor

Este índice resume los cuatro componentes base del entorno de MATLAB que se usan en el trabajo diario: edición de código, ejecución interactiva, inspección de variables y gestión de archivos.

En conjunto, estos elementos permiten pasar de una idea a una ejecución verificable: escribes código, lo pruebas por partes, inspeccionas resultados y organizas los archivos del proyecto.

## 1) Editor

El **Editor** es donde se escribe la mayor parte del código de forma estructurada. Es el espacio natural para scripts y funciones, con herramientas de apoyo como resaltado de sintaxis, autocompletado, secciones (`%%`) y acceso directo a depuración.

### ¿Para qué sirve principalmente?
- Crear y mantener archivos `.m`.
- Ejecutar todo el script o solo bloques específicos.
- Preparar código legible para documentación/publicación.

### Errores comunes relacionados
- Ejecutar partes sueltas sin mantener orden lógico de inicialización.
- No dividir en celdas cuando el análisis es largo.

**Ver nota:** [[Editor]]

## 2) Command Window

El **Command Window** es la consola interactiva para pruebas rápidas. Se usa para ejecutar comandos inmediatos, validar hipótesis, inspeccionar variables durante depuración y lanzar scripts o funciones por nombre.

### ¿Para qué sirve principalmente?
- Probar expresiones sin editar el archivo completo.
- Revisar resultados de forma iterativa.
- Ejecutar comandos de depuración (`db*`) y utilidades de entorno (`clc`, `clear`, `whos`, etc.).

### Errores comunes relacionados
- Depender solo de ejecución interactiva y no guardar cambios en scripts/funciones.
- Perder reproducibilidad por trabajar solo con historial de consola.

**Ver nota:** [[Command Window]]

## 3) Workspace

El **Workspace** muestra las variables en memoria de la sesión, con nombre, tipo, tamaño y valor resumido. Es clave para validar el estado del programa y detectar inconsistencias de datos.

### ¿Para qué sirve principalmente?
- Ver qué variables existen en cada momento.
- Confirmar dimensiones/tipos antes de operar.
- Detectar residuos de ejecuciones previas.

### Errores comunes relacionados
- Confiar en variables que quedaron de ejecuciones anteriores.
- No distinguir entre workspace base y workspace de función.

**Ver nota:** [[Workspace]]

## 4) Current Folder

**Current Folder** define la carpeta activa de trabajo. Desde allí se navega por archivos del proyecto y se controla qué scripts están disponibles directamente por nombre.

### ¿Para qué sirve principalmente?
- Abrir, crear y organizar archivos del proyecto.
- Ejecutar scripts de la carpeta activa.
- Mantener orden de trabajo por proyecto/sesión.

### Errores comunes relacionados
- Ejecutar desde una carpeta equivocada.
- Confundir disponibilidad por carpeta activa con disponibilidad por `path`.

**Ver nota:** [[Current Folder]]

## Relación práctica entre los cuatro componentes

- El flujo normal comienza en **Editor** (escritura) y pasa por **Command Window** (prueba rápida).
- El estado intermedio se verifica en **Workspace**.
- Todo ocurre en el contexto de archivos definido por **Current Folder**.

Si uno de estos cuatro está desalineado (por ejemplo, carpeta incorrecta o variables contaminadas), aparecen errores que parecen de código pero en realidad son de entorno.
