**Dive into the Data Job Market!**

Explore the data job market with a focus on data analyst roles. This project investigates 

💰 top-paying positions

🔥 in-demand skills

📈 where high demand intersects with high salaries in data analytics.

**Background**

Motivated by the need to better navigate the data analyst job market, this project was initiated to identify top-paying roles and sought-after skills, simplifying the process for others to find optimal opportunities.

The data includes valuable insights on job titles, salaries, locations, and essential skills.

The primary questions I aimed to address through my SQL queries were:

What are the highest-paying data analyst roles?

What skills are necessary for these top-paying positions?

What skills are most in-demand for data analysts?

Which skills correlate with higher salaries?

What are the best skills to learn for career advancement?


**Tools Utilized**
For this comprehensive analysis of the data analyst job market, I leveraged several key tools:

**SQL**: The core tool for my analysis, enabling me to query the database and uncover important insights.

**PostgreSQL**: The database management system selected for handling job posting data.

**Visual Studio Code**: My preferred tool for database management and running SQL queries.

**Git & GitHub**: Critical for version control, collaboration, and sharing my SQL scripts and analysis.


**Analysis**
Each query in this project was designed to explore specific facets of the data analyst job market. Here’s my approach to each question:

1. Highest-Paying Data Analyst Roles
   
To determine the top-paying positions, I filtered data analyst jobs by average yearly salary and location, focusing on remote opportunities. This query highlights lucrative roles in the field.

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
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND 
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;

**Insights**:

Wide Salary Range: The top 10 highest-paying data analyst roles range from $184,000 to $650,000, indicating substantial earning potential.

Diverse Employers: High salaries are offered by companies like SmartAsset, Meta, and AT&T, showing a broad interest across various industries.

Variety in Job Titles: There is significant diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within the field.

2. Skills Required for Top-Paying Roles
   
To understand the skills needed for the highest-paying jobs, I joined job postings with skills data, revealing what employers value for high-compensation positions.

WITH top_paying_jobs AS (
    SELECT    
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND 
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
    
**Insights**:

SQL: Leading with a count of 8.

Python: Close behind with a count of 7.

Tableau: Highly valued, with a count of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.


3. Most In-Demand Skills for Data Analysts
   
This query identified the skills most frequently requested in job postings, highlighting areas with high demand.

SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;

**Insights**:

SQL and Excel: Fundamental skills with high demand.

Python, Tableau, and Power BI: Essential for technical proficiency in data processing and visualization.

4. Skills Correlated with Higher Salaries
   
This query explored the average salaries associated with different skills to determine which skills are the highest paying.

SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;

**Insights**:

Big Data & ML Skills: High salaries are associated with skills in PySpark, Couchbase, and machine learning tools.

Software Development & Deployment: Proficiency in tools like GitLab and Kubernetes commands a premium.

Cloud Computing: Expertise in cloud platforms like Elasticsearch and Databricks boosts earning potential.


5. Optimal Skills to Learn
   
Combining insights from demand and salary data, this query pinpointed skills that are both in high demand and have high salaries, providing a strategic focus for skill development.

SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
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

**Insights**:

High-Demand Programming Languages: Python and R are in high demand.

Cloud Tools and Technologies: Skills in Snowflake, Azure, AWS, and BigQuery are valuable.

Business Intelligence Tools: Tableau and Looker are critical for data visualization.

Database Technologies: Skills in Oracle, SQL Server, and NoSQL databases remain essential.


**What I Learned**

Throughout this project, I significantly enhanced my SQL skills:

Complex Query Crafting: Mastered advanced SQL techniques, including complex joins and WITH clauses.

Data Aggregation: Proficiently used GROUP BY and aggregate functions like COUNT() and AVG().

Analytical Problem-Solving: Improved real-world problem-solving skills, translating questions into actionable SQL queries.


**Insights**

Top-Paying Data Analyst Jobs: The highest-paying remote jobs offer salaries up to $650,000.

Highest Paying Company: Mantys

Skills for Top-Paying Jobs: Advanced SQL proficiency is crucial for high-paying roles.

Most In-Demand Skills: SQL is the most demanded skill in the job market.

Skills with Higher Salaries: Specialized skills like SVN and Solidity are associated with higher salaries.

Optimal Skills for Job Market Value: SQL leads in demand and salary, making it a valuable skill to learn.


**Take Away**
This project not only improved my SQL capabilities but also provided valuable insights into the data analyst job market. The findings serve as a guide for prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive market by focusing on high-demand, high-salary skills, emphasizing the importance of continuous learning and adaptation to industry trends.
