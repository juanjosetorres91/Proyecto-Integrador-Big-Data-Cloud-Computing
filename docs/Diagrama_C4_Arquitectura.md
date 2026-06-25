flowchart TD
    %% Definición de estilos C4
    classDef person fill:#08427b,stroke:#052e56,color:#fff,stroke-width:2px
    classDef systemExt fill:#999999,stroke:#6b6b6b,color:#fff,stroke-width:2px
    classDef container fill:#438dd5,stroke:#2a5a8a,color:#fff,stroke-width:2px
    classDef database fill:#438dd5,stroke:#2a5a8a,color:#fff,shape:cylinder,stroke-width:2px
    classDef boundary fill:none,stroke:#cccccc,stroke-width:2px,stroke-dasharray: 5 5

    %% Actores y Sistemas Externos
    Stakeholders(("Stakeholders\n(Gerencia, Operaciones)")):::person
    ERP["ERP POS\n[Sistema Externo]\nVentas en Tiendas Físicas"]:::systemExt
    Ecom["E-commerce\n[Sistema Externo]\nVentas Digitales"]:::systemExt

    %% Límite del Sistema
    subgraph GCP ["Plataforma Big Data (Google Cloud Platform)"]
        
        Orquestador["Cloud Workflows\n[Serverless]\nOrquesta el pipeline batch diario"]:::container

        subgraph Dataproc ["Google Dataproc (Cómputo Ephemeral)"]
            JobBronze["Job Bronze\n[PySpark]\nIngesta a Delta Lake"]:::container
            JobSilver["Job Silver\n[PySpark]\nLimpieza y Particionamiento"]:::container
            JobGold["Job Gold\n[PySpark]\nGeneración de Data Marts"]:::container
        end

        subgraph DataLake ["Google Cloud Storage (Data Lake)"]
            Raw[(RAW\n[CSV crudos])]:::database
            Bronze[(Bronze Layer\n[Delta Lake])]:::database
            Silver[(Silver Layer\n[Delta particionado])]:::database
            Gold[(Gold Layer\n[Delta Data Marts])]:::database
        end

        subgraph BigQuery ["Google BigQuery (Data Warehouse)"]
            BQGold[("retail_gold\n[External Tables]")]:::database
            BQGov[("retail_governance\n[Tablas Nativas]")]:::database
        end

        Looker["Looker Studio\n[Dashboard]\nVisualización de KPIs comerciales"]:::container
    end

    %% Relaciones y Flujos de Datos
    ERP -- "Exporta CSV diario" --> Raw
    Ecom -- "Exporta CSV diario" --> Raw

    Orquestador -. "1. Crea clúster\n2. Lanza jobs\n3. Destruye clúster" .-> Dataproc

    Raw --> JobBronze --> Bronze
    Bronze --> JobSilver --> Silver
    Silver --> JobGold --> Gold

    JobBronze -. "Registra linaje y auditoría" .-> BQGov
    JobSilver -. "Registra linaje y auditoría" .-> BQGov
    JobGold -. "Registra linaje y auditoría" .-> BQGov

    Gold -- "Lectura directa desde GCS" --> BQGold
    BQGold -- "Consultas SQL (On-Demand)" --> Looker
    Looker -- "Visualiza KPIs y reportes" --> Stakeholders
