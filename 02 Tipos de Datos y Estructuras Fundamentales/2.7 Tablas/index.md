---
title: Tablas
aliases:
  - dataframes
  - datos tabulares
  - tablas MATLAB
  - importar datos
tags:
  - matlab
  - tablas
  - concepto
  - index
draft: true
---

# Tablas

Las tablas son estructuras de datos diseñadas para almacenar datos tabulares heterogéneos, similares a hojas de cálculo o dataframes. Cada columna puede tener un tipo diferente (numérico, string, datetime, etc.) y las columnas tienen nombre. Son la herramienta recomendada para trabajar con datos provenientes de archivos CSV, Excel o bases de datos.

## Creación y lectura

Existen múltiples formas de crear una tabla. La más directa es con la función `table()`, pasando las columnas como variables separadas. Cuando los datos ya están en otros formatos, se puede convertir usando `array2table` (desde matriz numérica), `struct2table` (desde arreglo de estructuras) o `cell2table` (desde cell array).

```matlab
% Creación manual
nombres = ["Ana"; "Juan"; "Luis"];
edades = [25; 30; 28];
T = table(nombres, edades, 'VariableNames', {'Nombre', 'Edad'});
```

Para importar datos desde archivos externos, la función principal es `readtable`. MATLAB detecta automáticamente delimitadores, encabezados y tipos de datos. Soporta CSV, Excel (`.xlsx`), archivos de texto delimitados, Parquet, SPSS y Stata. Para exportar se usa `writetable`. La función `summary` proporciona un resumen rápido de cada columna, y para inspeccionar los datos se usan `head` y `tail`. Para un desarrollo completo de estos temas, ver [[Creacion y lectura]].

## Manipulación básica

Una vez que los datos están en una tabla, las operaciones más comunes incluyen ordenamiento, filtrado, agrupación y transformación de columnas.

El ordenamiento se realiza con `sortrows`, permitiendo ordenar por una o múltiples columnas, en orden ascendente o descendente. Para encontrar valores únicos en una columna se usa `unique`. Cuando se necesita agrupar datos y calcular estadísticas por categoría, la función más potente es `groupsummary`, que permite calcular medias, desviaciones estándar, conteos, o funciones personalizadas para cada grupo.

```matlab
% Media de Edad y Peso por Nombre
resumen = groupsummary(T, 'Nombre', 'mean', {'Edad', 'Peso'});
```

El filtrado por condiciones se realiza con indexación lógica, similar a las matrices. Se pueden añadir nuevas columnas con notación de punto o con `addvars` para controlar la posición. Para eliminar columnas se usa `removevars` o se asigna `[]`. Todos estos temas se explican en profundidad en [[Manipulacion basica]].

## Joins y acceso

Cuando los datos están distribuidos en múltiples tablas relacionadas, se pueden combinar mediante operaciones similares a las bases de datos (joins). La función principal es `join` (equivalente a `innerjoin`), que combina filas basándose en variables clave comunes.

Existen variantes según qué filas se desea mantener: `outerjoin` (todas las filas de ambas tablas), `leftjoin` (todas las de la izquierda) y `rightjoin` (todas las de la derecha). Cuando las claves tienen nombres diferentes en cada tabla, se usan los parámetros `LeftKeys` y `RightKeys`.

```matlab
% Unión externa (pacientes sin cita también aparecen)
T_outer = outerjoin(pacientes, citas, 'Keys', 'ID');
```

El acceso a los datos dentro de una tabla es flexible. La forma más común es la notación de punto: `T.NombreColumna` devuelve el contenido de esa columna. Para extraer subconjuntos se puede usar indexación por posición o por nombres de columna. Las propiedades de la tabla (accesibles mediante `T.Properties`) permiten modificar metadatos como nombres de columnas (`VariableNames`), nombres de filas (`RowNames`), unidades (`VariableUnits`) o descripciones (`VariableDescriptions`). Para renombrar columnas existe la función `renamevars`, y para reordenar se usa `movevars`. Para más detalles, ver [[Joins y acceso]].
