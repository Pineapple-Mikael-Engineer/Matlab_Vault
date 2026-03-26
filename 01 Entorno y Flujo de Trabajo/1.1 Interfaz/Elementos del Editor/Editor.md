---
title: "Editor"
tags:
  - MATLAB
  - interfaz
  - editor
  - entorno
  - flujo-de-trabajo
aliases:
  - "Editor MATLAB"
---

# Editor

Es el espacio principal donde escribes, editas y guardas el código en MATLAB.

## ¿Qué puedes crear aquí?
- **Scripts** (`.m`): Secuencias de comandos lineales que comparten el [[Workspace]] con el [[Command Window]].
- **Funciones** (`.m`): Código con entrada/salida definida y workspace propio. Ver [[Scripts]].
- **Clases** (`.m`): Definiciones de programación orientada a objetos.
- **Live Scripts** (`.mlx`): Documentos interactivos que combinan código, texto, ecuaciones y gráficos. Ver archivos `.m`, `.mat`, `.mlx`.

## Características principales
- **Resaltado de sintaxis**: Colores diferenciados para variables, funciones, comentarios, strings, etc.
- **Autocompletado**: Presiona `Tab` para completar variables, funciones o nombres de archivos.
- **Debugging integrado**: Se puede establecer [[Puntos de interrupción|puntos de interrupción]] haciendo clic en el margen izquierdo (aparece un círculo rojo).
- **Ejecución selectiva**:
  - `F5` o botón "Run": ejecuta todo el archivo.
  - `Ctrl+Enter`: ejecuta la selección actual o la línea donde está el cursor. Ver [[Atajos de teclado esenciales en MATLAB|Atajos de teclado esenciales]].
  - `Ctrl+Shift+Enter`: ejecuta la selección y avanza a la siguiente sección.
- **Celdas de código**: Usa `%%` para dividir el código en secciones ejecutables independientemente.
- **Múltiples documentos**: Trabaja con pestañas para alternar entre diferentes archivos.

## Ejemplo práctico

```matlab
%% Sección 1: Generar datos
x = 0:0.1:10;
y = sin(x);

%% Sección 2: Visualizar
plot(x, y);
xlabel('x');
ylabel('sin(x)');
title('Gráfico de seno');
grid on;
```

Puedes ejecutar cada sección por separado con `Ctrl+Enter` mientras el cursor esté dentro de ella.

## Analogía
El Editor es tu **"cuaderno de programación"**: allí escribes el código que luego MATLAB ejecuta, a diferencia del [[Command Window]] donde ejecutas comandos sueltos de forma interactiva.

---
