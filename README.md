SELECT *
FROM sales_data;

/* To create a new column for profit */

ALTER TABLE sales_data
ADD COLUMN profit NUMERIC;

/* Add values for the profit column*/

UPDATE sales_data
SET profit = revenue - cost;

/* Q1.a Write a query that returns the total profit made by BuyBuy from 1Q11 to 4Q16 (all
quarters of every year).*/

SELECT
    SUM(profit) AS total_profit,
    EXTRACT(YEAR FROM sales_date) AS sales_year,
    EXTRACT(QUARTER FROM sales_date) AS quarter
FROM sales_data
GROUP BY EXTRACT(YEAR FROM sales_date), EXTRACT(QUARTER FROM sales_date)
ORDER BY sales_year, quarter;

/* Q1.b Write queries that return the total profit made by BuyBuy in Q2 of every year from
2011 to 2016.
*/

SELECT
    SUM(profit) AS total_profit,
    EXTRACT(YEAR FROM sales_date) AS sales_year,
    EXTRACT(QUARTER FROM sales_date) AS quarter
FROM sales_data
WHERE EXTRACT(QUARTER FROM sales_date) = 2
GROUP BY EXTRACT(YEAR FROM sales_date), EXTRACT(QUARTER FROM sales_date)
ORDER BY sales_year;

/* Q1.c Write a query that returns the annual profit made by BuyBuy from the year 2011 to
2016.
*/

SELECT
    SUM(profit) AS total_profit,
    EXTRACT(YEAR FROM sales_date) AS sales_year
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) IN (2011, 2012, 2013, 2014, 2015, 2016)
GROUP BY EXTRACT(YEAR FROM sales_date)
ORDER BY sales_year;

/* Q2.a Write 2 queries that return the countries where BuyBuy has made the most profit and
also the least profit of all-time. Your query must display both results on the same
output.
*/

SELECT
    'Most Profitable' AS type,
    cus_country,
    MAX(profit) AS profit
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) IN (2011, 2012, 2013, 2014, 2015, 2016)
GROUP BY cus_country

UNION ALL

SELECT
    'Least Profitable' AS type,
    cus_country,
    MIN(profit) AS profit
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) IN (2011, 2012, 2013, 2014, 2015, 2016)
GROUP BY cus_country

ORDER BY cus_country;

/* Q2.b Write a query that shows the Top-10 most profitable countries for BuyBuy sales
operations from 2011 to 2016
*/

SELECT
    'Most Profitable' AS type,
    cus_country, MAX(profit) AS profit
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY cus_country
ORDER BY type, cus_country
LIMIT 10;

/* Q2.c Write a query that shows the all-time Top-10 least profitable countries for BuyBuy
sales operations.
*/

SELECT
    'Least Profitable' AS type,
    cus_country, MIN(profit) AS profit
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY cus_country
ORDER BY type, cus_country
LIMIT 10;

SELECT DISTINCT cus_country
FROM sales_data;

/* Q3. a Write a query that ranks all product categories sold by Buybuy, from least amount to
the most amount of all-time revenue generated.
*/

SELECT
    'Least Amount' AS type,
    prod_category,
    MIN(cost) AS cost
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY prod_category

UNION ALL

SELECT
    'Most Amount' AS type,
    prod_category,
    MAX(cost) AS cost
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY prod_category
ORDER BY cost, prod_category;

/* Q3.b Write a query that returns Top-2 product categories offered by Buybuy with an all-time
high number of units sold.
*/

SELECT
	prod_category,MAX(ord_quantity)AS max_unit_sold
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY prod_category
ORDER BY max_unit_sold
LIMIT 2;

/* Q3.c Write a query that shows the Top 10 highest-grossing products sold by BuyBuy based
on all-time profits.
*/

SELECT
    'Highest grossing' AS type,
    product,
    MAX(profit) AS max_profit
FROM sales_data
WHERE EXTRACT(YEAR FROM sales_date) BETWEEN 2011 AND 2016
GROUP BY product
ORDER BY max_profit DESC
LIMIT 10;
