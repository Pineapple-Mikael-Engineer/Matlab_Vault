---
title: "Funciones en MATLAB"
tags:
  - MATLAB
  - funciones
  - index
  - referencia
aliases:
  - "Funciones"
  - "Function"
---

# Funciones en MATLAB

Una **función** es un archivo `.m` que encapsula lógica reutilizable. A diferencia de los [[Scripts]], las funciones tienen su propio workspace, reciben entradas y devuelven salidas de forma explícita.

> [!tip] Regla mental
> - [[Scripts]] = organizan el flujo principal
> - **Funciones** = ejecutan el trabajo real

---

## Estructura básica

```matlab
function [salida1, salida2] = nombre_funcion(entrada1, entrada2)
    % Cuerpo de la función
    salida1 = entrada1 + entrada2;
    salida2 = entrada1 * entrada2;
end
```

| Elemento | Descripción |
|----------|-------------|
| `function` | Palabra clave obligatoria |
| `salida1, salida2` | Variables de retorno (pueden ser una o varias) |
| `nombre_funcion` | Debe coincidir con el nombre del archivo `.m` |
| `entrada1, entrada2` | Parámetros de entrada |
| `end` | Opcional pero recomendado |

---

## Características fundamentales

### Workspace local

Cada función tiene su propio espacio de memoria. Las variables creadas dentro de una función **no existen** fuera de ella.

```matlab
function y = ejemplo(x)
    temp = x * 2;   % temp solo vive aquí
    y = temp + 1;
end
```

> [!info]
> Esto evita conflictos de nombres y hace que las funciones sean predecibles y reutilizables.

### Entradas y salidas

```matlab
% Una salida
function y = cuadrado(x)
    y = x^2;
end

% Múltiples salidas
function [suma, producto] = operaciones(a, b)
    suma = a + b;
    producto = a * b;
end
```

Se pueden ignorar salidas con `~`:

```matlab
[~, prod] = operaciones(2, 3);  % Solo interesa el producto
```

---

## Tipos de funciones

MATLAB ofrece varios tipos de funciones para diferentes necesidades:

| Tipo | Descripción | Enlace |
|------|-------------|--------|
| **Funciones en archivo** | La forma estándar: un archivo `.m` por función | [[Funciones en Archivo]] |
| **Funciones anónimas** | Funciones rápidas en una línea, sin archivo propio | [[Funciones Anónimas]] |
| **Funciones locales** | Funciones definidas dentro de un script o función principal | [[Funciones Locales y Privadas#Funciones locales]] |
| **Funciones privadas** | Funciones visibles solo dentro de una carpeta específica | [[Funciones Locales y Privadas#Funciones privadas]] |
| **Funciones anidadas** | Funciones dentro de otras funciones, comparten workspace | [[Funciones Anidadas]] |

---

## Manejo de argumentos

### Argumentos variables

Permiten crear funciones flexibles que aceptan número variable de entradas o salidas.

| Herramienta | Propósito |
|-------------|-----------|
| `nargin` | Número de argumentos de entrada proporcionados |
| `nargout` | Número de argumentos de salida solicitados |
| `varargin` | Celda que captura todos los inputs adicionales |
| `varargout` | Captura outputs adicionales |

→ Ver [[Argumentos variables]]

### Validación de argumentos (bloque `arguments`)

Desde R2019b, MATLAB permite validar tipos y tamaños de argumentos de forma declarativa:

```matlab
function y = cuadrado(x)
    arguments
        x (1,1) double   % x debe ser escalar double
    end
    y = x^2;
end
```

→ Ver [[Validación de argumentos]]

---

## Funciones vs Scripts

| Aspecto | [[Scripts]] | Funciones |
|---------|-------------|-----------|
| Workspace | Global (base) | Local (propio) |
| Entradas | No | Sí |
| Salidas | No | Sí |
| Reutilización | Baja | Alta |
| Uso típico | Flujo principal, pruebas | Lógica reutilizable |

---

## Buenas prácticas

### Nombres claros

```matlab
calcular_velocidad_promedio   % ✔️ Claro
f1                            % ❌ Ambiguo
```

### Una función por archivo

Salvo funciones locales, cada archivo `.m` debe contener una sola función principal.

### Funciones pequeñas

Una función debe hacer **una cosa** y hacerla bien. Si supera 20-30 líneas, considerar dividirla.

### Documentación básica

El primer bloque de comentarios aparece con `help`:

```matlab
function y = cuadrado(x)
% CUADRADO Calcula el cuadrado de un número
%   y = cuadrado(x) devuelve x^2
    y = x^2;
end
```

```matlab
>> help cuadrado
  CUADRADO Calcula el cuadrado de un número
    y = cuadrado(x) devuelve x^2
```

### Evitar dependencias ocultas

```matlab
function y = mala_funcion()
    y = variable_global + 1;   % ❌ Depende de algo externo
end
```

Las funciones deben ser autónomas; todas las dependencias deben ser entradas o definidas internamente.

---

## Flujo de trabajo típico

```
main.m (script)
    ├── addpath(...)              → Configuración
    ├── datos = cargar_datos()    → Función de carga
    ├── resultado = procesar(datos) → Función de procesamiento
    └── graficar(resultado)       → Función de visualización
```

---

## Errores comunes

| Error | Solución |
|-------|----------|
| Nombre de archivo ≠ nombre de función | Hacer que coincidan exactamente |
| Olvidar asignar salida | Asegurar que todas las salidas tengan valor |
| Función no encontrada | Verificar que el archivo está en el [[Configuración de la ruta de búsqueda\|path]] |
| Confusión con script | Si el archivo no comienza con `function`, MATLAB lo trata como script |

---

## Notas relacionadas

- [[Funciones en Archivo]]
- [[Funciones Anónimas]]
- [[Funciones Locales y Privadas]]
- [[Funciones Anidadas]]
- [[Argumentos variables]]
- [[Validación de argumentos]]
- [[Scripts]]
- [[Configuración de la ruta de búsqueda]]
