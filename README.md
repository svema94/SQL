# SQL
# Проект: задания
В проекте тебе нужно проанализировать данные о фондах и инвестициях и написать запросы к базе. Задания будут разные, но всё необходимое для их выполнения — операторы, функции, методы работы с базой — у тебя уже получилось узнать на спринте. К каждому заданию будет небольшая подсказка: она направит тебя в нужную сторону, но подробного плана действий не предложит.
Таблица https://code.s3.yandex.net/SQL%20for%20data%20and%20analytics/ER/basic_sql_project.pdf

# 1. Посчитай, сколько компаний закрылось.
SELECT COUNT(status)
FROM company
WHERE status LIKE '%closed%';

# 2. Отобрази количество привлечённых средств для новостных компаний США. Используй данные из таблицы company. Отсортируй таблицу по убыванию значений в поле funding_total.
SELECT funding_total
FROM company
WHERE category_code LIKE '%news%'
AND country_code LIKE '%USA%'
ORDER BY funding_total DESC;

# 3. Отобрази имя, фамилию и названия профиля в поле network_username, которые начинаются на 'Silver'.
SELECT first_name,
last_name,
network_username
FROM people
WHERE network_username LIKE 'Silver%';

# 4. Выведи на экран всю информацию о людях, у которых названия профиля фондов в поле network_username содержат подстроку 'money', а фамилия начинается на 'K'.
SELECT *
FROM people
WHERE network_username LIKE '%money%'
AND last_name LIKE 'K%';

# 5. Для каждой страны отобрази общую сумму привлечённых инвестиций, которые получили компании, зарегистрированные в этой стране. Страну, в которой зарегистрирована компания, можно определить по коду страны. Отсортируй данные по убыванию суммы.
SELECT country_code,
SUM(funding_total)
FROM company
GROUP BY country_code
ORDER BY SUM(funding_total) DESC;
