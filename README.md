Sales & Customer Segmentation Analysis (RFM Model)
A Power BI dashboard that applies RFM (Recency, Frequency, Monetary) customer segmentation to e-commerce order data (Superstore 2025). This project models transactional history into actionable customer insights to help business teams optimize target marketing and retention strategies.

📸 Dashboard Overview
🛠 Project Architecture & Data Model
The analysis is structured around dynamic Data Analysis Expressions (DAX) using calculated tables, calendar modeling, and relational schema.
<img width="1175" height="660" alt="Screenshot 2026-08-06 080210" src="https://github.com/user-attachments/assets/388ea978-93f3-4f57-9c2e-f5bf7ced8684" />



1. Data Schema (Star Schema Design)
Superstore 2025: Fact table containing order transactions, product sales, categories, and customer attributes.

RFM TABLE: Calculated dimension table derived via DAX to compute individual metrics per customer.

DATETABLE: Dynamic master calendar table enabling Time-Intelligence analytics across fiscal years and quarters.

🧮 Data Modeling & DAX Implementation
Dynamic Date Table Generation (DATETABLE)
Code snippet
DATETABLE = 
ADDCOLUMNS(
    CALENDAR(DATE(2021, 01, 01), DATE(2025, 12, 31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Year-Month", FORMAT([Date], "YYYY-MM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Day", DAY([Date]),
    "Day of Week", WEEKDAY([Date], 2),
    "Day Name", FORMAT([Date], "dddd"),
    "WeekNumber", WEEKNUM([Date], 2)
)
<img width="1882" height="812" alt="Screenshot 2026-08-06 080329" src="https://github.com/user-attachments/assets/5da59b02-d28b-43fe-9381-a545ec03cc34" />

RFM Aggregation Table (RFM TABLE)
Code snippet
RFM TABLE = 
SUMMARIZE(
    'Superstore 2025',
    'Superstore 2025'[Customer ID],
    'Superstore 2025'[Customer Name],
    "Recency", 
        DATEDIFF(
            MAX('Superstore 2025'[Order Date]),
            CALCULATE(MAX('Superstore 2025'[Order Date]), ALL('Superstore 2025')),
            DAY
        ),
    "Frequency", DISTINCTCOUNT('Superstore 2025'[Order ID]),
    "Monetary", SUM('Superstore 2025'[Sales])
)
<img width="1867" height="822" alt="Screenshot 2026-08-06 080439" src="https://github.com/user-attachments/assets/d1332e2a-5afb-4107-b505-2b4634a6eae0" />


🎯 Key Insights & Metrics
Recency: Days elapsed since the customer's last order relative to the latest date in the dataset.

Frequency: Total unique order count per customer (DISTINCTCOUNT).

Monetary Value: Cumulative spending per customer.

Customer Segments:

Big Spenders: High monetary yield, active recent purchases.

Loyal Customers: High frequency across multiple order cycles.

At Risk / Churn Candidates: High past value, extended inactive recency period.

Others: Standard low-touch order behavior.

💻 Tech Stack & Tools
Business Intelligence: Power BI Desktop

Language: DAX (Data Analysis Expressions)

Data Source: Superstore Transaction Dataset

🚀 How to Run Locally
Clone this repository:

Bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git
Open the .pbix file in Power BI Desktop.

Refresh data connections if prompting for local paths.<img width="1175" height="660" alt="Screenshot 2026-08-06 080210" src="https://github.com/user-attachments/assets/3a76f0a5-f0c1-42f9-8376-6ffedb57a592" />
