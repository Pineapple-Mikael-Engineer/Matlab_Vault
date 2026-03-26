---
title: "Configuración de la ruta de búsqueda (Path)"
tags:
  - MATLAB
  - path
  - addpath
  - savepath
  - entorno
  - flujo-de-trabajo
  - organización
aliases:
  - "Ruta de búsqueda MATLAB"
  - "MATLAB path"
  - "Configurar path"
---

# ¿Qué es el _path_ en MATLAB?

Antes de los comandos, es necesario tener claro este concepto:

👉 El **path** es la lista de carpetas donde MATLAB busca archivos:

- [[Scripts vs Funciones|Scripts]] (`.m`)
- [[Scripts vs Funciones|Funciones]] (`.m`)
- Datos (`.mat`, `.txt`, etc.)
- Clases (`.m`)
- Modelos de Simulink (`.slx`)

**Regla fundamental:** Si un archivo no está en el path, MATLAB no lo encuentra.

---

## Orden de búsqueda de MATLAB

Cuando se ejecuta un comando, MATLAB busca en este orden:

| Orden | Ubicación | Descripción |
|-------|-----------|-------------|
| 1 | **Current Folder** | La carpeta activa donde se está trabajando. Ver [[Editor, Command Window, Workspace, Current Folder#Current Folder]] |
| 2 | **Path** | Todas las carpetas agregadas con `addpath` |
| 3 | **Built-in functions** | Funciones propias de MATLAB (como `plot`, `sin`, etc.) |

**Importante:** Esto puede causar conflictos si existen funciones con el mismo nombre que una función nativa de MATLAB.

---

# `addpath` — agregar rutas (temporalmente)

## 🔹 ¿Qué hace?

Agrega una carpeta al path durante la sesión actual.

```matlab
addpath('/home/usuario/mis_funciones')
```

MATLAB puede usar cualquier `.m` dentro de esa carpeta.

---

## Ejemplo real

Estructura de proyecto:

```
/proyecto
   main.m
   /utils
      funcion_util.m
```

Desde `main.m`, al ejecutar:

```matlab
funcion_util()
```

Se produce un error porque MATLAB no encuentra la función en `utils`.

**Solución:**
```matlab
addpath('utils')
```

La función ahora es accesible.

---

## 🔹 Agregar subcarpetas automáticamente

```matlab
addpath(genpath('mi_carpeta'))
```

`genpath` genera una lista con:
- La carpeta principal
- **TODAS** las subcarpetas en cualquier nivel de profundidad

Útil en proyectos grandes con estructura jerárquica.

---

## 🔹 Múltiples rutas a la vez

```matlab
addpath('carpeta1', 'carpeta2', 'carpeta3')
```

O usando un arreglo de celdas:
```matlab
rutas = {'utils', 'lib', 'datos'};
addpath(rutas{:})
```

---

## Consideraciones

- `addpath` **no guarda cambios permanentemente**
- Al cerrar MATLAB, las rutas agregadas se pierden
- Las rutas se agregan al **inicio** del path por defecto (prioridad máxima). Para agregar al final: `addpath('carpeta', '-end')`

---

# `savepath` — guardar cambios permanentemente

## 🔹 ¿Qué hace?

Guarda la configuración actual del path para futuras sesiones de MATLAB.

```matlab
savepath
```

Lo agregado con `addpath` (o eliminado con `rmpath`) persiste después de cerrar MATLAB.

---

## 🔹 Flujo típico

```matlab
addpath('/home/usuario/mis_funciones')
savepath
```

La ruta queda guardada permanentemente.

---

## Problemas comunes

### “Permission denied” / No se puede guardar

MATLAB no tiene permisos para escribir en el archivo de configuración del path (`pathdef.m`).

**Soluciones:**
1. Ejecutar MATLAB como administrador (Windows) o con `sudo` (Linux/Mac)
2. Guardar manualmente en una ruta alternativa: `savepath('/ruta/mi_pathdef.m')`

---

# `pathtool` — interfaz gráfica

```matlab
pathtool
```

Abre una ventana gráfica donde se puede:
- Ver todas las rutas actuales
- Agregar carpetas con botones
- Quitar carpetas
- Reordenar la prioridad de búsqueda
- Guardar cambios (botón "Save" = `savepath`)

Ideal para quienes prefieren no usar comandos o necesitan visualizar el orden de búsqueda.

---

# Otros comandos relacionados

## 🔹 `path` — ver rutas actuales

```matlab
path
```

Muestra TODAS las rutas activas en formato de lista.

## 🔹 `rmpath` — quitar rutas

```matlab
rmpath('C:\mis_funciones')
```

Elimina una carpeta del path. También se puede usar `rmpath(genpath('carpeta'))` para quitar una carpeta y todas sus subcarpetas.

## 🔹 `which` — localizar un archivo

```matlab
which plot
which mi_funcion
```

Indica **dónde** se encuentra el archivo que MATLAB está utilizando. Esencial para depurar conflictos de nombres.

## 🔹 `rehash` — refrescar caché

```matlab
rehash
rehash path
```

Si se agregan archivos nuevos a una carpeta que ya estaba en el path, MATLAB puede no verlos inmediatamente. `rehash` actualiza el caché.

---

# Problemas comunes

## 1. Función no reconocida

```
Undefined function or variable 'mi_funcion'
```

-  **Solución:** `addpath('carpeta_donde_esta_la_funcion')`

## 2. Conflicto de nombres

Existe un archivo `plot.m` propio en alguna carpeta, pero se desea usar la función nativa de MATLAB.

-  **Solución:** Usar `which plot` para ver cuál está usando. Para evitar conflictos, **no usar nombres iguales a funciones nativas**.

## 3. Proyecto desordenado

Sin `addpath(genpath(...))`, es necesario agregar carpetas una por una.

-  **Solución:** Usar `genpath` o mantener una estructura bien organizada desde el inicio.

## 4. Cambios no persistentes

Rutas agregadas ayer ya no están disponibles hoy.

-  **Solución:** Ejecutar `savepath` después de `addpath` si se desea persistencia.

---

# Buenas prácticas

## 1. Script de inicialización

Crear un archivo `setup.m` en la raíz del proyecto:

```matlab
% setup.m
% Ejecutar una vez al abrir el proyecto

addpath(genpath('src'))
addpath(genpath('utils'))
addpath('datos')

fprintf('Rutas del proyecto configuradas correctamente\n')
```

Se ejecuta al iniciar el proyecto. **No** usar `savepath` con rutas de proyectos específicos.

## 2. No contaminar el path global

- Evitar `savepath` con rutas de proyectos temporales
- Preferir scripts de inicialización (`setup.m`) por proyecto
- Reservar `savepath` solo para rutas de uso **constante** (bibliotecas comunes)

## 3. Usar rutas relativas

```matlab
addpath('utils')                     % Relativa a Current Folder
addpath(fullfile(pwd, 'lib'))        % Alternativa explícita
```

-  Preferible sobre rutas absolutas:

```matlab
addpath('C:\Users\alumno\proyecto\utils')  % No portable
```

## 4. Verificar el path antes de ejecutar

```matlab
path
which mi_funcion  % Confirmar que MATLAB encuentra la función
```

---

# Resumen rápido

| Comando | Función | Persistencia |
|---------|---------|--------------|
| `addpath` | Agregar carpeta al path | Temporal (sesión actual) |
| `savepath` | Guardar configuración actual | Permanente |
| `rmpath` | Eliminar carpeta del path | Depende de `savepath` |
| `genpath` | Genera lista con subcarpetas | Solo genera texto |
| `pathtool` | Interfaz gráfica | Manual |
| `path` | Ver rutas actuales | - |
| `which` | Localizar archivo | - |

---

# Analogía

- `addpath` = “MATLAB, mirar también aquí”
- `savepath` = “y recordar para siempre”
- `genpath` = “y mirar también dentro de todas las subcarpetas”
- `pathtool` = “prefiero usar botones”

---

## Notas relacionadas

- [[Editor, Command Window, Workspace, Current Folder#Current Folder]]
- [[Scripts vs Funciones]]
- [[Archivos .m .mat .mlx]]
- [[Flujo de trabajo integrado]]

