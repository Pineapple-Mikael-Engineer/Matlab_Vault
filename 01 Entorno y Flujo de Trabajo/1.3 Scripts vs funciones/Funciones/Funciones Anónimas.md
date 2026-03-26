---
title: "Funciones anónimas"
tags:
  - MATLAB
  - funciones
  - anonimas
  - function-handle
  - inline
aliases:
  - "Función anónima"
  - "Anonymous function"
  - "Function handle"
---

# Funciones anónimas

Una **función anónima** es una función definida en una sola línea, sin necesidad de crear un archivo `.m` separado. Se almacena en una variable llamada *function handle* y puede ser pasada como argumento a otras funciones.

---

## Sintaxis básica

```matlab
nombre = @(argumentos) expresión;
```

| Elemento | Descripción |
|----------|-------------|
| `nombre` | Variable que almacena el handle de la función |
| `@` | Operador que crea el function handle |
| `(argumentos)` | Lista de parámetros de entrada (pueden ser uno o varios) |
| `expresión` | Código que se evalúa, usualmente una sola línea |

---

## Ejemplos básicos

### Una variable, una operación

```matlab
>> cuadrado = @(x) x^2;
>> cuadrado(5)
ans = 25
```

### Múltiples variables

```matlab
>> suma = @(a, b) a + b;
>> suma(3, 4)
ans = 7
```

### Expresiones más complejas

```matlab
>> hipotenusa = @(a, b) sqrt(a^2 + b^2);
>> hipotenusa(3, 4)
ans = 5
```

### Sin argumentos

```matlab
>> pi_constante = @() 3.1416;
>> pi_constante()
ans = 3.1416
```

---

## Usos principales

### 1. Pasarlas como argumento a otras funciones

Esta es la utilidad más poderosa de las funciones anónimas. Muchas funciones de MATLAB aceptan handles de función como entrada.

#### Integración numérica

```matlab
>> f = @(x) x.^2;
>> integral(f, 0, 1)
ans = 0.3333
```

#### Optimización

```matlab
>> f = @(x) (x - 3).^2;
>> fminbnd(f, 0, 5)
ans = 3.0000
```

#### Resolución de ecuaciones

```matlab
>> f = @(x) x.^3 - 2*x - 5;
>> fzero(f, 2)
ans = 2.0946
```

#### Graficado rápido

```matlab
>> f = @(x) sin(x) + cos(2*x);
>> fplot(f, [0, 2*pi])
>> grid on
```

### 2. Transformación de datos

```matlab
>> datos = [1, 2, 3, 4, 5];
>> cuadrado = @(x) x.^2;
>> cuadrados = arrayfun(cuadrado, datos)
cuadrados = [1, 4, 9, 16, 25]
```

### 3. Funciones de orden superior

```matlab
% Crear una función que devuelve otra función
>> multiplicador = @(n) @(x) n * x;
>> doble = multiplicador(2);
>> triple = multiplicador(3);
>> doble(5)
ans = 10
>> triple(5)
ans = 15
```

---

## Funciones anónimas vs funciones en archivo

| Aspecto | Función anónima | [[Funciones en Archivo]] |
|---------|-----------------|--------------------------|
| Archivo | No requiere archivo | Archivo `.m` propio |
| Reutilización | Limitada (en variable) | Global (por nombre) |
| Complejidad | Una expresión | Múltiples líneas |
| Debugging | Difícil | Fácil (breakpoints) |
| Uso típico | Callbacks, transformaciones simples | Lógica compleja y reusable |

> [!tip] Regla práctica
> Si la lógica cabe en una línea y se usa en un contexto específico (callback, integración, etc.), usar función anónima. Si es más compleja o se reutiliza en múltiples lugares, crear un archivo `.m`.

---

## Limitaciones

### Solo una expresión

Las funciones anónimas no pueden contener múltiples sentencias. Solo una expresión (aunque puede ser compleja).

```matlab
% ❌ No se puede
f = @(x) x = x + 1; y = x^2;

% ✔️ Se puede con funciones auxiliares
f = @(x) x^2 + sin(x);
```

