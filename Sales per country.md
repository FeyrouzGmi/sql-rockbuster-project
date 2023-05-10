Here is a SQL query with multiple joins to find customers counts and sales per country
```SQL
SELECT country,
       COUNT(DISTINCT A.customer_id AS customer_count,
       sum(AMOUNT) AS total_payment
FROM customer A
     INNER JOIN address B ON A.address_id = B.address_id
     INNER JOIN city C ON B.city_id = C.city_id
     INNER JOIN country D ON C.country_id = D.country_id
     INNER JOIN payment E ON a.customer_id = E.customer_id
GROUP BY country
```
