# Домашнее задание к занятию «Базы данных»

### Легенда

Заказчик передал вам [файл в формате Excel](https://github.com/netology-code/sdb-homeworks/blob/main/resources/hw-12-1.xlsx), в котором сформирован отчёт. 

На основе этого отчёта нужно выполнить следующие задания.

### Задание 1

Опишите не менее семи таблиц, из которых состоит база данных. Определите:

- какие данные хранятся в этих таблицах,
- какой тип данных у столбцов в этих таблицах, если данные хранятся в PostgreSQL.

Начертите схему полученной модели данных. Можете использовать онлайн-редактор: https://app.diagrams.net/

Этапы реализации:
1.	Внимательно изучите предоставленный вам файл с данными и подумайте, как можно сгруппировать данные по смыслу.
2.	Разбейте исходный файл на несколько таблиц и определите список столбцов в каждой из них. 
3.	Для каждого столбца подберите подходящий тип данных из PostgreSQL. 
4.	Для каждой таблицы определите первичный ключ (PRIMARY KEY).
5.	Определите типы связей между таблицами. 
6.	Начертите схему модели данных.
На схеме должны быть чётко отображены:
   - все таблицы с их названиями,
   - все столбцы  с указанием типов данных,
   - первичные ключи (они должны быть явно выделены),
   - линии, показывающие связи между таблицами.

**Результатом выполнения задания** должен стать скриншот получившейся схемы базы данных.

![Скриншот](https://github.com/karelinsf/project_db/blob/main/screen/shema.png)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению. Вы можете их выполнить, если хотите глубже и шире разобраться в материале.


### Задание 2*

1. Разверните СУБД Postgres на своей хостовой машине, на виртуальной машине или в контейнере docker.
2. Опишите схему, полученную в предыдущем задании, с помощью скрипта SQL.
3. Создайте в вашей полученной СУБД новую базу данных и выполните полученный ранее скрипт для создания вашей модели данных.

В качестве решения приложите SQL скрипт и скриншот диаграммы.

Для написания и редактирования sql удобно использовать  специальный инструмент dbeaver.

```sql
--Сотрудники (Employees)

CREATE TABLE employees (
    employee_id SERIAL PRIMARY KEY,
    full_name VARCHAR(255) NOT NULL,
    salary DECIMAL(10, 2),
    position VARCHAR(100),
    hire_date DATE
);

--Отделения (Departments)

CREATE TABLE departments (
    department_id SERIAL PRIMARY KEY,
    department_name VARCHAR(100),
    type VARCHAR(100)
);

--Филиалы (Branches)

CREATE TABLE branches (
    branch_id SERIAL PRIMARY KEY,
    address VARCHAR(255)
);
--Назначение сотрудника в отдел (Employee_Department):Эта таблица соединяет сотрудников с подразделениями.

CREATE TABLE employee_departments (
    employee_id INT REFERENCES employees(employee_id),
    department_id INT REFERENCES departments(department_id),
    PRIMARY KEY (employee_id, department_id)
);

--Расположение отдела в филиале (Department_Branch):Соединяет филиалы и подразделения.

CREATE TABLE department_branches (
    department_id INT REFERENCES departments(department_id),
    branch_id INT REFERENCES branches(branch_id),
    PRIMARY KEY (department_id, branch_id)
);

--Проекты (Projects)

CREATE TABLE projects (
    project_id SERIAL PRIMARY KEY,
    project_name VARCHAR(255) NOT NULL
);

--Участие сотрудников в проекте (Employee_Project)

CREATE TABLE employee_projects (
    employee_id INT REFERENCES employees(employee_id),
    project_id INT REFERENCES projects(project_id),
    PRIMARY KEY (employee_id, project_id)
);
```

![Скриншот](https://github.com/karelinsf/project_db/blob/main/screen/shema1.png)