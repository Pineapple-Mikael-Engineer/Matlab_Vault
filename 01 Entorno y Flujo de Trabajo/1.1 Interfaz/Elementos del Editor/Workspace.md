---
title: "Workspace"
tags:
  - MATLAB
  - interfaz
  - workspace
  - variables
  - entorno
aliases:
  - "Workspace MATLAB"
---

# Workspace

Es el espacio de memoria donde MATLAB almacena todas las variables que has creado durante la sesión actual. El Workspace visual (ventana) te permite inspeccionarlas y gestionarlas gráficamente.

## ¿Qué información muestra?

| Columna | Descripción |
|---------|-------------|
| **Nombre** | Identificador de la variable |
| **Valor** | Contenido (resumido si es grande) |
| **Tipo** | Clase de la variable: `double`, `cell`, `table`, `struct`, etc. Ver tipos de datos y estructuras |
| **Tamaño** | Dimensiones: `3x4`, `1x100`, etc. |
| **Atributos** | Si es global, persistente, etc. (opcional según versión) |

## Operaciones comunes

### Desde la ventana gráfica
- **Doble clic**: Abre el **Variable Editor** (una hoja de cálculo) para ver/editar el contenido.
- **Clic derecho**: Opciones para graficar, guardar, copiar, eliminar o inspeccionar más a fondo.
- **Botón "Import Data"**: Cargar datos desde archivos externos. Ver entrada/salida de datos.

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
2. **[[Comandos de depuración|Depuración]]**: Cuando el código se pausa (breakpoint), puedes inspeccionar el estado actual de las variables.
3. **Memoria**: Identificar variables grandes que consumen RAM (`whos` muestra los bytes).
4. **Organización**: Limpiar variables no utilizadas (`clear`) para evitar confusiones.
5. **Transferencia**: Guardar/exportar datos a archivos `.mat` para compartir o reutilizar. Ver archivos `.m`, `.mat`, `.mlx`.

## Tipos de workspace

| Tipo | Descripción |
|------|-------------|
| **Base workspace** | Workspace principal donde trabajas normalmente en Command Window y scripts. |
| **Function workspace** | Cada función tiene su propio workspace independiente. Ver [[Scripts]]. |
| **Debug workspace** | Cuando hay un breakpoint activo, el workspace refleja el contexto donde se pausó el código. |

## Buenas prácticas

- **No usar `clear all` indiscriminadamente**: Borra también funciones compiladas y puede causar errores inesperados.
- **Preasignar memoria**: Antes de bucles grandes, crea arreglos vacíos (`zeros`, `NaN`) para evitar que el workspace se redimensione constantemente. Ver rendimiento y optimización.
- **Limpiar variables temporales**: Si usas variables como `i`, `j`, `temp`, elimínalas al final para no confundirte.

## Analogía

El Workspace es tu **"inventario de datos"** en memoria. Mientras que el [[Command Window]] es la consola donde operas, el Workspace te muestra en todo momento qué materiales tienes disponibles para trabajar.

---

### Relación con otros elementos

- [[Editor]]: Cuando ejecutas un script, las variables creadas aparecen en el Workspace base.
- [[Command Window]]: Puedes manipular las variables directamente desde allí.
- [[Current Folder]]: Puedes guardar/ cargar archivos `.mat` con el contenido del Workspace.

---
