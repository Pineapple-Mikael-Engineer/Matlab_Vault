---
title: "Creación de matrices y arreglos — zeros, ones, eye, rand, linspace, logspace, repmat, reshape, corchetes, operador :"
aliases: ["inicializar matrices", "generar vectores", "matriz identidad", "espaciado lineal", "repeticion de matrices", "cambiar dimensiones"]
tags: [matlab, matrices, concepto, guia]
draft: true
---

# Creación de matrices y arreglos

## Creación básica con corchetes `[]`

Los corchetes son el operador fundamental para crear arreglos manualmente:

```matlab
% Vector fila (espacios o comas)
fila = [1 2 3 4]
fila = [1, 2, 3, 4]

% Vector columna (punto y coma)
columna = [1; 2; 3; 4]

% Matriz 2x3
matriz = [1 2 3; 4 5 6]
```

## Operador `:` (dos puntos)

Genera secuencias con paso fijo:

```matlab
% inicio:paso:fin
1:2:10        % [1 3 5 7 9]
1:10          % paso implícito = 1 → [1 2 3 4 5 6 7 8 9 10]
10:-2:1       % [10 8 6 4 2]
```

## Funciones para matrices predefinidas

| Función | Descripción | Ejemplo |
|---------|-------------|---------|
| `zeros(m,n)` | Matriz de ceros | `zeros(3,4)` → 3×4 de ceros |
| `ones(m,n)` | Matriz de unos | `ones(2,3)` → 2×3 de unos |
| `eye(n)` | Matriz identidad | `eye(4)` → 4×4 con diagonal de 1 |
| `rand(m,n)` | Números aleatorios uniformes [0,1] | `rand(2,2)` |
| `randn(m,n)` | Distribución normal (μ=0, σ=1) | `randn(100,1)` |
| `diag(v)` | Matriz diagonal con vector v | `diag([1 2 3])` |

## Matrices para casos específicos

| Función | Descripción | Ejemplo |
|---------|-------------|---------|
| `true(m,n)` | Matriz lógica de `true` | `true(2,3)` → máscara booleana |
| `false(m,n)` | Matriz lógica de `false` | `false(2,2)` → inicialización lógica |
| `NaN(m,n)` | Matriz de `NaN` (Not a Number) | `NaN(3,3)` → datos faltantes |
| `Inf(m,n)` | Matriz de `Inf` (infinito) | `Inf(1,5)` → inicialización para máximos |
| `magic(n)` | Cuadrado mágico n×n | `magic(3)` → [8 1 6; 3 5 7; 4 9 2] |
| `hilb(n)` | Matriz de Hilbert (mal condicionada) | `hilb(4)` → pruebas numéricas |
| `pascal(n)` | Matriz de Pascal | `pascal(3)` → combinatoria |

## Vectores con espaciado controlado

### `linspace(inicio, fin, n)`
Crea vector con **n** puntos **linealmente** espaciados entre inicio y fin:

```matlab
linspace(0, 10, 5)   % [0, 2.5, 5, 7.5, 10]
linspace(0, 1, 100)  % 100 puntos entre 0 y 1
```

### `logspace(inicio, fin, n)`
Crea vector con **n** puntos **logarítmicamente** espaciados entre $10^{inicio}$ y $10^{fin}$:

```matlab
logspace(0, 2, 4)    % [1, 4.64, 21.54, 100] (10^0 a 10^2)
logspace(-2, 2, 5)   % [0.01, 0.1, 1, 10, 100]
```

## Repetición de elementos y matrices

### `repmat` — repetir matriz en bloque
```matlab
repmat([1 2; 3 4], 2, 3)   % 2 filas × 3 columnas de bloques
% Resultado: [1 2 1 2 1 2; 3 4 3 4 3 4; 1 2 1 2 1 2; 3 4 3 4 3 4]
```

### `repelem` — repetir cada elemento individualmente
```matlab
repelem([1 2 3], 2)        % [1 1 2 2 3 3]
repelem([1 2; 3 4], 2, 3)  % cada elemento repetido 2×3 veces
```

### `blkdiag` — matriz diagonal por bloques
```matlab
A = [1 2; 3 4];
B = [5 6; 7 8];
blkdiag(A, B)  % [1 2 0 0; 3 4 0 0; 0 0 5 6; 0 0 7 8]
```

## Cambio de dimensiones

### `reshape` — reorganizar sin cambiar datos
```matlab
A = 1:12;                    % 1×12
B = reshape(A, 3, 4);        % 3×4 (rellena por columnas)
C = reshape(A, 2, 2, 3);     % 2×2×3 (arreglo 3D)
```

### `permute` — reordenar dimensiones (transposición generalizada)
```matlab
A = rand(3, 4, 5);
B = permute(A, [2, 1, 3]);   % dimensiones 3×4×5 → 4×3×5
C = permute(A, [3, 2, 1]);   % 5×4×3
```

## Mallas para funciones 2D/3D

### `meshgrid` — fundamental para gráficos 3D
```matlab
x = -2:0.2:2;
y = -2:0.2:2;
[X, Y] = meshgrid(x, y);     % X, Y son matrices 21×21
Z = X.^2 + Y.^2;             % evaluar función en toda la malla
surf(X, Y, Z);
```

### `ndgrid` — versión para N dimensiones
```matlab
[X, Y] = ndgrid(1:5, 1:5);   % orden diferente a meshgrid
[X, Y, Z] = ndgrid(1:10, 1:10, 1:10);  % malla 3D
```

## Creación a partir de otras matrices

Se puede crear una matriz extrayendo elementos de una matriz existente mediante [[Indexacion|Indexacion]].

```matlab
A = rand(5);
B = A(1:3, 2:4);     % Submatriz
C = A([1 3 5], :);   % Filas específicas
D = A(:, end:-1:1);  % Invertir columnas
```

## Matriz vacía y redimensionamiento dinámico

```matlab
% Matriz vacía (útil para inicializar)
vacia = [];           % 0×0
fila_vacia = ones(0,5);   % 0×5 (útil para concatenaciones)

% Crecimiento dinámico (funciona pero es ineficiente)
x = [];
for i = 1:1000
    x = [x, i];   % En cada iteración se reasigna memoria
end
% Mejor preasignar: x = zeros(1, 1000);
```

## Matrices dispersas (para datos con muchos ceros)

```matlab
% Crear matriz dispersa (ahorra memoria)
sparse_A = speye(1000);               % Identidad dispersa
sparse_B = sprand(1000, 1000, 0.05);  % 5% de densidad
full_A = full(sparse_A);              % Convertir a densa

% Uso típico: sistemas lineales grandes con matriz diagonal o banda
```

## Vectores específicos (combinaciones y permutaciones)

```matlab
perms([1 2 3])         % Todas las permutaciones (3! = 6 filas)
combinations(1:3, 1:3) % Producto cartesiano (9 combinaciones)
```

Para operaciones entre matrices, ver [[Operaciones|Operaciones]].

## Notas relacionadas

- [[Indexacion]]
- [[Operaciones]]
- [[Tipos numéricos y lógicos]]
- [[Matrices dispersas]]