### Sin variables condicionales directas

No se pueden usar `if`, `switch`, bucles `for` o `while`.

```matlab
% ❌ No se puede
f = @(x) if x > 0; x^2; else; 0; end;

% ✔️ Alternativa con operadores lógicos
f = @(x) (x > 0) .* x^2;
```

### Variables externas (captura)

Las funciones anónimas pueden usar variables del workspace en el momento de su creación.

```matlab
>> a = 5;
>> f = @(x) x + a;
>> f(10)
ans = 15

>> a = 10;   % Cambiar a después no afecta a f
>> f(10)
ans = 15    % Sigue usando a = 5
```

> [!info]
> La función anónima captura el valor de las variables externas en el momento de su creación, no cuando se ejecuta.

---

## Funciones anónimas con vectores

Trabajar con vectores requiere atención al operador de punto.

```matlab
% Elemento a elemento (recomendado)
>> f = @(x) x.^2 + sin(x);
>> f(0:0.1:pi)   % Funciona

% Operación matricial
>> g = @(x) x^2 + sin(x);
>> g(0:0.1:pi)   % Error: dimensiones inconsistentes
```

> [!tip]
> Usar operadores con punto (`.*`, `./`, `.^`) dentro de funciones anónimas que recibirán arreglos.

---

## Almacenamiento en estructuras y celdas

Las funciones anónimas pueden almacenarse en arreglos, estructuras o celdas.

```matlab
% Arreglo de celdas
operaciones = {@(x) x^2, @(x) sin(x), @(x) exp(x)};
resultados = cellfun(@(f) f(2), operaciones)

% Estructura
matematicas.suma = @(a,b) a + b;
matematicas.resta = @(a,b) a - b;
matematicas.suma(5,3)
```

---

## Callbacks en interfaces gráficas

Las funciones anónimas son comunes como callbacks en App Designer o GUIDE.

```matlab
% Botón que ejecuta código al presionarse
boton.Callback = @(src, event) disp('Botón presionado');
```

---

## Ejemplos prácticos

### 1. Filtro personalizado para arrayfun

```matlab
datos = 1:10;
es_par = @(x) mod(x, 2) == 0;
pares = datos(arrayfun(es_par, datos))
```

### 2. Función paramétrica

```matlab
crear_onda = @(A, f) @(t) A * sin(2 * pi * f * t);
senal = crear_onda(2, 5);
t = 0:0.01:1;
plot(t, senal(t))
```

### 3. Validación rápida

```matlab
validar_rango = @(x, min_val, max_val) (x >= min_val) && (x <= max_val);
validar_rango(5, 0, 10)   % true
validar_rango(15, 0, 10)  % false
```

### 4. Transformación de datos con cellfun

```matlab
nombres = {'juan', 'maria', 'carlos'};
mayusculas = cellfun(@(s) upper(s), nombres, 'UniformOutput', false)
```

---

## Depuración

Depurar funciones anónimas es más difícil que las funciones en archivo. Estrategias:

- **Dividir**: Separar la expresión en pasos intermedios
- **Convertir**: Si la lógica crece, convertir a función en archivo
- **Probar por partes**: Evaluar subexpresiones en Command Window

```matlab
% Si esto no funciona como se espera
f = @(x) exp(-x.^2) .* sin(10*x);

% Probar por partes en Command Window
x = 0:0.1:2;
parte1 = exp(-x.^2);
parte2 = sin(10*x);
resultado = parte1 .* parte2;
```

---

## Resumen rápido

| Concepto | Descripción |
|----------|-------------|
| **Sintaxis** | `nombre = @(args) expresión` |
| **Almacenamiento** | Function handle (variable) |
| **Uso principal** | Callbacks, integración, optimización, transformaciones |
| **Limitación** | Una sola expresión |
| **Ventaja** | Sin archivos, ideal para funciones simples y temporales |
| **Captura** | Toma variables externas al momento de creación |

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones en Archivo]]
- [[Funciones Locales y Privadas]]
- [[Argumentos variables]]
- [[Scripts]]
