# Certamen 2 - Inteligencia de Negocios

## Caso seleccionado: La Mueblería Albarrán
Empresa del rubro de la mueblería que ha crecido rápidamente y requiere incorporar Inteligencia de Negocios para apoyar la toma de decisiones de su propietario. La base de datos transaccional contiene sucursales, clientes, empleados, ventas y productos.

## Integrantes y sección

| Integrante | Usuario de GitHub |
| --- | --- |
| Cristóbal Carvacho | @CristobalCarvacho115 |
| Tania Contreras | @Netyer |
| Lisbeth Herrera | @Lisherrera |
| Ignacio Parada | @Chumlee197 |

- **Asignatura:** Inteligencia de Negocios
- **Sección:** 300
- **Profesor(a):** Hernan Saavedra

## Descripción del problema
La Mueblería Albarrán registra sus operaciones en una base de datos transaccional pensada para el día a día, no para el análisis. El propietario no cuenta con una vista consolidada que le permita saber qué sucursales, productos, categorías y vendedores generan más ventas, ni cómo cambian las ventas durante el año. Esto dificulta decisiones sobre inventario, surtido, desempeño de la fuerza de ventas y apertura o refuerzo de sucursales.

Los datos analizados corresponden a 365 ventas del año 2017 por un total de $414.022.446 (pesos chilenos), distribuidas en 3 sucursales, 20 productos de 13 categorías, 20 clientes y 6 vendedores.

## Objetivo del proyecto
Objetivo general: diseñar e implementar una solución de Inteligencia de Negocios para la Mueblería Albarrán que apoye la toma de decisiones del propietario.

## Modelo dimensional
Se diseñó un modelo estrella siguiendo los cuatro pasos de Kimball.

### Proceso de negocio
Venta de muebles.

### Granularidad
Una fila por línea de detalle de venta.

### Dimensiones

### DIM_TIEMPO

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_tiempo | int |
| | fecha | date |
| | hora | time |
| | dia_semana | varchar(9) |
| | dia | tinyint |
| | mes | tinyint |
| | anio | smallint |

### DIM_PRODUCTO

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_producto | int |
| | nombre_producto | varchar(50) |
| | categoria | varchar(50) |

### DIM_CLIENTE

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_cliente | int |
| | nombre_cliente | varchar(50) |
| | apellidos_cliente | varchar(50) |
| | sexo | varchar(9) |
| | fecha_nacimiento | date |
| | grupo_etario | varchar(20) |
| | estado_civil | varchar(20) |

### DIM_EMPLEADO

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_empleado | int |
| | nombre_empleado | varchar(50) |
| | apellidos_empleado | varchar(50) |
| | cargo | varchar(50) |
| | fecha_contrato | date |
| | sueldo | int |

### DIM_SUCURSAL

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_sucursal | int |
| | nombre_sucursal | varchar(50) |
| | comuna | varchar(50) |

### Tabla de hechos y medidas

**H_FACT_DETALLE_VENTA**

| Clave | Campo | Tipo |
| --- | --- | --- |
| PK | id_fact_ventas | int |
| FK | id_tiempo | int |
| FK | id_producto | int |
| FK | id_cliente | int |
| FK | id_empleado | int |
| FK | id_sucursal | int |
| | cantidad | tinyint |
| | precio_unitario | int |
| | descuento | float |
| | monto_total | int |
| | tipo_documento | varchar(50) |

## Tecnologías utilizadas

| Tecnología | Uso |
| --- | --- |
| SQL Server 2019 Developer | Base de datos transaccional y dimensional |
| SQL Server Management Studio (SSMS) | Administración y consultas |
| Visual Studio 2019 Community | Entorno de desarrollo |
| SSIS (SQL Server Integration Services) | Proceso ETL |
| Git y GitHub | Control de versiones y gestión de tareas |

## Estructura del repositorio
```
bi-muebleria-albarran-dw/
├── README.md
├── .gitignore
├── docs/
│   ├── modelo-dimensional/   Diagrama y documentación del modelo
│   └── presentacion/         Presentación del proyecto
├── database/
│   ├── scripts/              Scripts SQL del modelo y validaciones
│   └── backup/               Backup (.bak) de la base dimensional
├── etl/
│   └── proyecto-ssis/        Proyecto de Visual Studio con el ETL
├── olap/                     Fase 2
└── powerbi/                  Fase 2
```