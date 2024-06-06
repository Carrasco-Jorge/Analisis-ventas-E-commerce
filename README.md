# Analisis-ventas-E-commerce

![Reporte en Excel](./img/Ingresos%20y%20Margen%20bruto%20por%20Estado%20300%20gif.gif)

**Tabla de contenidos**

1. [Descripción del problema](#descripción-del-problema)
2. [Puntos clave del problema](#puntos-clave-del-problema)
3. [Propuesta de solución](#propuesta-de-solución)
4. [Implementación de solución en Excel](#implementación-de-solución-en-excel)
5. [Resultado final](#resultado-final)
6. [Conclusiones](#conclusiones)

## Descripción del problema

En este proyecto se aborda un problema de una empresa de E-commerce de la India, la cual tiene un [conjunto de datos](#referencias) de los pedidos realizados en el periodo **2018/04 — 2019/03** y los objetivos de ingresos (*revenue*) por mes. Con esta información se quiere:

- Detectar cuáles son las **categorías de productos** que generan **más ingresos** y tienen mayor **rendimiento económico**
- **Comparar** el desempeño financiero por **región geográfica**
- Identificar las **ubicaciones geográficas** que aportan **mayores ingresos promedio**
- Visualizar el **desempeño financiero** de la empresa **en el tiempo**

Donde se pueda utilizar una herramienta fácil de usar por personas no técnicas para consultar dicha información y, si se quiere, poder revisar con detalle la información de interés.

### El conjunto de datos

El conjunto de datos de la empresa consiste de tres archivos *CSV*, cada uno con una tabla. Estos archivos son: `List of Orders.csv`, `Order Details.csv` y `Sales target.csv`, los cuales contienen las siguientes características:

**List of Orders.csv**

Tabla con la información esencial de los pedidos. En principio debería contener solamente una fila por cada pedido realizado.

- **Order ID:** Identificador único de cada pedido
- **Order Date:** Fecha en que se hizo el pedido
- **CustomerName:** Nombre del cliente que realizó el pedido
- **State:** Estado a donde se enviará el pedido 
- **City:** Ciudad a donde se enviará el pedido

**Order Details.csv**

Tabla con la información de los productos incluidos en cada pedido. Puede haber más de una fila por pedido, puesto que cada pedido puede contener productos de diferente categoría y sub-categoría.

- **Order ID:** Identificador único de cada pedido
- **Amount:** Ingresos (*revenue*) generados por los artículos del pedido
- **Profit:** Ganancias o pérdidas generadas por los artículos del pedido
- **Quantity:** Cantidad de artículos en el pedido
- **Category:** Categoría de los artículos. Por ejemplo, electrónicos
- **Sub-Category:** Sub-categoría de los artículos. Por ejemplo, impresoras

**Sales target.csv**

Tabla con la información de los objetivos financieros de la empresa por mes, por categoría.

- **Month of Order Date:** Mes y año
- **Category:** Categoría de artículos. Por ejemplo, electrónicos
- **Target:** Ingresos (*revenue*) objetivo

## Puntos clave del problema

- La herramienta desarrollada debe poder usarse por **personas no técnicas**
- Se requiere poder **Visualizar** la información más importante
- Es necesario poder consultar **los detalles** de la información
- Detectar las **categorías/sub-categorías** que generan más ingreso y más ganancia o menos pérdida.
- Comparar el desempeño financiero por **región geográfica**
- Identificar las ubicaciones geográficas que aportan **mayores ingresos promedio**
- Visualizar el **desempeño financiero** de la empresa **en el tiempo**
- Variables más relevantes:
    - **Order ID:** conectar tablas **List of Orders** y **Order Detalis**; contar número de pedidos únicos
    - **Order Date:** transformar para conectar la información de los pedidos con la tabla **Sales target**
    - **Month of Order Date:** transformar para conectar la información de los pedidos con la tabla **Sales target**
    - **State:** agrupar información por región geográfica
    - **City:** agrupar información por región geográfica
    - **Amount:** información de revenue / ingresos
    - **Profit:** información de ganancias / pérdidas
    - **Category:** agrupar información por categoría
    - **Sub-category:** agrupar información por sub-categoría
    - **Target:** información de ingresos objetivo

## Propuesta de solución

Se propone crear un archivo / reporte de **Excel** con 3 hojas como se puede ver en este [ejemplo de propuesta](#ejemplo-de-propuesta):

1. Ingresos, Margen bruto y comparación con Ingresos objetivo en el tiempo

2. Ingresos y Margen bruto por Categoría y Sub-categoría de productos

3. Ingresos, Margen bruto e Ingresos promedio por Estado

Se puede decir que esta propuesta puede ser utilizada por gente no técnica de manera fácil ya que es desarrollada en Excel y cuenta con una interfaz bastante estándar (gráficas y tablas dinámicas). También, aborda los [puntos clave del problema](#puntos-clave-del-problema):

- El formato condicional con **íconos representativos** (❌❕✅) en las tablas dinámicas facilita **ver** qué tan bien se logró el desempeño objetivo de la empresa en margen bruto y en ingreso objetivo
- Las gráficas de barras y de pastel **resumen** la información **más importante** acerca de las categorías / sub-categorías y regiones geográficas con **mayor aporte de ingresos**
- Las gráficas también muestran la **comparación** entre las regiones geográficas y las categorías / sub-categorías
- Las tablas dinámicas cuentan con **filtros y/o campos desplegables** para proporcionar una manera fácil y visual de consultar los **detalles** de la información


### Ejemplo de propuesta

![Reporte 1](./img/Ingresos%20y%20Margen%20bruto%20en%20el%20tiempo.png)

![Reporte 2](./img/Ingresos%20y%20Margen%20bruto%20por%20Categoria%20y%20Subcategoria.png)

![Reporte 3](./img/Ingresos%20y%20Margen%20bruto%20por%20Estado.png)

## Implementación de solución en Excel

### Extraer Transformar y Cargar datos con Power Query y Power Pivot

Primero, se cargaron los tres archivos `List of Orders.csv`, `Order Details.csv` y `Sales target.csv` en **Power Query** como archivos *CSV* (posteriormente se utilizó una conexión a este repositorio de GitHub). Después, se aplicaron las siguientes *pipelines* a cada una de las tablas.

#### Pipeline `Order Details`

1. Source: extraer datos de este repositorio de GitHub
2. Promoted Headers: usar primera fila del archivo *CSV* como nombres de columnas
3. Changed Type: auto-detectar tipos de dato de las columnas

*--- Limpieza de datos ---*

4. Quitar filas vacías
5. Renombrar columna **Amount** a **Revenue**

#### Pipeline `List of Orders`

1. Source: extraer datos de este repositorio de GitHub
2. Promoted Headers: usar primera fila del archivo *CSV* como nombres de columnas
3. Changed Type: auto-detectar tipos de dato de las columnas

*--- Limpieza de datos ---*

4. Quitar filas vacías
5. Quitar duplicados de **Order ID**

*--- Transformación de datos ---*

6. Agregar columna **Month** a partir de **Order Date**
7. Agregar columna **Month number** a partir de **Order Date**
8. Cambiar tipo de dato de **Month number** a número
9. Agregar columna **Year** a partir de **Order Date**

#### Pipeline `Target Sales`

1. Source: extraer datos de este repositorio de GitHub
2. Promoted Headers: usar primera fila del archivo *CSV* como nombres de columnas
3. Changed Type: auto-detectar tipos de dato de las columnas

*--- Transformación de datos ---*

4. Agregar columna **Year** a partir de **Month of Order Date**
5. Agregar columna **Month** a partir de **Month of Order Date**
6. Agregar columna **Date id** a partir de las columnas **Year**, **Month** y **Category** (ejemplo de ID: `201804Furniture`)
    - Se puede construir un ID único para cada fila de esta manera puesto que solo hay un ingreso objetivo para cada categoría en cada año y cada mes de ese año
7. Cambiar tipo de dato de **Date id** a texto

![Datos en Power Query](./img/Power%20Query.png)

#### Modelo de datos en Power Pivot

Para poder juntar (JOIN) la información del conjunto de datos, las tablas se identificaron de la siguiente manera:

**Tablas con claves principales (*primary keys*)**

- `List of Orders`: **Order ID**
- `Sales target`: **Date id**

**Tabla con claves externas (*foreign keys*)**

- `Order Details`: **Order ID** y falta crear **Date id** con la información de `List of Orders`

Primero se hace la conexión de `Order Details` con `List of Orders` y se crean las columnas **Year** y **Monthname** usando las expresiones **DAX**:

```
=RELATED('List of Orders'[Month])

=RELATED('List of Orders'[Year])
```

Para luego crear la columna **Date id** con al siguiente expresión **DAX**:

```
=CONCATENATE(FORMAT([Year]*100+[Monthnumber],"General Number"),[Category])
```

Luego, ya es posible crear la conexión de `Order Details` con `Target Sales`. Así, se obtiene el siguiente modelo de datos:

![Modelo de datos en Power Pivot 2](./img/Power%20Pivot%202.png)

Finalmente, se usan expresiones **DAX** de la forma `=RELATED('Tabla'[Columna])` para juntar (JOIN) la información. Finalmente, se obtiene la siguiente tabla:

![Modelo de datos en Power Pivot 1](./img/Power%20Pivot%201.png)


### Creación de métricas con Power Pivot

Ya para finalizar el procesamiento de los datos, se crearon las siguientes métricas y métricas auxiliares para mostrar la información relevante y requerida en los reportes:

- *Expenses*: costos calculados usando los ingresos y la ganancia / pérdida de los productos. **DAX:** `=SUM('Order Details'[Revenue])-SUM('Order Details'[Profit])`
- *Margen bruto %*: eficiencia de la empresa en la producción y venta de los productos. **DAX** `=SUM([Profit])/[Suma de Revenue]`
- *Max target*: métrica **auxiliar** para calcular correctamente el ingreso objetivo en las tablas dinámicas. **DAX** `MAX('Order Details'[RevenueTarget])`
- *Target Revenue*: métrica para calcular correctamente el ingreso objetivo en las tablas dinámicas. **DAX** `SUMX(DISTINCT('Order Details'[Dateid]),[Max target])`
- *Actual - Target*: diferencia absoluta entre los ingresos reales y los ingresos objetivo. **DAX** `[Suma de Revenue]-[Target Revenue]`
- *Actual - Target %*: diferencia entre los ingresos reales y los ingresos objetivo relativa a los ingresos objetivo. **DAX** `[Actual - Target]/[Target Revenue]`
- *Avg Revenue / Order*: ingresos promedio por pedido. **DAX** `[Suma de Revenue]/[Recuento de Order ID]`

## Resultado final

![Reporte 1](./img/Ingresos%20y%20Margen%20bruto%20en%20el%20tiempo.png)

![Reporte 2-1](./img/Ingresos%20y%20Margen%20bruto%20por%20Categoria%20y%20Subcategoria%20300%20gif.gif)

![Reporte 2-2](./img/Ingresos%20y%20Margen%20bruto%20por%20Categoria%20y%20Subcategoria.png)

![Reporte 3-1](./img/Ingresos%20y%20Margen%20bruto%20por%20Estado%20300%20gif.gif)

![Reporte 3-2](./img/Ingresos%20y%20Margen%20bruto%20por%20Estado.png)

## Conclusiones



## Referencias

*Origen de los datos: https://www.kaggle.com/datasets/benroshan/ecommerce-data/data?select=Sales+target.csv*

*Licencia original de los datos: [CC0: Public Domain](https://creativecommons.org/publicdomain/zero/1.0/)*