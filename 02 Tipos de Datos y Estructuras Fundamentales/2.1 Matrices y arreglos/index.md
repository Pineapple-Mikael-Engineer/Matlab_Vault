---
title: Matrices y arreglos — creación, indexación y operaciones fundamentales en MATLAB
aliases:
  - arrays
  - vectores y matrices
  - arreglos numericos
tags:
  - matlab
  - matrices
  - concepto
  - index
draft: false
---

# Matrices y arreglos

La matriz es la estructura de datos fundamental de MATLAB. Todos los datos numéricos se almacenan y operan como arreglos multidimensionales, desde un escalar (matriz 1×1) hasta vectores y matrices de N dimensiones.

## Creación

Existen múltiples formas de crear matrices: manualmente con corchetes `[]`, mediante funciones predefinidas como `zeros`, `ones`, `eye` o `rand`, o generando secuencias con el operador `:` (dos puntos), `linspace` y `logspace`. Para casos más avanzados se dispone de funciones como `repmat`, `repelem`, `reshape` y `blkdiag`. Para un desarrollo completo de todas estas funciones, ver [[Creacion|Creacion]].

```matlab
% Creación manual
A = [1 2 3; 4 5 6];

% Con funciones
B = zeros(3, 4);
C = ones(2, 2);
```

## Indexación

El acceso a los elementos se realiza con subíndices: `A(2,3)` accede a fila 2, columna 3. MATLAB permite indexación lineal (un solo índice), uso del operador `end` para referirse al último elemento, e indexación lógica mediante máscaras booleanas. La asignación de `[]` elimina filas o columnas completas. Para una explicación detallada, consultar [[Indexacion|Indexacion]].

```matlab
A = [1 2 3; 4 5 6];
A(2, 3)    % 6
A(end, :)  % [4 5 6]
A(A > 3)   % [4 5 6]
```

## Operaciones

MATLAB distingue fundamentalmente entre operaciones elemento a elemento (con punto: `.*`, `./`, `.^`) y operaciones matriciales (`*`, `/`, `\`, `^`). Las primeras actúan sobre cada elemento individual; las segundas siguen las reglas del álgebra lineal. El operador división izquierda `\` es especialmente útil para resolver sistemas lineales `A * x = b`. Todos los detalles están en [[Operaciones|Operaciones]].

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];

% Elemento a elemento
C = A .* B;  % [5 12; 21 32]

% Matricial
D = A * B;   % [19 22; 43 50]
```
