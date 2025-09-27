# plsql-window-functions — Jean Bosco Ukwizagira

##  Project Overview

Our Daily Food is an online food ordering platform designed to provide customers with a convenient way to browse, order, and receive meals from a diverse menu of items.

### Business Context

Our Daily Food is a restaurant chain in Rwanda’s food service industry. The Sales & Marketing department manages customer orders and branch performance across Kigali, Musanze, and Igicumbi.

### Data Challenge

Although daily transactions are recorded, management cannot easily identify top products per region, track customer spending frequency, or measure monthly sales growth. Current reports are static and lack actionable insights.

#### Expected Outcome

Generate insights by:

1. Identifying top-selling products per region/month
2. Computing running and cumulative sales totals
3. Measuring month-over-month growth
4. Segmenting customers into quartiles
5. Calculating 3-month moving averages

## Success Criteria

1. Top 5 products per region/month `RANK()`
2. Running monthly sales totals `SUM() OVER()`
3. Month-over-month growth`LAG()/LEAD()`
4. Customer quartiles`NTILE(4)`
5. 3-month moving averages `AVG() OVER()`


##  Database Schema

customers: customer info (id, name, phone, region)
products: product catalog (id, name, category, price)
branches: restaurant branches (id, branch_name, region)
transactions: sales records (id, customer_id, product_id, branch_id, sale_date, quantity, amount)

##  Setup & Usage

1. Run SQL script to create tables.
2. Run SQL script to load sample data.
3. Execute queries in the queries file.
4. Check results and compare with screenshots under `/screenshots/`.


##  Key Queries & Insights

  Ranking (`RANK`, `DENSE_RANK`) Top products per region
  Aggregate (`SUM`, `AVG`) Running totals and moving averages
  Navigation (`LAG`, `LEAD`) Month-over-month sales growth
  Distribution (`NTILE`, `CUME_DIST`) Customer segmentation

Screenshots of query outputs are included in the `/screenshots/` folder.


##  Results Analysis

Descriptive: Coffee Beans ranked as the top product in Kigali; Musanze shows steady growth in French Fries.
Diagnostic: Kigali branch leads due to higher customer density and repeat orders, while Huye underperforms due to limited product variety.
Prescriptive: Increase inventory of Coffee Beans in Kigali, launch promotions for underperforming products in Huye, and target mid-tier customers with loyalty rewards.

##  References

1. Oracle Database SQL Language Reference (docs.oracle.com)
2. Oracle PL/SQL User’s Guide (docs.oracle.com)
3. Oracle SQL Developer User Guide
4. TutorialsPoint – SQL Window Functions
5. W3Schools – SQL Window Functions
6. GeeksforGeeks – SQL Analytics Functions
7. Mode Analytics SQL Tutorial
8. Vertabelo Academy SQL Window Functions Guide
9. SQLShack – Oracle ROW_NUMBER, RANK, DENSE_RANK
10. Oracle LiveSQL (livesql.oracle.com)

 ## Integrity Statement

All sources were properly cited. Implementations and analyses represent original work. No AI-generated content was copied without attribution or adaptation.
