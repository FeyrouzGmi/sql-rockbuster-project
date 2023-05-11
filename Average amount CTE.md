Here is a SQL query utilising CTE displaying the average amount paid by the top 5 customers (from the top 10 cities)
```SQL
WITH average_cte(customer_id, first_name, last_name, city, country)
AS
    (SELECT B.customer_id, B.first_name, B.last_name, D.city, E.country,
            SUM(A.amount) AS total_amount_paid
     FROM payment E
          INNER JOIN customer B ON A.customer_id = B.customer_id
          INNER JOIN address C ON B.address_id = C.address_id
          INNER JOIN city D on C.city_id = D.city_id
          INNER JOIN country E ON D.country_id = E.country_id
     WHERE city in ('Aurora','Acua','Citrus Heights','Iwaki','Ambattur','Shanwei','Tianjin','Cianjur','Hami','Xintai')
     GROUP BY total_amount_paid DESC
     LIMIT 5)
 SELECT AVG (total_amount_paid) AS avg_amount_paid
 FROM average_cte
 ```
