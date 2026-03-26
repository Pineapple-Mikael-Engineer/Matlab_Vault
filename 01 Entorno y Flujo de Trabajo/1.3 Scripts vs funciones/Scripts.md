---
title: "Scripts en MATLAB"
tags:
  - MATLAB
  - scripts
  - m-files
  - flujo-de-trabajo
  - editor
aliases:
  - "Script MATLAB"
  - "Archivo .m"
  - "Scripts"
---

# Scripts en MATLAB

Un **script** es un archivo con extensión `.m` que contiene una serie de instrucciones que MATLAB ejecuta **línea por línea** en orden secuencial.

## Características principales

| Característica | Descripción |
|----------------|-------------|
| **Sin entrada/salida** | No recibe argumentos ni devuelve valores formalmente |
| **Workspace global** | Comparte y opera en el mismo [[Workspace]] que el Command Window |
| **Ejecución secuencial** | Las instrucciones se ejecutan de arriba hacia abajo |
| **Formato texto plano** | Se edita en el [[Editor]] de MATLAB |

---

## Ejemplo básico

```matlab
x = 0:0.1:10;
y = sin(x);
plot(x, y);
title('Gráfica de seno');
```

Se guarda como `mi_script.m` y se ejecuta con `F5` o escribiendo `mi_script` en el [[Command Window]].

---

## Cómo funciona el workspace en scripts

Todas las variables creadas en un script permanecen en el **workspace base** después de la ejecución.

```matlab
% script_ejemplo.m
a = 5;
b = a * 2;
```

Después de ejecutar, se puede inspeccionar `a` y `b` desde el Command Window o la ventana [[Editor, Command Window, Workspace, Current Folder#Workspace|Workspace]].

> [!warning] Riesgo
> Si una variable ya existía en el workspace antes de ejecutar el script, puede ser sobrescrita o utilizada sin haber sido definida dentro del script.

---

## Scripts vs Funciones

| Característica | Script | [[index]] |
|----------------|--------|---------------|
| Entrada formal | No | Sí |
| Salida formal | No | Sí |
| Workspace | Global (base) | Local (propio) |
| Reutilización | Baja | Alta |
| Uso típico | Flujo principal, pruebas | Lógica reutilizable |

**Regla mental:**
- **Script** = el "director" que organiza el flujo
- **Función** = el "trabajador" que ejecuta lógica específica

---

## Estructura recomendada para un script

### 1. Limpieza inicial (opcional pero común)

```matlab
clc;        % Limpia Command Window
clear;      % Elimina variables del workspace
close all;  % Cierra todas las figuras
```

### 2. Secciones de código (`%%`)

```matlab
%% Datos
x = 0:0.1:10;

%% Procesamiento
y = sin(x);

%% Visualización
plot(x, y);
title('Gráfica de seno');
```

Cada sección se puede ejecutar por separado con `Ctrl+Shift+Enter` (ver [[Atajos de teclado esenciales]]).

### 3. Comentarios descriptivos

```matlab
% Calcula el seno de x para generar la gráfica
y = sin(x);
```

---

## Tipos de scripts

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| **Script simple** | Ejecución lineal de código | `analisis_datos.m` |
| **Live Script** | Formato interactivo con texto y gráficos embebidos | `informe.mlx` → ver [[Live Scripts]] |
| **Script de inicialización** | Configura el entorno del proyecto | `setup.m` con `addpath` |

---

## Problemas comunes

### 1. Variables "fantasma"

Un script funciona porque una variable ya existía en el workspace, aunque no esté definida dentro del script.

```matlab
% script.m
y = x + 1;   % x no está definida aquí
```

Si `x` existía previamente en el workspace, el script se ejecuta sin error, pero el comportamiento no es reproducible.

> [!tip] Solución
> Usar `clear` al inicio o trabajar con funciones.

### 2. Dependencia del orden de ejecución

MATLAB ejecuta el script de arriba a abajo. Si se ejecuta solo una sección, puede fallar si las variables de secciones anteriores no existen.

> [!tip] Solución
> Diseñar secciones independientes o ejecutar siempre desde el inicio.

### 3. Scripts demasiado largos

Más de 100-200 líneas dificultan el mantenimiento y la depuración.

> [!tip] Solución
> Dividir la lógica en [[index]] y usar el script como "main" que las orquesta.

---

## Buenas prácticas

### Usar scripts como "main"

```matlab
% main.m
%% Configuración
addpath(genpath('utils'))

%% Parámetros
parametro1 = 10;
parametro2 = 0.5;

%% Ejecutar análisis
resultado = funcion_principal(parametro1, parametro2);

%% Visualizar
graficar_resultado(resultado);
```

### Mover lógica compleja a funciones

Evitar lógica compleja dentro del script:

```matlab
% No recomendado: lógica extensa dentro del script
for i = 1:1000
    % 50 líneas de código complejo
end
```

Mejor: función separada

```matlab
resultado = procesar_datos(datos);
```

### Iniciar con limpieza controlada

```matlab
clc; clear; close all;
```

### Usar nombres claros

| Recomendado | Evitar |
|-------------|--------|
| `velocidad_promedio` | `v` |
| `matriz_covarianza` | `m` |
| `indices_validos` | `idx` (solo si es contexto claro) |

### Usar secciones (`%%`)

Las secciones hacen que el script sea:
- **Modular**: se puede probar por partes
- **Debuggable**: más fácil localizar errores
- **Documentado**: estructura visual clara

---

## Flujo real de uso

1. Crear `main.m` (script principal)
2. Configurar rutas con `addpath` (ver [[Configuración de la ruta de búsqueda]])
3. Definir datos y parámetros
4. Llamar funciones que contienen la lógica
5. Visualizar resultados
6. Ejecutar con `F5` o por secciones

---

## Ejemplo completo

```matlab
% main.m
%% Setup
clc; clear; close all;
addpath(genpath('utils'))

%% Parámetros
frecuencia = 2;
amplitud = 3;
t = 0:0.01:10;

%% Procesamiento
senal = amplitud * sin(2 * pi * frecuencia * t);

%% Resultados
plot(t, senal);
xlabel('Tiempo (s)');
ylabel('Amplitud');
title('Señal senoidal');
grid on;
```

---

## Resumen rápido

| Aspecto | Descripción |
|---------|-------------|
| **Extensión** | `.m` |
| **Workspace** | Global (comparte con Command Window) |
| **Entrada/salida** | No tiene formalmente |
| **Ejecución** | Secuencial, de arriba a abajo |
| **Uso principal** | Flujo principal, pruebas, automatización |
| **Secciones** | `%%` para ejecutar por bloques |

---

## Notas relacionadas

- [[index]]
- [[Live Scripts]]
- [[Atajos de teclado esenciales]]
- [[Editor, Command Window, Workspace, Current Folder]]
- [[Configuración de la ruta de búsqueda]]
- [[Depuración]]
