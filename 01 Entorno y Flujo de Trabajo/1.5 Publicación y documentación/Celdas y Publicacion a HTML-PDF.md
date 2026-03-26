---
title: "Celdas (%%) y publicación a HTML/PDF"
tags:
  - MATLAB
  - celdas
  - publish
  - documentacion
  - reportes
  - secciones
  - flujo-de-trabajo
aliases:
  - "Celdas MATLAB"
  - "Secciones de código"
  - "Publish MATLAB"
  - "Publicar código"
---

# Celdas (`%%`) y publicación a HTML/PDF

Las **celdas** (delimitadas por `%%`) permiten dividir un script en secciones ejecutables de forma independiente. Combinadas con la funcionalidad de **publicación** (`publish`), convierten un script en un documento reproducible en formatos como HTML, PDF, Word o LaTeX.

---

## ¿Qué son las celdas?

Una celda es un bloque de código delimitado por `%%`. Cada `%%` define el inicio de una nueva sección.

```matlab
%% Sección 1: Inicialización
a = 5;
b = 10;
c = a + b;

%% Sección 2: Procesamiento
d = c^2;

%% Sección 3: Resultados
fprintf('Resultado: %.2f\n', d);
```

### Características clave

| Característica | Descripción |
|----------------|-------------|
| **Ejecución independiente** | Cada sección se puede ejecutar por separado |
| **Documentación integrada** | Los comentarios se convierten en texto explicativo |
| **Títulos jerárquicos** | `%% Título` genera encabezados en el documento publicado |
| **Base para publicación** | El código con celdas es la entrada para `publish` |

---

## Ejecución de celdas

### Atajos de teclado

| Atajo | Acción |
|-------|--------|
| `Ctrl+Enter` | Ejecutar sección actual |
| `Ctrl+Shift+Enter` | Ejecutar sección actual y avanzar a la siguiente |

Ver también: [[Atajos de teclado esenciales]]

### Navegación entre secciones

MATLAB reconoce automáticamente las secciones y permite:
- Saltar entre ellas desde el editor
- Ver un índice visual en el margen izquierdo

---

## Texto dentro de celdas

Los comentarios (`%`) dentro de una sección se convierten en texto descriptivo al publicar.

```matlab
%% Cálculo de energía potencial
% Este bloque calcula la energía potencial gravitatoria.
% La fórmula utilizada es: E = m * g * h
%
% Parámetros:
%   m: masa (kg)
%   g: aceleración gravitatoria (9.81 m/s²)
%   h: altura (m)

m = 10;
g = 9.81;
h = 5;
E = m * g * h;

fprintf('Energía potencial: %.2f J\n', E);
```

### Tipos de contenido en celdas

| Elemento | Sintaxis | Resultado en publicación |
|----------|----------|--------------------------|
| **Encabezado** | `%% Título` | Título de sección |
| **Párrafo** | `% Texto` | Párrafo normal |
| **Código** | `x = 5;` | Bloque de código (si `showCode` es true) |
| **Resultados** | Salida de comandos | Output visible en el documento |

---

## Publicación (`publish`)

La función `publish` toma un script con celdas y genera un documento en el formato especificado.

### Sintaxis básica

```matlab
publish('archivo.m')
```

### Formatos disponibles

| Formato | Comando | Uso típico |
|---------|---------|------------|
| **HTML** | `publish('archivo.m', 'html')` | Documentos web, compartir fácilmente |
| **PDF** | `publish('archivo.m', 'pdf')` | Informes impresos, entregas formales |
| **Word (DOCX)** | `publish('archivo.m', 'docx')` | Documentos editables |
| **LaTeX** | `publish('archivo.m', 'latex')` | Publicaciones académicas |

### Opciones de publicación

```matlab
options = struct();
options.format = 'pdf';
options.outputDir = 'docs';
options.showCode = true;      % Mostrar código en el documento
options.evalCode = true;      % Ejecutar código al publicar
options.createThumbnail = true; % Crear miniatura de la figura principal

publish('analisis.m', options);
```

