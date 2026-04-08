---
title: 'Comentarios con % y bloques de ayuda (help, doc)'
tags:
  - MATLAB
  - comentarios
  - documentacion
  - help
  - doc
---

# Comentarios con `%` y bloques de ayuda (`help`, `doc`)

En MATLAB, los comentarios no son solo texto ignorado por el intérprete. Cumplen un papel fundamental en la documentación del código, ya que el sistema de ayuda (`help` y `doc`) se construye directamente a partir de ellos.

---

## Comentarios en MATLAB (`%`)

Los comentarios se escriben usando el símbolo `%`. Todo lo que sigue después de `%` en una línea es ignorado por MATLAB durante la ejecución.

### Tipos de comentarios

| Tipo | Sintaxis | Ejemplo |
|------|----------|---------|
| **En línea** | `código % comentario` | `x = 5; % valor inicial` |
| **Línea completa** | `% comentario` | `% Este script calcula energía` |
| **Bloque múltiple** | Varias líneas con `%` | Ver sección de bloques |

### Ejemplo

```matlab
% Cálculo de energía cinética
% Este script utiliza masa y velocidad para calcular energía

m = 10;   % masa en kg
v = 3;    % velocidad en m/s

E = 0.5 * m * v^2;  % energía en julios

disp(['Energía: ', num2str(E), ' J']);
```

---

## Bloques de comentarios (documentación de funciones)

Los comentarios que aparecen **inmediatamente después de la línea `function`** tienen un propósito especial: constituyen la documentación oficial de la función, accesible mediante `help`.

### Estructura básica

```matlab
function E = energia(m, v)
% ENERGIA Calcula la energía cinética
%   E = ENERGIA(m, v) retorna la energía cinética 1/2 * m * v^2
%
%   Parámetros:
%       m - masa (kg)
%       v - velocidad (m/s)
%
%   Ejemplo:
%       E = energia(10, 3)  % retorna 45

E = 0.5 * m * v^2;
end
```

### Componentes recomendados

| Componente | Descripción |
|------------|-------------|
| **Nombre en mayúsculas** | Identifica la función claramente |
| **Descripción breve** | Una línea que resume la funcionalidad |
| **Sintaxis** | Muestra cómo se usa la función |
| **Parámetros** | Explica cada argumento de entrada/salida |
| **Ejemplo** | Demostración de uso |
| **Notas adicionales** | Limitaciones, referencias, etc. |

---

## Comando `help`

El comando `help` muestra la documentación de una función basada en los comentarios del archivo.

### Sintaxis

```matlab
help nombre_funcion
help ruta/archivo
```

### Ejemplo

```matlab
>> help energia
  ENERGIA Calcula la energía cinética
    E = ENERGIA(m, v) retorna la energía cinética 1/2 * m * v^2
  
    Parámetros:
        m - masa (kg)
        v - velocidad (m/s)
  
    Ejemplo:
        E = energia(10, 3)  % retorna 45
```

### Características

| Aspecto | Descripción |
|---------|-------------|
| **Formato** | Texto plano en Command Window |
| **Fuente** | Comentarios del archivo `.m` |
| **Velocidad** | Muy rápida |
| **Uso típico** | Consulta rápida, desarrollo |

---

## Comando `doc`

El comando `doc` abre la documentación en una ventana del navegador interno de MATLAB.

### Sintaxis

```matlab
doc nombre_funcion
```

### Diferencias entre `help` y `doc`

| Característica | `help` | `doc` |
|----------------|--------|-------|
| **Formato** | Texto plano | Interfaz gráfica HTML |
| **Velocidad** | Inmediata | Más lenta (abre ventana) |
| **Contenido** | Comentarios del usuario | Documentación oficial + comentarios |
| **Navegación** | Lineal | Enlaces, búsqueda, ejemplos interactivos |
| **Uso típico** | Recordar sintaxis | Explorar funcionalidades, ejemplos detallados |

### Ejemplo

```matlab
>> doc plot
```

Abre la documentación completa de `plot` con ejemplos, sintaxis detallada y enlaces a funciones relacionadas.

---

## Comentarios en bloques y secciones

Aunque MATLAB no tiene comentarios multilínea como `/* ... */` en otros lenguajes, se pueden simular con varias líneas de `%` o usar `%%` para crear secciones.

### Bloques visuales

```matlab
% =========================================
% ==== SECCIÓN: CÁLCULO DE PARÁMETROS =====
% =========================================

parametro1 = 10;
parametro2 = 20;
```

### Secciones con `%%`

```matlab
%% Inicialización
% Esta sección configura los parámetros iniciales
x = 0:0.1:10;
y = zeros(size(x));

%% Procesamiento
% Bucle principal de cálculo
for i = 1:length(x)
    y(i) = sin(x(i));
end

%% Visualización
% Gráfica de resultados
plot(x, y);
title('Función seno');
```

Las secciones con `%%` también son la base para la publicación con `publish` (ver [[Celdas y Publicacion a HTML-PDF|Celdas (%%) y publicación a HTML-PDF]]).

---

## Formato recomendado para documentación de funciones

