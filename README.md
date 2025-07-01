# KQL Escape Room Datasets

This repository contains the datasets for the [*KQL Escape Room: Break Out with Your Query Skills*](https://rodtrent.substack.com/p/kql-escape-room-break-out-with-your) blog post. The datasets, `SecurityLogs` and `AuditLogs`, are designed for practicing Kusto Query Language (KQL) in a virtual escape room challenge. Below are instructions to set up and use the datasets to solve the KQL puzzles.

## Datasets Overview

- **SecurityLogs**: Tracks system access events (e.g., logins, file access).
- **AuditLogs**: Tracks admin actions (e.g., configuration changes, deletions).

Both datasets are provided as CSV files and KQL ingestion commands for use in Azure Data Explorer or compatible tools.

## Prerequisites

- **Azure Data Explorer**: Access to an Azure Data Explorer cluster (or use the [free cluster](https://dataexplorer.azure.com/freecluster)).
- **KQL-Compatible Tool**: Optionally, use [Kusto.Explorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer) or another tool that supports KQL and CSV imports.
- **Text Editor**: To copy KQL commands or edit CSV files.

## Setup Instructions

### Option 1: Use Azure Data Explorer

1. **Access Azure Data Explorer**:
   - Log in to your Azure Data Explorer cluster or use the free cluster at [dataexplorer.azure.com](https://dataexplorer.azure.com/freecluster).
   - Open the query editor.

2. **Create and Populate `SecurityLogs`**:
   - Copy the following KQL command to create and ingest the `SecurityLogs` table:
     ```kql
     .create table SecurityLogs (
         Timestamp: datetime,
         UserId: string,
         EventType: string,
         Device: string,
         Location: string
     )

     .ingest inline into table SecurityLogs <|
     2025-07-01T08:00:00,U101,Login,Laptop,Office
     2025-07-01T08:05:00,U102,Login,Desktop,Remote
     2025-07-01T08:10:00,U101,FileAccess,Laptop,Office
     2025-07-01T08:15:00,U103,Login,Tablet,Cafe
     2025-07-01T08:20:00,U102,FileAccess,Desktop,Remote
     ```
   - Run the command in the query editor.
   - Verify with `SecurityLogs | count` (should return 5 rows).

3. **Create and Populate `AuditLogs`**:
   - Copy the following KQL command to create and ingest the `AuditLogs` table:
     ```kql
     .create table AuditLogs (
         Timestamp: datetime,
         UserId: string,
         EventType: string,
         Device: string,
         Action: string
     )

     .ingest inline into table AuditLogs <|
     2025-07-01T08:02:00,U201,AdminLogin,Laptop,Config
     2025-07-01T08:12:00,U202,AdminLogin,Desktop,Update
     2025-07-01T08:18:00,U201,AdminAction,Laptop,Delete
     ```
   - Run the command in the query editor.
   - Verify with `AuditLogs | count` (should return 3 rows).

4. **Run KQL Queries**:
   - Use the queries from the [KQL Escape Room blog post](<insert-blog-post-link>) to solve the challenges.
   - Example query for Stage 1:
     ```kql
     let targetLocation = "Office";
     SecurityLogs
     | where EventType == "Login" and Location == targetLocation
     | summarize by UserId
     ```

### Option 2: Simulate Locally with CSV Files

1. **Download CSV Files**:
   - Find `SecurityLogs.csv` and `AuditLogs.csv` in this repository.
   - Contents:
     - `SecurityLogs.csv`:
       ```csv
       Timestamp,UserId,EventType,Device,Location
       2025-07-01T08:00:00,U101,Login,Laptop,Office
       2025-07-01T08:05:00,U102,Login,Desktop,Remote
       2025-07-01T08:10:00,U101,FileAccess,Laptop,Office
       2025-07-01T08:15:00,U103,Login,Tablet,Cafe
       2025-07-01T08:20:00,U102,FileAccess,Desktop,Remote
       ```
     - `AuditLogs.csv`:
       ```csv
       Timestamp,UserId,EventType,Device,Action
       2025-07-01T08:02:00,U201,AdminLogin,Laptop,Config
       2025-07-01T08:12:00,U202,AdminLogin,Desktop,Update
       2025-07-01T08:18:00,U201,AdminAction,Laptop,Delete
       ```

2. **Load in a KQL-Compatible Tool**:
   - Use [Kusto.Explorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer) to import the CSV files.
   - Follow the tool’s instructions to load CSV data and run KQL queries.
   - Alternatively, manually analyze the CSV data to verify query results.

3. **Run KQL Queries**:
   - Apply the blog post’s queries to the loaded data.
   - For manual analysis, use a spreadsheet tool (e.g., Excel) to filter and group data as described in the challenges.

## Verify the Datasets

- After setting up, confirm the data loaded correctly:
  - `SecurityLogs | count` should return 5 rows.
  - `AuditLogs | count` should return 3 rows.
- Check the data matches the tables in the blog post to ensure compatibility with the challenges.

## Using the Datasets

- Follow the [*KQL Escape Room* blog post](https://rodtrent.substack.com/p/kql-escape-room-break-out-with-your) to solve three stages:
  1. **Stage 1**: Use `let` to define variables (e.g., filter `SecurityLogs` for "Office" logins).
  2. **Stage 2**: Use `union` to combine `SecurityLogs` and `AuditLogs` (e.g., find "Laptop" events).
  3. **Stage 3**: Use `summarize` to aggregate data (e.g., count events per user).
- Experiment with the queries by modifying variables or filters to explore KQL further.

## Additional Resources

- [KQL Documentation](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/query/): Learn more about KQL operators.
- [Azure Data Explorer](https://dataexplorer.azure.com/): Try KQL in a web-based environment.
- [Kusto.Explorer](https://docs.microsoft.com/en-us/azure/data-explorer/kusto/tools/kusto-explorer): Desktop tool for KQL queries.

## Contributing

Feel free to fork this repository, add more KQL challenges, or expand the datasets. Submit a pull request with your changes, and let’s build more KQL adventures together!

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
