# How to Reproduce the Ride Booking Analytics Dashboard

## STQD6324 Data Management Assignment

This document explains how to reproduce the **Ride Booking Analytics Dashboard** developed for the STQD6324 Data Management project.

The repository provides **three different implementation approaches**, demonstrating different methods of integrating **Apache Hive**, **HDFS**, and **R Shiny**.

The three approaches are:

| Method | Description | Recommended For |
|---------|-------------|-----------------|
| **Method 1** | Hive → WinSCP → CSV → R Shiny | Users without Apache Hive (Recommended) |
| **Method 2** | Hive → ODBC → One Hive Table → R Shiny | Users with Apache Hive installed |
| **Method 3** | Hive → ODBC → Multiple Analytical Hive Tables → R Shiny | Demonstrating an enterprise-style data warehouse architecture |

> **Recommendation**
>
> If you simply want to experience the dashboard, **Method 1** is strongly recommended because it does **not require Apache Hive or Hadoop** after the cleaned dataset has been exported. Only **RStudio** is required.

---

# General Requirements

All methods require:

- Windows 10/11
- R 4.x
- RStudio

Additional requirements:

Method 2
- Hadoop Sandbox
- Hive
- ODBC Driver

Method 3
- Hadoop Sandbox
- Hive
- ODBC Driver

---

# Repository

Project Repository

<https://github.com/p161565-Lucas/Final-Report-STQD6324-Data-Management-SEMESTER-2-2025-2026-main-project>

---

# Required Software

Different reproduction methods require different software.

| Software | Method 1 | Method 2 | Method 3 |
|-----------|:--------:|:--------:|:--------:|
| Windows | ✔ | ✔ | ✔ |
| R | ✔ | ✔ | ✔ |
| RStudio | ✔ | ✔ | ✔ |
| Apache Hadoop Sandbox (HDP) | ✘ | ✔ | ✔ |
| Apache Hive | ✘ | ✔ | ✔ |
| HDFS | ✘ | ✔ | ✔ |
| Cloudera Hive ODBC Driver | ✘ | ✔ | ✔ |
| WinSCP | ✔ (Export CSV) | Optional | Optional |

---

# Repository Files

The repository contains the following important files.

| File | Description |
|------|-------------|
| Bookings.csv | Original ride booking dataset |
| Bookings_Cleaned_Hive.csv | Cleaned dataset exported from Hive |
| Data_management_final_connect_hive_one_table_stable.R | Recommended R Shiny dashboard |
| Data_management_final_connect_hive_ten_tables_clear.R | Data warehouse implementation |
| Ride_Booking_Big_Data_Project_Beautified.pdf | Complete Hive command guide |
| Ride Booking Big Data Project – Hive Data Cleaning Command.pdf | Hive data cleaning commands |
| ClouderaHiveODBC64.msi | Hive ODBC Driver |
| WinSCP-6.5.6-Setup.exe | WinSCP installer |
| www/ | Dashboard image resources |
| screenshots/ | Dashboard screenshots |

---

# Method 1 (Recommended)

# Reproduce the Dashboard Using the Exported CSV File

This is the easiest way to reproduce the dashboard.

No Apache Hive installation is required during dashboard execution.

The dashboard reads the cleaned CSV file exported from Hive instead of connecting directly to the Hive database.

This method is recommended for instructors, reviewers, and users who simply wish to experience the dashboard.

---

# Step 1 Download the Repository

Download or clone this repository.

Alternatively, download the following files individually.

## Required Files

```
Bookings_Cleaned_Hive.csv

Data_management_final_connect_hive_one_table_stable.R

www/
```

If you plan to export the cleaned CSV file yourself from Hive, you will also need

```
WinSCP-6.5.6-Setup.exe
```

---

# Step 2 (Optional) Export the Cleaned Dataset from Hive

If you already downloaded

```
Bookings_Cleaned_Hive.csv
```

from this repository,

**you may skip this step.**

Otherwise,

follow the Hive commands provided in

```
Ride_Booking_Big_Data_Project_Beautified.pdf
```

and

```
Ride Booking Big Data Project – Hive Data Cleaning Command.pdf
```

to

- Upload the original dataset to HDFS
- Create the Hive database
- Create the raw Hive table
- Clean the dataset
- Generate

```
bookings_cleaned
```

After the cleaned table has been generated,

install

```
WinSCP-6.5.6-Setup.exe
```

Connect to the Hadoop Sandbox and export the cleaned CSV file from HDFS.

