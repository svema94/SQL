# SQL
# Проект: задания
В проекте тебе нужно проанализировать данные о фондах и инвестициях и написать запросы к базе. Задания будут разные, но всё необходимое для их выполнения — операторы, функции, методы работы с базой — у тебя уже получилось узнать на спринте. К каждому заданию будет небольшая подсказка: она направит тебя в нужную сторону, но подробного плана действий не предложит.
Таблица https://code.s3.yandex.net/SQL%20for%20data%20and%20analytics/ER/basic_sql_project.pdf

# 1. Посчитай, сколько компаний закрылось.
````
SELECT COUNT(status)
FROM company
WHERE status LIKE '%closed%';
````
# 2. Отобрази количество привлечённых средств для новостных компаний США. Используй данные из таблицы company. Отсортируй таблицу по убыванию значений в поле funding_total.
````
SELECT funding_total
FROM company
WHERE category_code LIKE '%news%'
AND country_code LIKE '%USA%'
ORDER BY funding_total DESC;
````

# 3. Отобрази имя, фамилию и названия профиля в поле network_username, которые начинаются на 'Silver'.
````
SELECT first_name,
last_name,
network_username
FROM people
WHERE network_username LIKE 'Silver%';
````

# 4. Выведи на экран всю информацию о людях, у которых названия профиля фондов в поле network_username содержат подстроку 'money', а фамилия начинается на 'K'.
````
SELECT *
FROM people
WHERE network_username LIKE '%money%'
AND last_name LIKE 'K%';
````

# 5. Для каждой страны отобрази общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируй данные по убыванию суммы.
````
SELECT country_code,
SUM(funding_total)
FROM company
GROUP BY country_code
ORDER BY SUM(funding_total) DESC;
````

# 6. Отобрази имя и фамилию всех сотрудников стартапов. Добавь поле с названием учебного заведения, которое окончил сотрудник, если эта информация известна.
````
SELECT pep.first_name,
pep.last_name,
ed.instituition
FROM people AS pep
LEFT OUTER JOIN education AS ed ON pep.id=ed.person_id;
````

# 7. Найди общую сумму сделок по покупке одних компаний другими в долларах. Отбери сделки, которые осуществлялись только за наличные с 2011 по 2013 год включительно.
````
SELECT SUM(price_amount)
FROM acquisition
WHERE term_code='cash' 
AND EXTRACT (YEAR FROM CAST (acquired_at AS date)) BETWEEN '2011' AND '2013';  
````
# 8. Выясни, в каких странах находятся фонды, которые чаще всего инвестируют в стартапы. 
Для каждой страны посчитай минимальное, максимальное и среднее число компаний, в которые инвестировали фонды этой страны, основанные с 2010 по 2012 год включительно. Исключи страны с фондами, у которых минимальное число компаний, получивших инвестиции, равно нулю. 
Выгрузи десять самых активных стран-инвесторов: отсортируй таблицу по среднему количеству компаний от большего к меньшему. Затем добавь сортировку по коду страны в лексикографическом порядке.
````
SELECT country_code,
MIN(invested_companies),
MAX(invested_companies),
AVG(invested_companies)
FROM fund
WHERE EXTRACT (YEAR FROM CAST (founded_at AS date)) BETWEEN '2010' AND '2012'
GROUP BY country_code 
HAVING MIN(invested_companies) > 0
ORDER BY AVG(invested_companies) DESC, country_code
LIMIT 10;
````
