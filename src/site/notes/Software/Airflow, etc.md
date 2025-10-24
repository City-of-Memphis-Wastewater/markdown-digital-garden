---
{"dg-publish":true,"permalink":"/software/airflow-etc/","noteIcon":"","created":"2025-10-24T11:55:54.177-05:00"}
---

Date: [[-Daily Activity Log-/2025 10-October 24\|2025 10-October 24]]

The `pipeline-eds` software is an **open-source utility tool** designed for **interacting with an Emerson Ovation EDS Rest API** (Video Description). Its primary purposes include:

- **Plotting data:** Visualizing data retrieved from the EDS API (Video Description).
- **Transmitting data to third parties:** Sending data from the EDS API to other systems, such as RJN and Mission (0:58-1:03, 9:36-9:53, Video Description).
- **CLI (Command Line Interface) functionality:** Providing a command-line interface for users to interact with the system and its various clients (0:26, 2:18-2:28, Video Description).
- **System Integration:** Its `setup` (formerly `install`) command helps integrate the application with the operating system by creating AppData folders, context menu items on Windows, and Termux Widget shortcuts (Video Description).
- **Client Management:** It's built to manage multiple API clients (EDS, Mission, RJN) (0:46-0:52, 9:34-9:46), with a vision to expand its data transmission capabilities between these clients.

### Open-Source Alternatives for REST API Integration and Configuration:

While `pipeline-eds` is tailored for Emerson Ovation EDS, several open-source tools offer more generalized and potentially easier configuration for various REST APIs, especially for data integration, transmission, and plotting:

- **For Data Integration and Transmission:**
    
    - **Airbyte:** An open-source ELT (Extract, Load, Transform) platform that provides a wide range of connectors for moving data between various sources and destinations via APIs. It's known for its user-friendly interface and can be orchestrated with tools like Airflow.
    - **AtroCore:** A highly customizable open-source data integration platform specifically built around REST APIs for synchronizing different systems like ERP, PIM, and CRM. It supports both manual and automated data exchange through configurable feeds.
    - **DreamFactory:** This free, open-source REST API middleware platform helps you connect to various data sources and generate APIs instantly. It includes granular security controls and allows for custom endpoint creation.
    - **Kong:** While primarily an API gateway, Kong routes and secures API calls and supports REST natively. Its plugin ecosystem can extend its functionality for logging and analytics, which could be part of a data transmission pipeline.
- **For Data Plotting:**
    
    - **Matplotlib:** A widely used Python library for creating static, animated, and interactive visualizations from your data.
    - **GnuPlot:** A command-driven plotting program well-suited for scientific data visualization.
    - **Octave:** A high-level language with extensive plotting capabilities, primarily used for numerical computations.

These tools offer a more generic approach to connecting to and managing various REST APIs, potentially providing a more streamlined configuration experience if your needs extend beyond the specific Emerson Ovation EDS system.


```https://airbyte.com/top-etl-tools-for-sources/rest-api
## **What is ETL?**

[ETL (Extract, Transform, Load)](https://airbyte.com/data-engineering-resources/what-is-etl) is a process used to extract data from one or more data sources, transform the data to fit a desired format or structure, and then load the transformed data into a target database or data warehouse. ETL is typically used for batch processing and is most commonly associated with traditional data warehouses.

## ‍**What is ELT?**

More recently, ETL has been replaced by [ELT (Extract, Load, Transform)](https://airbyte.com/data-engineering-resources/what-is-elt). ELT Tool is a variation of ETL one that automatically pulls data from even more heterogeneous data sources, loads that data into the target data repository - databases, data warehouses or data lakes - and then performs data transformations at the destination level. ELT provides [significant benefits](https://airbyte.com/blog/etl-vs-elt-the-key-differences) over ETL, such as:

- Faster processing times and loading speed
- Better scalability at a lower cost
- Support of more data sources (including Cloud apps), and of unstructured data
- Ability to have no-code data pipelines
- More flexibility and autonomy for data analysts with lower maintenance
- Better data integrity and reliability, easier identification of data inconsistencies
- Support of many more automations, including automatic schema change migration

For simplicity, we will only use ETL as a reference to all data integration tools, ETL and ELT included, to integrate data from .



```
#ETL