Save the exported file as

```
Bookings_Cleaned_Hive.csv
```

on your local computer.

---

# Step 3 Install Required R Packages

Open RStudio and install the required packages.

```r
install.packages(c(
  "shiny",
  "shinydashboard",
  "tidyverse",
  "lubridate",
  "plotly",
  "DT",
  "scales",
  "dplyr",
  "DBI",
  "odbc"
))
```

Load the packages.

```r
library(shiny)
library(shinydashboard)
library(tidyverse)
library(lubridate)
library(plotly)
library(DT)
library(scales)
library(dplyr)
library(DBI)
library(odbc)
```

---

# Step 4 Open the Dashboard Source Code

Open

```
Data_management_final_connect_hive_one_table_stable.R
```

using RStudio.

---

# Step 5 Modify the CSV File Path

Locate the following code.

```r
df <- read.csv(
  "C:/Users/PC 20/Desktop/Bookings_Cleaned_Hive.csv",
  header = FALSE,
  stringsAsFactors = FALSE
)
```

Replace the file path with the location of your downloaded

```
Bookings_Cleaned_Hive.csv
```

Example

```r
df <- read.csv(
  "D:/RideBooking/Bookings_Cleaned_Hive.csv",
  header = FALSE,
  stringsAsFactors = FALSE
)
```

---

# Step 6 Enable the CSV Loading Section

Locate the following section inside the R script.

```text
##################################################################
(2) Data cleaning by hive and export the cleaned csv file from hive.
##################################################################
```

By default, this entire block is commented out because the dashboard is configured to connect directly to Hive.

Remove the comment symbols (`#`) from the **entire section**, including

- read.csv(...)
- colnames(...)
- Missing value processing
- Date conversion
- Numeric conversion
- Feature engineering
- Revenue_Category
- Distance_Category
- Is_Cancelled
- Is_Success

After uncommenting this block, the dashboard will load data directly from the exported CSV file instead of Apache Hive.

---

# Step 7 Disable the Hive Connection

Locate the following section.

```text
==========================================================
CONNECT TO HIVE AND READ DATA
==========================================================
```

Comment out the entire Hive connection block.

This section includes

```r
con <- dbConnect(...)
```

```r
df <- dbGetQuery(...)
```

and

```r
dbDisconnect(con)
```

Since the dashboard now reads the cleaned CSV file directly, the Hive connection is no longer required.

---

# Step 8 Configure the Dashboard Images (Optional)

The dashboard uses image resources stored inside the

```
www
```

folder.

Download the

```
www
```

folder from this repository.

Locate the following code.

```r
addResourcePath(
    "images",
    "C:/Users/PC 20/Desktop/data_management_2/www"
)
```

Replace the path with the location of your downloaded

```
www
```

folder.

Example

```r
addResourcePath(
    "images",
    "D:/RideBooking/www"
)
```

If this step is skipped, the dashboard will still run correctly, but the custom image displayed on the dashboard homepage will not appear.

---

# Step 9 Run the Dashboard

After completing all previous steps,

click

```
Run App
```

inside RStudio,

or execute

```r
runApp()
```

The dashboard should now launch successfully without requiring Apache Hive, Hadoop, HDFS, or an ODBC connection.

This is the simplest and fastest way to reproduce the project.


---



# Method 2

# Reproduce the Dashboard Using Apache Hive and a Single Hive Table

This method demonstrates the complete integration between **Apache Hive** and **R Shiny** using **ODBC**.

Unlike Method 1, the dashboard does **not** read a local CSV file. Instead, it connects directly to Apache Hive and retrieves the cleaned dataset from a single Hive table.

This method reflects a traditional database-connected dashboard architecture and eliminates the need to manually export CSV files.

---

# Workflow

The overall workflow is shown below.

```
Bookings.csv

        │

        ▼

      HDFS

        │

        ▼

  Apache Hive

        │

        ▼

Hive Data Cleaning

        │

        ▼

 bookings_cleaned

        │

        ▼

  ODBC Connection

        │

        ▼

     R Shiny
```

---

# Step 1 Prepare the Hadoop Environment

A working Hadoop Sandbox (HDP) environment is required.

The following components should already be installed and running.

- Apache Hadoop
- HDFS
- Apache Hive
- Ambari

Make sure that the Hive service is running normally before continuing.

---

# Step 2 Upload the Original Dataset

Download

```
Bookings.csv
```

from this repository.

