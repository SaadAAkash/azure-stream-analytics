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

**Note:** Using Azure Event Hubs as output is useful when we need other services subscribing to Event Hubs

### Live Event Processing

#### Time Windowing

1. Tumbling window
1. Hopping window
1. Sliding window
1. Session window

##### Tumbling window
The size of a tumbling window is fixed, and there is no overlapping between consequent windows

```sql
SELECT COUNT(*)
FROM Input
GROUP BY TumblingWindow(second,5)
```

##### Hopping window
The size of a hopping window is fixed, but the difference with the tumbling window is the consequent windows are overlapping.
The function will take 3 inputs: unit of time, size of the window & hopping size.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY HoppingWindow(second,10,5)
```

##### Sliding window
The size of sliding windows are fixed, but new windows will only get created/instantiated if new events take place. 
The function will take 2 inputs: unit of time & window size.
The new window will go back to start from the window size in secods/unit of time selected.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY SlidingWindow(second,10)
```

##### Session window
The session windows are not a fixed size on the consequent windows might not overlap. A session window is only created when there are new events present, so there are no windows in the moments of silence.

This function accepts three parameters: unit of time, timeout & maximum size.
If there is silence for more than the timeout specified, the current window will be closed. If the size of the window exceeds theb maximum size specified, the current window will also be closed.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY SessionWindow(second,5,10)
```

**Note:** Azure Stream Analytics only checks for the size of a session window at multiples of the maximum size. For an example, if maximum size is set to be 10 seconds, Azure Stream Analytics will only keep checking for the maximum size of the window at the 10‑second, 20‑second, and 10n-second mark.

