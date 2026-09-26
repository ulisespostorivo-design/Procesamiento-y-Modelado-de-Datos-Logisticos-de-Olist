* **Propósito del proyecto:** Desarrollo de un análisis integral de la cadena de suministro de Olist utilizando SQL, PostGIS y Power BI para evaluar el rendimiento logístico, los tiempos de entrega y la distribución geográfica de los envíos.

* **Herramientas principales:** SQL, PostGIS y Power BI

* **Principales resultados:** 
  * **Concentración Geográfica:** São Paulo opera en una escala completamente diferente al resto del país, actuando como un gigante que acapara el volumen de la plataforma y requiere un tratamiento analítico especial para evitar sesgos en la interpretación de los datos.
  * **Categorías Destacadas:** Identificación de las líneas con mayor facturación (salud y belleza, artículos para el hogar, deportes) y de productos de ticket alto, como computación.
  * **Impacto Logístico:** Demostración analítica de que los tiempos de entrega y retrasos son el factor determinante en la insatisfacción del cliente, por encima del precio o monto del producto.
  * **Análisis Temporal:** Tratamiento de picos de demanda atípicos generados durante eventos masivos como Black Friday y Cyber Monday (noviembre de 2017).

* **Competencias Técnicas Demostradas:**
  * Limpieza y procesamiento de datos (manejo de registros nulos y duplicados) mediante consultas SQL.
  * Creación y optimización de tableros de visualización de datos, implementando escalas logarítmicas y jerarquías visuales basadas en volumen de ventas.

## 📌 Descripción General
Este proyecto presenta un análisis integral de la cadena de suministro y el rendimiento comercial de **Olist**, un marketplace de comercio electrónico en Brasil. El desarrollo abarca un pipeline técnico completo: desde la ingesta, traducción y limpieza masiva de datos en **SQL** —gestionando valores nulos, duplicados y anomalías temporales como el impacto de eventos masivos— hasta la implementación de análisis geoespacial avanzado con **PostGIS** para calcular distancias de envío reales. Finalmente, toda la información se consolida en un panel ejecutivo minimalista en **Power BI**, optimizado para la experiencia de usuario, el análisis de rentabilidad y la toma de decisiones.

## 🎯 Contexto y Objetivo del Negocio
El proyecto se centra en el análisis integral del ecosistema de **Olist**, un marketplace de comercio electrónico en Brasil. Su objetivo principal consistió en auditar la cadena de suministro, evaluar el rendimiento logístico, los tiempos de entrega y la rentabilidad de las categorías, examinando cómo los retrasos impactan de forma directa en la satisfacción del cliente y resolviendo sesgos de concentración geográfica mediante métricas precisas.

## 🏗️ Arquitectura, Flujo de Datos y Herramientas Utilizadas
El pipeline analítico de Olist se estructuró de manera modular para transformar datos masivos y crudos en información lista para la toma de decisiones ejecutivas, integrando herramientas especializadas en cada etapa:

* **Ingesta y Limpieza de Datos (SQL / PostgreSQL):** 
  * Se gestionó la carga inicial de los archivos, traduciendo las categorías al español para mejorar la legibilidad.
  * Se implementaron vistas estratégicas y técnicas de limpieza masiva para tratar valores nulos mediante `NULLIF`, corregir errores de formato en fechas utilizando `epoch` y `::timestamp`, y eliminar registros duplicados y anomalías temporales críticas (como el impacto distorsivo del Black Friday y Cyber Monday). Esto permitió entregar los datos ya procesados y "masticados" al frontend.

* **Procesamiento Geoespacial (PostGIS):** 
  * Se integró la extensión PostGIS en PostgreSQL (un requisito técnico indispensable) para calcular distancias geodésicas reales y exactas entre compradores y vendedores. Esto superó las limitaciones de agrupar únicamente por estados o ciudades, logrando una precisión quirúrgica en el estudio de costos de flete por kilómetro y tiempos de entrega.

* **Capa de Visualización y Análisis (Power BI):** 
  * Se conectó directamente a las vistas SQL optimizadas para evitar la duplicación y sobrecarga de datos en el frontend.
  * Se diseñaron paneles bajo un estricto enfoque de minimalismo visual, incorporando escalas logarítmicas para mitigar el sesgo masivo de concentración en São Paulo, ordenamientos personalizados por volumen real de ventas y coloración dinámica condicionada por los precios de los artículos.

## ⚙️ Ingeniería y Procesamiento de Datos (SQL)
La capa de procesamiento en PostgreSQL se diseñó para transformar datos masivos y crudos en un modelo relacional limpio, estructurado y optimizado mediante vistas estratégicas:

* **Normalización y Tipado de Datos:** 
  * Se corrigieron errores de conversión mediante el uso de `::timestamp` y funciones basadas en `epoch` para procesar correctamente campos temporales originalmente almacenados como texto.
  * Se implementó `NULLIF` de forma sistemática para neutralizar celdas vacías o nulas que alteraban las consultas de agregación.
  * Se integraron traducciones al español para las categorías de productos, estandarizando la legibilidad del modelo.

