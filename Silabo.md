---
aliases:
  - Temario Programación Matlab
title: Silabo
draft: true
---
# Silabo
- Chat deepseek: [deepseek](https://chat.deepseek.com/a/chat/s/4b297f7d-db80-4aab-a7b1-f4ac3948de20)

## 1. Entorno y flujo de trabajo
- **1.1. La interfaz (Editor, Command Window, Workspace, Current Folder)**
  - Atajos de teclado esenciales (`Ctrl+Enter`, `F5`, `Ctrl+C`, `Tab` para autocompletar).
  - Configuración de la ruta de búsqueda (`addpath`, `savepath`, `pathtool`).
- **1.2. Scripts vs. funciones**
  - Scripts: ejecución lineal, comparten workspace.
  - Funciones: scope propio, entrada/salida definida.
- **1.3. Depuración**
  - Puntos de interrupción (breakpoints), `dbstop`, `dbstep`, `dbcont`.
  - Modo pausa y análisis de variables.
- **1.4. Publicación y documentación**
  - Celdas (`%%`), publicar a HTML/PDF.
  - Comentarios con `%` y bloques de ayuda (`help`, `doc`).

## 2. Tipos de datos y estructuras fundamentales
- **2.1. Matrices y arreglos**
  - Creación: corchetes, operador `:`, `linspace`, `logspace`.
  - Indexación lineal y por subíndices, `end`, indexación lógica.
  - Operaciones elemento a elemento (`.*`, `./`, `.^`) vs. matriciales (`*`, `\`, `/`, `^`).
- **2.2. Cadenas y caracteres**
  - Arreglos de char vs. string (`''` vs `""`), `string()`, `char()`.
  - Funciones: `strcat`, `split`, `join`, `contains`, `replace`.
- **2.3. Arreglos de celdas (`cell`)**
  - Creación con `{}`, indexación con `()` vs `{}`.
  - Almacenar datos heterogéneos.
- **2.4. Tablas (`table`)**
  - Creación desde variables, lectura de archivos (`readtable`).
  - Operaciones: `sortrows`, `unique`, `join`, `groupsummary`, acceso por nombre de columna.

---

## 3. Entrada/salida de datos
- **3.1. Archivos de texto**
  - `readtable`, `writetable` (CSV, TXT, etc.).
- **3.2. Archivos binarios propios**
  - `save`/`load` (formato `.mat`).

---

## 4. Control de flujo y manejo de código
- **4.1. Condicionales y bucles**
  - `if`, `elseif`, `else`, `switch`, `case`.
  - `for`, `while`, `break`, `continue`.
  - Preasignar arreglos (evitar crecimiento dinámico).
- **4.2. Manejo de errores**
  - `try`, `catch`.

---

## 5. Visualización y gráficos
- **5.1. Gráficos 2D**
  - `plot`, `scatter`, `bar`, `histogram`.
  - Personalización: `xlabel`, `title`, `legend`, `grid`, `subplot`.
- **5.2. Gráficos 3D**
  - `plot3`, `surf`, `mesh`, `contour`.
  - `colormap`, `colorbar`.
- **5.3. Exportación**
  - `saveas`, `exportgraphics`.

---

## 6. Álgebra lineal y matemática numérica (core del curso)
- **6.1. Operaciones matriciales**
  - Resolución de sistemas lineales: `\` (mldivide).
  - Autovalores y autovectores: `eig`.
  - Normas: `norm`.
- **6.2. Funciones matemáticas**
  - Trigonométricas, exponenciales, logaritmos.
  - Redondeo: `round`, `floor`, `ceil`.
- **6.3. Cálculo numérico**
  - Derivación: `diff`.
  - Integración: `trapz`, `integral`.
  - Ecuaciones diferenciales: `ode45`.
- **6.4. Ajuste de curvas**
  - `polyfit`.

---


## 7. Correspondencia laboratorios → temas esenciales

| Laboratorio | Tema | Temas del árbol que aplicarás |
|-------------|------|------------------------------|
| 1 | Introducción a MATLAB | 2.1 (matrices), 5.1 (gráficos básicos) |
| 2 | Errores y aritmética | 4.1 (bucles), `abs`, `fprintf` |
| 3 | Sistemas lineales - directos | 6.1 (`\`, `lu`), 4.1 |
| 4 | Sistemas lineales - iterativos | 4.1 (bucles), 6.1 (normas), criterios de parada |
| 5 | No lineales (1 variable) | 4.1 (bucles), implementación de bisección, Newton |
| 6 | No lineales (varias variables) | 4.1, Newton multivariable (usando `\` para resolver Jacobiano) |
| 7 | Aproximación de funciones | 2.1, `polyfit`, `spline`, implementación de Lagrange/Newton |
| 8 | Diferenciación/Integración | `diff`, `trapz`, implementación de Simpson |
| 9 | EDOs - PVI | 6.3 (`ode45`), implementación de Euler, Runge-Kutta |
| 10 | EDOs - PVF | diferencias finitas (matrices con `\`), método del disparo (`ode45` + búsqueda de raíz) |

# Ramificaciones:

## capitulo 1
chat: [deepseek](https://chat.deepseek.com/a/chat/s/6cef02df-f127-4a5d-bee0-4dff9e5b4289)

```bash
📁 01 Entorno y flujo de trabajo/
│
├── 📁 1.1 La interfaz/
│   ├── Editor, Command Window, Workspace, Current Folder.md
│   └── Atajos de teclado esenciales.md
│
├── 📁 1.2 Configuración de la ruta de búsqueda/
│   ├── addpath y savepath.md
│   └── pathtool.md
│
├── 📁 1.3 Scripts vs. funciones/
│   ├── Scripts.md
│   ├── Live Scripts (.mlx).md
│   └── 📁 Funciones/
│       ├── index.md
│       ├── Funciones en archivo.md
│       ├── Funciones anónimas.md
│       ├── Funciones locales y privadas.md
│       ├── Argumentos variables (varargin, varargout).md
│       ├── Validación de argumentos (arguments block).md
│       └── Funciones anidadas.md
│
├── 📁 1.4 Depuración/
│   ├── Puntos de interrupción (breakpoints).md
│   ├── dbstop, dbstep, dbcont.md
│   └── Modo pausa y análisis de variables.md
│
├── 📁 1.5 Publicación y documentación/
│   ├── Celdas (%%) y publicar a HTML-PDF.md
│   └── Comentarios con % y bloques de ayuda (help, doc).md
│
└── Flujo de trabajo integrado.md
```

