# 📺 Relational Database Design: TV Series Analytics

## 📌 Project Overview
This project demonstrates the end-to-end creation of a relational database from scratch. Rather than querying flat files, this project focuses on Data Definition Language (DDL) and database architecture, establishing normalized tables to track television series, reviewers, and user ratings. 

The accompanying SQL scripts not only build the schema but also execute complex business logic, including dynamic user segmentation and missing-data handling.

## 🛠️ Tools & Techniques
* **Database:** MySQL 8.0
* **Core Skills:** Relational Architecture (Primary & Foreign Keys), DDL (`CREATE TABLE`), Advanced Joins (`LEFT JOIN`), Conditional Logic (`CASE`, `IF`), and Null Handling (`IFNULL`).

## 🚧 Database Architecture 
To ensure data integrity, the database was built using a normalized schema consisting of three core tables:
1. **`reviewers`**: Stores user data with unique auto-incrementing IDs.
2. **`series`**: Catalogs television shows by release year and genre.
3. **`reviews`**: A mapping table utilizing Foreign Keys tied to both `reviewers` and `series` to track individual ratings.

## 📊 Key Business Logic & Analytics
Once the database was constructed and seeded, several analytical queries were written to extract insights, including:
* **User Segmentation:** Engineered a dynamic categorical column using `CASE` and `IF` statements to tag users as 'ACTIVE' or 'INACTIVE' based on their review volume.
* **Data Validation:** Utilized `LEFT JOIN` operations to identify anomalies, such as series that exist in the database but have 0 reviews.
* **Clean Aggregations:** Applied `IFNULL` functions to aggregate data (MIN, MAX, AVG) ensuring clean, dashboard-ready outputs without breaking calculations on missing data.

## 💻 Sample Code Highlight
*Dynamically segmenting users based on their engagement history:*
```sql
SELECT 
    first_name,
    last_name,
    COUNT(rating) AS total_reviews,
    ROUND(IFNULL(AVG(rating), 0), 2) AS average_rating,
    CASE
        WHEN COUNT(rating) > 0 THEN 'ACTIVE'
        ELSE 'INACTIVE'
    END AS 'USER_STATUS'
FROM reviewers
LEFT JOIN reviews 
    ON reviewers.id = reviews.reviewer_id
GROUP BY first_name, last_name;
