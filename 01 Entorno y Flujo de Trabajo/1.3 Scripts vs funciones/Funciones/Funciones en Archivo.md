---
title: "Funciones en archivo"
tags:
  - MATLAB
  - funciones
  - archivo-m
  - sintaxis-basica
aliases:
  - "Función en archivo"
  - "Función estándar"
  - "Function file"
---

# Funciones en archivo

Una **función en archivo** es la forma estándar de definir funciones en MATLAB. Cada función reside en su propio archivo `.m`, cuyo nombre debe coincidir exactamente con el nombre de la función principal.

---

## Estructura básica

```matlab
function [salida1, salida2] = nombre_funcion(entrada1, entrada2)
    % Cuerpo de la función
    % Cálculos y operaciones
    salida1 = entrada1 + entrada2;
    salida2 = entrada1 * entrada2;
end
```

### Componentes

| Componente | Descripción |
|------------|-------------|
| `function` | Palabra clave que indica el inicio de la definición |
| `salida1, salida2` | Variables de retorno (entre corchetes si hay múltiples) |
| `nombre_funcion` | Identificador de la función. Debe coincidir con el nombre del archivo |
| `entrada1, entrada2` | Parámetros que recibe la función |
| `end` | Opcional pero recomendado para delimitar el final |

---

## Regla fundamental: nombre del archivo

> [!important]
> El nombre del archivo `.m` debe ser **exactamente igual** al nombre de la función definida en su interior.

| Archivo | Función | Correcto |
|---------|---------|----------|
| `cuadrado.m` | `function y = cuadrado(x)` | ✔️ |
| `cuadrado.m` | `function y = calcular_cuadrado(x)` | ❌ |
| `mi_funcion.m` | `function resultado = mi_funcion(x)` | ✔️ |

Si no coinciden, MATLAB no puede encontrar la función cuando se llama por su nombre.

---

## Ejemplos por tipo

### Función con una entrada y una salida

Archivo: `cuadrado.m`

```matlab
function y = cuadrado(x)
    y = x^2;
end
```

Uso:
```matlab
>> resultado = cuadrado(5)
resultado = 25
```

### Función con múltiples entradas

Archivo: `suma.m`

```matlab
function total = suma(a, b)
    total = a + b;
end
```

Uso:
```matlab
>> resultado = suma(10, 20)
resultado = 30
```

### Función con múltiples salidas

Archivo: `estadisticas.m`

```matlab
function [media, desviacion] = estadisticas(datos)
    media = mean(datos);
    desviacion = std(datos);
end
```

Uso:
```matlab
>> [m, d] = estadisticas([1, 2, 3, 4, 5])
m = 3
d = 1.5811

>> solo_media = estadisticas([1, 2, 3, 4, 5])  % Solo primera salida
solo_media = 3
```

### Función sin salidas

Archivo: `mostrar_mensaje.m`

```matlab
function mostrar_mensaje(texto)
    disp(['Mensaje: ', texto]);
end
```

Uso:
```matlab
>> mostrar_mensaje('Hola MATLAB')
Mensaje: Hola MATLAB
```

### Función sin entradas

Archivo: `saludo.m`

```matlab
function saludo()
    disp('Bienvenido a MATLAB');
end
```

Uso:
```matlab
>> saludo()
Bienvenido a MATLAB
```

---

## Workspace local

Toda variable creada dentro de una función **no existe** fuera de ella.

```matlab
function y = ejemplo(x)
    temp = x * 2;   % Variable local
    y = temp + 1;
end
```

```matlab
>> resultado = ejemplo(5);
>> temp
Unrecognized function or variable 'temp'.
```

> [!info]
> Este aislamiento evita conflictos de nombres y hace que las funciones sean predecibles y reutilizables.

---

## Ignorar salidas

Cuando no se necesita una o más salidas, se usa el operador `~`.

```matlab
function [suma, producto, cociente] = operaciones(a, b)
    suma = a + b;
    producto = a * b;
    cociente = a / b;
end
```

