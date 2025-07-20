# 🛠️ Amazon Product Data ETL Pipeline using AWS Glue & Redshift

An efficient **ETL (Extract, Transform, Load)** pipeline built on **AWS** to clean and process raw e-commerce product data from Amazon for advanced analytics and business intelligence.

---

## 📌 Project Overview

In the era of **data-driven decision-making**, raw e-commerce datasets often contain inconsistencies, null values, and non-standard formats that hinder meaningful insights.

This project leverages AWS services such as:
- **AWS Glue**
- **AWS Glue Crawlers**
- **Amazon S3**
- **Amazon Redshift**

to build an end-to-end **ETL pipeline** that extracts Amazon product data, applies data cleaning and transformations, and loads the high-quality structured data into Redshift for fast querying and reporting.

---

## 📊 Dataset

**Dataset Name:** Amazon Products Dataset  
**Size:** 1000 records × 24 attributes  
**Purpose:** Structured Amazon product listings for sales and performance analysis  

### 🔑 Key Attributes
- **Product Metadata:** title, brand, description  
- **Pricing:** final_price, currency  
- **Performance Metrics:** reviews_count, rating, answered_questions  
- **Media Info:** images_count, video_count, image_url  
- **Additional:** asin, seller_id, manufacturer, availability  

---

## ⚙️ ETL Pipeline

### 📥 Extraction

- Raw Amazon product data (CSV) is uploaded to an **Amazon S3 bucket** (acts as a Data Lake).
- An **AWS Glue Crawler** scans the S3 path, infers the schema, and catalogs metadata in the **AWS Glue Data Catalog**.

### 🧹 Transformation

A custom **AWS Glue Job** (PySpark) applies transformation logic:

| Attribute          | Transformation Logic |
|-------------------|----------------------|
| **Availability**  | - "Only X left" → "Limited"  <br> - Variants of "in stock" → "In Stock" |
| **Number of Sellers** | Missing → `1` |
| **Domain**         | Short names → full URLs <br> Missing → `https://amazon.com/` |
| **Manufacturer**   | Missing → `unknown` |
| **Plus Content**   | Null → `False` |
| **Top Review**     | Missing → `"no review"` |
| **Null Handling**  | Drop rows/columns with all nulls |
| **Type Conversion**| Convert string/scientific notations → `double` format |

- **Output Format:** Parquet  
- **Transformation Destination:** Amazon S3

### 🚚 Load

- Transformed Parquet files are loaded into **Amazon Redshift**.
- A **custom schema** ensures optimal column mapping and query performance.
- Data is loaded via the Redshift `COPY` command from the S3 source.

---
## 🔁 ETL Pipeline Flow

1. **Data Upload (Extract)**
   - Raw Amazon product data (CSV format) is uploaded to an **Amazon S3 bucket**.

2. **Schema Inference**
   - An **AWS Glue Crawler** scans the S3 data and infers the schema.
   - Metadata is stored in the **AWS Glue Data Catalog**.

3. **Data Transformation (Transform)**
   - An **AWS Glue Job** is configured to:
     - Clean and standardize fields (e.g., availability, manufacturer, etc.)
     - Handle null/missing values
     - Convert domains and data types
   - Transformed data is written back to **Amazon S3** in **Parquet format**.

4. **Data Loading (Load)**
   - **Amazon Redshift** is used as the final data warehouse.
   - Data is loaded into Redshift tables via the **COPY command** from the S3 Parquet files.

## 💻 Technologies Used

- **Amazon S3** – Data lake for storing raw and transformed data
- **AWS Glue Crawler** – Automated schema inference and metadata generation
- **AWS Glue** – ETL jobs for data cleaning and transformation
- **AWS Glue Data Catalog** – Centralized metadata repository
- **Amazon Redshift** – Data warehouse for analytics and querying
- **Parquet Format** – Efficient columnar storage format for transformed data
- **Python (PySpark in Glue)** – Custom transformation logic in ETL jobs

---

## 🎯 Use Case

In the era of data-driven business intelligence, raw e-commerce product data is often noisy, incomplete, or inconsistently formatted. This project demonstrates a scalable and automated ETL pipeline using AWS services to process Amazon product listings.

The system:
- Extracts raw CSV data from Amazon S3.
- Uses AWS Glue Crawler to infer schema and catalog metadata.
- Applies cleaning rules (e.g., fixing availability, filling missing values, type conversions) via AWS Glue Job.
- Loads the cleaned data in Parquet format to S3 and finally into Amazon Redshift.
- Enables high-performance analytics and dashboarding with clean, structured product data.

---

## 📽️ Demo Video

▶️ [Click here to watch the project video explanation](https://drive.google.com/file/d/1oK8l6ipLLjtEj4OtIGCxEkz0WyWjhyLf/view)