Create a directory in HDFS.

```bash
hdfs dfs -mkdir /ride_final
```

Upload the dataset.

```bash
hdfs dfs -put Bookings.csv /ride_final
```

Verify that the upload was successful.

```bash
hdfs dfs -ls /ride_final
```

---

# Step 3 Create the Hive Database and Tables

Open Hive or Beeline.

Follow all commands provided in the following documents.

```
Ride_Booking_Big_Data_Project_Beautified.pdf
```

and

```
Ride Booking Big Data Project – Hive Data Cleaning Command.pdf
```

These documents contain the complete commands required to

- Create the Hive database
- Create the raw booking table
- Import the dataset
- Perform data cleaning
- Generate the cleaned Hive table

After completing all commands, the following table should exist.

```
bookings_cleaned
```

This table will be used by the dashboard.

---

# Step 4 Install the Hive ODBC Driver

Install

```
ClouderaHiveODBC64.msi
```

from this repository.

Complete the installation using the default settings.

After installation,

open

```
ODBC Data Source Administrator (64-bit)
```

in Windows.

---

# Step 5 Configure the Hive ODBC Connection

Configure the ODBC connection according to the settings shown in

```
screenshots/ODBC.png
```

Typical settings are

| Setting | Value |
|----------|-------|
| Driver | Cloudera ODBC Driver for Apache Hive |
| Host | localhost |
| Port | 10000 |
| Database (Schema) | ride_booking |
| User Name | maria_dev |
| Password | *(leave blank unless required)* |

If your Hive installation uses different settings, modify them accordingly.

After configuration, test the connection.

The connection should succeed before proceeding.

---

# Step 6 Download the Dashboard Source Code

Download

```
Data_management_final_connect_hive_one_table_stable.R
```

Open it using RStudio.

---

# Step 7 Verify the Hive Connection Code

Locate the following section.

```text
==========================================================
CONNECT TO HIVE AND READ DATA
==========================================================
```

The dashboard connects to Hive using

```r
con <- dbConnect(
  odbc(),
  Driver = "Cloudera ODBC Driver for Apache Hive",
  Host = "localhost",
  Port = 10000,
  Schema = "ride_booking",
  UID = "maria_dev",
  PWD = ""
)
```

The cleaned dataset is retrieved using

```r
df <- dbGetQuery(
    con,
    "SELECT * FROM bookings_cleaned"
)
```

Finally,

```r
dbDisconnect(con)
```

closes the database connection.

If your Hive configuration differs,

modify the connection parameters accordingly.

---

# Step 8 Disable the CSV Reading Section

Locate the following section.

```text
##################################################################
(2) Data cleaning by hive and export the cleaned csv file from hive.
##################################################################
```

This section is only used when reading the exported CSV file.

For Method 2,

leave this entire block commented out.

The dashboard should retrieve data directly from Hive.

---

# Step 9 Configure the Dashboard Images (Optional)

Download the

```
www
```

folder from this repository.

Locate

```r
addResourcePath(
    "images",
    "C:/Users/PC 20/Desktop/data_management_2/www"
)
```

Replace the path with your local

```
www
```

directory.

Example

```r
addResourcePath(
    "images",
    "D:/RideBooking/www"
)
```

If omitted,

the dashboard will still function correctly,

but the homepage image will not be displayed.

---

# Step 10 Run the Dashboard

Open

```
Data_management_final_connect_hive_one_table_stable.R
```

Click

```
Run App
```

or execute

```r
runApp()
```

The dashboard will now connect directly to Apache Hive,

retrieve the

```
bookings_cleaned
```

table,

and generate all visualizations dynamically.

---

# Expected Result

If all steps have been completed successfully,

the dashboard should

- Connect to Apache Hive through ODBC
- Retrieve the cleaned Hive table
- Perform filtering and aggregation in R
- Display all dashboard components normally
- Require no manually exported CSV file

This implementation represents the standard database-connected architecture used throughout the project and is the recommended Hive-based implementation because it is more stable than the multiple-table architecture while still demonstrating direct Hive integration.

---


---

# Method 3

# Reproduce the Dashboard Using Multiple Hive Analytical Tables (Enterprise Data Warehouse Architecture)

Method 3 demonstrates the final optimized implementation of this project.

Instead of retrieving one large cleaned table from Hive, Apache Hive generates multiple analytical tables during the data warehousing stage. Each dashboard component retrieves only the table that it requires.