### Opciones principales

| Opción | Descripción | Valores típicos |
|--------|-------------|-----------------|
| `format` | Formato de salida | `'html'`, `'pdf'`, `'docx'`, `'latex'` |
| `outputDir` | Carpeta donde guardar | `'docs'`, `'informes'`, `'./output'` |
| `showCode` | Mostrar código en el documento | `true`, `false` |
| `evalCode` | Ejecutar código al publicar | `true`, `false` |
| `figureSnapMethod` | Método para capturar figuras | `'print'`, `'getframe'` |

---

## Estructura del documento publicado

MATLAB interpreta los elementos del script de la siguiente manera:

| En el script | En el documento publicado |
|--------------|---------------------------|
| `%% Título principal` | Encabezado de nivel 1 (`<h1>`) |
| `%% Subtítulo` | Encabezado de nivel 2 (`<h2>`) |
| `% Texto explicativo` | Párrafo |
| Código MATLAB | Bloque de código con resaltado de sintaxis |
| Salida de comandos | Output formateado |
| Figuras (`plot`, `figure`) | Imágenes embebidas |

### Ejemplo de script publicable

```matlab
%% Análisis de señal senoidal
% Este script genera y analiza una señal senoidal.
% La frecuencia y amplitud pueden modificarse en la sección de parámetros.

%% Parámetros
% Configuración de la señal
frecuencia = 2;   % Hz
amplitud = 1.5;   % unidades
duracion = 2;     % segundos

%% Generación de la señal
% Se crea un vector de tiempo y la señal senoidal correspondiente
t = linspace(0, duracion, 1000);
y = amplitud * sin(2 * pi * frecuencia * t);

%% Visualización
% Gráfica de la señal generada
figure('Position', [100, 100, 600, 400]);
plot(t, y, 'b-', 'LineWidth', 1.5);
xlabel('Tiempo (s)');
ylabel('Amplitud');
title('Señal senoidal');
grid on;

%% Estadísticas básicas
% Cálculo de valores característicos
valor_maximo = max(y);
valor_minimo = min(y);
valor_medio = mean(y);

fprintf('Máximo: %.2f\n', valor_maximo);
fprintf('Mínimo: %.2f\n', valor_minimo);
fprintf('Media: %.2f\n', valor_medio);
```

---

## Generación de PDF

MATLAB genera PDF a través de una conversión intermedia a HTML.

### Requisitos

- Navegador web configurado (para la conversión HTML → PDF)
- En sistemas sin interfaz gráfica, puede requerir configuración adicional

### Problemas comunes y soluciones

| Problema | Solución |
|----------|----------|
| PDF mal formateado | Ajustar `options.figureSnapMethod = 'print'` |
| Figuras fuera de escala | Especificar tamaño de figura con `figure('Position', [x, y, ancho, alto])` |
| Error al generar PDF | Verificar que el navegador predeterminado está configurado |
| Caracteres especiales no se muestran | Usar codificación UTF-8 en comentarios |

---

## Figuras y gráficos en publicación

Los gráficos se incluyen automáticamente en el documento publicado.

### Buenas prácticas para figuras

```matlab
% Configurar tamaño de figura antes de graficar
figure('Position', [100, 100, 800, 500]);

% Generar gráfica
plot(x, y, 'LineWidth', 1.5);

% Agregar elementos de claridad
title('Título descriptivo', 'FontSize', 14);
xlabel('Etiqueta X', 'FontSize', 12);
ylabel('Etiqueta Y', 'FontSize', 12);
legend('Datos', 'Location', 'best');
grid on;
```

> [!tip]
> Las figuras con `Position` bien definido mantienen consistencia entre diferentes formatos de salida.

---

## Scripts con celdas vs Live Scripts

