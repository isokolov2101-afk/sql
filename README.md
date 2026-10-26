# Домашнее задание к занятию «SQL. Часть 2» Соколов И.В.

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

### Решение 1

SELECT 
	s.first_name AS staff_first_name,
    s.last_name AS staff_last_name,
    c.city AS store_city,
    COUNT(cust.customer_id) AS total_customers
FROM store st
JOIN staff s ON st.manager_staff_id = s.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cust ON st.store_id = cust.store_id
GROUP BY st.store_id, s.last_name, s.first_name, c.city
HAVING COUNT(cust.customer_id) > 300;

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

### Решение 2

SELECT COUNT(*) AS films_longer_than_average
FROM film
WHERE length > (SELECT AVG(length) FROM film);

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

### Решение 3

SELECT 
    DATE_FORMAT(payment_date, '%Y-%m') AS payment_month,
    SUM(amount) AS total_sales,
    COUNT(rental_id) AS total_rentals
FROM payment
GROUP BY DATE_FORMAT(payment_date, '%Y-%m')
ORDER BY total_sales DESC
LIMIT 1;
