# Azure Essentials

### Azure Stream Analytics
It is a fully managed, real‑time analytics service designed to process fast moving streams of data

### Azure Stream Analytics Data Flow

![](https://github.com/SaadAAkash/azure-essentials/blob/master/resources/azure-stream-analytics-data-flow.png)

#### Data Input Sources

An Azure Stream Analytics job can accept streaming data from three different input types.

1. Azure Event Hubs: The first one is Azure Event Hubs. events can be sent to Event Hubs and, in turn, ingest it into Azure Stream Analytics
1. Azure IoT Hub: Azure IoT Hub is an event ingestion service, which is highly optimized towards IoT scenarios. You can have drones or robots frequently send events to IoT Hub, which in turn will be ingested in to Azure Stream Analytics. 
1. Azure Blob Store: Azure Blob store can be used as a source of streaming data into Azure Stream Analytics, and this option is great for log files. You can configure your systems to create audit and system usage files in the form of blob storage, which in turn can be ingested to Azure Stream Analytics.
1. Reference Data: For the reference data, you can use Azure Blob storage and Azure SQL Database

#### Data Ingestion & Processing

The input data will be ingested by Azure Stream Analytics. The service is going to process the data using a query you provide. 

Azure Machine Learning can also be used in the process if you need to involve ML when processing your data.

#### Output

You can configure Azure Stream Analytics to write outputs into: 

1. Azure Blobs, Data Lake Gen1 & Data Lake Storage Gen2]
1. Azure SQL Database and  Azure SQL Data Warehouse (DW)
1. Azure Event Hubs
1. Power BI

Note: Using Azure Event Hubs as output is useful when we need other services subscribing to Event Hubs

### Live Event Processing

#### Time Windowing

1. Tumbling window
1. Hopping window
1. Sliding window
1. Session window

