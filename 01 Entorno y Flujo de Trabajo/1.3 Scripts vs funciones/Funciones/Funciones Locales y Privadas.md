---
title: "Funciones locales y privadas"
tags:
  - MATLAB
  - funciones
  - locales
  - privadas
  - visibilidad
  - organizacion
aliases:
  - "Función local"
  - "Función privada"
  - "Subfunción"
  - "Local function"
  - "Private function"
---

# Funciones locales y privadas

MATLAB ofrece mecanismos para controlar la visibilidad de las funciones. Las **funciones locales** y **funciones privadas** permiten organizar el código en módulos, ocultar implementaciones internas y evitar conflictos de nombres.

---

## Funciones locales

Una **función local** (también llamada *subfunción*) es una función definida dentro del mismo archivo que una función principal. Solo es visible dentro de ese archivo.

### Estructura básica

```matlab
% Archivo: analisis.m
function resultado = analisis(datos)
% Función principal
    resultado = procesar(datos);
end

function y = procesar(x)
% Función local (solo visible dentro de analisis.m)
    y = x.^2;
end

function mostrar(titulo)
% Otra función local
    disp(['Resultado: ', titulo]);
end
```

### Características

| Característica | Descripción |
|----------------|-------------|
| **Ubicación** | Dentro del mismo archivo que la función principal |
| **Visibilidad** | Solo dentro del archivo donde están definidas |
| **Orden** | Pueden aparecer en cualquier orden después de la función principal |
| **Acceso** | La función principal y otras locales pueden llamarlas |
| **Workspace** | Cada función local tiene su propio workspace |

### Reglas importantes

1. **Una función principal por archivo**: El archivo debe comenzar con la función principal (la que da nombre al archivo).

2. **Las funciones locales van después**: Todas las funciones locales se definen después de la función principal.

3. **La función principal puede llamar a las locales**: La función principal puede invocar cualquier función local dentro del archivo.

```matlab
% Archivo: operaciones.m
function resultado = operaciones(a, b)
    resultado.suma = sumar(a, b);
    resultado.producto = multiplicar(a, b);
    resultado.potencia = elevar(a, b);
end

function s = sumar(x, y)
    s = x + y;
end

function p = multiplicar(x, y)
    p = x * y;
end

function pot = elevar(x, y)
    pot = x ^ y;
end
```

### Múltiples funciones locales

Un archivo puede contener tantas funciones locales como sea necesario.

```matlab
% Archivo: estadisticas.m
function [media, varianza] = estadisticas(datos)
    media = calcular_media(datos);
    varianza = calcular_varianza(datos, media);
end

function m = calcular_media(d)
    m = sum(d) / length(d);
end

function v = calcular_varianza(d, m)
    v = sum((d - m).^2) / (length(d) - 1);
end

function mostrar_resumen(m, v)
    fprintf('Media: %.2f\n', m);
    fprintf('Varianza: %.2f\n', v);
end
```

### Ventajas de las funciones locales

| Ventaja | Descripción |
|---------|-------------|
| **Encapsulación** | Ocultar detalles de implementación |
| **Organización** | Dividir lógica compleja en partes manejables |
| **Reutilización interna** | Evitar duplicación de código dentro del archivo |
| **Sin contaminación** | No exponer funciones auxiliares a otros archivos |
| **Facilidad de mantenimiento** | Código más legible y modular |

---

## Funciones privadas

Una **función privada** es una función almacenada en una subcarpeta llamada `private`. Solo es visible para funciones dentro de la carpeta padre inmediata.

### Estructura de carpetas

```
/mi_proyecto/
    ├── analisis.m
    ├── visualizar.m
    └── /private/
        ├── procesar_datos.m
        └── validar_input.m
```

### Cómo funcionan

- Las funciones en `analisis.m` y `visualizar.m` pueden llamar a `procesar_datos` y `validar_input`
- Las funciones fuera de `/mi_proyecto` **no** pueden acceder a las funciones privadas
- Otras funciones dentro de la misma carpeta padre también tienen acceso

### Ejemplo

**Archivo: `mi_proyecto/analisis.m`**

```matlab
function resultado = analisis(datos)
    % Puede llamar a funciones privadas
    if ~validar_input(datos)
        error('Datos inválidos');
    end
    resultado = procesar_datos(datos);
end
```

**Archivo: `mi_proyecto/private/validar_input.m`**

```matlab
function es_valido = validar_input(datos)
    % Función privada: solo accesible desde mi_proyecto/
    es_valido = isnumeric(datos) && ~isempty(datos);
end
```

**Archivo: `mi_proyecto/private/procesar_datos.m`**

```matlab
function resultado = procesar_datos(datos)
    % Función privada: solo accesible desde mi_proyecto/
    resultado = datos.^2;
end
```

### Ventajas de las funciones privadas

| Ventaja | Descripción |
|---------|-------------|
| **Aislamiento** | Funciones no visibles fuera del módulo |
| **API limpia** | Solo exponer funciones públicas necesarias |
| **Refactorización segura** | Cambiar internas sin afectar otros módulos |
| **Evitar conflictos** | Nombres comunes no interfieren con otras carpetas |
| **Organización de proyectos grandes** | Separación clara de responsabilidades |

### Reglas importantes

1. **La carpeta debe llamarse `private`**: Exactamente ese nombre, en minúsculas.

2. **Ubicación**: La carpeta `private` debe estar dentro de la carpeta que contiene las funciones que la usarán.

