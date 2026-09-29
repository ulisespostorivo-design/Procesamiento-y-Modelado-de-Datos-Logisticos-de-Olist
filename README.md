# 📦 Procesamiento y Modelado de Datos Logísticos de Olist

> Análisis logístico y geoespacial de un marketplace de comercio electrónico en Brasil utilizando **PostgreSQL, PostGIS y Power BI** para estudiar tiempos de entrega, costos de flete, distribución geográfica y satisfacción del cliente.

---

## 📌 Resumen del Proyecto

Este proyecto utiliza el dataset público de **Olist**, un marketplace de comercio electrónico de Brasil ([dataset original en Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)).

El objetivo fue analizar el rendimiento logístico de la plataforma, desde la preparación y transformación de los datos hasta la visualización de resultados en Power BI.

El proyecto incluye:

* Limpieza y transformación de datos mediante **PostgreSQL**.
* Tratamiento de valores nulos, duplicados y problemas de formato.
* Análisis de fechas y comportamiento temporal.
* Cálculo de distancias entre compradores y vendedores mediante **PostGIS**.
* Análisis de costos de flete y tiempos de entrega.
* Desarrollo de dashboards interactivos en **Power BI**.

![Mapa de Brasil](04_assets/Mapa%20de%20Brasil.png)

## 🛠️ Tecnologías Utilizadas

| Tecnología     | Uso                                                                    |
| -------------- | ---------------------------------------------------------------------- |
| **PostgreSQL** | Almacenamiento, limpieza, transformación y modelado de datos          |
| **PostGIS**    | Cálculo de distancias y análisis espacial                              |
| **Power BI**   | Visualización, exploración y comunicación de resultados                |
---

## 🎯 Contexto y Objetivo de Negocio

El análisis se centró en tres áreas principales:

**Rendimiento comercial**

* Facturación por categoría.
* Distribución de las ventas por categoría y período.
* Identificación de las categorías con mayor facturación.

**Rendimiento logístico**

* Tiempos de entrega.
* Retrasos respecto a la fecha estimada.
* Relación entre distancia recorrida y costo de flete.

**Satisfacción del cliente**

* Relación entre retrasos y calificaciones.
* Variación de las valoraciones según el cumplimiento de la fecha estimada de entrega.

El objetivo fue analizar el desempeño logístico de la plataforma y explorar su relación con la satisfacción del cliente, utilizando indicadores comerciales, temporales y geográficos para obtener una visión integral de la operación.

---

## 🏗️ Flujo de Datos

El proyecto se estructuró en tres etapas principales:

### 1. Ingesta y preparación — PostgreSQL

Los datos originales fueron cargados en PostgreSQL y transformados mediante consultas SQL.

Entre los principales tratamientos realizados:

* Conversión y tipado de campos temporales.
* Tratamiento de valores vacíos y nulos mediante `NULLIF`.
* Detección y tratamiento de registros duplicados.
* Traducción de categorías de productos al español.
* Creación de vistas para organizar las distintas etapas del procesamiento.
* Preparación de datasets específicos para el análisis en Power BI.

### 2. Análisis geoespacial — PostGIS

Se incorporó **PostGIS** para trabajar con las coordenadas geográficas de compradores y vendedores.

Esto permitió calcular distancias entre ambos puntos y utilizarlas para analizar:

* Distancia de los envíos.
* Costos de flete.
* Distribución geográfica de las operaciones.
* Relación entre distancia y tiempos de entrega.

### 3. Visualización — Power BI

Las vistas preparadas en PostgreSQL fueron utilizadas como fuente para los dashboards de Power BI.

El diseño priorizó:

* Lectura rápida de los principales indicadores.
* Jerarquía visual y reducción de elementos innecesarios.
* Ordenamiento de categorías y regiones según volumen.
* Uso de escalas logarítmicas cuando las diferencias de magnitud dificultaban la comparación.
* Uso de formato condicional para facilitar la interpretación de determinadas métricas.

## ⚙️ Procesamiento y Modelado de Datos

### Normalización y tipado

Se realizaron transformaciones para adaptar los datos originales al modelo analítico y garantizar un formato consistente para su posterior análisis.

Entre los principales tratamientos se incluyeron:

* Conversión y tipado de campos temporales.
* Tratamiento de valores vacíos y nulos mediante `NULLIF()`.
* Conversión de registros temporales cuyo formato original no podía utilizarse directamente.
* Traducción de las categorías de productos al español para facilitar la interpretación de los análisis.

### Control de duplicados

Durante la combinación de distintas entidades se detectó que determinados joins podían multiplicar registros y alterar las métricas calculadas.

Para evitar este problema se utilizaron:

