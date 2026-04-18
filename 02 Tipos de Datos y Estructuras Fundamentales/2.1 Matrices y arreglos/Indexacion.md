---
title: Indexación de matrices
aliases:
  - acceso a elementos
  - extraer submatrices
  - indexado logico
  - indices
tags:
  - matlab
  - matrices
  - concepto
  - guia
draft: false
---

# Indexación de matrices

La indexación es el mecanismo para acceder, extraer o modificar elementos específicos dentro de un arreglo.

## Indexación por subíndices

Forma más común: `(fila, columna)`

```matlab
A = [1 2 3; 4 5 6; 7 8 9];
A(2, 3)        % Devuelve 6 (fila 2, columna 3)
A(1, :)        % Devuelve [1 2 3] (fila 1, todas las columnas)
A(:, 2)        % Devuelve [2; 5; 8] (columna 2, todas las filas)
A(2:3, 1:2)    % Devuelve [4 5; 7 8] (submatriz)
```

## El operador `end`

Representa el último índice de una dimensión:

```matlab
A(end, :)      % Última fila: [7 8 9]
A(:, end)      % Última columna: [3; 6; 9]
A(2:end-1, :)  % Todas excepto primera y última fila
A(end, end)    % Esquina inferior derecha: 9
```

## Indexación lineal

MATLAB almacena matrices por columnas (orden Fortran). Se puede indexar con un solo índice:

```matlab
A = [1 2 3; 4 5 6];
% Almacenamiento lineal: [1, 4, 2, 5, 3, 6]
A(3)           % Devuelve 2 (tercer elemento lineal)
A([1 3 5])     % Devuelve [1, 2, 3]
A(4:6)         % Devuelve [5, 3, 6]
```

Para convertir entre subíndices e índice lineal se usan las funciones [[Funciones de conversion indice|ind2sub]] y [[Funciones de conversion indice|sub2ind]].

## Indexación lógica (booleanos)

Usa máscaras del mismo tamaño que la matriz:

```matlab
A = [1 2 3; 4 5 6];
mascara = A > 3;    % [false false false; true true true]
A(mascara)          % Devuelve [4; 5; 6]
A(A > 3)            % Directo: [4; 5; 6]

% Modificar elementos que cumplen condición
A(A < 3) = 0        % [0 0 3; 4 5 6]
```

La indexación lógica es muy útil junto a [[Tipos numéricos y lógicos|true y false]].

## Indexación con vectores de índices

Se pueden usar vectores para seleccionar filas y columnas específicas:

```matlab
A = magic(5);
A([1 3 5], :)      % Filas 1, 3 y 5
A(:, [2 4])        % Columnas 2 y 4
A([2 4], [1 3 5])  % Submatriz 2×3

% Con repetición
A([1 1 2], :)      % Fila 1 repetida dos veces
```

## Asignación mediante indexación

Se puede modificar partes específicas de una matriz:

```matlab
A = zeros(3);
A(1, :) = [1 2 3];     % Asignar a primera fila
A(:, 2) = 5;           % Columna 2 completa a 5
A(2:3, 2:3) = [6 7; 8 9];

% Expandir matriz (crece automáticamente)
A(4, 4) = 10;          % Las posiciones no asignadas son cero
```

## Eliminación de filas o columnas

Asignando `[]` se elimina:

```matlab
A = [1 2 3; 4 5 6; 7 8 9];
A(2, :) = [];          % Elimina fila 2 → matriz 2×3
A(:, 1) = [];          % Elimina columna 1 → matriz 2×2
```

## Funciones útiles para indexación

| Función | Descripción |
|---------|-------------|
| `find` | Devuelve índices lineales de elementos que cumplen condición |
| `ind2sub` | Convierte índice lineal a subíndices |
| `sub2ind` | Convierte subíndices a índice lineal |
| `ismember` | Prueba si elementos están en un conjunto |

```matlab
A = [4 5 6; 7 8 9];
indices = find(A > 5);     % [2; 3; 4; 5; 6] (lineales)
[f, c] = ind2sub(size(A), indices);  % Filas y columnas correspondientes
```

El uso combinado de [[Creacion|Creacion]] e indexación permite construir y manipular arreglos de forma eficiente.

