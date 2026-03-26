---
title: "Validación de argumentos (arguments block)"
tags:
  - MATLAB
  - funciones
  - validacion
  - arguments
  - tipos
---

# Validación de argumentos (arguments block)

A partir de MATLAB R2019b, se introdujo el **bloque `arguments`** como una forma declarativa y concisa de validar los argumentos de entrada de una función. Este enfoque reemplaza la validación manual con `nargin`, `validateattributes` y condicionales.

---

## Sintaxis básica

```matlab
function resultado = mi_funcion(entrada1, entrada2)
    arguments
        entrada1 (1,1) double          % Escalar double
        entrada2 (1,:) char            % Vector fila de caracteres
    end
    resultado = entrada1 * length(entrada2);
end
```

### Estructura del bloque

| Elemento | Descripción |
|----------|-------------|
| `arguments` | Palabra clave que inicia el bloque |
| `entrada1 (1,1) double` | Nombre del argumento seguido de restricciones de tamaño y tipo |
| `end` | Cierra el bloque (opcional si es el único bloque) |

---

## Restricciones de tamaño

| Sintaxis | Significado |
|----------|-------------|
| `(1,1)` | Escalar |
| `(1,:)` | Vector fila |
| `(:,1)` | Vector columna |
| `(3,3)` | Matriz 3x3 |
| `(:,:)` | Matriz de cualquier tamaño |
| `(1,:)` o `(5,:)` | Vector fila, opcionalmente con 5 elementos |

### Ejemplos

```matlab
function validar_tamano(a, b, c)
    arguments
        a (1,1)           % Escalar (cualquier tipo)
        b (1,:) double    % Vector fila double
        c (3,3)           % Matriz 3x3 (cualquier tipo)
    end
    % Cuerpo de la función
end
```

---

## Restricciones de tipo

| Tipo | Descripción |
|------|-------------|
| `double` | Números de doble precisión |
| `single` | Precisión simple |
| `int8`, `int16`, `int32`, `int64` | Enteros con signo |
| `uint8`, `uint16`, `uint32`, `uint64` | Enteros sin signo |
| `logical` | Valores booleanos (true/false) |
| `char` | Caracteres individuales |
| `string` | Cadenas de texto |
| `cell` | Arreglos de celdas |
| `struct` | Estructuras |
| `function_handle` | Handles de función |
| `table` | Tablas |
| `datetime` | Fechas y horas |
| `duration` | Duraciones |

### Ejemplos

```matlab
function validar_tipos(a, b, c, d)
    arguments
        a double
        b char
        c string
        d logical
    end
    % Cuerpo de la función
end
```

---

## Combinación de tamaño y tipo

```matlab
function procesar_datos(datos, umbral, etiqueta, opciones)
    arguments
        datos (:,3) double          % Matriz de Nx3 double
        umbral (1,1) {mustBePositive, mustBeNumeric}  % Escalar positivo numérico
        etiqueta (1,:) char         % Vector fila de caracteres
        opciones (1,1) struct       % Escalar estructura
    end
    % Cuerpo de la función
end
```

---

## Funciones de validación (`{...}`)

Las llaves `{...}` permiten usar funciones de validación personalizadas o predefinidas.

### Funciones de validación predefinidas

| Función | Propósito |
|---------|-----------|
| `mustBeFinite` | Valores finitos (no Inf, no NaN) |
| `mustBePositive` | Valores positivos (>0) |
| `mustBeNonnegative` | Valores no negativos (>=0) |
| `mustBeNonzero` | Valores diferentes de cero |
| `mustBeInteger` | Valores enteros |
| `mustBeReal` | Valores reales (no complejos) |
| `mustBeNumeric` | Valores numéricos |
| `mustBeNonempty` | No vacío |
| `mustBeVector` | Vector (fila o columna) |
| `mustBeScalarOrEmpty` | Escalar o vacío |
| `mustBeMember` | Pertenece a un conjunto |
| `mustBeLessThan` | Menor que un valor |
| `mustBeGreaterThan` | Mayor que un valor |
| `mustBeLessThanOrEqual` | Menor o igual |
| `mustBeGreaterThanOrEqual` | Mayor o igual |
| `mustBeFile` | Archivo existente |
| `mustBeFolder` | Carpeta existente |

