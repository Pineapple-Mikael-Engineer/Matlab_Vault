---
title: "Argumentos variables (varargin, varargout)"
tags:
  - MATLAB
  - funciones
  - argumentos
  - varargin
  - varargout
---

# Argumentos variables (varargin, varargout)

MATLAB permite crear funciones que aceptan un **número variable de argumentos** de entrada o salida. Esto es útil para funciones como `sum`, que puede recibir cualquier cantidad de números, o `plot`, que acepta combinaciones flexibles de argumentos.

---

## Conceptos fundamentales

| Herramienta | Propósito |
|-------------|-----------|
| `nargin` | Número de argumentos de entrada realmente proporcionados |
| `nargout` | Número de argumentos de salida realmente solicitados |
| `varargin` | Celda que captura todos los argumentos de entrada adicionales |
| `varargout` | Captura argumentos de salida adicionales |

---

## `nargin` — número de argumentos de entrada

`nargin` devuelve la cantidad de argumentos con los que se llamó a la función. Permite definir valores por defecto y comportamientos condicionales.

### Ejemplo básico

```matlab
function y = potencia(x, exponente)
    if nargin < 2
        exponente = 2;   % Valor por defecto
    end
    y = x ^ exponente;
end
```

Uso:
```matlab
>> potencia(5)      % nargin = 1 → exponente = 2
ans = 25

>> potencia(5, 3)   % nargin = 2 → exponente = 3
ans = 125
```

### Ejemplo con múltiples valores por defecto

```matlab
function [area, perimetro] = circulo(radio, unidad)
    if nargin < 2
        unidad = 'cm';
    end
    if nargin < 1
        error('Se requiere al menos el radio');
    end
    
    area = pi * radio^2;
    perimetro = 2 * pi * radio;
    
    fprintf('Radio: %.2f %s\n', radio, unidad);
    fprintf('Área: %.2f %s^2\n', area, unidad);
end
```

---

## `nargout` — número de argumentos de salida

`nargout` permite que la función se comporte de manera diferente según cuántas salidas solicite el usuario.

### Ejemplo

```matlab
function [media, mediana, desviacion] = estadisticas(datos)
    media = mean(datos);
    
    if nargout > 1
        mediana = median(datos);
    end
    
    if nargout > 2
        desviacion = std(datos);
    end
end
```

Uso:
```matlab
>> m = estadisticas([1, 2, 3, 4, 10])
m = 4

>> [m, med] = estadisticas([1, 2, 3, 4, 10])
m = 4
med = 3

>> [m, med, d] = estadisticas([1, 2, 3, 4, 10])
m = 4
med = 3
d = 3.5355
```

> [!tip] Optimización
> Calcular solo lo necesario mejora el rendimiento, especialmente con operaciones costosas.

---

## `varargin` — capturar argumentos variables de entrada

`varargin` es un **arreglo de celdas** que contiene todos los argumentos de entrada adicionales después de los argumentos obligatorios. Debe ser el último argumento en la definición de la función.

### Sintaxis

```matlab
function nombre_funcion(arg1, arg2, varargin)
    % varargin{1} contiene el primer argumento adicional
    % varargin{2} contiene el segundo, etc.
end
```

### Ejemplo básico

```matlab
function sumar_todo(varargin)
    total = sum([varargin{:}]);
    fprintf('Suma total: %.2f\n', total);
end
```

Uso:
```matlab
>> sumar_todo(1, 2, 3, 4, 5)
Suma total: 15.00

>> sumar_todo(10, 20)
Suma total: 30.00
```

### Ejemplo con argumentos obligatorios y opcionales

```matlab
function graficar_funcion(f, limites, varargin)
% graficar_funcion(f, limites, opciones...)
%   f: función a graficar
%   limites: vector [xmin, xmax]
%   varargin: opciones de estilo (color, línea, etc.)
    
    x = linspace(limites(1), limites(2), 100);
    y = f(x);
    
    if isempty(varargin)
        plot(x, y)
    else
        plot(x, y, varargin{:})
    end
    
    grid on
    xlabel('x')
    ylabel('f(x)')
end
```

Uso:
```matlab
>> f = @(x) sin(x);
>> graficar_funcion(f, [0, 2*pi])                    % Sin opciones
>> graficar_funcion(f, [0, 2*pi], 'r--', 'LineWidth', 2)  % Con opciones
```

