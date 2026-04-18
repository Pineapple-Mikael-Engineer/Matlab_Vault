---
title: "Joins y acceso en tablas — join, innerjoin, outerjoin, acceso por nombre de columna, propiedades"
aliases: ["combinar tablas", "fusionar tablas", "left join", "right join", "key variables"]
tags: [matlab, tablas, concepto, guia]
draft: true
---

# Joins y acceso en tablas

Las tablas en MATLAB permiten combinarse mediante operaciones similares a las bases de datos (joins). También ofrecen múltiples formas de acceder a los datos mediante nombres de columna, índices y propiedades.

## Joins entre tablas

### `join` — combinación por variables clave

Combina dos tablas usando una o más variables clave comunes.

```matlab
% Tabla de pacientes
pacientes = table(...
    [101; 102; 103; 104], ...
    ["Ana"; "Juan"; "Luis"; "Maria"], ...
    [25; 30; 28; 35], ...
    'VariableNames', {'ID', 'Nombre', 'Edad'});

% Tabla de citas
citas = table(...
    [101; 101; 102; 103; 103; 103], ...
    datetime(2026, 4, [1; 15; 5; 10; 20; 30]), ...
    ["Consulta"; "Revision"; "Urgencias"; "Consulta"; "Revision"; "Revision"], ...
    'VariableNames', {'ID', 'Fecha', 'Tipo'});

% Join por ID (solo filas que coinciden en ambas)
T_join = join(pacientes, citas, 'Keys', 'ID');
%    ID    Nombre    Edad       Fecha          Tipo
%    __    ______    ____    ___________    __________
%    101   "Ana"     25      01-Apr-2026    Consulta
%    101   "Ana"     25      15-Apr-2026    Revision
%    102   "Juan"    30      05-Apr-2026    Urgencias
%    103   "Luis"    28      10-Apr-2026    Consulta
%    103   "Luis"    28      20-Apr-2026    Revision
%    103   "Luis"    28      30-Apr-2026    Revision
```

### `innerjoin` — intersección (equivalente a join por defecto)

```matlab
T_inner = innerjoin(pacientes, citas, 'Keys', 'ID');
% Mismo resultado que join
```

### `outerjoin` — unión externa

Mantiene todas las filas de ambas tablas, rellenando con NaN o vacío donde no hay coincidencia.

```matlab
% Paciente 104 no tiene citas
T_outer = outerjoin(pacientes, citas, 'Keys', 'ID');
%    ID_pacientes    Nombre    Edad    ID_citas       Fecha          Tipo
%    ____________    ______    ____    ________    ___________    __________
%        101         "Ana"     25        101       01-Apr-2026    Consulta
%        101         "Ana"     25        101       15-Apr-2026    Revision
%        102         "Juan"    30        102       05-Apr-2026    Urgencias
%        103         "Luis"    28        103       10-Apr-2026    Consulta
%        103         "Luis"    28        103       20-Apr-2026    Revision
%        103         "Luis"    28        103       30-Apr-2026    Revision
%        104         "Maria"   35        NaN           NaT         <missing>
```

### `leftjoin` — mantener todas las filas de la izquierda

```matlab
T_left = leftjoin(pacientes, citas, 'Keys', 'ID');
% Mantiene todos los pacientes (incluyendo 104)
% pero solo las citas que coinciden
```

### `rightjoin` — mantener todas las filas de la derecha

```matlab
T_right = rightjoin(pacientes, citas, 'Keys', 'ID');
% Mantiene todas las citas
% pacientes sin cita aparecen con datos NaN
```

## Joins con múltiples claves

```matlab
% Tabla con múltiples claves
ventas = table(...
    [2024; 2024; 2025; 2025], ...
    [1; 2; 1; 2], ...
    [100; 150; 200; 250], ...
    'VariableNames', {'Anio', 'Trimestre', 'Ventas'});

objetivos = table(...
    [2024; 2024; 2025; 2025], ...
    [1; 2; 1; 2], ...
    [120; 140; 180; 260], ...
    'VariableNames', {'Anio', 'Trimestre', 'Objetivo'});

% Join por ambas columnas
T_completo = join(ventas, objetivos, 'Keys', {'Anio', 'Trimestre'});
%    Anio    Trimestre    Ventas    Objetivo
%    ____    _________    ______    ________
%    2024        1          100        120
%    2024        2          150        140
%    2025        1          200        180
%    2025        2          250        260
```

## Opciones de joins

### Especificar variables clave diferentes

```matlab
% Si las columnas tienen nombres distintos
citas2 = table(...
    [101; 102; 103], ...
    datetime(2026, 4, [1; 5; 10]), ...
    'VariableNames', {'PacienteID', 'Fecha'});

T_join = join(pacientes, citas2, 'LeftKeys', 'ID', 'RightKeys', 'PacienteID');
```

