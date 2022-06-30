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

#### Data Output Source

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

![](https://github.com/SaadAAkash/azure-essentials/blob/master/resources/tumbling-window.png)

```sql
SELECT COUNT(*)
FROM Input
GROUP BY TumblingWindow(second,5)
```

##### Hopping window
The size of a hopping window is fixed, but the difference with the tumbling window is the consequent windows are overlapping.

![](https://github.com/SaadAAkash/azure-essentials/blob/master/resources/hopping-window.png)

The function will take 3 inputs: unit of time, size of the window & hopping size.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY HoppingWindow(second,10,5)
```

##### Sliding window
The size of sliding windows are fixed, but new windows will only get created/instantiated if new events take place. 

![](https://github.com/SaadAAkash/azure-essentials/blob/master/resources/sliding-window.png)

The function will take 2 inputs: unit of time & window size.
The new window will go back to start from the window size in secods/unit of time selected.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY SlidingWindow(second,10)
```

##### Session window
The session windows are not a fixed size on the consequent windows might not overlap. A session window is only created when there are new events present, so there are no windows in the moments of silence.

![](https://github.com/SaadAAkash/azure-essentials/blob/master/resources/session-window.png)

This function accepts three parameters: unit of time, timeout & maximum size.
If there is silence for more than the timeout specified, the current window will be closed. If the size of the window exceeds theb maximum size specified, the current window will also be closed.

```sql
SELECT COUNT(*)
FROM Input
GROUP BY SessionWindow(second,5,10)
```

**Note:** Azure Stream Analytics only checks for the size of a session window at multiples of the maximum size. For an example, if maximum size is set to be 10 seconds, Azure Stream Analytics will only keep checking for the maximum size of the window at the 10‑second, 20‑second, and 10n-second mark.

### Streaming Units (SUs)
- A pool of computation resources available to process a query. They can be machines dedicated to execute your Azure Stream Analytics queries.
- It is recommended that SU percentage utilization should be below 80% all the time. If it goes above, we can scale the SUs while provisioning a new job or after the job being provisioned.


### Data Input Categories & Configuration

#### Categories

- Reference Inputs:
The data which doesn't change or changes very slowly
Supported reference inputs: Azure Blob storage and Azure SQL Database

- Stream Inputs:
The data stream as an ongoing sequence of events over time 
Supported stream inputs: Azure Event Hubs, IoT Hub, and Blob storage

#### Data Formats
Supported data formats as input data streams are:
1. CSV
1. JSON
1. Apache Avro


### Analytics Timing & Event Ordering Policies

Azure Stream Analytics uses **arrival time** as the timestamp by default, not event time. So using the `TIMESTAMP BY` clause, you can specify custom timestamp values, and these custom values can be anything (arrival or event time or your custom time value).

- For Event HUbs & IoT Hubs, the arrival time of an event is the time the event was received.
- In case of Azure Blob storage input, the arrival time is the last modified time of the blob object.
- An example opf using event time as timestamp can be:
  ```tsql
  SELECT
        AlertTime,
        SomeOtherData
  FROM Input TIMESTAMP BY AlertTime
  ```

We can use event ordering policies to handle late arrivals/out-of-order events/messages.

- Late arrival policy: Accept events within the policy window and adjust/drop the rest
- Out‑of‑order policy: If you have messages coming in and newer messages are already received, Azure Stream Analytics is going to compare the out‑of‑order message timestamp with the out‑of‑order policy window time. If older, the message will be dropped or its timestamp will be adjusted to the current time. If the out‑of‑order time difference is less than the out‑of‑order policy window, the message will be accepted.
- Event ordering policies are applied **only if** TIMESTAMP BY is used in your queries.

### Development Notes
- Every Job topology consists of four main actions: Inputs, Functions, Query & Outputs.
- While configuring outputs for a job, there are two important settings that we need to be mindful of:
  1. The first one is the minimum rows. If I set X to this field, this means no output batch or output file will get created unless I have at least X rows in my output.
  2. The next setting is the maximum time. It means the maximum time the job is going to wait to have the minimum rows requirement satisfied. So if I put 1 minute here, this means that the job is going to wait for 1 minute to have the minimum rows completed. After the 1 minute is up, the job is going to create an output regardless of the number of rows I already have in my output.
- Stream Analytics Query Language is similar to T-SQL.
- Type conversion errors can happen when you are reading the input data. In this case, the job is going to **drop** the event
- If the type conversion error is happening when you're outputting the data in your query, this error will be handled by the error policy, which can be configured to **drop or retry** the work.
  