### Ejemplos con funciones de validación

```matlab
function configurar(iteraciones, tolerancia, modo)
    arguments
        iteraciones (1,1) {mustBeInteger, mustBePositive}
        tolerancia (1,1) {mustBePositive, mustBeLessThanOrEqual(tolerancia, 1)}
        modo (1,:) char {mustBeMember(modo, {'rapido', 'preciso', 'balanceado'})}
    end
    % Cuerpo de la función
end
```

---

## Argumentos opcionales

Para definir un argumento como opcional, se le asigna un valor por defecto.

```matlab
function procesar(datos, metodo, verbose)
    arguments
        datos (:,2) double
        metodo (1,:) char = 'promedio'
        verbose (1,1) logical = false
    end
    % Cuerpo de la función
end
```

Uso:
```matlab
>> procesar(datos)                    % metodo = 'promedio', verbose = false
>> procesar(datos, 'mediana')         % verbose = false
>> procesar(datos, 'mediana', true)   % Todos especificados
```

---

## Argumentos repetidos (`...`)

El operador `...` permite capturar múltiples argumentos del mismo tipo.

```matlab
function sumar_todo(valores, varargin)
    arguments
        valores (1,:) double
        varargin (1,:) double {mustBePositive}
    end
    total = sum([valores, varargin{:}]);
    fprintf('Suma total: %.2f\n', total);
end
```

Uso:
```matlab
>> sumar_todo([1, 2], 3, 4, 5)
Suma total: 15.00
```

---

## Argumentos nombre-valor (name-value)

MATLAB permite definir argumentos en pares `'Nombre', valor`.

### Sintaxis básica

```matlab
function graficar(x, y, options)
    arguments
        x (:,1) double
        y (:,1) double
        options.Color (1,3) double = [0, 0, 1]
        options.LineWidth (1,1) {mustBePositive} = 1
        options.Title (1,:) char = ''
        options.ShowGrid (1,1) logical = true
    end
    
    plot(x, y, 'Color', options.Color, 'LineWidth', options.LineWidth)
    if options.ShowGrid, grid on; end
    if ~isempty(options.Title), title(options.Title); end
end
```

Uso:
```matlab
>> graficar(x, y, 'Color', [1, 0, 0], 'LineWidth', 2, 'Title', 'Mi gráfica')
```

### Argumentos nombre-valor con estructura

```matlab
function analizar(datos, opts)
    arguments
        datos (:,1) double
        opts.tolerancia (1,1) {mustBePositive} = 1e-6
        opts.max_iter (1,1) {mustBeInteger, mustBePositive} = 100
        opts.metodo (1,:) char {mustBeMember(opts.metodo, {'newton', 'gradiente'})} = 'newton'
    end
    % Cuerpo de la función
end
```

Uso:
```matlab
>> analizar(datos, 'tolerancia', 1e-8, 'max_iter', 200)
```

---

## Bloques `arguments` múltiples

Se pueden tener bloques separados para argumentos de entrada y salida.

```matlab
function [media, varianza] = estadisticas(datos, opciones)
    arguments (Input)
        datos (:,1) double
        opciones.normalizar (1,1) logical = false
    end
    
    arguments (Output)
        media (1,1) double
        varianza (1,1) double
    end
    
    if opciones.normalizar
        datos = (datos - mean(datos)) / std(datos);
    end
    
    media = mean(datos);
    varianza = var(datos);
end
```

---

## Validación manual vs bloque `arguments`