### Plantilla estándar

```matlab
function [salida1, salida2] = nombre_funcion(entrada1, entrada2)
% NOMBRE_FUNCION Breve descripción de la funcionalidad
%   [out1, out2] = NOMBRE_FUNCION(in1, in2) realiza una operación
%   específica y devuelve resultados en out1 y out2.
%
%   Parámetros de entrada:
%       entrada1 - descripción (tipo, dimensiones, restricciones)
%       entrada2 - descripción
%
%   Parámetros de salida:
%       salida1 - descripción
%       salida2 - descripción
%
%   Ejemplo:
%       [a, b] = nombre_funcion(5, 10)
%
%   Ver también: OTRAS_FUNCIONES_RELACIONADAS

% Cuerpo de la función
salida1 = entrada1 + entrada2;
salida2 = entrada1 * entrada2;
end
```

### Ejemplo completo

```matlab
function [media, desviacion] = estadisticas(datos)
% ESTADISTICAS Calcula media y desviación estándar de un vector
%   [m, d] = ESTADISTICAS(datos) devuelve la media aritmética y
%   la desviación estándar muestral del vector datos.
%
%   Parámetros de entrada:
%       datos - vector numérico (double) de cualquier longitud
%
%   Parámetros de salida:
%       media - promedio aritmético de los datos
%       desviacion - desviación estándar muestral (N-1)
%
%   Ejemplo:
%       [m, d] = estadisticas([1, 2, 3, 4, 5])
%       % m = 3, d = 1.5811
%
%   Ver también: mean, std, var

    media = sum(datos) / length(datos);
    
    % Cálculo de desviación estándar
    diferencias = datos - media;
    varianza = sum(diferencias.^2) / (length(datos) - 1);
    desviacion = sqrt(varianza);
end
```

---

## Buenas prácticas

### Claridad

- Explicar **por qué** se hace algo, no solo **qué** se hace

❌ Comentario redundante:
```matlab
x = x + 1; % suma 1 a x
```

✔ Comentario útil:
```matlab
x = x + 1; % incremento para evitar división por cero
```

### Documentación de funciones

- **Toda función pública debe tener documentación** con `help`
- La primera línea debe ser una descripción concisa (aparece en `help` y `lookfor`)

### Organización

- Usar bloques de comentarios para separar secciones lógicas
- Mantener un formato consistente en todo el proyecto

### Actualización

- Actualizar comentarios cuando se modifica el código
- Comentarios desactualizados son peores que no tener comentarios

---

## Errores comunes

| Error | Consecuencia | Solución |
|-------|--------------|----------|
| **No documentar funciones** | `help` no muestra información | Agregar bloque de comentarios después de `function` |
| **Comentarios desactualizados** | Documentación incorrecta | Revisar al modificar el código |
| **Explicar lo obvio** | Ruido visual | Comentar solo lo que no es evidente |
| **Formato inconsistente** | Dificulta lectura | Establecer y seguir un estándar |
| **Primera línea demasiado larga** | `help` corta la línea | Mantener primera línea breve |

---

## Comentarios y publicación (`publish`)

Cuando se usa `publish` (ver [[Celdas y Publicacion a HTML-PDF|Celdas (%%) y publicación a HTML-PDF]]), MATLAB interpreta los comentarios de manera especial:

| Elemento | En el script | En el documento publicado |
|----------|--------------|---------------------------|
| `%% Título` | Encabezado de sección | Título de nivel 1 |
| `% Texto` | Comentario | Párrafo |
| Código | Instrucciones MATLAB | Bloque de código (opcional) |

### Ejemplo

```matlab
%% Método de Euler
% Este método aproxima la solución de una ecuación diferencial
% ordinaria de primer orden.

f = @(x, y) x + y;
x0 = 0; y0 = 1;
h = 0.1;
x_final = 1;

% Implementación del método
x = x0:h:x_final;
y = zeros(size(x));
y(1) = y0;

for i = 1:length(x)-1
    y(i+1) = y(i) + h * f(x(i), y(i));
end

% Visualización
plot(x, y, 'b-o');
xlabel('x');
ylabel('y');
title('Solución aproximada con Euler');
grid on;
```

---

## Resumen rápido

| Concepto | Descripción |
|----------|-------------|
| **Comentario** | Línea que comienza con `%` |
| **Bloque de ayuda** | Comentarios después de `function` |
| **`help`** | Muestra documentación en Command Window |
| **`doc`** | Abre documentación en navegador interno |
| **`%%`** | Define secciones ejecutables y encabezados |
| **Primera línea** | Debe ser breve (aparece en `help` y `lookfor`) |

---

## Notas relacionadas

- [[Celdas y Publicacion a HTML-PDF|Celdas (%%) y publicación a HTML-PDF]]
- [[Scripts]]
- [[Funciones en Archivo|Funciones en archivo]]
- [[01 Entorno y Flujo de Trabajo/1.1 Interfaz/Elementos del Editor/index|Editor, Command Window, Workspace, Current Folder]]
- [[Atajos de teclado esenciales en MATLAB|Atajos de teclado esenciales]]
