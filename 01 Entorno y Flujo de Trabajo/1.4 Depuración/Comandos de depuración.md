---
title: "Comandos de depuración (dbstop, dbstep, dbcont)"
tags:
  - MATLAB
  - debug
  - comandos
  - dbstop
  - dbstep
  - dbcont
  - depuración
  - command-window
aliases:
  - "Debug commands"
  - "Comandos debug"
  - "dbstop"
  - "dbstep"
  - "dbcont"
---

# Comandos de depuración (dbstop, dbstep, dbcont)

Los comandos de depuración permiten controlar la ejecución de código MATLAB desde el [[Editor, Command Window, Workspace, Current Folder#Command Window|Command Window]], especialmente útil para depuración avanzada, scripts sin interfaz gráfica, o cuando se necesita automatizar el proceso de depuración.

---

## Control de breakpoints: `dbstop`

### Sintaxis básica

```matlab
dbstop in archivo at linea
dbstop in archivo if condicion
dbstop if error
dbstop if warning
```

### Ejemplos

| Comando | Efecto |
|---------|--------|
| `dbstop in mi_script` | Detiene al inicio de `mi_script.m` |
| `dbstop in mi_funcion at 15` | Detiene en la línea 15 de `mi_funcion.m` |
| `dbstop in analisis if x > 10` | Detiene cuando `x > 10` en `analisis.m` |
| `dbstop if error` | Detiene ante cualquier error |
| `dbstop if warning` | Detiene ante cualquier warning |
| `dbstop if naninf` | Detiene si aparece NaN o Inf |

### Breakpoints condicionales con `dbstop`

```matlab
% Detener en un bucle cuando i == 523
dbstop in procesar at 25 if i == 523

% Detener cuando una variable es negativa
dbstop in calcular at 10 if resultado < 0
```

---

## Control de ejecución paso a paso

### `dbcont` — continuar

```matlab
dbcont
```

Continúa la ejecución hasta el siguiente breakpoint o hasta el final del programa.

### `dbstep` — avanzar paso a paso

| Comando | Efecto |
|---------|--------|
| `dbstep` | Avanza una línea (sin entrar en funciones llamadas) |
| `dbstep 3` | Avanza 3 líneas |
| `dbstep in` | Entra dentro de la función en la línea actual (Step In) |
| `dbstep out` | Sale de la función actual y se detiene en la línea siguiente a la llamada (Step Out) |

### Equivalencia con atajos de teclado

| Comando | Atajo de teclado | Acción |
|---------|------------------|--------|
| `dbstep` | `F10` | Step Over (no entra en funciones) |
| `dbstep in` | `F11` | Step Into (entra en funciones) |
| `dbstep out` | `Shift+F11` | Step Out (sale de la función actual) |
| `dbcont` | `F5` | Continue (hasta siguiente breakpoint) |

---

## Gestión de breakpoints: `dbclear` y `dbstatus`

### `dbclear` — eliminar breakpoints

```matlab
dbclear all                    % Elimina todos los breakpoints
dbclear in mi_script           % Elimina breakpoints de un archivo
dbclear in mi_funcion at 15    % Elimina breakpoint específico
dbclear if error               % Desactiva dbstop if error
dbclear if warning             % Desactiva dbstop if warning
```

### `dbstatus` — ver breakpoints activos

```matlab
dbstatus                       % Muestra todos los breakpoints activos
dbstatus mi_funcion            % Muestra breakpoints de un archivo específico
```

**Salida típica:**
```matlab
>> dbstatus
Breakpoint for analisis.m:
  Line 15   Stop if x > 10
Breakpoint for procesar.m:
  Line 8   Stop if i == 523
dbstop if error
```

---

## Pila de llamadas: `dbstack`

Cuando la ejecución está pausada, `dbstack` muestra la secuencia de llamadas que llevaron al punto actual.

```matlab
dbstack
```

**Salida típica:**
```matlab
> In analisis (line 25)
  In procesar (line 8)
  In main (line 5)
```

Esto indica:
- `analisis.m` línea 25 es donde está el breakpoint
- `analisis` fue llamada desde `procesar` línea 8
- `procesar` fue llamada desde `main` línea 5

### Opciones de `dbstack`

| Comando | Efecto |
|---------|--------|
| `dbstack` | Muestra toda la pila |
| `dbstack -completenames` | Muestra rutas completas de los archivos |
| `[st, idx] = dbstack` | Devuelve estructura con información de la pila |

---

## Salir de depuración: `dbquit`

```matlab
dbquit
```

Termina el modo de depuración y vuelve al modo normal. **No elimina breakpoints**, solo sale del estado de pausa.

---

## Depuración automática con `dbstop if error`

Esta es una de las herramientas más poderosas para encontrar errores difíciles de rastrear.

```matlab
dbstop if error
```

**Flujo típico:**
1. Activar `dbstop if error`
2. Ejecutar el código que falla
3. MATLAB se detiene **exactamente en la línea que causa el error**
4. Inspeccionar variables en ese contexto
5. Corregir el error

### Variantes

| Comando | Efecto |
|---------|--------|
| `dbstop if error` | Se detiene en cualquier error |
| `dbstop if warning` | Se detiene en cualquier warning |
| `dbstop if naninf` | Se detiene si aparece NaN o Inf |
| `dbstop if caught error` | Se detiene también en errores capturados por `try/catch` |

---

## Depuración en funciones recursivas

Las funciones recursivas se pueden depurar con los mismos comandos.

```matlab
function resultado = factorial(n)
    if n <= 1
        resultado = 1;
    else
        resultado = n * factorial(n-1);   % Breakpoint aquí
    end
end
```

Durante la depuración:
- `dbstack` muestra los niveles de recursión
- Cada llamada recursiva tiene su propio workspace
- Se puede inspeccionar `n` en cada nivel

---

## Depuración remota y desde scripts

Los comandos de depuración se pueden incluir en scripts para depuración automatizada.

```matlab
% debug_setup.m
dbstop if error
dbstop in analisis at 25 if x > 100
fprintf('Configuración de depuración activada\n');
```

---

## Comparación: Editor vs Command Window

| Acción | Desde Editor | Desde Command Window |
|--------|--------------|----------------------|
| Poner breakpoint | Clic en margen / F12 | `dbstop in archivo at línea` |
| Poner condicional | Clic derecho → condición | `dbstop in archivo if condición` |
| Eliminar breakpoints | Clic en margen | `dbclear` |
| Ver breakpoints | Visual en margen | `dbstatus` |
| Step Over | F10 | `dbstep` |
| Step Into | F11 | `dbstep in` |
| Step Out | Shift+F11 | `dbstep out` |
| Continue | F5 | `dbcont` |

> [!tip]
> La combinación de ambas interfaces es lo más efectivo: usar el Editor para visualizar y poner breakpoints, y el Command Window para comandos avanzados como `dbstop if error`.

---

## Ejemplo completo: depuración de una función

### Archivo `analisis.m`

```matlab
function resultado = analisis(datos, umbral)
    % ANALISIS Procesa datos con un umbral
    if nargin < 2
        umbral = 0;
    end
    
    resultado = zeros(size(datos));
    
    for i = 1:length(datos)
        if datos(i) > umbral
            resultado(i) = procesar(datos(i));
        else
            resultado(i) = 0;
        end
    end
end

function valor = procesar(x)
    % PROCESAR Transformación compleja
    valor = sqrt(x) * exp(-x/10);
end
```

### Sesión de depuración

```matlab
% Activar detención en errores
>> dbstop if error

% Activar breakpoint condicional
>> dbstop in analisis at 8 if datos(i) > 100

% Ejecutar
>> datos = [10, 50, 200, 5, 150];
>> resultado = analisis(datos, 20)

% MATLAB se detiene cuando datos(i) > 100
% En i = 3, datos(3) = 200

% Inspeccionar
>> i
i = 3

>> datos(i)
ans = 200

% Ver pila de llamadas
>> dbstack
> In analisis (line 8)
  In command window

% Avanzar paso a paso dentro de procesar
>> dbstep in

% Ver el valor calculado
>> valor
valor = 14.1421 * exp(-0.2)  % aproximado

% Continuar
>> dbcont

% Limpiar breakpoints al terminar
>> dbclear all
>> dbclear if error
```

---

## Comandos de depuración: resumen

| Comando | Función |
|---------|---------|
| `dbstop in archivo at línea` | Poner breakpoint |
| `dbstop in archivo if condicion` | Breakpoint condicional |
| `dbstop if error` | Detener en errores |
| `dbstop if warning` | Detener en warnings |
| `dbcont` | Continuar ejecución |
| `dbstep` | Avanzar una línea (Step Over) |
| `dbstep in` | Entrar en función (Step Into) |
| `dbstep out` | Salir de función (Step Out) |
| `dbstack` | Mostrar pila de llamadas |
| `dbstatus` | Ver breakpoints activos |
| `dbclear` | Eliminar breakpoints |
| `dbquit` | Salir del modo depuración |

---

## Buenas prácticas

| Práctica | Descripción |
|----------|-------------|
| **Activar `dbstop if error` por defecto** | Especialmente al desarrollar código nuevo |
| **Usar breakpoints condicionales en bucles** | Evita detenerse innecesariamente |
| **Limpiar breakpoints después** | `dbclear all` para no olvidar breakpoints en producción |
| **Combinar Editor y Command Window** | Lo visual y lo programable se complementan |
| **Documentar breakpoints temporales** | Si se usan en scripts compartidos, notificar a otros |

---

## Notas relacionadas

- [[Puntos de interrupción]]
- [[Atajos de teclado esenciales]]
- [[Editor, Command Window, Workspace, Current Folder]]
- [[Manejo de errores (try, catch)]]
- [[Scripts]]
- [[Funciones en archivo]]
