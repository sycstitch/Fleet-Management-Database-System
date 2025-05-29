# Fleet Management Database System

A comprehensive SQL-based project simulating a real-world taxi fleet operation, with deep-dive analysis into trip data, vehicle usage, customer behavior, and driver specialization.



## 🔍 Project Summary

This project showcases the full lifecycle of a MySQL database-driven analytics system—designed, queried, and analyzed from scratch. It's structured as a professional-grade case study covering fleet performance, operational efficiency, and customer insights through progressively complex SQL.

The queries are grouped into **Beginner**, **Intermediate**, and **Advanced** levels, culminating in a strategic business analysis section that mirrors the kind of insights expected from an in-house data analyst or BI team.



## 📊 Skills Demonstrated

- **SQL Mastery:** JOINs, GROUP BYs, HAVING, CASE statements, subqueries, window functions, and CTEs
- **Analytical Thinking:** Each query includes a written business rationale and conclusion—not just code
- **Relational Modeling:** Understanding and usage of normalized schema with proper keys and relationships
- **Operational Storytelling:** Synthesizes technical outputs into business-focused recommendations



## 🧠 Key Insights

- Clear differentiation between high-volume drivers and high-responsibility drivers using workload metrics
- Identification of high-value customers for loyalty targeting
- Revenue concentration and potential collection issues from a small customer subset
- Maintenance prioritization based on vehicle utilization data
- Certification bottlenecks in workforce utilization

> See [`queries.md`](./SQL/queries.md) for full analysis and output.



## 🛠️ Tech Stack

- **Database:** MySQL / MariaDB
- **Language:** SQL
- **Tools:** CLI, local MySQL instance



## 🗂️ Repository Structure

```

├── fleet\_database.sql       # Database creation script
├── fleet\_schema.txt         # Table descriptions
├── fleet\_queries.md         # Query logic + business conclusions
└── README.md                 # You're here

````



## 🚧 Future Improvements (Planned)

While this project is already strong in SQL depth and business logic, these are on the roadmap to improve its real-world utility:

- [ ] Add Python integration to run queries and export to CSV or Excel
- [ ] Basic visualizations using Tableau or matplotlib (e.g. trip volume, driver workload)
- [ ] Stored procedures for common tasks like monthly driver summaries
- [ ] Performance improvements via indexing and query profiling
- [ ] Expanded scenario: fuel cost analysis and dynamic pricing strategies



## ⚙️ Setup (optional)

```bash
mysql -u your_user -p
CREATE DATABASE fleet_database;
USE fleet_database;
SOURCE fleet_database.sql;
````

This will create all schema and data locally so you can run the queries yourself.



## 📫 Contact

If you're a hiring manager or peer and want to chat about SQL, analytics, or extending this project, feel free to reach out via GitHub or LinkedIn.