| Aspecto | Validación manual | Bloque `arguments` |
|---------|-------------------|-------------------|
| **Código** | Múltiples líneas con `nargin`, `validateattributes` | Declarativo, compacto |
| **Legibilidad** | Menos clara | Muy clara, auto-documentada |
| **Mantenimiento** | Propenso a errores | Centralizado, menos errores |
| **Flexibilidad** | Total | Alta, pero con estructura definida |
| **Compatibilidad** | Todas las versiones | R2019b o posterior |
| **Documentación** | Se debe documentar manualmente | La validación es auto-documentada |

### Ejemplo comparativo

**Validación manual (antes de R2019b):**

```matlab
function procesar(datos, umbral, metodo, verbose)
    if nargin < 4
        verbose = false;
    end
    if nargin < 3
        metodo = 'promedio';
    end
    if nargin < 2
        error('Se requiere al menos datos y umbral');
    end
    
    validateattributes(datos, {'double'}, {'2d', 'nonempty'}, 'procesar', 'datos');
    validateattributes(umbral, {'numeric'}, {'scalar', 'positive'}, 'procesar', 'umbral');
    validateattributes(metodo, {'char'}, {'nonempty'}, 'procesar', 'metodo');
    validateattributes(verbose, {'logical'}, {'scalar'}, 'procesar', 'verbose');
    
    % Cuerpo de la función
end
```

**Con bloque `arguments` (R2019b+):**

```matlab
function procesar(datos, umbral, metodo, verbose)
    arguments
        datos (:,:) double {mustBeNonempty}
        umbral (1,1) {mustBeNumeric, mustBePositive}
        metodo (1,:) char = 'promedio'
        verbose (1,1) logical = false
    end
    % Cuerpo de la función
end
```

---

## Funciones de validación personalizadas

Se pueden crear funciones propias para validaciones específicas.

### Crear una función de validación

```matlab
function mustBeEven(x)
% mustBeEven Valida que el valor sea par
    if ~isnumeric(x) || mod(x, 2) ~= 0
        error('El valor debe ser un número par');
    end
end
```

### Uso en bloque `arguments`

```matlab
function configurar(param)
    arguments
        param (1,1) {mustBeInteger, mustBePositive, mustBeEven}
    end
    fprintf('Parámetro configurado: %d\n', param);
end
```

### Ejemplo de validación personalizada compleja

```matlab
function mustBeValidColor(x)
% mustBeValidColor Valida que el color sea formato RGB válido
    if ~isnumeric(x) || numel(x) ~= 3
        error('El color debe ser un vector RGB de 3 elementos');
    end
    if any(x < 0) || any(x > 1)
        error('Los valores RGB deben estar entre 0 y 1');
    end
end
```

Uso:
```matlab
function dibujar(opciones)
    arguments
        opciones.Color (1,3) {mustBeValidColor} = [0, 0, 1]
        opciones.Tamanio (1,1) {mustBePositive} = 1
    end
    % Cuerpo de la función
end
```

---

## Validaciones avanzadas

### Múltiples funciones de validación

```matlab
function analizar(datos, factor)
    arguments
        datos (:,2) double {mustBeFinite, mustBeNonempty}
        factor (1,1) {mustBeNumeric, mustBePositive, mustBeLessThan(factor, 10)}
    end
    % Cuerpo de la función
end
```

### Validación condicional

```matlab
function procesar(datos, metodo, opciones)
    arguments
        datos (:,1) double
        metodo (1,:) char {mustBeMember(metodo, {'promedio', 'mediana'})}
        opciones.umbral (1,1) double = 0
    end
    
    if opciones.umbral > 0
        % Validación adicional dentro del cuerpo
        mustBePositive(opciones.umbral);
    end
    
    % Cuerpo de la función
end
```

---

## Ejemplo completo: función con validación robusta

