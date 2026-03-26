---
title: "Current Folder"
tags:
  - MATLAB
  - interfaz
  - current-folder
  - archivos
  - entorno
aliases:
  - "Current Folder MATLAB"
---

# Current Folder

Es el explorador de archivos integrado de MATLAB que muestra el contenido de la carpeta activa (working directory) y te permite gestionar archivos sin salir del entorno.

## ¿Qué puedes ver y hacer?

| Acción                 | Cómo                                                                          |
| ---------------------- | ----------------------------------------------------------------------------- |
| **Ver archivos**       | Lista de archivos `.m`, `.mat`, `.mlx`, `.fig`, datos, etc.       |
| **Abrir archivos**     | Doble clic en un script, función o dato.                                      |
| **Cambiar de carpeta** | Navegar por el árbol de directorios o usar la barra de dirección.             |
| **Ejecutar scripts**   | Clic derecho → "Run" o simplemente escribir el nombre en [[Command Window]].  |
| **Gestionar archivos** | Crear, renombrar, copiar, pegar, eliminar (similar a un explorador estándar). |
| **Ver detalles**       | Tamaño, fecha de modificación, tipo de archivo.                               |

## La carpeta activa: concepto clave

MATLAB siempre tiene una **carpeta activa** (working directory). Este es el lugar donde:
- Busca archivos cuando escribes un comando.
- Guarda archivos por defecto (si no especificas ruta).
- Ejecuta [[Scripts|scripts]] si están en esa ubicación.

```matlab
% Desde Command Window
pwd                  % Muestra la carpeta activa actual (print working directory)
cd nombre_carpeta    % Cambia a una subcarpeta
cd ..                % Sube un nivel
cd /ruta/completa    % Va a una ruta absoluta
```

## ¿Cómo saber qué carpeta está activa?

- **Barra superior** de la ventana Current Folder muestra la ruta actual.
- **[[Command Window]]**: `pwd` devuelve la ruta.
- **Indicador visual**: En la barra de herramientas, la carpeta actual aparece resaltada.

## Buenas prácticas con Current Folder

### Recomendado
- **Organizar proyectos en carpetas independientes**: Cada proyecto tiene su propia carpeta con scripts, datos y resultados.
- **Usar `cd` con moderación**: Prefiere usar **rutas relativas** y mantener el proyecto en una sola ubicación.
- **Añadir rutas con `addpath`**: Si necesitas acceder a funciones desde otra carpeta sin moverte, añade esa ruta. Ver [[addpath y savepath|Configuración de la ruta de búsqueda]].

### Evitar
- **Trabajar en carpetas del sistema** (`C:\Program Files`, `C:\Windows`).
- **Tener archivos sueltos sin organización**: Difícil de mantener y compartir.
- **Confiar solo en "Save As"**: Asegúrate de saber dónde estás guardando.

## Configuración de inicio

Puedes definir la carpeta inicial al abrir MATLAB:
- **Preferencias** → **General** → **Initial working folder**.
- Opciones: última carpeta usada, carpeta específica, o carpeta de MATLAB.

## Analogía

El Current Folder es el **"administrador de archivos"** dentro de MATLAB. Funciona como el explorador de Windows o Finder de Mac, pero integrado al entorno: lo que ves ahí es lo que MATLAB puede ejecutar fácilmente.

---

### Relación con otros elementos

| Elemento | Conexión |
|----------|---------|
| [[Command Window]] | Desde allí puedes navegar con `cd`, `pwd`, y ejecutar scripts de la carpeta actual. |
| [[Editor]] | Al guardar un archivo, por defecto se guarda en Current Folder. |
| **Ruta de búsqueda** | MATLAB busca en Current Folder **antes** que en otras rutas (prioridad máxima). Ver [[addpath y savepath|Configuración de la ruta de búsqueda]] |

---

### Dato importante: Current Folder vs Path

| Concepto | Función |
|----------|---------|
| **Current Folder** | Carpeta activa donde estás "parado". MATLAB ejecuta scripts de aquí directamente. |
| **Path (ruta de búsqueda)** | Lista de carpetas donde MATLAB busca funciones cuando no están en Current Folder. Ver [[addpath y savepath|Configuración de la ruta de búsqueda]] |

Puedes tener scripts en otras carpetas añadidas al path y ejecutarlos sin cambiar de carpeta activa.

---

## Notas relacionadas

- [[Atajos de teclado esenciales en MATLAB|Atajos de teclado esenciales]]
- [[Scripts]]
- [[Comandos de depuración|Depuración]]
- archivos `.m`, `.mat`, `.mlx`
- [[addpath y savepath|Configuración de la ruta de búsqueda]]
- tipos de datos y estructuras