```matlab
% Solo interesa la suma
>> s = operaciones(10, 2)
s = 12

% Solo interesa el producto
>> [~, p] = operaciones(10, 2)
p = 20

% Interesan suma y cociente, no el producto
>> [s, ~, c] = operaciones(10, 2)
s = 12
c = 5
```

---

## La sentencia `end`

Aunque `end` es opcional al final de una función, su uso es recomendado por varias razones:

- **Claridad**: Delimita explícitamente dónde termina la función
- **Consistencia**: Necesario si la función contiene funciones anidadas
- **Buenas prácticas**: Facilita la lectura del código

```matlab
function y = cuadrado(x)
    y = x^2;
end   % <-- Opcional pero recomendado
```

---

## Localización de funciones

Cuando se llama a una función, MATLAB la busca en este orden:

1. **Current Folder** (carpeta activa)
2. **Rutas del path** (en orden de prioridad)
3. **Funciones integradas** (built-in)

> [!tip]
> Usar `which nombre_funcion` para saber qué archivo está ejecutando MATLAB realmente.

```matlab
>> which cuadrado
C:\Users\usuario\Documentos\MATLAB\cuadrado.m
```

---

## Buenas prácticas

### Una función por archivo

Cada archivo `.m` debe contener una única función principal (aunque puede tener [[Funciones Locales y Privadas#Funciones locales\|funciones locales]] adicionales).

### Nombres descriptivos

```matlab
calcular_area_circulo     % ✔️
calc_area_circ            % ✔️
f1                        % ❌
```

### Documentación con comentarios

El primer bloque de comentarios se convierte en la ayuda de la función:

```matlab
function area = calcular_area_circulo(radio)
% CALCULAR_AREA_CIRCULO Calcula el área de un círculo
%   area = calcular_area_circulo(radio) devuelve el área
%   usando la fórmula pi * radio^2
%
%   Ejemplo:
%       a = calcular_area_circulo(5)
    area = pi * radio^2;
end
```

```matlab
>> help calcular_area_circulo
  CALCULAR_AREA_CIRCULO Calcula el área de un círculo
    area = calcular_area_circulo(radio) devuelve el área
    usando la fórmula pi * radio^2
  
    Ejemplo:
        a = calcular_area_circulo(5)
```

### Funciones pequeñas y enfocadas

Una función debe hacer **una cosa** y hacerla bien. Si una función supera las 30-50 líneas, considerar dividirla en funciones más pequeñas.

---

## Errores comunes

### Nombre de archivo no coincide

| Situación | Error |
|-----------|-------|
| Archivo: `cuadrado.m` con función `function y = cuad(x)` | `Undefined function 'cuadrado'` o `'cuad'` |

**Solución:** Hacer coincidir nombre de archivo y función.

### Olvidar asignar salida

```matlab
function y = cuadrado(x)
    x^2;   % ❌ No se asigna a y
end
```

**Solución:** `y = x^2;`

### Función no encontrada

```matlab
>> mi_funcion(5)
Undefined function or variable 'mi_funcion'.
```

**Solución:** Verificar que el archivo está en la [[Configuración de la ruta de búsqueda\|ruta de búsqueda]] o en la carpeta activa.

---

## Ejemplo completo

Archivo: `analisis_datos.m`

```matlab
function [media, mediana, desviacion] = analisis_datos(datos)
% ANALISIS_DATOS Calcula estadísticas básicas de un conjunto de datos
%   [media, mediana, desviacion] = analisis_datos(datos) devuelve:
%       media: promedio aritmético
%       mediana: valor central
%       desviacion: desviación estándar
%
%   Ejemplo:
%       [m, md, d] = analisis_datos([1, 2, 3, 4, 10])
    media = mean(datos);
    mediana = median(datos);
    desviacion = std(datos);
end
```

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones Anónimas]]
- [[Funciones Locales y Privadas]]
- [[Argumentos variables]]
- [[Validación de argumentos]]
- [[Scripts]]

