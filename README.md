# 📊 SQL Data Job Market Analysis

> **A deep-dive SQL analysis of 2023 data analyst job postings** — uncovering top-paying roles, most in-demand skills, and the optimal skill set to maximize career value in the data job market.

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![VS Code](https://img.shields.io/badge/VS%20Code-007ACC?style=for-the-badge&logo=visual-studio-code&logoColor=white)](https://code.visualstudio.com/)
[![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)](https://git-scm.com/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/uttkarshyadavv)

---

## 📌 Table of Contents

- [Introduction](#-introduction)
- [Background & Motivation](#-background--motivation)
- [Project Structure](#-project-structure)
- [Dataset Overview](#-dataset-overview)
- [Tools & Technologies](#-tools--technologies)
- [The Analysis — 5 Key Questions](#-the-analysis--5-key-questions)
  - [1. Top Paying Data Analyst Jobs](#1-top-paying-data-analyst-jobs)
  - [2. Skills Required for Top-Paying Jobs](#2-skills-required-for-top-paying-jobs)
  - [3. Most In-Demand Skills](#3-most-in-demand-skills)
  - [4. Skills Associated with Higher Salaries](#4-skills-associated-with-higher-salaries)
  - [5. Most Optimal Skills to Learn](#5-most-optimal-skills-to-learn)
- [Advanced SQL Concepts Practiced](#-advanced-sql-concepts-practiced)
- [Key Takeaways](#-key-takeaways)
- [Conclusions](#-conclusions)
- [How to Run This Project](#-how-to-run-this-project)
- [Connect with Me](#-connect-with-me)

---

## 🧭 Introduction

This project is a **comprehensive SQL-based analysis** of the 2023 data analyst job market. The goal was to answer five critical questions that any aspiring data professional would want to know:

- Where is the money? 💰
- What skills do top-paying jobs require? 🔑
- What does the market demand most? 📈
- Which skills command the highest salaries? 🎯
- What's the single most optimal skill to invest in? 🏆

All insights are derived entirely through **SQL queries on a real-world dataset** of job postings — no BI tools, no notebooks, pure SQL.

> 🔍 **SQL Queries:** All project queries are in the [`project_sql/`](./project_sql/) folder.
> 🧪 **Advanced Practice:** Supplementary SQL concept exercises are in [`advanced_sql/`](./advanced_sql/).

---

## 🎯 Background & Motivation

As a **Chemical Engineering student** (SVNIT Surat) actively transitioning into Data Analytics and ML, I built this project to simultaneously sharpen my SQL skills and gain real, data-backed answers about the job market I'm preparing to enter.

Rather than guessing which skills to learn or what salaries to expect, I decided to **query the data itself**. The dataset — sourced from Luke Barousse's SQL for Data Analytics course — contains thousands of real job postings with fields like job title, salary, location, required skills, and company details.

This project covers the full SQL workflow:
- Loading raw CSV data into a relational database
- Writing complex multi-table queries
- Aggregating and filtering to extract meaningful insights
- Drawing actionable conclusions for skill development

---

## 📁 Project Structure

```
SQL-Project/
│
├── project_sql/                  # 5 core analytical queries (main project)
│   ├── 1_top_paying_jobs.sql
│   ├── 2_top_paying_job_skills.sql
│   ├── 3_in_demand_skills.sql
│   ├── 4_top_paying_skills.sql
│   └── 5_optimal_skills.sql
│
├── advanced_sql/                 # Advanced SQL concept practice
│   ├── cases.sql
│   ├── dates.sql
│   ├── subqueries_CTEs.sql
│   ├── unions.sql
│   └── ...
│
├── sql_load/                     # Database setup & CSV data loading scripts
│   ├── create_tables.sql
│   ├── modified_tables.sql
│   └── ...
│
├── .vscode/                      # VS Code workspace settings
├── .gitignore
└── README.md
```

---

## 🗃️ Dataset Overview

The dataset consists of **real 2023 job postings** and is structured across four relational tables:

| Table | Description |
|---|---|
| `job_postings_fact` | Core table — job title, location, salary, schedule type, posting date, work-from-home flag |
| `company_dim` | Company names linked to job postings via `company_id` |
| `skills_dim` | Skill names and types (e.g., Python, SQL, Tableau) |
| `skills_job_dim` | Bridge/junction table linking jobs to their required skills |

**Key fields used in analysis:**
- `salary_year_avg` — average annual salary for the role
- `job_title_short` — standardized job title (e.g., "Data Analyst")
- `job_location` — posting location (`'Anywhere'` = fully remote)
- `job_work_from_home` — boolean remote work flag
- `skills` — specific technical skill name

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **PostgreSQL** | Database management system — all data is stored and queried here |
| **VS Code** | Primary IDE for writing, organizing, and executing SQL scripts |
| **Git & GitHub** | Version control and public portfolio hosting |
| **pgAdmin 4** | GUI for database inspection and visual query testing |

---

## 🔬 The Analysis — 5 Key Questions

Each query targets a specific aspect of the data job market. Together, they form a complete picture of where opportunity and compensation intersect.

---

### 1. Top Paying Data Analyst Jobs

**Question:** What are the highest-paying Data Analyst roles that are fully remote?

**Approach:** Filter for `'Data Analyst'` title, `'Anywhere'` location (remote), exclude null salaries, join with company names, and sort by descending salary.

```sql
SELECT	
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim 
    ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_location = 'Anywhere' 
    AND salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

**Key Findings:**

| Insight | Detail |
|---|---|
| Salary range (top 10) | **$184,000 — $650,000** |
| Top-paying employer | Mantys — $650,000 |
| Other notable employers | SmartAsset, Meta, AT&T |
| Role diversity | Ranges from "Data Analyst" to "Director of Analytics" |

- The salary ceiling for remote data analyst work is **remarkably high** — the spread from $184K to $650K reflects how widely specialization and seniority are rewarded.
- **Diverse industries** are hiring: fintech, social media, telecom — not just tech-pure companies.
- Senior and director-level titles in analytics command disproportionately higher salaries, signaling the value of domain expertise + leadership.

---

### 2. Skills Required for Top-Paying Jobs

**Question:** What specific skills do the top 10 highest-paying Data Analyst jobs require?

**Approach:** Use a CTE to isolate the top 10 paying jobs, then join with `skills_job_dim` and `skills_dim` to reveal required skills per role.

```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim 
        ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' 
        AND job_location = 'Anywhere' 
        AND salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim 
    ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```

**Key Findings:**

| Skill | Frequency in Top 10 |
|---|---|
| SQL | 8 |
| Python | 7 |
| Tableau | 6 |
| R | 4 |
| Snowflake | 3 |
| Pandas | 3 |
| Excel | 3 |

- **SQL is non-negotiable** — present in 8 of 10 top-paying roles. It's the baseline.
- **Python** is nearly as critical, showing that scripting + analysis go hand-in-hand at high salary levels.
- **Tableau** at 6 appearances confirms that data storytelling and visualization are valued even at the highest tiers.
- Niche tools like **Snowflake** appearing in top-paying roles hints at cloud data warehousing as a salary differentiator.

---

### 3. Most In-Demand Skills

**Question:** Which skills appear most frequently across all remote Data Analyst job postings?

**Approach:** Join job postings with skills tables, filter for remote DA roles, group and count skill occurrences.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```

**Key Findings:**

| Skill | Demand Count |
|---|---|
| SQL | 7,291 |
| Excel | 4,611 |
| Python | 4,330 |
| Tableau | 3,745 |
| Power BI | 2,609 |

- **SQL leads by a massive margin** — 7,291 job postings require it. No other skill is even close in raw demand.
- **Excel** remains essential despite being considered "basic" — nearly 4,611 postings list it. Employers still expect it.
- The presence of both **Tableau** and **Power BI** confirms that BI visualization tools are treated as standard requirements, not differentiators.
- **Python** at 4,330 shows the market increasingly expects analysts to code, not just query.

---

### 4. Skills Associated with Higher Salaries

**Question:** Which skills are linked to the highest average annual salaries for Data Analysts?

**Approach:** Filter for DA roles with reported salaries (remote), join with skills, then group by skill and compute average salary — sorted descending.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```

**Key Findings (Top 10 by Avg Salary):**

| Skill | Average Salary (USD) |
|---|---|
| PySpark | $208,172 |
| Bitbucket | $189,155 |
| Couchbase | $160,515 |
| Watson | $160,515 |
| DataRobot | $155,486 |
| GitLab | $154,500 |
| Swift | $153,750 |
| Jupyter | $152,777 |
| Pandas | $151,821 |
| Elasticsearch | $145,000 |

- **Big Data & ML tools dominate the top** — PySpark, DataRobot, and Pandas reflect that ML-adjacent analysts earn significantly more.
- **DevOps/engineering crossover skills** (GitLab, Bitbucket, Kubernetes, Airflow) command premium salaries — indicating employers pay up for analysts who can operate closer to the engineering stack.
- **Cloud & search infrastructure** (Elasticsearch, GCP, Databricks) underscore that cloud fluency is a high-value differentiator.
- Notably, SQL and Excel — the most in-demand skills — are **not** in the top salary list. Volume of demand ≠ salary premium.

---

### 5. Most Optimal Skills to Learn

**Question:** Which skills offer the best combination of high demand AND high salary — i.e., the highest ROI for skill development?

**Approach:** Filter for DA remote roles with salary data, group by skill, compute both demand count and average salary, filter for skills with at least 10 postings (statistically meaningful), and sort by salary descending.

```sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim 
    ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim 
    ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```

**Key Findings (Top 10 Optimal Skills):**

| Skill | Demand Count | Avg Salary (USD) |
|---|---|---|
| Go | 27 | $115,320 |
| Confluence | 11 | $114,210 |
| Hadoop | 22 | $113,193 |
| Snowflake | 37 | $112,948 |
| Azure | 34 | $111,225 |
| BigQuery | 13 | $109,654 |
| AWS | 32 | $108,317 |
| Java | 17 | $106,906 |
| SSIS | 12 | $106,683 |
| Jira | 20 | $104,918 |

**Broader Tier Analysis:**

| Category | Skills | Demand | Avg Salary |
|---|---|---|---|
| Programming languages | Python (236), R (148) | Very High | ~$100K |
| Cloud platforms | Snowflake, Azure, AWS, BigQuery | High | $108K–$113K |
| BI tools | Tableau (230), Looker (49) | High | $99K–$104K |
| Databases | Oracle, SQL Server, NoSQL | Moderate | $98K–$105K |

- **Cloud platforms (Snowflake, Azure, AWS)** hit the sweet spot: high demand + above-average salary. Strong ROI for learning.
- **Python and R** are universally demanded but salary-saturated — wide availability means they're expected, not premium.
- **Tableau/Looker** show high demand + solid salary — excellent for portfolios targeting BI-heavy DA roles.
- **Go, Hadoop, and Confluence** appear niche but are worth noting for engineering-adjacent analyst tracks.

---

## 🧠 Advanced SQL Concepts Practiced

Beyond the core project queries, the `advanced_sql/` folder documents hands-on practice with:

| Concept | Description |
|---|---|
| **CTEs (Common Table Expressions)** | Modular, readable multi-step queries using `WITH` clauses |
| **Subqueries** | Nested queries for filtering and computation |
| **Window Functions** | `RANK()`, `ROW_NUMBER()`, `PARTITION BY` for analytical calculations |
| **CASE Statements** | Conditional logic inside queries |
| **Date Functions** | Extracting, formatting, and filtering by date/time |
| **UNION / UNION ALL** | Combining result sets from multiple queries |
| **Multi-Table JOINs** | INNER, LEFT, RIGHT joins across 3+ tables |
| **Aggregate Functions** | `COUNT()`, `AVG()`, `SUM()` with `GROUP BY` and `HAVING` |

---

## 💡 Key Takeaways

**What I learned building this project:**

**Complex Query Design** — Chaining CTEs with multi-table joins made queries readable and modular. Breaking problems into sub-steps (CTE → filter → join → aggregate) is how production-level SQL is written.

**Aggregation Thinking** — `GROUP BY` + `HAVING` is a powerful filter pair. Using `HAVING COUNT(...) > 10` in Query 5 ensured statistically meaningful skill groupings rather than noise from rare listings.

**Insight vs. Raw Data** — Running a query is step one. Interpreting results in context — why PySpark pays more than Python, what it means that SQL is #1 in demand but not #1 in salary — is where the real analytical work happens.

**Relational Database Fluency** — Working with 4 related tables reinforced foreign key relationships, junction tables, and the importance of clean joins when mixing dimension and fact tables.

---

## 📝 Conclusions

| # | Insight |
|---|---|
| 1 | Top-paying remote DA roles can reach **$650,000/year** — the ceiling is higher than most assume |
| 2 | **SQL is the single most important skill** — required by 8 of 10 top-paying jobs AND most demanded overall |
| 3 | **Python + Tableau** form the core "second tier" — present in top-paying roles and high-demand counts |
| 4 | **Niche/specialized skills** (PySpark, DataRobot, Elasticsearch) command the highest salaries but are rare in postings |
| 5 | **Cloud platforms** (Snowflake, Azure, AWS) offer the best balance — high demand AND high salary — making them ideal targets for skill development |

**Strategic implication:** Learn SQL first, then Python. Add Tableau or Power BI for visualization. Layer in a cloud platform (Snowflake or Azure) for salary differentiation. This is the evidence-based path to maximizing value as a Data Analyst.

---

## 🚀 How to Run This Project

### Prerequisites

- [PostgreSQL](https://www.postgresql.org/download/) (v14+)
- [VS Code](https://code.visualstudio.com/) with the SQLTools extension, **or** [pgAdmin 4](https://www.pgadmin.org/)
- Git

### Setup Steps

```bash
# 1. Clone the repository
git clone https://github.com/uttkarshyadavv/SQL-Project.git
cd SQL-Project

# 2. Open PostgreSQL and create a new database
# In pgAdmin or psql:
CREATE DATABASE sql_job_analysis;

# 3. Run the table creation and data load scripts (in order)
# Execute all files inside sql_load/ in your PostgreSQL client

# 4. Open project_sql/ and run queries 1–5 in sequence
# Results match the analysis tables documented in this README
```

### Folder Execution Order

```
sql_load/        → Run first  (creates tables, loads CSV data)
project_sql/     → Run second (5 analytical queries)
advanced_sql/    → Optional   (supplementary SQL concept practice)
```

---

## 🔗 Connect with Me

**Utkarsh Yadav** — Chemical Engineering student | Aspiring Data & ML Engineer

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/utkarsh-yadavv)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/uttkarshyadavv)

---

*Dataset sourced from [Luke Barousse's SQL for Data Analytics Course](https://lukebarousse.com/sql). Analysis, queries, and interpretations are my own.*
