---
title: "Puntos de interrupción (breakpoints)"
tags:
  - MATLAB
  - debug
  - breakpoints
  - depuración
  - editor
aliases:
  - "Breakpoints"
  - "Puntos de pausa"
---

# Puntos de interrupción (breakpoints)

Un **breakpoint** (punto de interrupción) es una marca en el código que indica a MATLAB dónde detener la ejecución. Al llegar a un breakpoint, el programa se pausa y permite inspeccionar variables, ejecutar comandos y avanzar paso a paso.

---

## Tipos de breakpoints

### Breakpoint normal

Se detiene siempre al llegar a la línea marcada.

**Cómo ponerlo:**
- Hacer clic en el margen izquierdo del [[Editor]] (aparece un círculo rojo)
- Presionar `F12` sobre la línea deseada

### Breakpoint condicional

Solo se activa si se cumple una condición específica.

**Cómo ponerlo:**
- Clic derecho en el margen → "Set Conditional Breakpoint"
- Ingresar la condición, por ejemplo: `x > 10` o `i == 523`

**Usos típicos:**
- Bucles largos donde el error ocurre en una iteración específica
- Detenerse solo cuando una variable alcanza cierto valor

### Breakpoint por error (dbstop if error)

MATLAB se detiene automáticamente cuando ocurre un error, en lugar de mostrar el mensaje de error y continuar.

```matlab
dbstop if error
```

**Variantes:**
```matlab
dbstop if warning   % Se detiene ante cualquier warning
dbstop if naninf    % Se detiene si aparece NaN o Inf
```

> [!tip]
> Activar `dbstop if error` al inicio de una sesión de depuración es una de las prácticas más útiles.

---

## ¿Qué sucede cuando se alcanza un breakpoint?

Al llegar a un breakpoint:

1. La línea se resalta en **amarillo** en el Editor
2. La ejecución se **pausa**
3. El [[Editor, Command Window, Workspace, Current Folder#Workspace|Workspace]] muestra todas las variables actuales
4. El [[Editor, Command Window, Workspace, Current Folder#Command Window|Command Window]] queda disponible para comandos interactivos
5. Aparecen botones de control de depuración en la barra de herramientas

---

## Inspección de variables

Mientras la ejecución está pausada, se puede:

```matlab
x                    % Ver valor de x
A(2,3)               % Ver elemento específico
size(A)              % Ver dimensiones
whos                 % Listar todas las variables
```

Desde la ventana Workspace también se puede:
- Ver arreglos completos (doble clic)
- Editar valores manualmente (cambiar para probar hipótesis)

---

## Breakpoints en bucles

En un bucle largo:

```matlab
for i = 1:1000
    x = procesar(i);
    resultado(i) = calcular(x);
end
```

No se desea detenerse en cada iteración. Soluciones:

| Estrategia | Descripción |
|------------|-------------|
| **Breakpoint condicional** | Poner condición `i == 523` para detenerse solo en esa iteración |
| **Breakpoint dentro del bucle** | Poner breakpoint en `resultado(i) = calcular(x)` y usar Step Over para avanzar iteración por iteración |

---

## Breakpoints en funciones

Se pueden poner breakpoints en:
- [[Scripts]]
- [[Funciones en archivo]]
- [[Funciones locales y privadas|Funciones locales]]
- [[Funciones Anidadas]]

```matlab
function y = cuadrado(x)
    y = x^2;   % Breakpoint aquí
end
```

Cuando se alcanza un breakpoint dentro de una función:
- El workspace muestra las variables locales de esa función
- `dbstack` muestra la pila de llamadas (qué función llamó a esta)

---

## Gestión de breakpoints

### Desde el Editor
- **Clic en el margen**: Activa/desactiva breakpoint
- **Clic derecho → Disable Breakpoint**: Desactiva temporalmente (mantiene la marca)

### Desde el Command Window
```matlab
dbclear all              % Eliminar todos los breakpoints
dbclear in archivo.m     % Eliminar breakpoints de un archivo
dbstatus                 % Listar todos los breakpoints activos
```

---

## Buenas prácticas

### Hacer

- Activar `dbstop if error` al inicio de la depuración
- Usar breakpoints condicionales en bucles largos
- Inspeccionar variables antes de modificar código
- Limpiar breakpoints después de depurar (`dbclear all`)

### Evitar

- Poner breakpoints en **todas** las líneas (dificulta el flujo)
- Dejar breakpoints activos en código que se entrega a otros
- Confiar solo en `disp()` para depurar (el debugger es más poderoso)

---

## Alternativa: debug con `disp`

Una alternativa tradicional es usar `disp` para mostrar valores:

```matlab
disp(['x = ', num2str(x)]);
```

**Ventajas del debugger sobre `disp`:**
- No ensucia el código con mensajes temporales
- Es interactivo (se puede probar cambios)
- Permite ver el estado completo, no solo lo que se imprime

---

## Resumen rápido

| Concepto | Descripción |
|----------|-------------|
| **Breakpoint normal** | Se detiene siempre (clic o F12) |
| **Breakpoint condicional** | Se detiene si se cumple condición |
| **dbstop if error** | Se detiene automáticamente en errores |
| **Inspección** | Ver y modificar variables mientras está pausado |
| **dbclear** | Eliminar breakpoints |
| **dbstatus** | Ver breakpoints activos |

---

## Notas relacionadas

- [[Comandos de depuración]]
- [[Editor, Command Window, Workspace, Current Folder]]
- [[Atajos de teclado esenciales]]
- [[Scripts]]
- [[Funciones en archivo]]
