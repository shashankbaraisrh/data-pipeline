# Stock Data ETL Pipeline Project

This project implements an automated **ETL (Extract, Transform, Load)** pipeline for real-time stock data using **Alpha Vantage API** and **Google Cloud Platform (GCP)** services. The pipeline collects, processes, and visualizes stock data, while Looker dashboards provide actionable insights into stock trends and investor behavior.

---

## Project Structure

### Chapter 1: Dataset

**Data Source**

* Alpha Vantage API provides detailed financial data, including monthly adjusted stock prices and essential financial metrics.

**Data Features**

* **Date**: Date of stock data
* **Open**: Opening price
* **High**: Highest price
* **Low**: Lowest price
* **Close**: Closing price
* **Adjusted Close**: Closing price adjusted for corporate actions
* **Volume**: Number of shares traded
* **Dividend Amount**: Dividend paid by the company
* **Symbol**: Stock symbol

**Data Preparation**

* During development, the **Faker library** was used to generate synthetic data to simulate API requests and avoid hitting API rate limits. The synthetic data mimicked Alpha Vantage data structure for realistic testing.

---

### Chapter 2: Solution

**Data Collection**

* **Python Script** fetches data from Alpha Vantage API:

  * Initialize API client with API key
  * Loop through 20 company symbols to fetch monthly adjusted stock data
  * Save data as CSV files
  * Introduce delays (e.g., 12 seconds) to comply with rate limits

**Data Processing**

* **Google Cloud Functions** process new files in **Google Cloud Storage (GCS)**:

  * Trigger on new CSV upload
  * Clean invalid or missing entries
  * Aggregate data to compute monthly statistics
  * Transform data for BigQuery
  * Load processed data into BigQuery

**Data Storage**

* **BigQuery** stores processed data organized by company and date:

  * Create tables and define schema
  * Load processed data from Cloud Functions

**Pipeline Automation**

* **Google Cloud Composer** (managed Apache Airflow) automates the ETL workflow:

  * **DAG Definition** orchestrates fetching, processing, and loading stock data
  * **Default Arguments**:

    * owner: 'airflow'
    * depends_on_past: False
    * start_date: datetime(2023, 1, 1)
    * email_on_failure: False
    * email_on_retry: False
    * retries: 1
    * retry_delay: timedelta(minutes=5)
  * **DAG Name**: 'stock_data_pipeline'
  * **Schedule**: '@monthly'

**Task Details**

* `fetch_stock_data`: Fetches monthly stock data from API
* `process_stock_data`: Processes raw stock data
* `load_data_to_bigquery`: Loads processed data into BigQuery

**Task Sequence**

* `fetch_stock_data >> process_stock_data >> load_data_to_bigquery`

**Data Visualization**

* **Looker Dashboards** connect to BigQuery for real-time visualizations:

  * Stock Price Trends: Line charts over time
  * Volume Comparison: Bar charts for company trading volumes
  * Dividend vs Volume: Scatter plots for correlation analysis
  * Price Comparisons: Combined charts for open, high, low, close prices

---

### Chapter 3: Summary and Outlook

**Results**

* The automated pipeline efficiently collects, processes, and visualizes stock data.
* GCP services ensure scalability and reliability.
* Looker dashboards enable intuitive, interactive data exploration.

**Conclusion**

* The project demonstrates a fully automated, end-to-end ETL pipeline using GCP.
* Real-time stock data analysis and visualization is made scalable, reliable, and actionable for business insights.
