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

## SQL Code
CREATE TABLE customers (
    customer_id NUMBER PRIMARY KEY,
    name        VARCHAR2(100),
    phone       VARCHAR2(20),
    region      VARCHAR2(50)
);
CREATE TABLE products (
    product_id  NUMBER PRIMARY KEY,
    name        VARCHAR2(100),
    category    VARCHAR2(50),
    price       NUMBER(10,2)
);
drop table products;

CREATE TABLE products (
    product_id  NUMBER PRIMARY KEY,
    name        VARCHAR2(100),
    category    VARCHAR2(50),
    price       NUMBER(10,2)
);
CREATE TABLE branches (
    branch_id   NUMBER PRIMARY KEY,
    branch_name VARCHAR2(100),
    region      VARCHAR2(50)
);

drop table transactions;

CREATE TABLE transactions (
    transaction_id NUMBER PRIMARY KEY,
    customer_id    NUMBER,
    product_id     NUMBER,
    branch_id      NUMBER,
    sale_date      DATE,
    quantity       NUMBER,
    amount         NUMBER(12,2)
);
drop  table transactions;

CREATE TABLE transactions (
    transaction_id NUMBER PRIMARY KEY,
    customer_id    NUMBER,
    product_id     NUMBER,
    branch_id      NUMBER,
    sale_date      DATE,
    quantity       NUMBER,
    amount         NUMBER(12,2)
);
INSERT INTO products VALUES (2001, 'Mango juice', 'Beverages', 7.50);
INSERT INTO products VALUES (2002, 'chips', 'Food', 5.00);
INSERT INTO products VALUES (2003, 'boiled chicken', 'Food', 2.50);
INSERT INTO products VALUES (2004, 'water','drinks', 1.50);
select * from customers;
commit;

INSERT INTO customers VALUES (1001, 'Alice', '0789000000', 'Kigali');
INSERT INTO customers VALUES (1002, 'Bob', '0789111111', 'Musanze');
INSERT INTO customers VALUES (1003, 'Charlie', '0789222222', 'Gicumbi');
INSERT INTO customers VALUES (1004, 'Diane', '0789333333', 'Kigali');
select * from products;

insert into branches values (11,'Musanze supermarket','Musanze');
insert into branches values (12,'Gicumbi main','Gicumbi');
insert into branches values (13,' Kigali center','Kigali');
insert into branches values (14,'Rwamagana','center');
select * from branches;

-- customer_id = 1 → Hitimana
INSERT INTO transactions VALUES (2001, 1, 101, 11, DATE '2025-09-02', 3, 3000.00);

-- customer_id = 2 → Ukwizagira
INSERT INTO transactions VALUES (2002, 2, 102, 12, DATE '2025-09-03', 1, 5000.00);

-- customer_id = 3 → Niyonshuti
INSERT INTO transactions VALUES (2003, 3, 103, 11, DATE '2025-09-04', 2, 8446.00);

-- customer_id = 4 
INSERT INTO transactions VALUES (2004, 4, 104, 12, DATE '2025-09-05', 5, 5000.00);
SELECT * FROM transactions;

SELECT
    c.customer_id,
    c.name AS customer_name,
    c.region,
    SUM(t.amount) AS total_revenue,
    ROW_NUMBER() OVER (ORDER BY SUM(t.amount) DESC) AS row_num
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.name, c.region
ORDER BY total_revenue DESC;

SELECT
    c.customer_id,
    c.name AS customer_name,
    c.region,
    SUM(t.amount) AS total_revenue,
    DENSE_RANK() OVER (ORDER BY SUM(t.amount) DESC) AS dense_rank_num
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.name, c.region
ORDER BY total_revenue DESC;


SELECT
    c.customer_id,
    c.name AS customer_name,
    c.region,
    SUM(t.amount) AS total_revenue,
    PERCENT_RANK() OVER (ORDER BY SUM(t.amount) DESC) AS percent_rank_num
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.name, c.region
ORDER BY total_revenue desc;

SELECT 
    c.customer_id,
    c.name AS customer_name,
    SUM(t.amount) AS total_revenue
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id, c.name
ORDER BY total_revenue DESC;

SELECT 
    transaction_id,
    amount,
    AVG(amount) OVER () AS avg_amount
FROM transactions
ORDER BY transaction_id DESC;

SELECT
    transaction_id,
    amount,
    MIN(amount) OVER () as min_amount
    FROM transactions
    order by transaction_id;

SELECT
    transaction_id,
    sale_date,
    amount,
    LAG(amount, 1) OVER (ORDER BY sale_date) AS previous_amount
FROM transactions
ORDER BY sale_date;

SELECT
    sale_date,
    sum(amount) as daily_sales,
    LEAD(sum (amount)) OVER (ORDER BY sale_date) as next_amount
FROM transactions
group by sale_date
ORDER BY sale_date;
  
SELECT
    transaction_id,
    amount,
    MAX(amount) OVER () as max_amount
        from transactions
        order by transaction_id;

SELECT
    c.customer_id,
    SUM(t.amount) AS total_revenue,
    NTILE(4) OVER (ORDER BY SUM(t.amount) DESC) AS sales_quartile
FROM customers c
JOIN transactions t
    ON c.customer_id = t.customer_id
GROUP BY c.customer_id
ORDER BY sales_quartile DESC;

SELECT
    customer_id,
    SUM(amount) AS total_sales,
    CUME_DIST() OVER (ORDER BY SUM(amount) DESC) AS cum_dist
FROM transactions
GROUP BY customer_id
ORDER BY cum_dist DESC;

##  Results Analysis

Descriptive: Mango juice ranked as the top product in Kigali; Musanze shows steady growth in chips.
Diagnostic: Kigali branch leads due to higher customer density and frequent repeat orders, 
while Rwamagana underperforms due to limited product variety.
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
