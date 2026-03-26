---
title: "1.2 Configuración de la ruta de Búsqueda"
tags:
  - MATLAB
  - path
  - índice
  - configuración
  - obsidian
aliases:
  - "Configuración del path"
  - "Ruta de búsqueda MATLAB"
---

# 1.2 Configuración de la ruta de Búsqueda

Esta sección aborda uno de los problemas más frecuentes en MATLAB: “la función existe, pero MATLAB no la encuentra”. El origen suele estar en la relación entre **Current Folder** y **path**.

La ruta de búsqueda define dónde MATLAB mira cuando invocas un script o función por nombre. Si no comprendes este mecanismo, tendrás errores intermitentes, conflictos de nombres y sesiones poco reproducibles.

## Regla mental

- **Current Folder** = prioridad local inmediata.
- **Path** = red de carpetas habilitadas para resolución de nombres.

Si algo falla, revisa primero esos dos niveles.

## Notas núcleo de la sección

### [[addpath y savepath]]

Cobertura programática del path: añadir, quitar, persistir y automatizar configuración.

### [[pathtool]]

Cobertura visual del path desde interfaz, útil para inspección manual y ajustes puntuales.

## Ejemplo práctico

```matlab
% Agregar carpeta temporalmente
addpath('C:/mis_funciones')

% Verificar que se pueda invocar
resultado = miFuncion(10);

% Guardar configuración para sesiones futuras
savepath
```

Sin `savepath`, el ajuste suele perderse al reiniciar MATLAB.

## Tabla comparativa

| Método | Fortalezas | Cuándo usar |
|---|---|---|
| `addpath` / `savepath` | Reproducible, scriptable | Proyectos, automatización, equipos |
| `pathtool` | Visual, rápido para explorar | Ajustes manuales y diagnóstico inicial |

## Comparaciones clave

### Ruta temporal vs persistente

- Temporal: válida durante sesión actual.
- Persistente: se conserva entre sesiones (`savepath`).

### Organización por carpeta activa vs por path

- Carpeta activa es simple para trabajo local inmediato.
- Path evita cambiar de carpeta constantemente, pero exige control.

## Buenas prácticas

1. Usa rutas de proyecto claras y estables.
2. Evita agregar demasiadas carpetas “por si acaso”.
3. Prefiere configuración por script para reproducibilidad.
4. Documenta dependencias de ruta en README o scripts de inicio.
5. Revisa colisiones de nombres entre funciones.

## Errores comunes

- Confiar solo en la carpeta activa sin gestionar `path`.
- Olvidar persistir cambios con `savepath`.
- Sobrecargar path con directorios innecesarios.
- Tener funciones con mismo nombre en carpetas distintas.

## Notas relacionadas

- [[addpath y savepath]]
- [[pathtool]]
- [[Current Folder]]
- [[01 Entorno y Flujo de Trabajo/1.3 Scripts vs funciones/index|Scripts vs funciones]]

## Escenario típico de fallo

Caso común: tienes `procesarDatos.m` en otra carpeta, la llamas desde tu script y MATLAB responde “Undefined function or variable”. Muchas veces el archivo existe y está bien escrito, pero el problema es de visibilidad: MATLAB no lo encuentra porque ni está en Current Folder ni en el `path` activo.

La solución correcta no es copiar y pegar archivos en cualquier lugar, sino diseñar una estructura de proyecto y una política de rutas clara.

## Estrategia recomendada por proyecto

1. Define una carpeta raíz del proyecto.
2. Separa scripts principales de funciones auxiliares.
3. Crea un script de inicio que configure rutas con `addpath`.
4. Si necesitas persistencia global, usa `savepath` con criterio.
5. Documenta la configuración para que otros la reproduzcan.

## Checklist de diagnóstico rápido

- ¿El archivo está realmente en disco?
- ¿Coincide el nombre del archivo con la función principal?
- ¿Estoy en la carpeta correcta?
- ¿La carpeta está en el path?
- ¿Hay conflicto de nombres con otra función?
