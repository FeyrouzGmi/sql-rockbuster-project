Here is a SQL query using nested subqueries and joins to return the top five Rockbuster customers based on the top 10 most profitable cities
```SQL
SELECT all_customers.country,
	     all_customers.all_customer_count,
	     top_5_customers.top_customer_count
FROM

--customer count by country
			(SELECT D.country,
					    COUNT(DISTINCT A.customer_id) AS all_customer_count
			 FROM customer A
			 	    INNER JOIN address B ON A.address_id = B.address_id
			 	    INNER JOIN city C ON B.city_id = C.city_id
			 	    INNER JOIN country D on C.country_id = D.country_id
			 GROUP BY D.country
			 ORDER BY all_customer_count DESC) AS all_customers

   LEFT JOIN
-- top 5 customers
			(SELECT top_customers.country,
					    COUNT(top_customers.customer_id) AS top_customer_count
			 FROM(SELECT B.customer_id,
						       B.first_name,
						       B.last_name,
						       D.city,
						       E.country,
				           SUM (A.amount) AS total_amount_paid
			      FROM customer B
					       INNER JOIN payment A ON B.customer_id = A.customer_id
					       INNER JOIN address C ON B.address_id = C.address_id
					       INNER JOIN city D ON C.city_id = D.city_id
					       INNER JOIN country E ON D.country_id = E.country_id
			      WHERE D.city IN ('Aurora', 'Acua', 'Citrus Heights', 'Iwaki', 'Ambattur'
							'Shanwei', 'Pingxiang', 'Sivas', 'Celaya', 'So Leopoldo')
			      GROUP BY B.customer_id, B.first_name, B.last_name, D.city, E.country
			      ORDER BY total_amount_paid DESC
			      LIMIT 5) AS top_customers
			GROUP BY top_customers.country) AS top_5_customers
			
ON all_customers.country = top_5_customers.country -- LEFT JOIN SYNTAX
WHERE top_customer_count IS NOT NULL
ORDER BY top_customer_count DESC
```
