# Домашнее задание к занятию «Базы данных» - Мельник Юрий Александрович


## Задание 1

### Заказчик передал вам 
[файл](https://github.com/netology-code/sdb-homeworks/blob/main/resources/hw-12-1.xlsx) в формате Excel, в котором сформирован отчёт.

### На основе этого отчёта нужно выполнить следующие задания.

1. `Опишите не менее семи таблиц, из которых состоит база данных:`
- какие данные хранятся в этих таблицах;
- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.  

Приведите решение к следующему виду:  

Сотрудники (  

- идентификатор, первичный ключ, serial,  
- фамилия varchar(50),  
- ...  
- идентификатор структурного подразделения, внешний ключ, integer).  

 
## Решение 1  
1. `Анализ задачи`   
Перед нами пример денормализации данных. Предприятие имеет несколько филиалов, зарплата сотрудников зависит от из должности, места работы и в каком проекте они работают. Филиалы присутствуют в разных городах страны.  
Нужны таблицы:  
1. Сотрудники - у каждого сотрудника есть ФИО, дата найма, оклад, должность и идентификатор подразделения где он работает
2. Должности  
3. Типы подразделений  
4. Структурные подразделения - подразделения физически расположены, имеют какие-то адреса, их сотрудники могут работать как в офисе так и быть прикомандированы в другие организации
5. Филиалы фактически это юридические адреса подразделений  
6. Проекты – имеет номер и описание  
7. Сотрудники в проекте каждый сотрудник может быть задействован в нескольких проектах  
ИД  проекта, ид сотрудника этим сочетанием можно обозначить участие рабокника в конкретном проекте. Это отношение многие ко многим
8. Должностные оклады- должностной оклад в данной организации зависит от проекта, структурного подразделения, должности человека. 
Каждая должность подразумевает какой-то оклад, сотрудники занимая одинаковую должность например инженер, могут получать различную заработную плату в зависимости от подразделения или проекта в котором они участвуют, или один сотрудник может работать сразу в нескольких проектах.  
Создадим схему учитывающие все выше перечисленное


2. `Структура таблиц БД`
1. Сотрудники (  
- идентификатор сотрудника, первичный ключ, serial,  
- Ф.И.О. varchar(100),  
- дата найма date,  
- оклад , внешний ключ, integer  
- идентификатор должности, внешний ключ, integer,  
- идентификатор структурного подразделения, внешний ключ, integer)  

2. Должности (  
- идентификатор должности, первичный ключ, serial,  
- наименование должности varchar(50))  

3. Типы подразделений (  
- идентификатор типа подразделения, первичный ключ, serial,  
- тип подразделения varchar())  

4. Структурные подразделения (  
- идентификатор структурного подразделения, первичный ключ, serial,  
- наименование структурного подразделение varchar(100),  
- идентификатор типа подразделения, внешний ключ, integer  
- идентификатор филиала, внешний ключ, integer)  

5. Филиалы (  
- идентификатор филиала, первичный ключ, serial,  
- адрес филиала varchar(200))  

6. Проекты (  
- идентификатор проекта, первичный ключ, serial,  
- название проекта varchar(50))  

7. Сотрудники в проекте (  
- идентификатор сотрудника, внешний ключ, integer,  
- идентификатор проекта, внешний ключ, integer,)  
 
8. Должностные оклады(  
- идентификатор должностного оклада, первичный ключ, serial,  
- идентификатор проекта, внешний ключ, integer  
- идентификатор структурного подразделения, внешний ключ, integer  
- идентификатор должности, внешний ключ, integer  
- оклад, decimal(10,2)  
)
 
 
3. `Схема таблиц со связями`  
 ![alt text](https://github.com/ysatii/DB-HW1/blob/main/img/image1.jpg)  


## Задание 2

1. ` Перечислите, какие, на ваш взгляд, в этой денормализованной таблице встречаются функциональные зависимости и какие правила вывода нужно применить, чтобы нормализовать данные.`  

## Решение 2
1. Выделить в отдельную таблицу должности - они повторяються!  
2. Выделить в отдельную таблицу типы подразделений.  
3. Выделить в отдельную таблицу нименования структурных подразделений  
4. Выделить в отдельную таблицу адреса подразделений   
5. Выделить в отдельную таблицу наименования проектов  
6. через связь многие-ко-многим можно между таблицами сотрудники и проекты, используя промежуточную таблицу сотрудники в проекте 
привязать кадого сотрудника к проекту! Также уучитывем то факт что один сотрудник может участвовам во многих проектах  
Это позволит назначать одинкаковые оклады сотрудникам работающим в одном подразделении на одной должности и в одном проекте!  

Схема пункта 1.3 ВСЕ ЭТО учитывает!   

 
 