This approach significantly reduces unnecessary data transfer between Hive and R and better reflects an enterprise-style Business Intelligence (BI) architecture.

Although this architecture is theoretically more efficient, it may occasionally experience Hive ODBC string or schema compatibility issues depending on the execution environment.

---

# Workflow

```
Bookings.csv

        │

        ▼

      HDFS

        │

        ▼

   Apache Hive

        │

        ▼

Data Cleaning

        │

        ▼

Generate Multiple Analytical Tables

        │

        ▼

ODBC Connection

        │

        ▼

     R Shiny Dashboard
```

---

# Step 1 Prepare the Hadoop Environment

Before reproducing Method 3, make sure the following software has been installed and configured correctly.

- Apache Hadoop Sandbox (HDP)
- HDFS
- Apache Hive
- Apache Ambari
- R
- RStudio
- Cloudera Hive ODBC Driver

Hive should be running normally before continuing.

---

# Step 2 Upload the Dataset

Download

```
Bookings.csv
```

from this repository.

Upload it into HDFS.

```bash
hdfs dfs -mkdir /ride_final

hdfs dfs -put Bookings.csv /ride_final
```

Verify the upload.

```bash
hdfs dfs -ls /ride_final
```

---

# Step 3 Generate the Hive Data Warehouse

Follow the commands provided in

```
Ride_Booking_Big_Data_Project_Beautified.pdf
```

and

```
Ride Booking Big Data Project – Hive Data Cleaning Command.pdf
```

These documents contain the SQL commands required to

- create the Hive database
- create the raw booking table
- clean the raw dataset
- generate all analytical Hive tables

After executing all commands, the following analytical tables should exist inside Hive.

```
dashboard_kpi

dashboard_trend

dashboard_vehicle

dashboard_payment

dashboard_cancel

dashboard_rating

dashboard_detail

dashboard_pickup

dashboard_customer

dashboard_revenue
```

These ten tables provide the data source for the dashboard.

---

# Step 4 Install the Hive ODBC Driver

Install

```
ClouderaHiveODBC64.msi
```

included in this repository.

After installation,

configure the ODBC connection according to

```
screenshots/ODBC.png
```

Typical configuration

| Setting | Value |
|----------|-------|
| Driver | Cloudera ODBC Driver for Apache Hive |
| Host | localhost |
| Port | 10000 |
| Database | ride_booking |
| User Name | maria_dev |
| Password | *(blank)* |

Test the ODBC connection before continuing.

---

# Step 5 Download the Dashboard Source Code

Download

```
Data_management_final_connect_hive_ten_tables_clear.R
```

Open it in RStudio.

---

# Step 6 Verify the Hive Connection

The dashboard connects directly to Hive.

```r
con <- dbConnect(
    odbc(),
    Driver="Cloudera ODBC Driver for Apache Hive",
    Host="localhost",
    Port=10000,
    Schema="ride_booking",
    UID="maria_dev",
    PWD=""
)
```

Modify these parameters if your local Hive configuration is different.

---

# Step 7 Verify the Analytical Tables

The dashboard should retrieve the following Hive tables.

```r
dashboard_kpi
```

```r
dashboard_trend
```

```r
dashboard_vehicle
```

```r
dashboard_payment
```

```r
dashboard_cancel
```

```r
dashboard_rating
```

```r
dashboard_detail
```

```r
dashboard_pickup
```

```r
dashboard_customer
```

```r
dashboard_revenue
```

Each dashboard component retrieves only its corresponding Hive table.

---

# Step 8 Configure the Dashboard Images (Optional)

Download the

```
www
```

folder.

Locate

```r
addResourcePath(
    "images",
    "C:/Users/PC 20/Desktop/data_management_2/www"
)
```

Modify the directory.

Example

```r
addResourcePath(
    "images",
    "D:/RideBooking/www"
)
```

Without this folder,

only the homepage image will be missing.

The dashboard itself will still function correctly.

---

# Step 9 Run the Dashboard

Open

```
Data_management_final_connect_hive_ten_tables_clear.R
```

Click

```
Run App
```

or execute

```r
runApp()
```

The dashboard should retrieve data directly from the ten Hive analytical tables.

---

# Expected Result

After successful execution,

the dashboard should

- Connect directly to Hive
- Retrieve only the required analytical tables
- Reduce unnecessary data transfer
- Demonstrate a modular data warehouse architecture
- Produce the same visualizations as Method 1 and Method 2

---

# Comparison of the Three Methods

