---
title: "Atajos de teclado esenciales en MATLAB"
tags:
  - MATLAB
  - atajos
  - teclado
  - productividad
  - editor
---

# ⌨️ Atajos de teclado esenciales en MATLAB

Los atajos de teclado son lo que separa a alguien que programa de alguien que *vuela* en MATLAB. Esta nota recopila los esenciales organizados por categorías para desarrollar memoria muscular.

---

## ⚡ Ejecución de código

| Atajo | Función | Cuándo usarlo |
|-------|---------|---------------|
| `F5` | Ejecutar script completo | Cuando ya terminaste el código y quieres probar todo el flujo |
| `Ctrl+Enter` | Ejecutar línea o selección | Probar partes del código, debug rápido. **Tu mejor amigo** |
| `Ctrl+Shift+Enter` | Ejecutar sección (`%%`) | Trabajar por bloques (estilo notebook) |
| `F9` | Ejecutar selección (alternativa) | Similar a `Ctrl+Enter` |

---

## ✍️ Edición y escritura rápida

| Atajo | Función | Ejemplo |
|-------|---------|---------|
| `Tab` | Autocompletar | `plo` + `Tab` → `plot` |
| `Ctrl+R` | Comentar línea/selección | Añade `%` al inicio |
| `Ctrl+T` | Descomentar línea/selección | Elimina `%` del inicio |
| `Ctrl+I` | Auto-indent (ordenar código) | Arregla la indentación automáticamente |

---

## 🔍 Navegación y búsqueda

| Atajo | Función |
|-------|---------|
| `Ctrl+F` | Buscar dentro del archivo |
| `Ctrl+H` | Buscar y reemplazar |
| `Ctrl+G` | Ir a línea específica (útil en archivos largos) |

---

## 🧠 Workspace y entorno (comandos rápidos)

No son atajos de teclado en sentido estricto, pero son "atajos mentales obligatorios" que se escriben rápido en el [[Command Window]]:

```matlab
clc;        % Limpiar Command Window
clear;      % Borrar variables
close all;  % Cerrar todas las gráficas
```

**Combo típico de inicio/limpieza:**
```matlab
clc; clear; close all;
```

---

## 🔄 Historial y reutilización

| Atajo | Función |
|-------|---------|
| `↑` / `↓` | Navegar por comandos anteriores en el [[Command Window]] |

Esto te ahorra MUCHO tiempo al reutilizar comandos que ya escribiste.

---

## 🛠️ Debugging (nivel pro)

| Atajo | Función |
|-------|---------|
| `F12` | Poner/quitar breakpoint (punto de pausa) |
| `F10` | Step Over — ejecuta línea y pasa a la siguiente (no entra en funciones) |
| `F11` | Step Into — entra dentro de una función para depurarla |
| `F5` (en debug) | Continuar ejecución hasta el siguiente breakpoint o final |

Ver también: [[Comandos de depuración|Depuración]]

---

## 🚀 Flujo real usando atajos

Un flujo típico de alguien eficiente:

1. Escribes en el [[Editor]]
2. Usas `Ctrl+Enter` para probar partes del código
3. Usas `↑` para repetir comandos en el Command Window
4. Si algo falla → `F12` para poner breakpoint y depurar con `F10`/`F11`
5. Limpias el entorno con `clc; clear; close all;`
6. Ejecutas todo con `F5`

---

## 🧠 Tips para desarrollar memoria muscular

**Prioridad 1 (aprende estos primero):**
- `Ctrl+Enter` — probar código rápido
- `F5` — ejecutar completo
- `Ctrl+R` / `Ctrl+T` — comentar/descomentar

👉 Con esos ya mejoras ~70% tu velocidad.

**Prioridad 2:**
- `Tab` — autocompletar
- `Ctrl+I` — ordenar código
- `F12`, `F10`, `F11` — debugging

**Buenas prácticas:**
- Usa secciones con `%%` para convertir MATLAB en estilo notebook
- No abuses del mouse — MATLAB se siente lento si no usas atajos

---

## 🧩 Resumen rápido (memoria muscular)

| Categoría | Atajos |
|-----------|--------|
| **Ejecutar** | `F5`, `Ctrl+Enter`, `Ctrl+Shift+Enter` |
| **Editar** | `Tab`, `Ctrl+R/T`, `Ctrl+I` |
| **Buscar** | `Ctrl+F`, `Ctrl+H`, `Ctrl+G` |
| **Debug** | `F12`, `F10`, `F11` |
| **Limpiar** | `clc; clear; close all;` |

---

## Notas relacionadas

- [[01 Entorno y Flujo de Trabajo/1.1 Interfaz/Elementos del Editor/index|Editor, Command Window, Workspace, Current Folder]]
- [[Comandos de depuración|Depuración]]
- [[Scripts]]
