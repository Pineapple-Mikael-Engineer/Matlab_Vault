---
title: "Funciones anidadas"
tags:
  - MATLAB
  - funciones
  - anidadas
  - nested-functions
  - scope
  - closures
aliases:
  - "Nested functions"
  - "Funciones dentro de funciones"
  - "Closures MATLAB"
---

# Funciones anidadas

Una **función anidada** es una función definida dentro del cuerpo de otra función. A diferencia de las [[Funciones locales y privadas|funciones locales]], las funciones anidadas tienen acceso especial al workspace de la función que las contiene.

---

## Sintaxis básica

```matlab
function padre(x, y)
    % Función principal (padre)
    
    function anidada(z)
        % Función anidada
        resultado = x + y + z;
        disp(resultado);
    end
    
    % La función padre puede llamar a la anidada
    anidada(10);
end
```

### Estructura

| Elemento | Descripción |
|----------|-------------|
| `function padre(...)` | Función principal que contiene las anidadas |
| `function anidada(...)` | Función definida dentro de la padre |
| `end` | Necesario para delimitar cada función (obligatorio en funciones anidadas) |

> [!important]
> En funciones anidadas, la sentencia `end` es **obligatoria** para cada función (padre e hijas).

---

## Características fundamentales

### Acceso al workspace de la función padre

La característica más importante de las funciones anidadas es que pueden **leer y modificar** las variables de la función que las contiene.

```matlab
function procesar(datos)
    % Variables de la función padre
    acumulado = 0;
    contador = 0;
    
    function acumular(valor)
        % La función anidada puede acceder a acumulado y contador
        acumulado = acumulado + valor;
        contador = contador + 1;
    end
    
    function mostrar_promedio()
        if contador > 0
            promedio = acumulado / contador;
            fprintf('Promedio: %.2f\n', promedio);
        else
            disp('No hay datos');
        end
    end
    
    % Procesar cada dato
    for i = 1:length(datos)
        acumular(datos(i));
    end
    
    mostrar_promedio();
end
```

```matlab
>> procesar([1, 2, 3, 4, 5])
Promedio: 3.00
```

### Variables compartidas

Las funciones anidadas comparten el workspace con la función padre. Esto significa:

- Pueden **leer** variables definidas en la función padre
- Pueden **modificar** variables definidas en la función padre
- Las modificaciones son visibles para la función padre y otras anidadas

```matlab
function ejemplo()
    x = 10;
    
    function modificar()
        x = x + 5;   % Modifica la x de la función padre
    end
    
    function mostrar()
        fprintf('x = %d\n', x);
    end
    
    mostrar();      % x = 10
    modificar();
    mostrar();      % x = 15
end
```

---

## Funciones anidadas vs funciones locales