* Vistas intermedias para separar las distintas etapas del procesamiento.
* Consultas independientes por entidad antes de realizar determinadas combinaciones.
* `DISTINCT` en los casos en que era necesario controlar registros repetidos.
* Agregaciones previas a determinados joins para preservar la granularidad de los datos.

Esto permitió mantener la consistencia de los volúmenes y evitar distorsiones en métricas como ventas, pedidos y costos de flete.

### Tratamiento de anomalías temporales

Se identificaron picos excepcionales de transacciones asociados a **Black Friday y Cyber Monday en noviembre de 2017**.

Estos eventos se analizaron de forma diferenciada en los análisis temporales en los que podían distorsionar la tendencia general, permitiendo distinguir el comportamiento habitual de los períodos de demanda excepcional.

### Preparación de vistas

Las principales transformaciones y agregaciones se organizaron mediante vistas en PostgreSQL.

Estas vistas permitieron:

* Estructurar los datos según las necesidades de cada análisis.
* Mantener una granularidad consistente antes de su visualización.
* Centralizar parte de la lógica de transformación en PostgreSQL.
* Proporcionar a Power BI datasets preparados para el análisis y la visualización.

## 📍 Análisis Geoespacial con PostGIS

El componente geoespacial permitió ir más allá de una agrupación por estado o ciudad.

A partir de las coordenadas disponibles en los datos, se calcularon distancias entre compradores y vendedores mediante **PostGIS**.

Estas distancias fueron posteriormente utilizadas para analizar:

* Kilómetros recorridos por los envíos.
* Distribución geográfica de las operaciones.
* Costos de flete.
* Relación entre distancia y logística de entrega.

Esto permitió incorporar una dimensión espacial al análisis comercial y logístico.

![Logística por distancias](04_assets/Logistica%20por%20distancias.png)

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

![Comparativa de Brasil](04_assets/Comparativa%20de%20Brasil.jpg)

### Categorías con mayor facturación

El análisis identificó a **salud y belleza, artículos para el hogar y deportes** entre las categorías con mayor facturación.

También se observaron productos de mayor valor unitario en categorías como **computación**.

### Entregas y satisfacción

El análisis de las calificaciones mostró una relación marcada entre los retrasos de entrega y una menor valoración del pedido.

Cuando el pedido cumple con la fecha prevista, pequeñas diferencias adicionales en el tiempo de transporte presentan un efecto menor sobre las calificaciones que los retrasos respecto a la fecha estimada.

**Implicación:** la precisión de la promesa de entrega tiene mayor impacto en la satisfacción del cliente que la distancia o el tiempo de transporte en sí.

### Comportamiento temporal

Los eventos de **Black Friday y Cyber Monday** generaron picos excepcionales de actividad durante noviembre de 2017.

Estos valores fueron aislados en determinados análisis para evitar que alteraran la interpretación de la tendencia habitual.

---

## 📁 Estructura del Repositorio

El repositorio está organizado de forma modular para separar los datos, las transformaciones y la visualización.

```text
📦 Procesamiento-y-Modelado-de-Datos-Logisticos-de-Olist
 ┣ 📂 01_data/          # Datasets originales y tablas auxiliares normalizadas (CSV)
 ┣ 📂 02_sql/           # Scripts de creación de vistas, limpieza de nulos y consultas PostGIS
 ┣ 📂 03_dashboards/    # Archivos del reporte y tableros interactivos (Power BI)
 ┣ 📂 04_assets/        # Capturas de pantalla e imágenes clave del panel
 ┗ 📜 README.md         # Documentación técnica y caso de estudio completo
```

## 🔎 Competencias Demostradas

**SQL / PostgreSQL**

* Limpieza y transformación de datos en entornos reales.
* Tratamiento de nulos y valores vacíos con `NULLIF`.
* Conversión y tipado de fechas (`::timestamp` y funciones basadas en epoch).
* Diseño de joins multi-tabla con control de fan-out y duplicados.
* Creación de vistas intermedias para garantizar consistencia de métricas.
* Preparación de datasets optimizados para herramientas de BI.

**PostGIS**

* Manejo de coordenadas geográficas.
* Cálculo de distancias entre compradores y vendedores.
* Integración de la dimensión espacial en el análisis de costos de flete y tiempos de entrega.

**Power BI**

* Diseño de dashboards con jerarquía visual y enfoque minimalista.
* Ordenamiento personalizado por volumen de operaciones.
* Uso de escalas logarítmicas para manejar grandes diferencias de magnitud.
* Formato condicional basado en precio unitario.
* Análisis integrado de indicadores comerciales, logísticos y de satisfacción del cliente.