### Ejemplo con pares nombre-valor

```matlab
function crear_figura(titulo, varargin)
% crear_figura(titulo, 'Color', 'red', 'Tamaño', 10, ...)
    
    figure
    
    % Procesar pares nombre-valor
    color = 'blue';      % valor por defecto
    tamanio = 5;         % valor por defecto
    
    for i = 1:2:length(varargin)
        switch lower(varargin{i})
            case 'color'
                color = varargin{i+1};
            case 'tamaño'
                tamanio = varargin{i+1};
        end
    end
    
    plot(1:10, rand(1,10), 'o', 'Color', color, 'MarkerSize', tamanio)
    title(titulo)
end
```

Uso:
```matlab
>> crear_figura('Mi gráfica', 'Color', 'red', 'Tamaño', 12)
```

---

## `varargout` — capturar argumentos variables de salida

`varargout` es un **arreglo de celdas** que permite devolver un número variable de resultados. Debe ser el último argumento de salida en la definición de la función.

### Ejemplo básico

```matlab
function varargout = multiples_salidas(x)
    for i = 1:nargout
        varargout{i} = x ^ i;
    end
end
```

Uso:
```matlab
>> a = multiples_salidas(5)
a = 5

>> [a, b] = multiples_salidas(5)
a = 5
b = 25

>> [a, b, c] = multiples_salidas(5)
a = 5
b = 25
c = 125
```

### Ejemplo: función que devuelve estadísticas flexibles

```matlab
function varargout = estadisticas_flexibles(datos)
% estadisticas_flexibles(datos) devuelve hasta 3 estadísticas
%   [media] = estadisticas_flexibles(datos)
%   [media, mediana] = estadisticas_flexibles(datos)
%   [media, mediana, desviacion] = estadisticas_flexibles(datos)
    
    varargout{1} = mean(datos);
    
    if nargout > 1
        varargout{2} = median(datos);
    end
    
    if nargout > 2
        varargout{3} = std(datos);
    end
end
```

---

## Combinación de `varargin` y `varargout`

Una función puede tener ambos: entrada variable y salida variable.

### Ejemplo: función de agregación flexible

```matlab
function varargout = agregar(operacion, varargin)
% agregar(operacion, valores...)
%   operacion: 'suma', 'producto', 'maximo', 'minimo'
%   valores: números a procesar
    
    valores = [varargin{:}];
    
    switch lower(operacion)
        case 'suma'
            resultado = sum(valores);
        case 'producto'
            resultado = prod(valores);
        case 'maximo'
            resultado = max(valores);
        case 'minimo'
            resultado = min(valores);
        otherwise
            error('Operación no reconocida: %s', operacion);
    end
    
    if nargout > 0
        varargout{1} = resultado;
    else
        fprintf('Resultado: %.2f\n', resultado);
    end
end
```

Uso:
```matlab
>> agregar('suma', 1, 2, 3, 4, 5)
Resultado: 15.00

>> total = agregar('producto', 2, 3, 4)
total = 24

>> maximo = agregar('maximo', 10, 5, 20, 8)
maximo = 20
```

---

## Validación con argumentos variables

### Combinación con bloques `arguments`

A partir de R2019b, se puede usar el bloque `arguments` con `varargin`.

```matlab
function graficar(f, limites, opciones)
    arguments
        f function_handle
        limites (1,2) double
        opciones.Color = 'blue'
        opciones.LineWidth = 1
        opciones.Titulo = ''
    end
    
    x = linspace(limites(1), limites(2), 100);
    y = f(x);
    plot(x, y, 'Color', opciones.Color, 'LineWidth', opciones.LineWidth)
    title(opciones.Titulo)
    grid on
end
```

> [!info]
> El bloque `arguments` es más moderno y recomendado para funciones nuevas. Ver [[Validación de argumentos]].

---

## Patrones comunes

### 1. Valores por defecto con `nargin`

```matlab
function configurar(param1, param2, param3)
    if nargin < 1, param1 = 10; end
    if nargin < 2, param2 = 'default'; end
    if nargin < 3, param3 = false; end
    % Cuerpo de la función
end
```

### 2. Procesamiento de pares nombre-valor