### Manejo de nombres duplicados

```matlab
% Por defecto añade sufijo '_left' y '_right'
T_join = join(pacientes, citas, 'Keys', 'ID', 'Suffixes', {'_pac', '_cita'});
```

## Acceso a columnas por nombre

### Notación de punto

```matlab
T = table(["Ana"; "Juan"], [25; 30], [55.5; 70.2], ...
    'VariableNames', {'Nombre', 'Edad', 'Peso'});

% Acceder a una columna
nombres = T.Nombre;      % string array
edades = T.Edad;         % double vector

% Acceder a múltiples columnas
subtabla = T(:, {'Nombre', 'Peso'});
```

### Notación con llaves `{}`

```matlab
% Extraer contenido como arreglo (similar a cell)
nombres = T{'Nombre'};    % equivalente a T.Nombre

% Extraer filas específicas
valor = T{2, 'Edad'};     % 30
```

### Acceso por índice

```matlab
% Por número de columna
columna1 = T{:, 1};       % primera columna
columna2 = T.(2);         % segunda columna

% Por índice de fila
fila2 = T(2, :);
valor = T(2, 3);          % fila 2, columna 3
```

## Propiedades de la tabla

Las propiedades permiten modificar metadatos y acceder a la tabla de forma más descriptiva.

```matlab
T = table(["Ana"; "Juan"], [25; 30], [55.5; 70.2]);

% Ver todas las propiedades
T.Properties

% Nombres de variables (columnas)
T.Properties.VariableNames = {'Nombre', 'Edad', 'Peso'};

% Nombres de filas
T.Properties.RowNames = {'Paciente1', 'Paciente2'};

% Descripción
T.Properties.Description = 'Datos de pacientes';

% Unidades de las variables
T.Properties.VariableUnits = {'', 'años', 'kg'};

% Descripción de cada variable
T.Properties.VariableDescriptions = {
    'Nombre completo del paciente', ...
    'Edad en años', ...
    'Peso en kilogramos'};

% Ver propiedades actualizadas
disp(T.Properties)
```

## Acceso mediante variables lógicas

```matlab
T = table(["Ana"; "Juan"; "Luis"; "Ana"], [25; 30; 28; 22], [55.5; 70.2; 65.8; 54.0]);

% Crear máscara lógica
es_ana = T.Nombre == "Ana";        % [true; false; false; true]

% Acceder a filas que cumplen
T_ana = T(es_ana, :);

% Acceder a valores específicos
pesos_ana = T.Peso(es_ana);        % [55.5; 54.0]
```

## Funciones útiles para acceso

| Función | Descripción | Ejemplo |
|---------|-------------|---------|
| `vartype` | Seleccionar columnas por tipo | `T(:, vartype('numeric'))` |
| `contains` | Buscar en nombres de columnas | `T(:, contains(T.Properties.VariableNames, 'Edad'))` |
| `renamevars` | Renombrar columnas | `T = renamevars(T, 'Edad', 'Age')` |
| `movevars` | Reordenar columnas | `T = movevars(T, 'Peso', 'Before', 'Edad')` |
| `splitvars` | Dividir columna múltiple | `splitvars(T, 'NombreCompleto')` |
| `mergevars` | Combinar columnas | `mergevars(T, {'Nombre', 'Apellido'})` |

## Ejemplo completo

```matlab
% Tabla de productos
productos = table(...
    [1; 2; 3], ...
    ["Laptop"; "Mouse"; "Teclado"], ...
    [800; 25; 60], ...
    'VariableNames', {'ID', 'Producto', 'Precio'});

% Tabla de ventas
ventas = table(...
    [1; 1; 2; 3; 3; 3], ...
    [10; 5; 50; 2; 3; 1], ...
    'VariableNames', {'ID', 'Cantidad'});

% Join para obtener detalle de ventas
detalle = join(productos, ventas, 'Keys', 'ID');
detalle.Total = detalle.Precio .* detalle.Cantidad;

% Resumen por producto
resumen = groupsummary(detalle, 'Producto', 'sum', 'Total');

% Renombrar columnas para claridad
resumen = renamevars(resumen, {'GroupCount', 'sum_Total'}, {'Unidades', 'IngresoTotal'});

% Acceder a resultados
disp(['Ingreso total: $', num2str(sum(resumen.IngresoTotal))]);
```

Para creación de tablas, ver [[Creacion y lectura|Creacion y lectura]]. Para operaciones básicas, ver [[Manipulacion_basica|Manipulacion basica]].