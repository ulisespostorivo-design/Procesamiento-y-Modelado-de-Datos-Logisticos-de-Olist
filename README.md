# 📦 Procesamiento y Modelado de Datos Logísticos de Olist

> Análisis logístico y geoespacial de un marketplace de e-commerce en Brasil utilizando **PostgreSQL, SQL, PostGIS y Power BI** para estudiar tiempos de entrega, costos de flete, distribución geográfica y satisfacción del cliente.

---

## 📌 Resumen del Proyecto

Proyecto de análisis de datos basado en el dataset público de **Olist**, un marketplace de comercio electrónico de Brasil.

El objetivo fue analizar el rendimiento comercial y logístico de la plataforma, desde la preparación de los datos hasta la visualización de resultados en Power BI.

El proyecto incluye:

* Limpieza y transformación de datos mediante **SQL / PostgreSQL**.
* Tratamiento de valores nulos, duplicados y problemas de formato.
* Análisis de fechas y comportamiento temporal.
* Cálculo de distancias entre compradores y vendedores mediante **PostGIS**.
* Análisis de costos de flete y tiempos de entrega.
* Desarrollo de dashboards interactivos en **Power BI**.

## 🛠️ Tecnologías Utilizadas

| Tecnología | Uso |
|---|---|
| **PostgreSQL** | Almacenamiento, limpieza, transformación y modelado de datos mediante SQL |
| **PostGIS** | Cálculo y análisis de distancias geográficas |
| **Power BI** | Visualización, exploración y análisis de resultados |

---

## 🎯 Contexto y Objetivo de Negocio

El análisis se centró en tres áreas principales:

**Rendimiento comercial**

* Facturación por categoría.
* Distribución de ventas.
* Identificación de productos y categorías de mayor valor.

**Rendimiento logístico**

* Tiempos de entrega.
* Retrasos respecto a la fecha estimada.
* Relación entre distancia y costo de flete.

**Satisfacción del cliente**

* Relación entre retrasos y calificaciones.
* Comportamiento de las valoraciones frente al cumplimiento de las fechas de entrega.

El objetivo fue transformar los datos originales en información estructurada que permitiera explorar estos indicadores de forma consistente.

---

## 🏗️ Flujo de Datos

El proyecto se estructuró en tres etapas principales:

### 1. Ingesta y preparación — PostgreSQL / SQL

Los datos originales fueron cargados en PostgreSQL y posteriormente transformados mediante consultas SQL.

Entre los principales tratamientos realizados:

* Conversión y tipado de campos temporales.
* Tratamiento de valores vacíos y nulos mediante `NULLIF`.
* Eliminación y control de registros duplicados.
* Traducción de categorías de productos al español.
* Creación de vistas para separar las diferentes etapas del procesamiento.
* Preparación de datasets específicos para el análisis en Power BI.

### 2. Análisis geoespacial — PostGIS

Se incorporó **PostGIS** para trabajar con las coordenadas geográficas de compradores y vendedores.

Esto permitió calcular distancias entre ambos puntos y utilizarlas posteriormente para analizar:

* Distancia de los envíos.
* Costos de flete.
* Distribución geográfica de las operaciones.
* Relación entre distancia y tiempos de entrega.

### 3. Visualización — Power BI

Las vistas preparadas en PostgreSQL fueron utilizadas como fuente para los dashboards de Power BI.

El diseño priorizó:

* Lectura rápida de los principales indicadores.
* Jerarquía visual.
* Reducción de elementos innecesarios.
* Ordenamiento de categorías y regiones según volumen.
* Uso de escalas logarítmicas cuando las diferencias de magnitud dificultaban la comparación.
* Coloración condicionada para facilitar la interpretación de determinadas métricas.

---

## ⚙️ Procesamiento y Modelado de Datos

### Normalización y tipado

Se realizaron transformaciones para adaptar los datos originales al modelo analítico.

Entre ellas:

```sql
::timestamp
```

para la conversión de campos temporales y funciones basadas en `epoch` para resolver registros cuyo formato original no podía utilizarse directamente.

También se utilizó:

```sql
NULLIF()
```

para tratar valores vacíos o nulos que podían afectar las agregaciones.

Las categorías de productos fueron además traducidas al español para facilitar la interpretación de los dashboards.

### Control de duplicados

Uno de los problemas encontrados durante la combinación de tablas fue la generación de duplicados al relacionar compradores, vendedores y transacciones.

Para evitar que estos duplicados alteraran los resultados de ventas y otras métricas, se utilizaron:

