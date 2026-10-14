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

## KPI definidos

| Categoría | Nombre del KPI | Fórmula / Métrica | Justificación |
| --- | --- | --- | --- |
| Ventas y rendimiento financiero | Ventas Totales ($) | $\sum (\text{monto\_total})$ | Mide los ingresos brutos totales generados por la mueblería en un periodo determinado. Es el indicador base de crecimiento del negocio. |
| Ventas y rendimiento financiero | Ticket promedio de venta | $\frac{\sum \text{monto\_total}}{\text{Número único de } \text{id\_fact\_ventas}}$ | Evalúa el valor promedio invertido por transacción. Permite identificar la efectividad de estrategias de *upselling* o venta cruzada. |
| Ventas y rendimiento financiero | Porcentaje promedio de descuento | $\text{Promedio}(\text{descuento}) \times 100$ | Controla el margen de ganancia sacrificado en promociones y valida si los descuentos impulsan el volumen sin comprometer la rentabilidad. |
| Gestión de productos | Volumen de unidades vendidas | $\sum (\text{cantidad})$ | Mide la rotación física del inventario. Permite identificar la demanda de muebles según la dimensión `DIM_PRODUCTO`. |
| Gestión de productos | Ingreso por categoría de producto | $\sum (\text{monto\_total})$ agrupado por `DIM_PRODUCTO.categoria` | Identifica qué líneas de productos (dormitorio, oficina, livings, etc.) generan mayor facturación y cuáles requieren revisión comercial. |
| Gestión de sucursales | Ventas totales por sucursal / comuna | $\sum (\text{monto\_total})$ agrupado por `DIM_SUCURSAL.nombre_sucursal` o `comuna` | Compara el desempeño operativo entre distintas ubicaciones geográficas para definir decisiones estratégicas de expansión o asignación de recursos. |
| Capital humano | Ventas totales por empleado | $\sum (\text{monto\_total})$ agrupado por `DIM_EMPLEADO.id_empleado` | Evalúa el desempeño individual del equipo comercial para establecer comisiones, metas de ventas o programas de capacitación. |
| Capital humano | Rendimiento según cargo | $\sum (\text{monto\_total})$ agrupado por `DIM_EMPLEADO.cargo` | Permite analizar el aporte a los ingresos según el perfil y nivel del trabajador dentro de la estructura de la empresa. |
| Demografía de clientes | Distribución de ingresos por grupo etario y sexo | $\sum (\text{monto\_total})$ agrupado por `DIM_CLIENTE.grupo_etario` y `DIM_CLIENTE.sexo` | Ayuda a caracterizar el perfil del comprador principal de Mueblería Albarrán, orientando las campañas de marketing directo. |

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