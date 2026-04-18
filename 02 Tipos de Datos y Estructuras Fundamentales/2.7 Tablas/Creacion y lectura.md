---
title: "Creación y lectura de tablas — table, array2table, struct2table, readtable, writetable"
aliases: ["crear tablas", "importar datos", "exportar tablas", "tablas desde arreglos"]
tags: [matlab, tablas, concepto, guia]
draft: true
---

# Creación y lectura de tablas

Las tablas (`table`) son estructuras de datos diseñadas para almacenar datos tabulares heterogéneos, como los que se encuentran en hojas de cálculo o bases de datos. Cada columna puede tener un tipo diferente (numérico, string, datetime, etc.) y las columnas tienen nombre.

## Creación desde cero

### Con función `table()`

```matlab
% Variables como columnas
nombre = ["Ana"; "Juan"; "Luis"];
edad = [25; 30; 28];
peso = [55.5; 70.2; 65.8];

T = table(nombre, edad, peso)

% Resultado:
%    nombre    edad    peso
%    ______    ____    _____
%    "Ana"     25      55.5
%    "Juan"    30      70.2
%    "Luis"    28      65.8
```

### Especificar nombres de columna

```matlab
T = table(nombre, edad, peso, 'VariableNames', {'Nombre', 'Edad', 'Peso'});
```

### Especificar nombres de fila

```matlab
apellido = {"Garcia"; "Lopez"; "Martinez"};
T = table(edad, peso, 'RowNames', apellido);

%                 Edad    Peso
%                 ____    ____
%    Garcia       25      55.5
%    Lopez        30      70.2
%    Martinez     28      65.8
```

## Conversión desde otros tipos

### `array2table` — desde matriz numérica

```matlab
% Matriz numérica
M = [25 55.5; 30 70.2; 28 65.8];
T = array2table(M, 'VariableNames', {'Edad', 'Peso'});

% Con nombres de fila
T = array2table(M, 'VariableNames', {'Edad', 'Peso'}, 'RowNames', {'Ana','Juan','Luis'});
```

### `struct2table` — desde arreglo de estructuras

```matlab
% Arreglo de estructuras
pacientes(1).nombre = "Ana";
pacientes(1).edad = 25;
pacientes(2).nombre = "Juan";
pacientes(2).edad = 30;

T = struct2table(pacientes);
%    nombre    edad
%    ______    ____
%    "Ana"     25
%    "Juan"    30
```

### `cell2table` — desde cell array

```matlab
% Cell array con datos mezclados
C = {'Ana', 25, 55.5; 'Juan', 30, 70.2; 'Luis', 28, 65.8};
T = cell2table(C, 'VariableNames', {'Nombre', 'Edad', 'Peso'});
```

## Lectura desde archivos

### `readtable` — formato automático

MATLAB detecta automáticamente el formato y tipos de datos.

```matlab
% CSV (comma-separated values)
T = readtable('datos.csv');

% Excel
T = readtable('datos.xlsx');
T = readtable('datos.xlsx', 'Sheet', 'Hoja1');

% Texto delimitado (tab, espacio, etc.)
T = readtable('datos.txt', 'Delimiter', '\t');

% Sin encabezado
T = readtable('datos.csv', 'VariableNamingRule', 'preserve');
```

### Opciones comunes de `readtable`

```matlab
% Especificar rangos (como Excel)
T = readtable('datos.xlsx', 'Range', 'A1:C10');

% Solo ciertas columnas
T = readtable('datos.csv', 'SelectedVariableNames', {'Nombre', 'Edad'});

% Especificar tipos de columna
opts = detectImportOptions('datos.csv');
opts = setvartype(opts, {'Edad', 'Peso'}, 'double');
opts = setvartype(opts, 'Nombre', 'string');
T = readtable('datos.csv', opts);

% Ignorar filas con errores
T = readtable('datos.csv', 'TreatAsMissing', 'NA');
```

### Tipos de archivos soportados

| Extensión | Formato |
|-----------|---------|
| `.csv`, `.txt`, `.dat` | Texto delimitado |
| `.xls`, `.xlsx`, `.xlsm` | Excel |
| `.parquet` | Parquet |
| `.sav` | SPSS |
| `.dta` | Stata |

## Escritura de tablas

### `writetable`

```matlab
% CSV
writetable(T, 'salida.csv');

% Excel
writetable(T, 'salida.xlsx');
writetable(T, 'salida.xlsx', 'Sheet', 'Resultados');

% Sin nombres de fila
writetable(T, 'datos.csv', 'WriteRowNames', false);
```

## Inspección de tablas

```matlab
T = readtable('datos.csv');

% Ver primeras filas
head(T, 5)      % primeras 5
tail(T, 3)      % últimas 3

% Resumen de la tabla
summary(T)      % tipo, min, max, NaN por columna

% Dimensiones
size(T)         % [n_filas, n_columnas]
height(T)       % número de filas
width(T)        % número de variables (columnas)

% Nombres
T.Properties.VariableNames    % nombres de columnas
T.Properties.RowNames          % nombres de filas (si existen)
```

## Propiedades de la tabla

```matlab
% Modificar propiedades
T.Properties.VariableNames = {'Nombre', 'Edad', 'Peso', 'Fecha'};
T.Properties.Description = 'Datos de pacientes';
T.Properties.VariableUnits = {'', 'años', 'kg', ''};
T.Properties.VariableDescriptions = {'Nombre completo', 'Edad en años', 'Peso en kg', 'Fecha de registro'};
```

## Ejemplo completo

```matlab
% Crear tabla manualmente
nombres = ["Ana"; "Juan"; "Luis"; "Maria"];
edades = [25; 30; 28; 35];
pesos = [55.5; 70.2; 65.8; 60.1];
fechas = datetime(2026, 4, [1;2;3;4]);

T = table(nombres, edades, pesos, fechas, ...
    'VariableNames', {'Nombre', 'Edad', 'Peso', 'Fecha'});

% Añadir descripción
T.Properties.Description = 'Datos de pacientes';

% Guardar
writetable(T, 'pacientes.xlsx');

% Leer después
T2 = readtable('pacientes.xlsx');

% Verificar
summary(T2)
```

Para operaciones básicas con tablas, ver [[Manipulacion_basica|Manipulacion basica]]. Para combinar tablas, ver [[Joins y acceso|Joins y acceso]].