```matlab
function [resultados, estadisticas] = analizar_series(tiempo, senales, opciones)
% ANALIZAR_SERIES Analiza series temporales con validación de argumentos
%   [resultados, stats] = analizar_series(tiempo, senales)
%   [resultados, stats] = analizar_series(tiempo, senales, 'Metodo', 'fft')
%
%   Parámetros de entrada:
%       tiempo - Vector columna de tiempos
%       senales - Matriz de señales (columnas = canales)
%       opciones.Metodo - 'fft', 'wavelet', 'estadistico'
%       opciones.Ventana - Tamaño de ventana para suavizado
%       opciones.Verbose - Mostrar información de progreso

    arguments
        tiempo (:,1) double {mustBeFinite, mustBeNonempty}
        senales (:,:) double {mustBeFinite}
        opciones.Metodo (1,:) char {mustBeMember(opciones.Metodo, {'fft', 'wavelet', 'estadistico'})} = 'fft'
        opciones.Ventana (1,1) {mustBeInteger, mustBePositive} = 10
        opciones.Verbose (1,1) logical = false
    end
    
    arguments (Output)
        resultados struct
        estadisticas struct
    end
    
    % Verificar compatibilidad de dimensiones
    if size(tiempo, 1) ~= size(senales, 1)
        error('tiempo y senales deben tener el mismo número de filas');
    end
    
    % Procesamiento según método
    switch opciones.Metodo
        case 'fft'
            if opciones.Verbose
                fprintf('Aplicando FFT con ventana %d\n', opciones.Ventana);
            end
            resultados.fft = fft(senales, [], 1);
        case 'wavelet'
            if opciones.Verbose
                fprintf('Aplicando análisis wavelet\n');
            end
            % resultados.wavelet = ...
        case 'estadistico'
            if opciones.Verbose
                fprintf('Calculando estadísticas\n');
            end
            resultados.media = mean(senales, 1);
            resultados.std = std(senales, 0, 1);
    end
    
    % Estadísticas generales
    estadisticas.duracion = max(tiempo) - min(tiempo);
    estadisticas.canales = size(senales, 2);
    estadisticas.muestras = size(senales, 1);
end
```

---

## Compatibilidad con versiones anteriores

Si se necesita compatibilidad con versiones anteriores a R2019b, se puede mantener la validación manual.

### Detección de versión

```matlab
function procesar(datos, opciones)
    if verLessThan('matlab', '9.7')  % R2019b es la versión 9.7
        % Validación manual
        if nargin < 2, opciones = struct(); end
        if ~isfield(opciones, 'umbral'), opciones.umbral = 1; end
        validateattributes(datos, {'double'}, {'2d'});
        validateattributes(opciones.umbral, {'numeric'}, {'scalar', 'positive'});
    else
        % Bloque arguments (R2019b+)
        arguments
            datos (:,:) double
            opciones.umbral (1,1) {mustBePositive} = 1
        end
    end
    % Cuerpo de la función
end
```

---

## Buenas prácticas

| Práctica | Descripción |
|----------|-------------|
| **Validar en la entrada** | Detectar errores temprano, antes de procesar |
| **Mensajes de error claros** | Las funciones `mustBe*` ya proporcionan mensajes descriptivos |
| **Valores por defecto sensatos** | Elegir valores que tengan sentido en el contexto |
| **Documentar restricciones** | Incluir en la ayuda las validaciones principales |
| **Usar funciones de validación predefinidas** | Preferir `mustBePositive` a validaciones manuales |
| **Validaciones personalizadas** | Crear funciones reutilizables para validaciones complejas |

---

## Resumen rápido

| Concepto | Sintaxis | Ejemplo |
|----------|----------|---------|
| **Tamaño** | `(filas, columnas)` | `(1,:)` vector fila |
| **Tipo** | `double`, `char`, `string`, etc. | `datos (:,2) double` |
| **Validación** | `{mustBePositive, mustBeInteger}` | `{mustBePositive, mustBeInteger}` |
| **Opcional** | `= valor_default` | `verbose (1,1) logical = false` |
| **Nombre-valor** | `options.nombre tipo = default` | `options.Color (1,3) double = [0,0,1]` |
| **Output** | `arguments (Output)` | `arguments (Output) media (1,1) double` |

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones en Archivo]]
- [[Funciones Anónimas]]
- [[Funciones Locales y Privadas]]
- [[Argumentos variables|Argumentos variables]]
- [[Funciones Anidadas]]
