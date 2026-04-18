---
title: Manipulación básica de tablas
aliases:
  - ordenar tablas
  - valores unicos
  - agrupar datos
  - estadisticas por grupo
  - añadir columnas
tags:
  - matlab
  - tablas
  - concepto
  - guia
draft: false
---

# Manipulación básica de tablas

Una vez creada una tabla, las operaciones más comunes son ordenar, encontrar valores únicos, agrupar para calcular estadísticas, y añadir o eliminar columnas.

## Ordenar filas con `sortrows`

### Orden básico

```matlab
% Crear tabla de ejemplo
T = table(...
    ["Ana"; "Juan"; "Luis"; "Ana"; "Juan"], ...
    [25; 30; 28; 22; 35], ...
    [55.5; 70.2; 65.8; 54.0; 72.1], ...
    'VariableNames', {'Nombre', 'Edad', 'Peso'});

% Ordenar por una columna (ascendente por defecto)
T_ordenado = sortrows(T, 'Edad');
%    Nombre    Edad    Peso
%    ______    ____    ____
%    "Ana"     22      54
%    "Ana"     25      55.5
%    "Luis"    28      65.8
%    "Juan"    30      70.2
%    "Juan"    35      72.1

% Orden descendente
T_desc = sortrows(T, 'Edad', 'descend');

% Ordenar por índice de columna
T_ordenado = sortrows(T, 2);  % columna 2 (Edad)
```

### Orden múltiple

```matlab
% Ordenar por Nombre (asc), luego por Edad (desc)
T_multiple = sortrows(T, {'Nombre', 'Edad'}, {'ascend', 'descend'});
%    Nombre    Edad    Peso
%    ______    ____    ____
%    "Ana"     25      55.5
%    "Ana"     22      54
%    "Juan"    35      72.1
%    "Juan"    30      70.2
%    "Luis"    28      65.8
```

### Ordenar por nombre de fila

```matlab
T.Properties.RowNames = {'A', 'B', 'C', 'D', 'E'};
T_ordenado = sortrows(T, 'RowNames');
```

## Valores únicos con `unique`

### Encontrar valores únicos en una columna

```matlab
% Valores únicos en columna Nombre
nombres_unicos = unique(T.Nombre)      % ["Ana", "Juan", "Luis"]

% Con conteo de ocurrencias
[nombres_unicos, ~, idx] = unique(T.Nombre);
conteo = accumarray(idx, 1);           % [2, 2, 1]

% Usando groupsummary (más directo)
conteo = groupsummary(T, 'Nombre', @numel);
```

### Filas únicas en toda la tabla

```matlab
% Eliminar filas duplicadas
T_sin_duplicados = unique(T);
```

## Agrupar y resumir con `groupsummary`

### Estadísticas básicas por grupo

```matlab
% Calcular media de Edad y Peso por Nombre
resumen = groupsummary(T, 'Nombre', {'mean', 'std'}, {'Edad', 'Peso'});
%    Nombre    GroupCount    mean_Edad    std_Edad    mean_Peso    std_Peso
%    ______    __________    _________    ________    _________    ________
%    "Ana"         2           23.5         2.12        54.75       1.06
%    "Juan"        2           32.5         3.54        71.15       1.34
%    "Luis"        1             28          NaN         65.8        NaN
```

### Múltiples grupos

```matlab
% Añadir categoría
T.Categoria = ["A"; "B"; "A"; "B"; "A"];
resumen = groupsummary(T, {'Categoria', 'Nombre'}, 'mean', 'Edad');
```

### Funciones personalizadas

```matlab
% Rango (max - min) por grupo
rango_edad = groupsummary(T, 'Nombre', @(x) max(x) - min(x), 'Edad');

% Percentiles
p75_peso = groupsummary(T, 'Nombre', @(x) prctile(x, 75), 'Peso');
```

## Añadir y eliminar columnas

### `addvars` — añadir al final o posición específica

```matlab
% Añadir columna al final
IMC = T.Peso ./ ((T.Edad/100).^2);  % inventado para ejemplo
T = addvars(T, IMC, 'NewVariableNames', 'IMC');

% Añadir al principio
T = addvars(T, (1:height(T))', 'Before', 1, 'NewVariableNames', 'ID');

% Añadir después de una columna específica
T = addvars(T, T.Peso * 0.45, 'After', 'Peso', 'NewVariableNames', 'Peso_lb');
```

### Notación directa (más simple)

```matlab
% Añadir columna directamente
T.IMC = T.Peso ./ ((T.Edad/100).^2);
T.ID = (1:height(T))';

% Eliminar columna
T.IMC = [];           % elimina la columna IMC
```

### `removevars` — eliminar múltiples columnas

```matlab
T = removevars(T, {'IMC', 'Peso_lb', 'ID'});
T = removevars(T, 2:3);  % eliminar columnas 2 y 3
```

## Selección de filas y columnas

### Selección por condición (indexación lógica)

```matlab
% Filtrar filas
T_jovenes = T(T.Edad < 30, :);
T_ana = T(T.Nombre == "Ana", :);

% Múltiples condiciones
T_filtro = T(T.Edad >= 25 & T.Edad <= 30, :);
T_filtro2 = T(T.Nombre == "Ana" | T.Nombre == "Juan", :);
```

### Selección por nombre de columna

```matlab
% Una columna (como vector)
edades = T.Edad;

% Múltiples columnas (como tabla)
T_subset = T(:, {'Nombre', 'Edad'});

% Rangos de columnas por posición
T_subset2 = T(:, 2:3);
```

## Funciones estadísticas básicas

```matlab
% Por columna (ignorando NaN por defecto)
mean(T.Edad)      % media
std(T.Peso)       % desviación estándar
min(T.Edad)       % mínimo
max(T.Edad)       % máximo
median(T.Edad)    % mediana

% Resumen completo
summary(T)

% Correlación entre columnas numéricas
corr(T.Edad, T.Peso)
```

## Transformar y crear columnas derivadas

```matlab
% Operaciones elemento a elemento
T.Edad_dias = T.Edad * 365;
T.Peso_kg = T.Peso;

% Condicionales
T.Categoria_edad = strings(height(T), 1);
T.Categoria_edad(T.Edad < 28) = "Joven";
T.Categoria_edad(T.Edad >= 28) = "Adulto";

% Usando varfun para aplicar función a múltiples columnas
medias = varfun(@mean, T(:, {'Edad', 'Peso'}));
```

## Ejemplo completo

```matlab
% Tabla inicial
T = table(...
    ["Ana"; "Juan"; "Luis"; "Ana"; "Juan"], ...
    [25; 30; 28; 22; 35], ...
    [55.5; 70.2; 65.8; 54.0; 72.1], ...
    'VariableNames', {'Nombre', 'Edad', 'Peso'});

% 1. Ordenar por Edad descendente
T = sortrows(T, 'Edad', 'descend');

% 2. Añadir columna IMC
T.IMC = T.Peso / 1.75^2;  % altura supuesta 1.75m

% 3. Agrupar y calcular media por nombre
resumen = groupsummary(T, 'Nombre', 'mean', {'Edad', 'Peso', 'IMC'});

% 4. Filtrar mayores de 30
T_mayores = T(T.Edad > 30, :);

% 5. Eliminar columna IMC
T.IMC = [];

% Resultados
disp('Tabla original:');
disp(T);
disp('Resumen por nombre:');
disp(resumen);
```

Para combinar múltiples tablas, ver [[Joins y acceso|Joins y acceso]]. Para creación de tablas, ver [[Creacion y lectura|Creacion y lectura]].