```matlab
function mi_funcion(varargin)
    % Configurar valores por defecto
    opciones.color = 'red';
    opciones.tamanio = 5;
    opciones.transparencia = 0.5;
    
    % Procesar argumentos
    for i = 1:2:length(varargin)
        if isfield(opciones, varargin{i})
            opciones.(varargin{i}) = varargin{i+1};
        end
    end
    
    % Usar opciones
    disp(opciones)
end
```

### 3. Funciones con comportamiento según número de salidas

```matlab
function varargout = dividir(a, b)
    cociente = a / b;
    residuo = rem(a, b);
    
    varargout{1} = cociente;
    
    if nargout > 1
        varargout{2} = residuo;
    end
end
```

---

## Limitaciones y consideraciones

| Consideración | Descripción |
|---------------|-------------|
| **`varargin` debe ser último** | Siempre como último argumento de entrada |
| **`varargout` debe ser último** | Siempre como último argumento de salida |
| **Rendimiento** | Acceder a `varargin{:}` tiene costo; para muchas llamadas, considerar otras alternativas |
| **Validación** | Los argumentos en `varargin` no se validan automáticamente |
| **Documentación** | Explicar claramente los argumentos variables en la ayuda de la función |

---

## Ejemplo completo: función flexible de trazado

```matlab
function varargout = graficar_flexible(x, y, varargin)
% GRAFICAR_FLEXIBLE Gráfica con opciones flexibles
%   graficar_flexible(x, y)
%   graficar_flexible(x, y, 'color', 'red', 'linewidth', 2)
%   graficar_flexible(x, y, 'type', 'scatter')
%   [h, stats] = graficar_flexible(...)
%
%   Opciones disponibles:
%       'color'      - Color de la línea/marcador
%       'linewidth'  - Ancho de línea
%       'type'       - 'line', 'scatter', 'both'
%       'title'      - Título de la gráfica
%       'xlabel'     - Etiqueta del eje X
%       'ylabel'     - Etiqueta del eje Y

    % Valores por defecto
    color = 'blue';
    linewidth = 1;
    tipo = 'line';
    titulo = '';
    etiqueta_x = '';
    etiqueta_y = '';
    
    % Procesar argumentos variables
    for i = 1:2:length(varargin)
        switch lower(varargin{i})
            case 'color'
                color = varargin{i+1};
            case 'linewidth'
                linewidth = varargin{i+1};
            case 'type'
                tipo = varargin{i+1};
            case 'title'
                titulo = varargin{i+1};
            case 'xlabel'
                etiqueta_x = varargin{i+1};
            case 'ylabel'
                etiqueta_y = varargin{i+1};
        end
    end
    
    % Crear gráfica según tipo
    figure
    switch tipo
        case 'line'
            h = plot(x, y, 'Color', color, 'LineWidth', linewidth);
        case 'scatter'
            h = scatter(x, y, 50, color, 'filled');
        case 'both'
            plot(x, y, 'Color', color, 'LineWidth', linewidth)
            hold on
            h = scatter(x, y, 30, color, 'filled');
            hold off
    end
    
    % Configurar etiquetas y título
    if ~isempty(etiqueta_x), xlabel(etiqueta_x); end
    if ~isempty(etiqueta_y), ylabel(etiqueta_y); end
    if ~isempty(titulo), title(titulo); end
    
    grid on
    
    % Devolver salidas según lo solicitado
    if nargout > 0
        varargout{1} = h;
    end
    
    if nargout > 1
        stats.r2 = 1 - var(y - polyval(polyfit(x, y, 1), x)) / var(y);
        varargout{2} = stats;
    end
end
```

---

## Resumen rápido

| Herramienta | Propósito | Ejemplo de uso |
|-------------|-----------|----------------|
| `nargin` | Conocer cuántos inputs se dieron | `if nargin < 2, arg2 = default; end` |
| `nargout` | Conocer cuántos outputs se pidieron | `if nargout > 1, calcular_extra; end` |
| `varargin` | Capturar inputs adicionales | `sum([varargin{:}])` |
| `varargout` | Devolver outputs adicionales | `varargout{1} = resultado` |

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones en Archivo]]
- [[Funciones Anónimas]]
- [[Funciones Locales y Privadas]]
- [[Validación de argumentos]]
- [[Funciones Anidadas]]
