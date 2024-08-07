Stock Data ETL Pipeline Project

Overview

This project presents an automated ETL (Extract, Transform, Load) data pipeline that collects, processes, and visualizes real-time stock data from the Alpha Vantage API using Google Cloud Platform (GCP). The pipeline leverages various GCP services such as Cloud Functions, BigQuery, and Cloud Composer to ensure efficient and reliable data handling. The visualizations are created using Looker, providing valuable insights into stock market trends and investor behavior.

Project Structure

Chapter 1: Dataset

Data Source
We chose Alpha Vantage as our data source due to its comprehensive API that provides detailed financial data, including monthly adjusted stock prices and essential financial metrics.

Data Features
The dataset includes the following features:

Date: The date of the stock data.
Open: The opening price of the stock on the given date.
High: The highest price of the stock on the given date.
Low: The lowest price of the stock on the given date.
Close: The closing price of the stock on the given date.
Adjusted Close: The closing price adjusted for corporate actions.
Volume: The number of shares traded.
Dividend Amount: The dividend paid by the company.
Symbol: The stock symbol of the company.

Data Preparation
To avoid API rate limits during the development phase, we used the Faker library to generate synthetic data. This allowed us to simulate API requests and test the data pipeline without risking API hit limits. The synthetic data mimicked the structure and characteristics of the real data from Alpha Vantage, providing a realistic test environment.

Chapter 2: Solution

Data Collection
Python Script for Data Collection
A Python script was developed to fetch data from the Alpha Vantage API. The script includes:

API Initialization: Initialize the Alpha Vantage API client with the API key.
Fetch Data: Loop through the list of 20 company symbols, making API requests to fetch monthly adjusted stock data.
Data Storage: Save the fetched data into CSV files.
Rate Limiting: Introduce delays between API requests to comply with rate limits (e.g., 12 seconds between requests).

Data Processing
Google Cloud Functions
Data processing is handled by Google Cloud Functions, triggered by new files in Google Cloud Storage. Each function performs data cleaning, aggregation, and transformation. Steps include:

Trigger on New File: A function is triggered when a new CSV file is uploaded to a specific GCS bucket.
Data Cleaning: Remove any invalid or missing data entries.
Data Aggregation: Aggregate data to compute monthly statistics.
Data Transformation: Transform data into a format suitable for loading into BigQuery.
Loading Data: Load the processed data into BigQuery.

Data Storage
BigQuery
BigQuery is used for storing processed data. Data is organized by company and date, enabling efficient querying and analysis. Steps include:

Create Table: Create a table in BigQuery for storing stock data.
Schema Definition: Define the schema for the table, including data types for each column.
Load Data: Load processed data from Google Cloud Functions into the BigQuery table.

Pipeline Automation
Google Cloud Composer
Cloud Composer, a managed workflow orchestration service built on Apache Airflow, automates the data pipeline. It ensures tasks are executed in the correct sequence and manages dependencies between tasks.

DAG (Directed Acyclic Graph) Definition
Purpose: Orchestrates and automates the data pipeline for fetching, processing, and loading stock data into Google BigQuery.
Importance: Automates the workflow, ensuring data is regularly updated and available for analysis and visualization in Looker.

DAG Configuration
Default Arguments:
owner: 'airflow'
depends_on_past: False
start_date: datetime(2023, 1, 1)
email_on_failure: False
email_on_retry: False
retries: 1
retry_delay: timedelta(minutes=5)
DAG Name and Schedule:
DAG Name: 'stock_data_pipeline'
Schedule Interval: '@monthly'

Task Details
fetch_stock_data:
Fetches monthly adjusted stock data from the Alpha Vantage API.
process_stock_data:
Processes the raw stock data fetched from the API.
load_data_to_bigquery:
Loads the processed data into Google BigQuery.
Task Dependencies
Sequence: fetch_task >> process_task >> load_task

Data Visualization
Looker Dashboards
Looker is used to create interactive dashboards, connecting to BigQuery for real-time querying and visualization of the processed data. Key visualizations include:

Stock Price Trends: Line charts showing the trends of stock prices over time.
Volume Comparison: Bar charts comparing the trading volumes of different companies.
Dividend vs Volume: Scatter plots analyzing the correlation between dividends and trading volume.
Price Comparisons: Combined charts for comparing opening, high, low, and closing prices.

Chapter 3: Summary and Outlook
Own Results
The automated data pipeline effectively collected, processed, and visualized stock data, providing valuable insights through interactive dashboards. The use of GCP ensured scalability and reliability, while Looker enabled intuitive and comprehensive data exploration.

Conclusion
The project showcases an end-to-end automated data pipeline that leverages GCP services to provide a scalable and reliable solution for real-time stock data analysis. The integration with Looker enhances data exploration and visualization, delivering actionable insights for business analysis.
