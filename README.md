# End-to-End-data-pipeline-for-public-employment-data-using-MS-Fabric

**Sources:** France Travail & Dares

**Technologies:** Microsoft Fabric · Azure Data Factory · SQL · PySpark · Power BI Service

## Objectives
**Business:** Understand job market trends in France (2015–2024).

**Technical:** Build an automated end-to-end pipeline with Microsoft Fabric.

## Datasets
**Job Offers**: Jobs collected and filled, by department (including French overseas territories), with duration type (short/long-term).

**Job Seekers**: National level data by age and category.



## Process Recap

<img width="2200" height="1236" alt="Schema_Recap_process_ETL" src="https://github.com/user-attachments/assets/3cf5c62d-d622-407f-83d5-46bb583b864d" />



  ### From Bronze to Gold layer

**Ingestion**: From Data.Gouv website to the lakehouse storage using the MS Fabric native activity "Copy Data" 

**Major Transformations**:

Tranformations are made using PySpark notebooks.

- Cleaning, type corrections, handling null values

- Adding columns (Perimètre, Age groups, GeographyKey)

- Filtering for the last 10 years

| Data          | Bronze Layer  | Silver Layer  | Gold Layer|
| ------------- |:-------------:| -------------:|:-------------:|
| Geography table     | `31 sec` `91 866 lines` | `1min 17 sec`   `120 lines` Removing duplicate values and adding columns Pivoting columns       |         $1600 |
| Job seekers (*Offres*) table      | `31 sec` `91 866 lines`       |   $12         |         $1600 |
| Job seekers (*Inscrits*) table | `41 sec`   `128 128 lines`       |    $1         |         $1600 |

**Monitoring**: Track pipeline executions in a data warehouse. Every pipeline run, no matter the outcome, is registered in a tailored table. This table is also used as a lookup table.

**Alerting:** Automatic email in case of failure with a message with all pipeline log and further information to quickly identify the bottleneck.

<img width="903" height="318" alt="ETL process  to LKH _ data offres d'emploi" src="https://github.com/user-attachments/assets/2ba168fd-928e-4e66-9bbe-a011a2bfd32a" />

*####Screenshot of Pipeline 'Offres et Geography France Travail' - Exécution Time : 5min 15sec*



  ### From Lakehouse to Datawarehouse
Pipelines are schedueled to be triggered automatically every 30 days (or manually if needed)

Child pipelines run are automatically executed when the parent pipeline is executed.

Conditional update if last execution > 30 days

Creation of a Date table (20 years) for temporal analysis

<img width="2539" height="1002" alt="ETL process to DWH _ Fact Offres et DimGeo" src="https://github.com/user-attachments/assets/d2a3e4d5-1bfc-4809-b02e-14bffc4d3e4c" />

## Table transformation

<img width="2178" height="1218" alt="Tables_per_layer" src="https://github.com/user-attachments/assets/65a52dd0-eed1-4660-bfe6-4a6b943edbf8" />





## Semantic model
Facts tables: job seekers (*Inscrits*), job offers (*Offres*)

Dimensions tables: geography, date

---

## Dashboard (Power BI Service)

<img width="2292" height="1296" alt="Dashboard 2 France Travail KPI" src="https://github.com/user-attachments/assets/ae344a54-e2ed-48d8-a7d9-077ec2b9f7b7" />


**Key Indicators (example in 2019)**:

- Job seekers: 214.7k (-1.26% vs. previous year)

- Collected offers: 3.23M (-2.78% vs. previous year)

- Offers per 100 job seekers: 1.51

- Share of long-term jobs: 59%

**Possible Analyses**:
- Monthly and yearly evolution for job seekers and collected job offers

- Distribution of job offers by department and job seekers by age

- Top 5 departments with the most job offers collected

**Example of concrete use cases:**

The dashboard allows local authorities to identify stressed departments and target resources to the most affected populations (youth, seniors).

At the national level, it helps decision-makers (Dares, Ministry of Labor) track macroeconomic trends, evaluate public policy impact, and compare regional disparities.