3. **No anidar**: No se recomienda crear carpetas `private` dentro de otras `private`.

4. **Prioridad**: MATLAB busca en `private` antes que en otras rutas del path.

---

## Comparación: locales vs privadas

| Aspecto | Funciones locales | Funciones privadas |
|---------|-------------------|-------------------|
| **Ubicación** | Mismo archivo que la función principal | Carpeta `private` separada |
| **Visibilidad** | Solo dentro del archivo | Dentro de toda la carpeta padre |
| **Número de funciones** | Múltiples por archivo | Múltiples archivos en una carpeta |
| **Reutilización** | Solo por funciones del mismo archivo | Por todas las funciones de la carpeta padre |
| **Uso típico** | Dividir una función compleja | Crear bibliotecas internas de un módulo |

### Cuándo usar cada una

| Situación | Recomendación |
|-----------|---------------|
| Dividir una función larga en partes más pequeñas | Funciones locales |
| Función auxiliar usada solo por una función | Funciones locales |
| Conjunto de utilidades compartidas por varias funciones del mismo módulo | Funciones privadas |
| Ocultar implementación de una biblioteca interna | Funciones privadas |
| Evitar conflictos con funciones del mismo nombre en otros proyectos | Funciones privadas |

---

## Ejemplos completos

### Ejemplo 1: Funciones locales para procesamiento de imágenes

```matlab
% Archivo: procesar_imagen.m
function resultado = procesar_imagen(imagen)
% Función principal
    if ~validar_imagen(imagen)
        error('Imagen inválida');
    end
    
    gris = convertir_gris(imagen);
    filtrada = aplicar_filtro(gris);
    resultado = ecualizar(filtrada);
end

function es_valida = validar_imagen(img)
    es_valida = isnumeric(img) && ndims(img) <= 3;
end

function gris = convertir_gris(img)
    if ndims(img) == 3
        gris = 0.2989 * img(:,:,1) + ...
               0.5870 * img(:,:,2) + ...
               0.1140 * img(:,:,3);
    else
        gris = img;
    end
end

function filtrada = aplicar_filtro(img)
    filtrada = img;
    % Lógica de filtrado
end

function ecualizada = ecualizar(img)
    ecualizada = img;
    % Lógica de ecualización
end
```

### Ejemplo 2: Funciones privadas para un módulo de análisis

**Estructura:**
```
/analisis/
    ├── analizar_serie.m
    ├── comparar_series.m
    └── /private/
        ├── calcular_tendencia.m
        ├── detectar_estacionalidad.m
        ├── suavizar.m
        └── validar_serie.m
```

**Archivo: `analisis/analizar_serie.m`**

```matlab
function [tendencia, estacionalidad] = analizar_serie(datos)
    if ~validar_serie(datos)
        error('Serie temporal inválida');
    end
    
    datos_suavizados = suavizar(datos);
    tendencia = calcular_tendencia(datos_suavizados);
    estacionalidad = detectar_estacionalidad(datos);
end
```

**Archivo: `analisis/comparar_series.m`**

```matlab
function similitud = comparar_series(serie1, serie2)
    % También puede usar las mismas funciones privadas
    if ~validar_serie(serie1) || ~validar_serie(serie2)
        error('Series inválidas');
    end
    
    s1_suav = suavizar(serie1);
    s2_suav = suavizar(serie2);
    similitud = corrcoef(s1_suav, s2_suav);
    similitud = similitud(1,2);
end
```

**Archivo: `analisis/private/validar_serie.m`**

```matlab
function es_valida = validar_serie(datos)
    es_valida = isnumeric(datos) && ...
                isvector(datos) && ...
                all(isfinite(datos));
end
```

**Archivo: `analisis/private/suavizar.m`**

```matlab
function datos_suav = suavizar(datos)
    % Filtro de media móvil simple
    ventana = 5;
    datos_suav = movmean(datos, ventana);
end
```

---

## Consideraciones de depuración

### Funciones locales

- Se pueden establecer breakpoints dentro de funciones locales
- El debugger permite "Step Into" (`F11`) para entrar en ellas
- Aparecen en la pila de llamadas con su nombre local

### Funciones privadas

- Se comportan como funciones normales durante la depuración
- Se puede usar `which` para verificar su ubicación

```matlab
>> which -all validar_serie
C:\Users\usuario\analisis\private\validar_serie.m  % Private function
```

---

## Buenas prácticas

### Funciones locales

1. **Mantenerlas cortas**: Idealmente menos de 20-30 líneas
2. **Nombres descriptivos**: Usar nombres que indiquen su propósito
3. **Agrupar por funcionalidad**: Ordenar las funciones locales de manera lógica
4. **Documentar**: Incluir comentarios de ayuda para cada función local

### Funciones privadas

1. **Usar para bibliotecas internas**: Cuando múltiples funciones comparten utilidades
2. **Documentar igual que funciones públicas**: Incluir ayuda aunque sean privadas
3. **No abusar**: Si solo una función usa una utilidad, considerar función local
4. **Estructura clara**: Mantener la carpeta `private` organizada

---

## Resumen rápido

| Concepto | Definición | Visibilidad |
|----------|------------|-------------|
| **Función local** | Función dentro del mismo archivo | Solo ese archivo |
| **Función privada** | Función en carpeta `private` | Toda la carpeta padre |

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones en Archivo]]
- [[Funciones Anónimas]]
- [[Argumentos variables]]
- [[Validación de argumentos]]
- [[Funciones Anidadas]]
