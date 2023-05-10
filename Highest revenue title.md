Here is a SQL query with multiple joins, to find Rockbuster top 10 highest revenue movie title
```SQL
SELECT A.title,
       SUM(E.amount) AS total_amount
FROM film A
       INNER JOIN inventory C ON A.film_id = C.film_id
       INNER JOIN rental D ON C.inventory_id = D.inventory_id
       INNER JOIN payment E ON D.rental_id = E.rental_id
GROUP BY A.title
ORDER BY total_amount DESC
LIMIT 10
