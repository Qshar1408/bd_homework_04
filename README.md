# Домашнее задание к занятию «SQL. Часть 2»

### Грибанов Антон. FOPS-31

### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```bash
SELECT CONCAT(s.first_name , " ", s.last_name) AS 'Сотрудник магазина', cm.city AS 'Город нахождения магазина', COUNT(c.customer_id) AS 'Количество покупателей'
FROM staff AS s
JOIN address AS a ON a.address_id = s.address_id
JOIN city AS cm ON cm.city_id = a.city_id
JOIN store AS st ON st.store_id = s.store_id
JOIN customer AS c ON c.store_id = s.store_id
GROUP BY staff_id
HAVING COUNT(c.customer_id) > 300;
```

 ![bd_004](https://github.com/Qshar1408/bd_homework_04/blob/main/img/bd_04_001.png)

### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```bash
SELECT COUNT(length) AS 'Количество фильмов больше средней продолжительности'
FROM film
WHERE length > (SELECT AVG(length) FROM film);
```

 ![bd_004](https://github.com/Qshar1408/bd_homework_04/blob/main/img/bd_04_002.png)

### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```bash
SELECT DATE_FORMAT(payment_date, '%Y.%m') AS 'Год и месяц c наибольшей суммой платежей', COUNT(rental_id) AS 'Количество аренд за месяц'
FROM payment
GROUP BY DATE_FORMAT(payment_date, '%Y.%m')
ORDER BY SUM(amount) DESC
LIMIT 1;
```

 ![bd_004](https://github.com/Qshar1408/bd_homework_04/blob/main/img/bd_04_003.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```bash
SELECT  CONCAT(s.first_name, ' ', s.last_name) AS Продавец,
COUNT(p.payment_id) AS Количество_продаж,
CASE WHEN COUNT(payment_id) > 8000 THEN 'Да' ELSE 'Нет' END AS Премия
FROM payment p
JOIN staff s ON s.staff_id = p.staff_id GROUP BY p.staff_id;
```

![bd_004](https://github.com/Qshar1408/bd_homework_04/blob/main/img/bd_04_004.png)

### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

```bash
SELECT f.title AS Фильм
FROM film f
LEFT JOIN inventory i ON i.film_id = f.film_id
LEFT JOIN rental r ON r.inventory_id = i.inventory_id
WHERE r.rental_id IS NULL
ORDER BY f.title ASC;
```

![bd_004](https://github.com/Qshar1408/bd_homework_04/blob/main/img/bd_04_005.png)
