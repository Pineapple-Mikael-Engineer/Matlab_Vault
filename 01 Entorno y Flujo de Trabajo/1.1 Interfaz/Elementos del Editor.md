---
title: "Editor, Command Window, Workspace, Current Folder"
tags:
  - MATLAB
  - entorno
  - interfaz
  - flujo-de-trabajo
  - editor
  - command-window
  - workspace
  - current-folder
  - archivos
  - variables
  - debug
aliases:
  - "Entorno MATLAB"
  - "Interfaz MATLAB"
  - "Editor MATLAB"
  - "Command Window MATLAB"
  - "Workspace MATLAB"
  - "Current Folder MATLAB"
---


# Editor

Es el espacio principal donde escribes, editas y guardas el código en MATLAB.

## ¿Qué puedes crear aquí?
- **Scripts** (`.m`): Secuencias de comandos lineales que comparten el [[Workspace]] con el [[Command Window]].
- **Funciones** (`.m`): Código con entrada/salida definida y workspace propio. Ver [[Scripts vs Funciones]].
- **Clases** (`.m`): Definiciones de [[Programación orientada a objetos]].
- **Live Scripts** (`.mlx`): Documentos interactivos que combinan código, texto, ecuaciones y gráficos. Ver [[Archivos .m .mat .mlx]].

## Características principales
- **Resaltado de sintaxis**: Colores diferenciados para variables, funciones, comentarios, strings, etc.
- **Autocompletado**: Presiona `Tab` para completar variables, funciones o nombres de archivos.
- **Debugging integrado**: Se puede establecer [[Depuración|puntos de interrupción]] haciendo clic en el margen izquierdo (aparece un círculo rojo).
- **Ejecución selectiva**:
  - `F5` o botón "Run": ejecuta todo el archivo.
  - `Ctrl+Enter`: ejecuta la selección actual o la línea donde está el cursor. Ver [[Atajos de teclado esenciales]].
  - `Ctrl+Shift+Enter`: ejecuta la selección y avanza a la siguiente sección.
- **Celdas de código**: Usa `%%` para dividir el código en secciones ejecutables independientemente.
- **Múltiples documentos**: Trabaja con pestañas para alternar entre diferentes archivos.

## Ejemplo práctico

```matlab
%% Sección 1: Generar datos
x = 0:0.1:10;
y = sin(x);

%% Sección 2: Visualizar
plot(x, y);
xlabel('x');
ylabel('sin(x)');
title('Gráfico de seno');
grid on;
```

Puedes ejecutar cada sección por separado con `Ctrl+Enter` mientras el cursor esté dentro de ella.

## Analogía
El Editor es tu **"cuaderno de programación"**: allí escribes el código que luego MATLAB ejecuta, a diferencia del [[Command Window]] donde ejecutas comandos sueltos de forma interactiva.

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

- **Llamar [[Scripts vs Funciones|scripts y funciones]]** que hayas guardado:
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

- **[[Depuración|Depurar]]**: cuando el código se pausa en un breakpoint, el Command Window te permite inspeccionar variables y ejecutar comandos en ese contexto.

## Características clave

| Característica | Uso |
|----------------|-----|
| **Historial** | Flechas arriba/abajo para recuperar comandos anteriores. `↑` varias veces para navegar. Ver [[Atajos de teclado esenciales]] |
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
4. **Control del entorno**: Añadir rutas (`addpath`), cargar datos (`load`), gestionar workspace. Ver [[Configuración de la ruta de búsqueda]].

## Analogía

El Command Window es la **"calculadora inteligente + consola"** — pero con superpoderes: no solo calcula, sino que también controla todo el entorno MATLAB, inspecciona variables y ejecuta cualquier comando del sistema.

---

### Relación con otros elementos

- [[Editor]]: Escribes código que luego ejecutas desde el Command Window o directamente desde el Editor.
- [[Workspace]]: El Command Window te permite ver y modificar las variables que aparecen allí.
- [[Current Folder]]: Puedes navegar por directorios con `cd` y ejecutar scripts ubicados en la carpeta actual.

---

# Workspace

Es el espacio de memoria donde MATLAB almacena todas las variables que has creado durante la sesión actual. El Workspace visual (ventana) te permite inspeccionarlas y gestionarlas gráficamente.

## ¿Qué información muestra?

| Columna | Descripción |
|---------|-------------|
| **Nombre** | Identificador de la variable |
| **Valor** | Contenido (resumido si es grande) |
| **Tipo** | Clase de la variable: `double`, `cell`, `table`, `struct`, etc. Ver [[Tipos de datos y estructuras]] |
| **Tamaño** | Dimensiones: `3x4`, `1x100`, etc. |
| **Atributos** | Si es global, persistente, etc. (opcional según versión) |

## Operaciones comunes

### Desde la ventana gráfica
- **Doble clic**: Abre el **Variable Editor** (una hoja de cálculo) para ver/editar el contenido.
- **Clic derecho**: Opciones para graficar, guardar, copiar, eliminar o inspeccionar más a fondo.
- **Botón "Import Data"**: Cargar datos desde archivos externos. Ver [[Entrada/salida de datos]].

