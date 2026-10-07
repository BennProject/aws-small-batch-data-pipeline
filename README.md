# AWS Small Batch Data Pipeline

## Project Overview

This project is a hands-on AWS batch data pipeline built to develop practical experience transforming raw data into a structured, queryable format for analysis.

The pipeline uses Amazon S3 for storage, AWS Glue for ETL processing, AWS Glue Data Catalog for metadata, Amazon Athena for SQL queries, and AWS IAM for access control. Raw CSV data stored in Amazon S3 was transformed into Snappy-compressed Parquet using AWS Glue, then cataloged and queried with Amazon Athena.

I validated the transformation by confirming that all 8,869 source records were preserved in the processed dataset. The project also included diagnosing an IAM authorization failure during the Glue ETL process and correcting the permissions using least-privilege principles.

## Architecture

The pipeline follows this data flow:

Raw CSV Data → Amazon S3 → AWS Glue ETL → Snappy-Compressed Parquet → Amazon S3 → AWS Glue Data Catalog → Amazon Athena

### Pipeline Stages

1. Raw CSV data was stored in an Amazon S3 raw data location.
2. AWS Glue read the cataloged raw data and transformed it from CSV into Snappy-compressed Parquet.
3. The transformed Parquet data was written to a separate processed location in Amazon S3.
4. An AWS Glue crawler cataloged the processed dataset in the AWS Glue Data Catalog.
5. Amazon Athena used the catalog metadata to query and analyze the processed data with SQL.

## AWS Services and Technologies

- Amazon S3: Stored the raw CSV data and processed Parquet data.
- AWS Glue: Performed the ETL transformation from CSV to Parquet.
- AWS Glue Data Catalog: Provided metadata used to access the cataloged data.
- AWS Glue Crawlers: Cataloged the processed Parquet dataset.
- Amazon Athena: Queried and analyzed the processed data using SQL.
- AWS IAM: Controlled permissions required by the Glue ETL job and crawler.
- Apache Parquet: Columnar format used for the processed dataset.
- Snappy Compression: Compression used for the processed Parquet data.
- SQL: Used in Athena for validation and analysis.

## Data Transformation

The source dataset was stored as CSV in Amazon S3. AWS Glue processed the source data and converted it into Snappy-compressed Parquet, which was written to a separate processed S3 location.

A separate AWS Glue crawler then cataloged the processed Parquet dataset so it could be queried through Amazon Athena.

## IAM Troubleshooting

The AWS Glue ETL job initially failed with an S3 PutObject authorization error.

I reviewed the IAM permissions and determined that the Glue role had access to the raw S3 location but did not have the required `s3:PutObject` permission for the processed location.

Instead of broadly expanding the role's permissions, I added separate write permission for the processed S3 location. I also configured separate crawler permissions so the crawler for the processed dataset only required read access to the processed objects.

After correcting the IAM permissions, the Glue ETL job completed successfully.

This troubleshooting process reinforced how IAM permissions, actions, and resource scopes affect AWS services and why least-privilege access should be used instead of granting broader permissions than necessary.

## Data Validation

After the ETL job completed, I used Amazon Athena to validate the transformed dataset.

I compared the source and processed row counts and confirmed that all 8,869 source records were preserved through the CSV-to-Parquet transformation.

This provided a basic data-quality check that the transformation had not unexpectedly lost records.

## Athena Analysis

After validating the processed dataset, I used Amazon Athena and SQL to analyze the Parquet data.

The analysis included yearly average egg prices and yearly average product prices. Queries, results, and screenshots were saved as project documentation.

## Project Evidence

Project documentation includes:

- AWS Glue ETL job execution
- Raw and processed Amazon S3 objects
- AWS Glue Data Catalog tables
- Saved Athena queries
- Athena query screenshots
- Exported Athena query results

Selected screenshots from this documentation are included in the repository to demonstrate the pipeline and analysis.

## What I Learned

This project gave me hands-on experience working through a batch data pipeline from raw data to queryable analytical data.

The most valuable part of the project was troubleshooting the failed Glue job. Resolving the IAM issue required identifying which AWS service needed access, which S3 resource required that access, and which specific permission was missing.

The project strengthened my understanding of AWS batch data pipelines, CSV-to-Parquet transformation, data cataloging, SQL-based validation and analysis, and the practical role of least-privilege IAM permissions.
