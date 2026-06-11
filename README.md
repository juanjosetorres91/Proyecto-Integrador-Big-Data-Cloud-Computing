# Plataforma Big Data para Analítica Omnicanal de Ventas Retail

Proyecto Integrador para la asignatura de Big Data & Cloud Computing del Magíster en Data Science, Universidad del Desarrollo[cite: 1, 2].

## 👥 Equipo del Proyecto
* Claudio Ballerini[cite: 1]
* Juan José Torres[cite: 1]
* Cristian Vargas[cite: 1]
* Christian Vásquez[cite: 1]
* **Profesor:** Luis Castillo[cite: 1]

## 📝 Descripción del Problema
Este proyecto resuelve el desafío de una empresa chilena de retail que opera un ecosistema omnicanal, cuya información comercial se encontraba dispersa en distintas fuentes operacionales[cite: 1]. 

El objetivo es diseñar e implementar una plataforma Big Data capaz de integrar, procesar y analizar grandes volúmenes de información (más de 10 millones de registros transaccionales) correspondientes a ventas POS, e-commerce, inventario, clientes y promociones[cite: 1]. Esta solución busca apoyar a la Gerencia Comercial, Operaciones y Marketing respondiendo preguntas críticas sobre rotación de productos, quiebres de stock y comportamiento de clientes[cite: 1].

## 🏛️ Arquitectura Propuesta
La solución se basa en una arquitectura **Medallion** implementada íntegramente sobre **Google Cloud Platform (GCP)** con un patrón de procesamiento Batch Incremental diario[cite: 1]:

* **Capa Bronze:** Almacenamiento de datos crudos (ventas, inventario, clientes) preservando el formato original y la trazabilidad histórica en Google Cloud Storage usando Delta Lake[cite: 1].
* **Capa Silver:** Limpieza, normalización y deduplicación de datos procesados mediante Apache Spark[cite: 1].
* **Capa Gold:** Data marts analíticos optimizados con KPIs comerciales, demanda agregada y rotación[cite: 1].

## 🛠️ Stack Tecnológico
* **Ingesta:** Python[cite: 1].
* **Orquestación:** Apache Airflow / Cloud Composer[cite: 1].
* **Data Lake:** Google Cloud Storage[cite: 1].
* **Procesamiento:** Apache Spark (PySpark) ejecutado en Google Dataproc[cite: 1].
* **Formato de Almacenamiento:** Delta Lake (proporciona transacciones ACID y Time Travel)[cite: 1].
* **Data Warehouse:** BigQuery[cite: 1].
* **Visualización:** Looker Studio[cite: 1].
* **Seguridad y Control:** IAM y GitHub[cite: 1].

## 📂 Estructura del Repositorio
```text
mi-proyecto/
├── data/               <- Datos de prueba o subconjuntos anonimizados (ignorados por git)
├── notebooks/          <- Cuadernos Jupyter con demostraciones del pipeline 
├── src/                <- Scripts de PySpark para las capas Bronze, Silver y Gold
├── dags/               <- Archivos de orquestación para Airflow/Composer
├── docs/               <- Documentación técnica adicional, diagramas (Modelo C4) y ADRs 
├── requirements.txt    <- Dependencias del proyecto
├── .gitignore          <- Archivos a ignorar en el control de versiones
└── README.md           <- Este archivo