### Desde el [[Command Window]]
```matlab
% Listar variables
who          % Nombres de variables
whos         % Detalles completos (tipo, tamaño, memoria)

% Eliminar variables
clear a b    % Elimina variables específicas
clear all    % Elimina TODO (con cuidado)
clearvars -except a b  % Elimina todo excepto a y b

% Guardar y cargar workspace
save archivo.mat        % Guarda todo
save archivo.mat a b    % Guarda solo a y b
load archivo.mat        % Carga variables
```

## Ejemplo práctico

```matlab
>> a = [1 2 3];
>> b = rand(5);
>> c = 'hola mundo';
>> d = {1, 'texto', [4 5 6]};

>> whos
  Name      Size            Bytes  Class     Attributes
  a         1x3                24  double
  b         5x5               200  double
  c         1x10               20  char
  d         1x3               432  cell
```

## Casos de uso importantes

1. **Verificar resultados**: Confirma que las operaciones produjeron lo esperado.
2. **[[Depuración]]**: Cuando el código se pausa (breakpoint), puedes inspeccionar el estado actual de las variables.
3. **Memoria**: Identificar variables grandes que consumen RAM (`whos` muestra los bytes).
4. **Organización**: Limpiar variables no utilizadas (`clear`) para evitar confusiones.
5. **Transferencia**: Guardar/exportar datos a archivos `.mat` para compartir o reutilizar. Ver [[Archivos .m .mat .mlx]].

## Tipos de workspace

| Tipo | Descripción |
|------|-------------|
| **Base workspace** | Workspace principal donde trabajas normalmente en Command Window y scripts. |
| **Function workspace** | Cada función tiene su propio workspace independiente. Ver [[Scripts vs Funciones]]. |
| **Debug workspace** | Cuando hay un breakpoint activo, el workspace refleja el contexto donde se pausó el código. |

## Buenas prácticas

- **No usar `clear all` indiscriminadamente**: Borra también funciones compiladas y puede causar errores inesperados.
- **Preasignar memoria**: Antes de bucles grandes, crea arreglos vacíos (`zeros`, `NaN`) para evitar que el workspace se redimensione constantemente. Ver [[Rendimiento y optimización]].
- **Limpiar variables temporales**: Si usas variables como `i`, `j`, `temp`, elimínalas al final para no confundirte.

## Analogía

El Workspace es tu **"inventario de datos"** en memoria. Mientras que el [[Command Window]] es la consola donde operas, el Workspace te muestra en todo momento qué materiales tienes disponibles para trabajar.

---

### Relación con otros elementos

- [[Editor]]: Cuando ejecutas un script, las variables creadas aparecen en el Workspace base.
- [[Command Window]]: Puedes manipular las variables directamente desde allí.
- [[Current Folder]]: Puedes guardar/ cargar archivos `.mat` con el contenido del Workspace.

---

# Current Folder

Es el explorador de archivos integrado de MATLAB que muestra el contenido de la carpeta activa (working directory) y te permite gestionar archivos sin salir del entorno.

## ¿Qué puedes ver y hacer?

| Acción                 | Cómo                                                                          |
| ---------------------- | ----------------------------------------------------------------------------- |
| **Ver archivos**       | Lista de [[Archivos .m .mat .mlx \| .m, .mat, .mlx, .fig]], datos, etc.       |
| **Abrir archivos**     | Doble clic en un script, función o dato.                                      |
| **Cambiar de carpeta** | Navegar por el árbol de directorios o usar la barra de dirección.             |
| **Ejecutar scripts**   | Clic derecho → "Run" o simplemente escribir el nombre en [[Command Window]].  |
| **Gestionar archivos** | Crear, renombrar, copiar, pegar, eliminar (similar a un explorador estándar). |
| **Ver detalles**       | Tamaño, fecha de modificación, tipo de archivo.                               |

## La carpeta activa: concepto clave

MATLAB siempre tiene una **carpeta activa** (working directory). Este es el lugar donde:
- Busca archivos cuando escribes un comando.
- Guarda archivos por defecto (si no especificas ruta).
- Ejecuta [[Scripts vs Funciones|scripts]] si están en esa ubicación.

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
- **Añadir rutas con `addpath`**: Si necesitas acceder a funciones desde otra carpeta sin moverte, añade esa ruta. Ver [[Configuración de la ruta de búsqueda]].

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
| **Ruta de búsqueda** | MATLAB busca en Current Folder **antes** que en otras rutas (prioridad máxima). Ver [[Configuración de la ruta de búsqueda]] |

---

### Dato importante: Current Folder vs Path

| Concepto | Función |
|----------|---------|
| **Current Folder** | Carpeta activa donde estás "parado". MATLAB ejecuta scripts de aquí directamente. |
| **Path (ruta de búsqueda)** | Lista de carpetas donde MATLAB busca funciones cuando no están en Current Folder. Ver [[Configuración de la ruta de búsqueda]] |

Puedes tener scripts en otras carpetas añadidas al path y ejecutarlos sin cambiar de carpeta activa.

---

## Notas relacionadas

- [[Atajos de teclado esenciales]]
- [[Scripts vs Funciones]]
- [[Depuración]]
- [[Archivos .m .mat .mlx]]
- [[Configuración de la ruta de búsqueda]]
- [[Tipos de datos y estructuras]]