| Característica | Función anidada | [[Funciones locales y privadas#Funciones locales\|Función local]] |
|----------------|-----------------|-------------------|
| **Acceso a variables de la función padre** | Sí, puede leer y modificar | No, solo recibe argumentos |
| **Uso de `end`** | Obligatorio | Opcional (recomendado) |
| **Visibilidad** | Solo dentro de la función padre | Solo dentro del archivo |
| **Reutilización** | Limitada al contexto de la padre | Puede ser llamada por cualquier función del archivo |
| **Complejidad** | Mayor, requiere cuidado con el scope | Menor, más predecible |

### Cuándo usar cada una

| Situación | Recomendación |
|-----------|---------------|
| Necesitar compartir estado entre funciones | Función anidada |
| Funciones auxiliares sin estado compartido | Función local |
| Implementar closures o funciones con memoria | Función anidada |
| Simplificar interfaces reduciendo parámetros | Función anidada |
| Código modular y fácil de testear | Función local |

---

## Múltiples niveles de anidación

Las funciones pueden anidarse en múltiples niveles. Cada función anidada tiene acceso a los workspaces de todas las funciones que la contienen.

```matlab
function nivel1()
    a = 1;
    
    function nivel2()
        b = 2;
        
        function nivel3()
            c = 3;
            % Puede acceder a a, b, c
            resultado = a + b + c;
            disp(['Suma: ', num2str(resultado)]);
        end
        
        nivel3();
    end
    
    nivel2();
end
```

```matlab
>> nivel1()
Suma: 6
```

### Reglas de acceso

| Nivel de acceso | Descripción |
|-----------------|-------------|
| **Variables propias** | Acceso directo |
| **Variables de la función padre** | Acceso directo (lectura y escritura) |
| **Variables de funciones abuelo** | Acceso directo |
| **Variables de funciones hermanas** | No se accede directamente; se accede a través de la función padre común |

```matlab
function padre()
    comun = 0;
    
    function hija1()
        comun = comun + 1;
    end
    
    function hija2()
        % También puede ver y modificar comun
        comun = comun * 2;
    end
    
    hija1();   % comun = 1
    hija2();   % comun = 2
    disp(comun);
end
```

---

## Closures: funciones con estado

Las funciones anidadas pueden actuar como **closures**: funciones que "recuerdan" el estado del contexto en el que fueron creadas.

### Ejemplo: generador de contadores

```matlab
function contador = crear_contador(inicial)
    % Devuelve un handle de función que mantiene un contador interno
    valor = inicial;
    
    function resultado = incrementar()
        valor = valor + 1;
        resultado = valor;
    end
    
    contador = @incrementar;
end
```

Uso:
```matlab
>> cont1 = crear_contador(10);
>> cont2 = crear_contador(100);

>> cont1()
ans = 11
>> cont1()
ans = 12

>> cont2()
ans = 101
>> cont2()
ans = 102

% Cada contador mantiene su propio estado independiente
```

### Ejemplo: función con parámetros persistentes

```matlab
function derivada = crear_derivada(funcion, h)
    % Crea una función que calcula la derivada numérica de otra función
    % con un paso h fijo
    
    function d = derivar(x)
        d = (funcion(x + h) - funcion(x)) / h;
    end
    
    derivada = @derivar;
end
```

Uso:
```matlab
>> f = @(x) x.^2;
>> derivada_fina = crear_derivada(f, 1e-6);
>> derivada_gruesa = crear_derivada(f, 0.1);

>> derivada_fina(3)
ans = 6.0000   % Aproximación precisa

>> derivada_gruesa(3)
ans = 6.1000   % Aproximación menos precisa
```

---

## Ventajas de las funciones anidadas

### 1. Reducción de parámetros

Eliminan la necesidad de pasar variables repetidamente entre funciones.

**Sin anidación:**
```matlab
function resultado = analizar(datos, param1, param2, param3)
    resultado = procesar(datos, param1, param2, param3);
end

function salida = procesar(d, p1, p2, p3)
    salida = calcular(d, p1, p2, p3);
end
```

**Con anidación:**
```matlab
function resultado = analizar(datos, param1, param2, param3)
    resultado = procesar(datos);
    
    function salida = procesar(d)
        salida = calcular(d);
    end
    
    function s = calcular(d)
        % Accede directamente a param1, param2, param3
        s = d + param1 + param2 + param3;
    end
end
```

### 2. Encapsulación de estado

Mantienen el estado interno sin exponer variables globales.

```matlab
function filtro = crear_filtro_promedio(ventana)
    buffer = [];
    
    function valor_filtrado = aplicar(nuevo_valor)
        buffer = [buffer, nuevo_valor];
        if length(buffer) > ventana
            buffer = buffer(end-ventana+1:end);
        end
        valor_filtrado = mean(buffer);
    end
    
    filtro = @aplicar;
end
```

Uso:
```matlab
>> filtro = crear_filtro_promedio(3);
>> for x = [5, 10, 15, 20]
       disp(filtro(x))
   end
5.00    % [5]
7.50    % [5, 10]
10.00   % [5, 10, 15]
15.00   % [10, 15, 20]
```

### 3. Funciones auxiliares con contexto

Crear funciones auxiliares que comparten datos sin pasarlos explícitamente.

```matlab
function resolver_ecuacion(f, x0, tolerancia)
    iter = 0;
    x_actual = x0;
    
    function detener = criterio_parada(x_ant, x_act)
        iter = iter + 1;
        detener = abs(x_act - x_ant) < tolerancia || iter > 100;
        if detener
            fprintf('Convergió en %d iteraciones\n', iter);
        end
    end
    
    function x_sig = paso_newton(x)
        % Calcula paso de Newton usando f
        x_sig = x - f(x) / derivada(x);
    end
    
    function df = derivada(x)
        % Derivada numérica con acceso a la función f
        h = 1e-6;
        df = (f(x + h) - f(x)) / h;
    end
    
    % Iteración principal
    while true
        x_nuevo = paso_newton(x_actual);
        if criterio_parada(x_actual, x_nuevo)
            break;
        end
        x_actual = x_nuevo;
    end
end
```

---

## Limitaciones y consideraciones

| Limitación | Descripción |
|------------|-------------|
| **Uso obligatorio de `end`** | Cada función debe terminar con `end` |
| **No se pueden anidar en scripts** | Solo dentro de funciones |
| **Visibilidad limitada** | No se pueden llamar desde fuera de la función padre |
| **Depuración más compleja** | El flujo puede ser menos obvio que con funciones locales |
| **Rendimiento** | Puede haber overhead por el manejo de workspaces compartidos |

### Depuración de funciones anidadas

```matlab
function depurar_ejemplo()
    a = 10;
    b = 20;
    
    function sumar()
        c = a + b;
        disp(c);  % Punto de interrupción aquí
    end
    
    sumar();
end
```

> [!tip]
> Durante la depuración, el workspace muestra todas las variables accesibles: las propias de la función anidada y las de la función padre.

---

## Funciones anidadas vs funciones privadas vs funciones locales

| Aspecto | Anidadas | Locales | Privadas |
|---------|----------|---------|----------|
| **Acceso a variables padre** | Sí | No | No |
| **Visibilidad** | Solo dentro de función padre | Todo el archivo | Todo el módulo (carpeta) |
| **`end` obligatorio** | Sí | No | No |
| **Uso principal** | Closures, estado compartido | Dividir lógica de una función | Biblioteca interna de módulo |
| **Reutilización** | Baja | Media | Alta |

---

## Ejemplo completo: sistema de adquisición de datos

```matlab
function [iniciar, detener, obtener] = sistema_adquisicion(tasa_muestreo)
% SISTEMA_ADQUISICION Crea un sistema de adquisición de datos simple
%   [iniciar, detener, obtener] = sistema_adquisicion(tasa_muestreo)
%   
%   Devuelve handles a funciones para controlar la adquisición:
%       iniciar() - Comienza la adquisición
%       detener() - Detiene la adquisición
%       obtener() - Devuelve los datos adquiridos

    % Variables de estado (compartidas por todas las funciones anidadas)
    adquiriendo = false;
    datos = [];
    tiempo_inicio = 0;
    contador = 0;
    
    function iniciar()
        % Inicia la adquisición
        if ~adquiriendo
            adquiriendo = true;
            tiempo_inicio = tic;
            contador = 0;
            datos = [];
            fprintf('Adquisición iniciada (tasa: %.1f Hz)\n', tasa_muestreo);
            
            % Simular adquisición en segundo plano
            while adquiriendo
                pause(1/tasa_muestreo);
                adquirir_muestra();
            end
        end
    end
    
    function detener()
        % Detiene la adquisición
        if adquiriendo
            adquiriendo = false;
            fprintf('Adquisición detenida. Muestras: %d\n', length(datos));
        end
    end
    
    function adquirir_muestra()
        % Adquiere una muestra (simulada)
        contador = contador + 1;
        tiempo_actual = toc(tiempo_inicio);
        
        % Simular una señal senoidal con ruido
        muestra = sin(2 * pi * 2 * tiempo_actual) + 0.1 * randn();
        datos = [datos, muestra];
        
        % Mostrar progreso cada 100 muestras
        if mod(contador, 100) == 0
            fprintf('Muestras adquiridas: %d\n', contador);
        end
    end
    
    function datos_adquiridos = obtener()
        % Devuelve los datos adquiridos
        if adquiriendo
            warning('Adquisición en curso, datos incompletos');
        end
        datos_adquiridos = datos;
    end
end
```

Uso del sistema:
```matlab
% Crear el sistema
[iniciar, detener, obtener] = sistema_adquisicion(100);  % 100 Hz

% Iniciar en segundo plano (simulado)
iniciar();  % Inicia la adquisición

% Después de unos segundos...
pause(3);
detener();  % Detiene

% Obtener datos
datos = obtener();
plot(datos)
```

---

## Buenas prácticas

| Práctica | Descripción |
|----------|-------------|
| **Mantenerlas pequeñas** | Una función anidada debe tener una responsabilidad clara y limitada |
| **Documentar propósito** | Explicar qué hace y qué variables externas utiliza |
| **Evitar dependencias excesivas** | Muchas dependencias pueden dificultar la comprensión |
| **Usar para closures** | Aprovechar su capacidad de mantener estado |
| **Considerar alternativas** | Si no se necesita acceso a variables padre, usar funciones locales |
| **Limitar niveles de anidación** | Demasiados niveles dificultan la depuración |

---

## Resumen rápido

| Concepto | Descripción |
|----------|-------------|
| **Definición** | Función dentro de otra función |
| **Acceso** | Puede leer y modificar variables de la función padre |
| **`end` obligatorio** | Cada función debe terminar con `end` |
| **Visibilidad** | Solo dentro de la función padre |
| **Uso principal** | Closures, reducción de parámetros, encapsulación de estado |
| **Alternativa** | Funciones locales cuando no se necesita acceso a variables padre |

---

## Notas relacionadas

- [[index|Índice: Funciones en MATLAB]]
- [[Funciones en archivo]]
- [[Funciones anónimas]]
- [[Funciones locales y privadas]]
- [[Argumentos variables|Argumentos variables]]
- [[Validación de argumentos|Validación de argumentos]]

