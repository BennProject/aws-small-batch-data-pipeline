# AWS Small Batch Data Pipeline

## Project Overview

This project is a hands-on AWS batch data pipeline built to develop practical experience transforming raw data into a structured, queryable format for analysis.

The pipeline uses Amazon S3 for storage, AWS Glue for ETL processing, AWS Glue Data Catalog for metadata, Amazon Athena for SQL queries, and AWS IAM for access control. Raw CSV data stored in Amazon S3 was transformed into Snappy-compressed Parquet using AWS Glue, then cataloged and queried with Amazon Athena.

I validated the transformation by confirming that all 8,869 source records were preserved in the processed dataset. The project also included diagnosing an IAM authorization failure during the Glue ETL process and correcting the permissions using least-privilege principles.