| Feature | Method 1 | Method 2 | Method 3 |
|----------|:--------:|:--------:|:--------:|
| Requires Hive | ✘ | ✔ | ✔ |
| Requires ODBC | ✘ | ✔ | ✔ |
| Uses CSV | ✔ | ✘ | ✘ |
| Reads One Hive Table | ✘ | ✔ | ✘ |
| Reads Multiple Hive Tables | ✘ | ✘ | ✔ |
| Easy to Reproduce | ★★★★★ | ★★★★☆ | ★★★☆☆ |
| Demonstrates Data Warehouse | ★☆☆☆☆ | ★★★☆☆ | ★★★★★ |

---

# Recommended Reproduction Method

For most users,

**Method 1** is recommended.

Advantages

- No Hadoop installation required
- No Hive installation required
- No ODBC configuration required
- Fastest setup
- Suitable for project demonstration
- Produces exactly the same dashboard interface

Method 2 is recommended for users who wish to demonstrate database connectivity.

Method 3 is recommended for users interested in reproducing the complete enterprise-style data warehouse architecture.

---

# Verification

After running the dashboard successfully, you should see:

✓ Dashboard homepage

✓ KPI cards

✓ Booking trend chart

✓ Revenue analysis

✓ Payment distribution

✓ Cancellation prediction

✓ Data Explorer

---

# Troubleshooting

## Cannot connect to Hive

Possible causes

- Hive service is not running.
- Incorrect Host.
- Incorrect Port.
- Incorrect Schema.
- Incorrect User Name.
- ODBC Driver not installed.

Solution

- Verify Hive is running.
- Check the ODBC configuration.
- Confirm the database name.
- Test the connection in ODBC Administrator.

---

## ODBC Driver Not Found

Install

```
ClouderaHiveODBC64.msi
```

included in this repository.

Restart RStudio after installation.

---

## CSV File Cannot Be Found

Check

```r
read.csv(...)
```

and verify that the file path points to the downloaded

```
Bookings_Cleaned_Hive.csv
```

---

## Dashboard Image Does Not Appear

Verify

```r
addResourcePath(...)
```

points to the correct

```
www
```

folder.

---

## Dashboard Fails to Launch

Ensure all required R packages have been installed.

Run

```r
install.packages(c(
    "shiny",
    "shinydashboard",
    "tidyverse",
    "plotly",
    "DT",
    "lubridate",
    "dplyr",
    "DBI",
    "odbc",
    "scales"
))
```

Restart RStudio and run the dashboard again.

---

## Hive Tables Cannot Be Found

Verify that all Hive SQL commands in

```
Ride_Booking_Big_Data_Project_Beautified.pdf
```

and

```
Ride Booking Big Data Project – Hive Data Cleaning Command.pdf
```

have been executed successfully.

---

# Frequently Asked Questions (FAQ)

## Which reproduction method should I use?

For most users,

**Method 1** is recommended because it requires only RStudio and the exported CSV dataset.

---

## Do I need Apache Hive for Method 1?

No.

Method 1 uses the exported

```
Bookings_Cleaned_Hive.csv
```

file and does not require Apache Hive or HDFS during dashboard execution.

---

## Why are there three implementations?

The purpose of this project is to compare different data management architectures.

Method 1 demonstrates the traditional CSV-based workflow.

Method 2 demonstrates direct database connectivity.

Method 3 demonstrates an enterprise-style Hive data warehouse architecture using multiple analytical tables.

---

## Why do Method 2 and Method 3 produce the same dashboard?

The dashboard interface is identical.

The difference lies in how the data are retrieved.

Method 2 retrieves one cleaned Hive table.

Method 3 retrieves multiple pre-computed analytical Hive tables.

---

## Which method is the fastest?

Method 1.

---

## Which method best demonstrates database technology?

Method 2.

---

## Which method best demonstrates enterprise data warehousing?

Method 3.

---

# Final Notes

This project demonstrates three progressively more advanced approaches for integrating Apache Hive with R Shiny.

Beginning with a simple CSV-based workflow, progressing to direct Hive connectivity, and finally extending to an enterprise-style analytical data warehouse, the project illustrates how different data management architectures can support the same business intelligence dashboard while varying in scalability, efficiency, and deployment complexity.

Users are encouraged to begin with **Method 1** for quick reproduction and then explore **Method 2** and **Method 3** to better understand the role of Hive, HDFS, ODBC, and data warehousing in large-scale analytics applications.
