---
title: Operaciones con matrices
aliases:
  - producto elemento a elemento
  - division matricial
  - potencia matricial
  - operador punto
  - algebra lineal
tags:
  - matlab
  - matrices
  - concepto
  - guia
draft: false
---

# Operaciones con matrices

MATLAB distingue entre dos tipos fundamentales de operaciones: las **elemento a elemento** (con punto) y las **matriciales** (sin punto).

## Operaciones elemento a elemento (`.operador`)

Se aplican a cada elemento individual de la matriz. Requieren que las matrices tengan el mismo tamaño.

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];

% Multiplicación elemento a elemento
C = A .* B;      % [1*5, 2*6; 3*7, 4*8] = [5, 12; 21, 32]

% División elemento a elemento (derecha)
D = A ./ B;      % [1/5, 2/6; 3/7, 4/8]

% División izquierda elemento a elemento
E = A .\ B;      % [5/1, 6/2; 7/3, 8/4]

% Potencia elemento a elemento
F = A .^ 2;      % [1^2, 2^2; 3^2, 4^2] = [1, 4; 9, 16]
G = 2 .^ A;      % [2^1, 2^2; 2^3, 2^4] = [2, 4; 8, 16]
```

## Operaciones matriciales (álgebra lineal)

Siguen las reglas del álgebra lineal.

### Multiplicación matricial (`*`)

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];
C = A * B;       % [1*5+2*7, 1*6+2*8; 3*5+4*7, 3*6+4*8] = [19, 22; 43, 50]

% Producto vector fila × vector columna
v = [1; 2; 3];
w = [4 5 6];
vw = w * v;      % 1×3 × 3×1 = 1×1 (producto punto)
```

### División matricial derecha (`/`)

Resuelve `X * B = A` → `X = A / B` (equivalente a `A * inv(B)`)

```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];
X = A / B;       % X = A * inv(B)
```

### División matricial izquierda (`\`)

Resuelve `A * X = B` → `X = A \ B` (equivalente a `inv(A) * B`)

```matlab
A = [1 2; 3 4];
B = [5; 6];
X = A \ B;       % Resuelve sistema lineal [1 2; 3 4] * X = [5; 6]
```

El operador `\` es más eficiente y numéricamente estable que usar `inv()` para resolver sistemas lineales.

### Potencia matricial (`^`)

```matlab
A = [1 2; 3 4];
A^2 = A * A;     % [7, 10; 15, 22]
A^3 = A * A * A;
```

## Tabla comparativa rápida

| Operación | Elemento a elemento | Matricial |
|-----------|-------------------|-----------|
| Multiplicación | `.*` | `*` |
| División derecha | `./` | `/` |
| División izquierda | `.\` | `\` |
| Potencia | `.^` | `^` |
| Transpuesta | `.'` | `'` (conjugada) |

## Transpuesta

```matlab
A = [1+2i, 3+4i; 5+6i, 7+8i];

% Transpuesta conjugada (matricial)
B = A';           % [1-2i, 5-6i; 3-4i, 7-8i]

% Transpuesta simple (elemento a elemento)
C = A.';          % [1+2i, 5+6i; 3+4i, 7+8i]
```

## Broadcasting (expansión implícita)

MATLAB expande automáticamente dimensiones singleton en operaciones elemento a elemento (desde R2016b):

```matlab
% Vector columna + vector fila
v = [1; 2; 3];    % 3×1
w = [4 5 6];      % 1×3
M = v + w;        % 3×3: [1+4,1+5,1+6; 2+4,2+5,2+6; 3+4,3+5,3+6]

% Escalar + matriz
A = [1 2; 3 4];
B = A + 10;       % [11,12; 13,14]
```

Para más detalles sobre cómo se construyen las matrices, ver [[Creacion|Creacion]].

