# Python Notebook Examples

This repository contains example Python notebooks that demonstrate approaches for reading data from outside sources and writing it to cloud storage (Azure Storage Account and Microsoft Fabric OneLake) using Spark Notebooks. The examples are intended for learning and demonstration only and show common patterns used in data integrations.

**ANY CODE PROVIDED IN THIS REPO IS FOR DEMONSTRATION USAGE ONLY.**

## Structure

- `Snowflake/`
  - `Snowflake_Load_Bronze.ipynb` — Reads tables from Snowflake and writes them to a Bronze layer. The notebook contains:
    - A `small_tables` loop that selects full tables and writes each to CSV (or Parquet) in a data lake path.
    - A `largeTables` chunking example that demonstrates how to read large tables in ID-range chunks and save chunked CSV files.
    - Example code that shows how to upload files to Azure Storage (abfss path) and helper functions (recent additions) that demonstrate uploading to Microsoft Fabric OneLake via the Microsoft Graph API using app-only credentials and chunked upload sessions.
  - `Snowflake_Build_Lists.ipynb` — A helper notebook that builds the lists of `small_tables` and `largeTables` and other control metadata used by the loader notebook. Run this first (or import/execute its variables) to populate the table lists used by `Snowflake_Load_Bronze.ipynb`.

## Quick start

1. Open the notebooks in VS Code, Jupyter, or a Fabric-compatible environment.
2. Install required Python packages (see `requirements.txt`).
3. Review and set environment variables or configuration values before running the notebooks:
   - Snowflake connection details (connection string/credentials) — typically provided in the code or a separate variables notebook.
   - Azure Storage / OneLake configuration (storage account, container, Data Lake paths).
   - If using the OneLake examples with Microsoft Graph, set the following environment variables (or replace placeholders in the notebook):
     - `AZURE_TENANT_ID`
     - `AZURE_CLIENT_ID`
     - `AZURE_CLIENT_SECRET`
     - `ONELAKE_DRIVE_ID`
4. Run `Snowflake_Build_Lists.ipynb` first to generate the table lists, then run `Snowflake_Load_Bronze.ipynb`.

## Dependencies

A minimal set of Python packages used by the notebooks:

- pandas
- requests
- msal
- pyarrow (optional, for parquet support)
- pytz

A `requirements.txt` is included for convenience.

## Security & permissions

- The OneLake examples use application-level (client credentials) Graph access; your Azure AD app must be granted the needed Graph permissions (for example, `Files.ReadWrite.All`) by an administrator.
- Do not commit secrets (client secrets, passwords) to source control. Use environment variables or secure secret stores.

## Notes and next steps

- The notebooks are examples and intentionally show patterns rather than production-grade code. If you want, I can:
  - Replace local file writes in `Snowflake_Load_Bronze.ipynb` with direct OneLake upload calls (small files and chunked uploads) so the notebook writes exclusively to Fabric OneLake.
  - Add more robust retry/error handling and tests for the upload helpers.

If you want me to make the automatic replacements in the notebook (small_tables and/or largeTables), tell me which one(s) to change and whether to prefer CSV or Parquet.
