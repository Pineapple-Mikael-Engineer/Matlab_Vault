---
title: 02 Tipos de datos y estructuras fundamentales
aliases:
  - tipos MATLAB
  - estructuras de datos MATLAB
tags:
  - matlab
  - index
  - concepto
draft: false
---

# Tipos de datos y estructuras fundamentales

MATLAB ofrece una rica variedad de tipos de datos y estructuras para organizar la información. Desde la matriz, que es la estructura fundamental del lenguaje, hasta tipos más especializados como tablas o diccionarios.

## Matrices y arreglos

La matriz es el corazón de MATLAB. Todo dato numérico se almacena como un arreglo multidimensional. Las operaciones pueden ser elemento a elemento (con punto) o matriciales (siguiendo álgebra lineal). La indexación permite acceder a subconjuntos mediante subíndices, índices lineales o máscaras lógicas. Para un desarrollo completo, ver [[2.1 Matrices y arreglos]].

```matlab
A = [1 2; 3 4];
B = A * A;        % Multiplicación matricial
C = A .* A;       % Elemento a elemento
A(2, :)           % Segunda fila
```

## Tipos numéricos y lógicos

MATLAB maneja `double` (precisión doble, por defecto), `single` (precisión simple) y enteros con signo o sin signo (`int8`, `uint8`, `int16`, etc.). Los valores especiales `NaN` (indefinido) e `Inf` (infinito) son útiles para datos faltantes o desbordamientos. El tipo `logical` representa valores booleanos `true` y `false`. Ver [[2.2 Tipos numéricos y lógicos]].

```matlab
x = int8(127);     % Entero de 8 bits
datos = [1, NaN, 3];
media = mean(datos, 'omitnan');
mascara = [true, false, true];
```

## Cadenas y caracteres

MATLAB tiene dos formas de representar texto: arreglos de caracteres (`char`) con comillas simples `''`, y cadenas (`string`) con comillas dobles `""`. Las segundas son más convenientes para la mayoría de casos. Funciones como `split`, `join`, `contains` y `replace` facilitan la manipulación. Ver [[2.3 Cadenas y caracteres]].

```matlab
c = 'Hola';        % char
s = "Mundo";       % string
texto = s + " " + c;
partes = split("a,b,c", ",");
```

## Fechas y horas

Los tipos `datetime` (puntos en el tiempo), `duration` (duraciones exactas) y `calendarDuration` (duraciones de calendario) permiten trabajar con fechas de forma robusta. Soporte de zonas horarias, formatos personalizados y operaciones aritméticas. Ver [[2.4 Fechas y horas]].

```matlab
hoy = datetime('today');
proxima_semana = hoy + days(7);
diff = between(datetime(2026,1,1), hoy);
```

## Arreglos de celdas

Los `cell` arrays son contenedores que pueden almacenar cualquier tipo de dato en cada posición. La diferencia entre `()` (subarreglo de celdas) y `{}` (contenido) es fundamental. Ideales para datos heterogéneos. Ver [[2.5 Arreglos de celdas (cell)]].

```matlab
C = {1, "texto", [1 2; 3 4], @sin};
contenido = C{2};     % "texto"
sub = C(1:2);         % {1, "texto"}
```

## Estructuras

Las `struct` agrupan datos en campos con nombre. Pueden ser escalares o arreglos. Los campos son dinámicos y se accede con notación de punto. Útiles para agrupar variables relacionadas sin crear una tabla. Ver [[2.6 Estructuras (struct)]].

```matlab
persona.nombre = "Ana";
persona.edad = 25;
nombres = {persona.nombre};
```

## Tablas

Las `table` son la estructura más potente para datos tabulares heterogéneos. Cada columna tiene nombre y puede ser de diferente tipo. Soporte para importación/exportación (`readtable`, `writetable`), ordenamiento (`sortrows`), agrupación (`groupsummary`), y combinación mediante joins (`join`, `outerjoin`). Ver [[2.7 Tablas]].

```matlab
T = table(nombres, edades);
T = sortrows(T, 'edades', 'descend');
resumen = groupsummary(T, 'nombre', 'mean', 'edades');
```

## Tipos especiales

Tipos adicionales para casos específicos: `categorical` para datos categóricos (eficiente en memoria), `function_handle` para referenciar funciones (útil en programación funcional), y `containers.Map` para diccionarios clave-valor. Ver [[2.8 Tipos especiales]].

```matlab
colores = categorical(["rojo", "azul", "rojo"]);
f = @(x) x^2;
dic = containers.Map({'Ana','Juan'}, {25,30});
```

Cada uno de estos tipos tiene su propio propósito y caso de uso. La elección depende del problema: matrices para cálculos numéricos, tablas para datos heterogéneos, estructuras para agrupar variables, y tipos especiales para necesidades concretas.