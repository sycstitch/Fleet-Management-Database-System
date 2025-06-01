# Fleet SQL Analytics

![MySQL](https://img.shields.io/badge/MySQL-005C84?style=for-the-badge&logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

*Because someone needs to figure out why Driver 101 keeps taking all the short trips while Driver 104 handles the marathon routes*

A comprehensive SQL project that simulates real taxi fleet operations—complete with the messy business questions, competing priorities, and "wait, why is this customer's balance so high?" moments you'd actually encounter as a data analyst.

## What This Actually Is

I built this to demonstrate SQL skills that matter in the real world. Not just "write a JOIN"—but "write a JOIN, then explain why this customer might be getting screwed on pricing, and what we should do about it."

The project covers everything from basic SELECT statements to complex window functions, but more importantly: **every query comes with business context and actionable conclusions**. Because knowing SQL syntax is table stakes—knowing what the results *mean* is what gets you hired.

## 🎯 The Story

You're the new data analyst at a small taxi company. The owner hands you a database and says "figure out what's going on with our drivers and customers." 

- Why does Driver 101 have the most trips but Driver 104 seems to work harder?
- Should we be worried about that $4,300 in outstanding customer balances?
- Which vehicles are about to break down?
- Are our certified drivers actually better than the unlicensed ones?

This project answers those questions using 18 progressively complex SQL queries, from beginner-friendly filters to advanced CTEs that would make a senior analyst nod approvingly.

## 💡 What You'll See

**Technical Skills:**
- **Schema Design:** Properly normalized tables with realistic relationships
- **Query Complexity:** Beginner → Intermediate → Advanced progression
- **Real SQL:** JOINs, subqueries, window functions, CTEs—the works
- **Business Logic:** Every query explains *why* you'd run it and *what* the results mean

**Analytical Thinking:**
- Driver specialization analysis (volume vs. responsibility)
- Customer segmentation and loyalty opportunities  
- Fleet maintenance prioritization
- Revenue concentration and collection risks
- Workforce optimization recommendations

## 🔍 Sample Insights

> *"While Driver 101 has the most trips (4), Driver 104 carries greater responsibility with higher-intensity routes averaging 1,051 miles per trip versus 101's 598 miles. This specialization maximizes both efficiency and driver expertise."*

> *"Only 45% of employees are certified drivers, creating operational bottlenecks. The 6 non-certified employees represent untapped capacity that could alleviate workload pressure."*

> *"Customer Dunne appears twice with both trips to downtown, suggesting loyalty program opportunities for location-based discounts."*

## 📁 What's Inside

```
├── database.sql           # Complete schema + sample data
├── queries.md             # 18 queries with business analysis
├── schema.md              # Database structure documentation
├── notes.md               # My process notes (the messy stuff)
└── README.md              # You're here
```

## 🚀 Running It Yourself

```bash
# Create the database
mysql -u your_user -p
CREATE DATABASE fleet_analytics;
USE fleet_analytics;
SOURCE database.sql;

# Now run queries from queries.md
```

**Or** just read through `queries.md`—it's designed to be readable without actually running the code.

## 🎨 Why This Project Works

**For Hiring Managers:** Every query includes business context. You can see I don't just write SQL—I solve problems with SQL.

**For Fellow Developers:** The progression from basic queries to complex CTEs shows technical growth. The schema design demonstrates understanding of normalization and relationships.

**For Data People:** The conclusions aren't just "here's what the data shows"—they're "here's what we should *do* about what the data shows."

## 🔧 What's Next

This project is strong, but I'm already thinking about extensions:
- [ ] Python integration for automated reporting
- [ ] Tableau dashboards for the visual learners
- [ ] Performance optimization (indexing, query profiling)
- [ ] Machine learning for driver-customer matching

## 💭 The Real Talk

This project exists because I wanted to show SQL skills in context—not just technical ability, but the kind of analytical thinking that actually moves the needle in business. Anyone can write a JOIN; fewer people can explain why the results matter and what to do next.

If you're a hiring manager looking for someone who can bridge the gap between technical skills and business impact, or a fellow developer interested in extending this project, let's chat.

---

*Built with MySQL, curiosity, and the firm belief that good data analysis should always answer "so what?" and "now what?"*