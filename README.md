# Домашнее задание к занятию «Индексы» - Кощеев Иван


### Задание 1

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

### Ответ

```

SELECT table_schema as DB_name,
CONCAT(ROUND((SUM(index_length))*100/(SUM(data_length+index_length)),2),'%') '% of index'
FROM information_schema.TABLES where TABLE_SCHEMA = 'sakila'


```
![image1](https://github.com/SirSeoPro/11-05/blob/main/1.png)

### Задание 2

Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.
 
### Ответ

```

SELECT DISTINCT 
  CONCAT(c.last_name, ' ', c.first_name) AS full_name,
  SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title) AS total_amount
FROM 
  payment p
JOIN rental r ON p.payment_date = r.rental_date
JOIN customer c ON r.customer_id = c.customer_id
JOIN inventory i ON i.inventory_id = r.inventory_id
JOIN film f ON f.film_id = i.film_id
WHERE 
  p.payment_date >= '2005-07-30 00:00:00'
  AND p.payment_date <  '2005-07-31 00:00:00';



```

