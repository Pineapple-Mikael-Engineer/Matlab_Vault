---
title: "Live Scripts (.mlx)"
tags:
  - MATLAB
  - live-scripts
  - mlx
  - documentacion
  - notebook
---

# Live Scripts (.mlx)

Un **live script** es un archivo con extensión `.mlx` que combina código, texto formateado, ecuaciones, gráficos y resultados en un único documento interactivo. Funciona como un cuaderno (notebook) estilo Jupyter dentro de MATLAB.

---

## Diferencias con los scripts tradicionales

| Característica | [[Scripts]] (.m) | Live Script (.mlx) |
|----------------|------------------|-------------------|
| **Formato** | Texto plano | Documento interactivo (XML/HDF5) |
| **Código + texto** | Solo código (comentarios como texto) | Código y texto enriquecido integrados |
| **Resultados** | Aparecen en Command Window | Aparecen junto al código que los genera |
| **Gráficos** | Ventanas separadas | Embebidos en el documento |
| **Ecuaciones** | Solo como comentarios | LaTeX renderizado |
| **Exportación** | No aplica | HTML, PDF, LaTeX, Word, Markdown |
| **Interactividad** | No | Controles interactivos (sliders, botones) |

---

## ¿Para qué sirven los live scripts?

| Uso | Descripción |
|-----|-------------|
| **Enseñanza y aprendizaje** | Explicar conceptos con texto y código juntos |
| **Reportes y documentación** | Generar informes ejecutables que muestran código y resultados |
| **Análisis exploratorio** | Probar ideas documentando el proceso paso a paso |
| **Presentaciones** | Exportar a HTML o PDF para compartir |

---

## Estructura de un live script

Un live script se organiza en **bloques** que pueden ser de dos tipos:

### Bloque de código

Contiene código MATLAB ejecutable.

```matlab
x = linspace(0, 10, 100);
y = sin(x);
plot(x, y)
```

Al ejecutar, el resultado aparece debajo del bloque.

### Bloque de texto

Contiene texto formateado con:
- Encabezados (`# Título`, `## Subtítulo`)
- Listas (ordenadas y desordenadas)
- Negritas, cursivas, etc.
- Ecuaciones LaTeX: `$y = \sin(x)$`
- Imágenes y enlaces

---

## Ecuaciones LaTeX

Las ecuaciones se escriben entre símbolos `$`:

- Ecuación en línea: `$E = mc^2$` → $E = mc^2$
- Ecuación en bloque:

```latex
$$
\int_{0}^{\pi} \sin(x) \, dx = 2
$$
```

---

## Controles interactivos

Los live scripts permiten agregar controles que modifican parámetros sin editar el código directamente.

| Control | Ejemplo |
|---------|---------|
| **Slider numérico** | `f = 2;` con slider de 0 a 10 |
| **Lista desplegable** | `tipo = 'seno';` con opciones |
| **Botón** | Ejecutar sección específica |

```matlab
% Al añadir un control, MATLAB genera código como:
f = 2;  % slider from 0 to 10
A = 1;  % slider from 0 to 5

t = 0:0.01:10;
y = A * sin(2 * pi * f * t);
plot(t, y)
```

---

## Exportación

Los live scripts se pueden exportar a múltiples formatos:

| Formato | Comando | Uso |
|---------|---------|-----|
| **HTML** | `export('mi_script.mlx', 'mi_documento.html')` | Compartir en navegador |
| **PDF** | Desde el menú "Save As" → PDF | Informes impresos |
| **LaTeX** | Desde el menú "Export to LaTeX" | Publicaciones académicas |
| **Word** | Desde el menú "Export to Word" | Documentos editables |
| **Markdown** | Desde el menú "Export to Markdown" | Wikis o documentación |

---

## Flujo de trabajo típico

1. **Crear live script**: Archivo → New → Live Script
2. **Estructurar**: Alternar entre bloques de texto y código
3. **Explicar**: Usar texto para describir qué hace cada paso
4. **Ejecutar**: Ejecutar bloques individuales o el documento completo
5. **Refinar**: Ajustar parámetros con controles interactivos
6. **Exportar**: Compartir como HTML o PDF

---

## Ejemplo completo

```matlab
%% Análisis de señal senoidal
% Este live script genera y analiza una señal senoidal.
% El usuario puede ajustar frecuencia y amplitud.

% Parámetros (controles interactivos)
f = 2;      % slider from 0.5 to 10
A = 1.5;    % slider from 0.1 to 5
t = 0:0.01:2;

%% Generar señal
% La señal se define como:
% 
% $$y(t) = A \cdot \sin(2\pi f t)$$
y = A * sin(2 * pi * f * t);

%% Visualización
plot(t, y, 'b-', 'LineWidth', 1.5)
xlabel('Tiempo (s)')
ylabel('Amplitud')
title('Señal senoidal')
grid on

%% Análisis
% Valor máximo de la señal:
max_valor = max(y);
fprintf('Valor máximo: %.2f\n', max_valor)
```

---

## Ventajas y desventajas

| Ventajas | Desventajas |
|----------|-------------|
| Documentación integrada (código + explicación) | No es compatible con control de versiones (diff difícil) |
| Resultados visibles junto al código | Más pesados que scripts `.m` |
| Ideal para enseñanza y reportes | No se pueden ejecutar desde Command Window directamente |
| Controles interactivos sin GUI compleja | Algunas funcionalidades requieren versión reciente |
| Exportación a múltiples formatos | No todos los usuarios usan Live Editor |

---

## Live Scripts vs Funciones en documentación

| Uso | Recomendación |
|-----|---------------|
| **Explicar un concepto** | Live Script |
| **Reporte ejecutable** | Live Script |
| **Biblioteca de funciones** | Scripts `.m` y funciones |
| **Automatización** | Scripts `.m` o funciones |
| **Compartir con no-programadores** | Live Script exportado a HTML |

---

## Notas relacionadas

- [[Scripts]]
- [[index]]
- [[Atajos de teclado esenciales en MATLAB|Atajos de teclado esenciales]]
- [[01 Entorno y Flujo de Trabajo/1.1 Interfaz/Elementos del Editor/index|Editor, Command Window, Workspace, Current Folder]]
- publicación y documentación
