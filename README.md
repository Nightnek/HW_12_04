# 12_04 Стасенко Григорий Домашнее задание к занятию «SQL. Часть 2» 12_04

## Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

фамилия и имя сотрудника из этого магазина;
город нахождения магазина;
количество пользователей, закреплённых в этом магазине.

![image](https://github.com/Nightnek/HW_12_04/assets/127677631/5f3201ed-7204-4a06-b17a-2c7504c8e1c9)

````
SELECT c.store_id, COUNT(c.store_id) AS count_of_Customers, CONCAT(st.first_name, ' ', st.last_name), ct.city
FROM customer c
JOIN staff AS st ON st.store_id = c.store_id
JOIN address a ON a.address_id = st.address_id
JOIN city AS ct ON ct.city_id = a.city_id
GROUP BY c.store_id, CONCAT(st.first_name, ' ', st.last_name), ct.city
HAVING COUNT(c.store_id) > 300;
````
---
## Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

![image](https://github.com/Nightnek/HW_12_04/assets/127677631/2ed1a63e-3042-40a5-b6a8-5dcf1f8bc408)


````
SELECT COUNT(length)
FROM film
WHERE length > (SELECT AVG(length) from film);
````
---

## Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.
![image](https://github.com/Nightnek/HW_12_04/assets/127677631/2bedfae9-cb4f-47dd-ad59-a2483c795db6)

````
SELECT MONTH(p.payment_date) as payment_month, SUM(p.amount) as amount_sum, COUNT(r.rental_id) as count_of_rents
FROM payment p
JOIN rental r ON r.rental_id = p.rental_id 
GROUP BY MONTH(p.payment_date)
ORDER BY SUM(p.amount) DESC
LIMIT 1;

````
---

## Задание 4*
Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

![image](https://github.com/Nightnek/HW_12_04/assets/127677631/416876ff-a014-4d7b-aa1c-3936ecd222d3)


````
SELECT CONCAT(st.first_name, ' ', st.last_name) Saller_name, COUNT(r.staff_id) AS count_of_sales,
	CASE
		WHEN COUNT(r.staff_id) > 8000 THEN 'Will have bonus'
		WHEN COUNT(r.staff_id) < 8000 THEN 'Will not have bonus'
	END AS Bonus	
FROM rental r
JOIN staff AS st ON st.staff_id = r.staff_id
GROUP BY r.staff_id;
````
---

## Задание 5*
Найдите фильмы, которые ни разу не брали в аренду.

![image](https://github.com/Nightnek/HW_12_04/assets/127677631/6f6efe96-e76c-4658-a561-9e7517e18a21)

````
SELECT f.title, r.rental_id 
FROM film f 
LEFT JOIN inventory i ON i.film_id = f.film_id 
LEFT JOIN rental r ON r.inventory_id = i.inventory_id 
WHERE  r.rental_id IS NULL;
````
---