* **Control Riguroso de Duplicados en Joins:** 
  * Al cruzar entidades complejas (compradores, vendedores y transacciones), se mitigaron problemas de duplicación de registros. Se rediseñaron las consultas mediante vistas aisladas y el uso selectivo de `DISTINCT`, previniendo la alteración de volúmenes de venta reales y garantizando métricas financieras coherentes.

* **Neutralización de Anomalías Temporales ("Datos Envenenados"):** 
  * Se identificó que los picos de transacciones del Black Friday y el Cyber Monday distorsionaban severamente las tendencias de la serie temporal. Se aislaron y filtraron estas ventanas críticas para neutralizar el sesgo de marketing masivo y establecer una línea base analítica confiable.

* **Optimización de Vistas ("Datos Masticados"):** 
  * Se evitaron transformaciones pesadas en el frontend estructurando consultas SQL modulares. Esto permitió alimentar las herramientas de visualización con datos pre-procesados, reduciendo la sobrecarga de procesamiento y optimizando el rendimiento de los paneles.

## 📍 Análisis Geoespacial (PostGIS)
El análisis logístico trascendió la simple agrupación administrativa por estados o ciudades mediante la integración de la extensión **PostGIS** en PostgreSQL. Esta implementación permitió calcular distancias geodésicas reales entre las coordenadas de los compradores y los vendedores.

* **Cálculo de Distancias Reales:** Se procesaron las tablas de geolocalización para medir distancias kilométricas exactas, permitiendo evaluar el comportamiento real de los envíos en lugar de estimaciones teóricas por región.
* **Optimización Logística y Costos de Flete:** Al cruzar las distancias geodésicas con los costos de transporte, se logró un análisis quirúrgico de la eficiencia de la cadena de suministro, identificando con precisión cómo la distancia impacta en los márgenes y en la rentabilidad de las entregas.

## 📊 Visualización y Tableros (Power BI)
La capa de visualización se estructuró bajo una filosofía de diseño minimalista y ejecutiva, conectándose directamente a las vistas SQL preprocesadas ("datos masticados") para garantizar un rendimiento fluido del panel y evitar duplicaciones de datos en el frontend.

* **Minimalismo y Optimización de la Interfaz:** Se eliminó la saturación de elementos visuales, priorizando espacios limpios y centrados para reducir la fatiga del usuario y enfocar la atención en las métricas verdaderamente relevantes del negocio.
* **Ordenamiento Personalizado por Volumen de Ventas:** Se superó la limitación del orden alfabético predeterminado (que posicionaba erróneamente a regiones con mínima actividad, como Acre o Amazonas, por encima de los mercados principales) reconfigurando los segmentadores en función del volumen real de transacciones, destacando primero a plazas clave como São Paulo.
* **Escalas Logarítmicas para Neutralizar Sesgos:** Se implementaron escalas logarítmicas en los componentes gráficos y cartográficos para mitigar el sesgo visual provocado por la monstruosa concentración transaccional de São Paulo, permitiendo auditar con equidad el rendimiento de los demás estados.
* **Coloración Dinámica Condicionada por Precios:** Se estableció un esquema de colores funcionales basado en el precio unitario de los artículos: tonos rojos para productos de bajo valor (que exigen alta rotación para generar margen), amarillos para el promedio del mercado y verdes para aquellos con mayor retorno por unidad.

## 💡 Hallazgos e Insights Principales
El análisis de los datos permitió extraer conclusiones críticas sobre la dinámica logística y el comportamiento del consumidor en el marketplace:

* **Neutralización de Anomalías Estacionales (Black Friday y Cyber Monday):** Se detectó que los picos masivos de transacciones en estas fechas clave distorsionaban severamente no solo sus meses de ocurrencia, sino toda la serie temporal. Aislar y filtrar una ventana crítica de apenas 3 días permitió estabilizar la línea base analítica y evitar sesgos masivos de marketing en los tableros.
* **Prioridad Absoluta del Tiempo de Despacho sobre el Precio:** El análisis demostró que el precio o valor monetario del producto casi no influye en la satisfacción final del usuario (ya que al momento de la compra se asume el costo con absoluta certeza); el factor determinante y analítico real es la eficiencia en el cumplimiento del despacho frente a una fecha estimada.
* **Deterioro Crítico de la Reputación por Retrasos:** Al auditar el sistema de calificaciones de los clientes, se identificó que las puntuaciones caen de forma drástica y abrupta a medida que aumentan los días de retraso en las entregas. En contraste, si el pedido cumple con los plazos previstos, las variaciones menores en la velocidad de transporte no alteran negativamente la percepción del comprador.

## 📁 Estructura del Repositorio
El proyecto se encuentra organizado de forma modular para facilitar su auditoría, revisión técnica y replicación:

```text
📦 olist-ecommerce-analytics
 ┣ 📂 data/                 # Datasets originales y tablas auxiliares normalizadas (CSV)
 ┣ 📂 sql/                  # Scripts de creación de vistas, limpieza de nulos y consultas PostGIS
 ┣ 📂 dashboards/           # Archivos del reporte y tableros interactivos (Power BI)
 ┣ 📂 assets/               # Capturas de pantalla e imágenes clave del panel
 ┗ 📜 README.md             # Documentación técnica y caso de estudio completo
