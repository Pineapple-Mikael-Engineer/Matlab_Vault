---
title: "pathtool — Interfaz gráfica del path"
tags:
  - MATLAB
  - path
  - pathtool
  - interfaz-grafica
  - GUI
  - herramientas
aliases:
  - "Set Path (ventana)"
  - "GUI del path"
  - "Administrador de rutas"
---

# `pathtool` — Interfaz gráfica del path

`pathtool` abre una ventana gráfica llamada **"Set Path"** que permite gestionar las rutas de búsqueda de MATLAB sin necesidad de escribir comandos.

```matlab
pathtool
```

---

## ¿Qué se puede hacer desde esta ventana?

| Acción | Descripción |
|--------|-------------|
| **Ver todas las rutas** | Lista completa de carpetas en el path actual |
| **Agregar carpetas** | Botón "Add Folder..." para añadir una carpeta específica |
| **Agregar con subcarpetas** | Botón "Add with Subfolders..." para añadir una carpeta y toda su jerarquía (equivalente a `addpath(genpath(...))`) |
| **Quitar carpetas** | Seleccionar una o varias carpetas y presionar "Remove" |
| **Reordenar prioridad** | Botones "Move Up" / "Move Down" para cambiar el orden de búsqueda (las carpetas arriba tienen prioridad) |
| **Guardar cambios** | Botón "Save" (equivalente a `savepath`) |
| **Revertir cambios** | Botón "Revert" para volver a la configuración guardada anteriormente |
| **Cerrar sin guardar** | Botón "Close" |

---

## Estructura de la ventana

```
┌─────────────────────────────────────────────────────────┐
│  MATLAB Path (Set Path)                                 │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Carpetas en el path:                           │   │
│  │  C:\Program Files\MATLAB\R2023a\toolbox\matlab  │   │
│  │  C:\Users\usuario\Documentos\MATLAB             │   │
│  │  D:\proyectos\mis_funciones                     │   │
│  │  ...                                            │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  [Add Folder...]  [Add with Subfolders...]  [Remove]   │
│                                                         │
│  [Move Up]  [Move Down]                                 │
│                                                         │
│  [Save]  [Revert]  [Close]  [Help]                     │
└─────────────────────────────────────────────────────────┘
```

---

## ¿Cuándo usar `pathtool`?

| Situación | Por qué |
|-----------|---------|
| **Exploración visual** | Ver todas las rutas de una vez, entender el orden de prioridad |
| **Usuarios ocasionales** | No recordar comandos como `addpath(genpath(...))` |
| **Configuración inicial** | Configurar el path permanente después de instalar nuevas toolboxes |
| **Depuración de conflictos** | Visualizar qué carpetas están en el path y en qué orden |

---

## Equivalencia con comandos

| Acción en pathtool | Comando equivalente |
|--------------------|---------------------|
| Add Folder... | `addpath('carpeta')` |
| Add with Subfolders... | `addpath(genpath('carpeta'))` |
| Remove | `rmpath('carpeta')` |
| Save | `savepath` |
| Move Up / Move Down | No tiene equivalente directo en comandos (manejo de orden) |

---

## Limitaciones

- **No es programable**: No se puede automatizar ni incluir en scripts
- **Manual**: Cada cambio requiere interacción del usuario
- **Sesión actual**: Los cambios solo afectan la sesión actual hasta que se presione "Save"

---

## Flujo típico con pathtool

1. Ejecutar `pathtool`
2. Agregar carpetas necesarias con los botones correspondientes
3. Reordenar prioridad si es necesario (arrastrar o usar botones)
4. Presionar "Save" para guardar permanentemente
5. Cerrar la ventana

---

## Notas relacionadas

- [[Configuración de la ruta de búsqueda]]
- [[addpath y savepath]]
- [[Editor, Command Window, Workspace, Current Folder]]