| Característica | Script (.m) con celdas | [[Live Scripts (.mlx)]] |
|----------------|------------------------|-------------------------|
| **Formato** | Texto plano | XML/HDF5 interactivo |
| **Edición** | Editor tradicional | Live Editor visual |
| **Texto enriquecido** | Solo comentarios | Formato completo, ecuaciones LaTeX |
| **Resultados en línea** | No | Sí, junto al código |
| **Controles interactivos** | No | Sí (sliders, botones) |
| **Exportación** | `publish` | Menú "Save As" o `export` |
| **Control de versiones** | Excelente (diff claro) | Difícil (archivo binario) |
| **Uso típico** | Código + documentación básica | Reportes complejos, enseñanza |

### Cuándo usar cada uno

| Situación | Recomendación |
|-----------|---------------|
| Informe académico con código | Script con celdas (control de versiones) |
| Documentación interactiva para enseñanza | Live Script |
| Proyecto con colaboradores que usan Git | Script con celdas |
| Presentación ejecutiva con resultados embebidos | Live Script |
| Publicación automática de resultados periódicos | Script con celdas + `publish` en script |

---

## Buenas prácticas

### Organización de celdas

- **Una idea por celda**: Cada celda debe tener un propósito claro
- **Títulos descriptivos**: `%% Inicialización de parámetros` en lugar de `%% Sección 1`
- **Orden lógico**: Configuración → Cálculo → Visualización → Resultados

### Documentación efectiva

```matlab
%% Cálculo de raíces por método de Newton
% Este método iterativo encuentra raíces de funciones.
% Se detiene cuando |x_nuevo - x_anterior| < tolerancia.
%
% Parámetros:
%   f: función objetivo
%   df: derivada de f
%   x0: valor inicial
%   tol: tolerancia de convergencia
```

### Reproducibilidad

- **Inicializar variables**: No depender del workspace previo
- **Rutas relativas**: Usar `addpath` con rutas relativas
- **Limpiar al inicio**: Incluir `clc; clear; close all;` si es apropiado

```matlab
%% Configuración inicial
clc; clear; close all;
addpath(genpath('utilidades'));
```

---

## Casos de uso reales

| Escenario | Aplicación |
|-----------|------------|
| **Informes de laboratorio** | Script con cálculos, gráficos y conclusiones, publicado a PDF |
| **Tareas de métodos numéricos** | Entregables que muestran código, resultados y análisis |
| **Documentación de algoritmos** | Scripts que explican y demuestran el funcionamiento |
| **Reportes automáticos** | Generación periódica de informes con `publish` en script automatizado |
| **Validación de resultados** | Scripts que reproducen resultados experimentales con documentación |

---

## Limitaciones

| Limitación | Descripción |
|------------|-------------|
| **Formato limitado** | Menos opciones de estilo que Live Scripts |
| **Sin ecuaciones LaTeX** | Las ecuaciones deben estar en comentarios, no se renderizan |
| **Personalización reducida** | Opciones de estilo limitadas comparado con exportación manual |
| **Dependencia del motor** | La generación de PDF depende del navegador configurado |
| **Sin interactividad** | No se pueden agregar controles interactivos (sliders, botones) |

---

## Resumen rápido

| Concepto | Descripción |
|----------|-------------|
| **Celda** | Bloque de código delimitado por `%%` |
| **Ejecutar sección** | `Ctrl+Enter` |
| **Publicar** | `publish('archivo.m', 'pdf')` |
| **Formatos** | HTML, PDF, Word, LaTeX |
| **Texto documental** | Comentarios `%` después del título de celda |
| **Uso principal** | Código reproducible con documentación integrada |

---

## Notas relacionadas

- [[Live Scripts (.mlx)]]
- [[Scripts]]
- [[Atajos de teclado esenciales]]
- [[Editor, Command Window, Workspace, Current Folder]]
- [[Comentarios con % y bloques de ayuda (help, doc)]]