* Vistas intermedias.
* Consultas separadas por entidad.
* `DISTINCT` cuando era necesario.
* Agregaciones realizadas antes de determinados joins.

El objetivo fue mantener consistencia en los volúmenes y métricas calculadas.

### Tratamiento de anomalías temporales

Se identificaron picos excepcionales de transacciones asociados a **Black Friday y Cyber Monday en noviembre de 2017**.

Estos eventos fueron tratados por separado para evitar que su comportamiento excepcional dominara determinados análisis temporales y dificultara la interpretación de la tendencia general.

### Preparación de vistas

Parte de las transformaciones se realizaron directamente en PostgreSQL mediante vistas.

Esto permitió que Power BI recibiera datos previamente estructurados y redujo la necesidad de realizar transformaciones complejas directamente en el frontend.

---

## 📍 Análisis Geoespacial con PostGIS

El componente geoespacial permitió ir más allá de una agrupación por estado o ciudad.

A partir de las coordenadas disponibles en los datos, se calcularon distancias entre compradores y vendedores mediante **PostGIS**.

Estas distancias fueron posteriormente utilizadas para analizar:

* Kilómetros recorridos por los envíos.
* Distribución geográfica de las operaciones.
* Costos de flete.
* Relación entre distancia y logística de entrega.

Esto permitió incorporar una dimensión espacial al análisis comercial y logístico.

---

## 📊 Power BI — Visualización y Dashboards

El dashboard fue diseñado con un enfoque minimalista, priorizando la lectura de los indicadores principales.

### Ordenamiento por volumen

Los segmentadores y categorías fueron ordenados utilizando el volumen de operaciones en lugar del orden alfabético.

Esto permite que las regiones con mayor actividad aparezcan primero y facilita la exploración de los mercados principales.

### Escalas logarítmicas

La distribución geográfica presenta diferencias de magnitud muy grandes, especialmente por la concentración de operaciones en **São Paulo**.

Para determinados gráficos y componentes se utilizaron escalas logarítmicas para permitir una comparación más útil entre regiones con volúmenes muy diferentes.

### Coloración condicionada

Se implementó coloración dinámica basada en el precio unitario de los productos para facilitar la identificación visual de diferentes rangos de valor.

---

## 💡 Principales Hallazgos

### Concentración geográfica

São Paulo concentra un volumen de operaciones muy superior al de otras regiones del dataset.

Esta diferencia de escala debe tenerse en cuenta al analizar visualmente el resto de los estados, ya que puede ocultar variaciones de menor magnitud.

### Categorías con mayor facturación

El análisis identificó a **salud y belleza, artículos para el hogar y deportes** entre las categorías con mayor facturación.

También se observaron productos de mayor valor unitario en categorías como **computación**.

### Entregas y satisfacción

El análisis de las calificaciones mostró una relación marcada entre los retrasos de entrega y una menor valoración del pedido.

Cuando el pedido cumple con la fecha prevista, pequeñas diferencias adicionales en el tiempo de transporte presentan un efecto menor sobre las calificaciones que los retrasos respecto a la fecha estimada.

### Comportamiento temporal

Los eventos de **Black Friday y Cyber Monday** generaron picos excepcionales de actividad durante noviembre de 2017.

Estos valores fueron aislados en determinados análisis para evitar que alteraran la interpretación de la tendencia habitual.

---

## 📁 Estructura del Repositorio

El repositorio está organizado de forma modular para separar los datos, las transformaciones y la visualización.

```text
📦 olist-ecommerce-analytics
 ┣ 📂 data/                 # Datasets originales y tablas auxiliares normalizadas (CSV)
 ┣ 📂 sql/                  # Scripts de creación de vistas, limpieza de nulos y consultas PostGIS
 ┣ 📂 dashboards/           # Archivos del reporte y tableros interactivos (Power BI)
 ┣ 📂 assets/               # Capturas de pantalla e imágenes clave del panel
 ┗ 📜 README.md             # Documentación técnica y caso de estudio completo
```

---

## 🔎 Competencias Demostradas

**SQL / PostgreSQL**

* Limpieza y transformación de datos.
* Manejo de valores nulos.
* Conversión y tratamiento de fechas.
* Joins entre múltiples entidades.
* Control de duplicados.
* Creación de vistas.
* Preparación de datos para herramientas de BI.

**PostGIS**

* Trabajo con datos geográficos.
* Cálculo de distancias.
* Integración de información espacial con datos logísticos.

**Power BI**

* Diseño de dashboards.
* Jerarquía visual.
* Ordenamiento personalizado.
* Escalas logarítmicas.
* Formato condicional.
* Análisis de indicadores comerciales y logísticos.

