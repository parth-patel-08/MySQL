# MySQL

## Data Definition Operations (Create/Modify Database/Table)

### Create Database

```sql
CREATE DATABASE db_name;
CREATE DATABASE IF NOT EXISTS `db_name`;
```

**Note:** Backticks (``) are used if the database name is a MySQL keyword or contains special characters.

### Delete Database

```sql
DROP DATABASE db_name;
```

### MySQL Data Types

| Category      | Data Type    | Description                                                     | Storage Details                                 | Example                            | Stored Value Example              |
| ------------- | ------------ | --------------------------------------------------------------- | ----------------------------------------------- | ---------------------------------- | --------------------------------- |
| **Numeric**   | INT          | Standard integer type                                           |                                                 | `age INT`                          | `25`                              |
|               | TINYINT      | Very small integer                                              |                                                 | `is_active TINYINT(1)`             | `1`                               |
|               | BIGINT       | Large integer values                                            |                                                 | `population BIGINT`                | `9876543210`                      |
|               | DECIMAL(M,D) | Exact decimal numbers where M is total digits and D is decimals | Precise decimal values, good for financial data | `price DECIMAL(10,2)`              | `199.99`                          |
|               | FLOAT        | Approximate floating point numbers                              | 4 bytes, approximate precision                  | `weight FLOAT`                     | `72.5`                            |
| **String**    | VARCHAR(M)   | Variable length string where M is maximum length                | Stores only what's needed up to M characters    | `name VARCHAR(100)`                | `'John Smith'`                    |
|               | CHAR(M)      | Fixed length string always using M characters                   | Padded with spaces to specified length          | `code CHAR(5)`                     | `'ABC12'`                         |
|               | TEXT         | Variable unlimited length string                                | Max size: 65,535 characters                     | `description TEXT`                 | `'This is a long description...'` |
|               | LONGTEXT     | Very large text content                                         | Max size: 4GB                                   | `content LONGTEXT`                 | `'Very long article content...'`  |
| **Date/Time** | DATE         | Date without time                                               | Format: 'YYYY-MM-DD'                            | `birth_date DATE`                  | `'2023-12-25'`                    |
|               | TIME         | Time without date                                               | Format: 'HH:MM:SS'                              | `start_time TIME`                  | `'14:30:00'`                      |
|               | DATETIME     | Combined date and time                                          | Format: 'YYYY-MM-DD HH:MM:SS'                   | `created_at DATETIME`              | `'2023-12-25 14:30:00'`           |
|               | TIMESTAMP    | Timestamp with timezone awareness                               | Converts to UTC for storage                     | `last_update TIMESTAMP`            | `'2023-12-25 14:30:00'`           |
| **Others**    | ENUM         | List of possible string values                                  |                                                 | `status ENUM('active','inactive')` | `'active'`                        |
|               | BOOLEAN      | True/False values (1 or 0)                                      |                                                 | `is_valid BOOLEAN`                 | `true`                            |
|               | JSON         | Structured JSON document                                        |                                                 | `metadata JSON`                    | `'{"key": "value"}'`              |

### Create Table

```sql
CREATE TABLE table_name (
    name_field VARCHAR(100),
    description_field TEXT,
    status_field ENUM('pending', 'in-progress', 'done'),
    yearly_salary INT,
    exact_decimal_field DECIMAL(10,2),
    approximate_decimal_field FLOAT,
    only_date_field DATE,
    date_with_time_field TIMESTAMP,
    boolean_field BOOLEAN,
    json_field JSON
);
```

### Table with Default Values

```sql
CREATE TABLE table_name (
    name_field VARCHAR(100) DEFAULT 'Unnamed',
    description_field TEXT DEFAULT 'No description provided',
    status_field ENUM('pending', 'in-progress', 'done') DEFAULT 'pending',
    yearly_salary INT DEFAULT 50000,
    exact_decimal_field DECIMAL(10,2) DEFAULT 0.00,
    approximate_decimal_field FLOAT DEFAULT 0.0,
    only_date_field DATE DEFAULT CURRENT_DATE,
    date_with_time_field TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    boolean_field BOOLEAN DEFAULT false,
    json_field JSON DEFAULT (JSON_OBJECT('initialized', true)),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### Insert Data into Table

**Note:** Field names are optional in INSERT queries. If omitted, values must be provided in the same order as defined in the table structure.

```sql
-- With field names (recommended for clarity)
INSERT INTO table_name (name_field, description_field, status_field, yearly_salary, exact_decimal_field, approximate_decimal_field, only_date_field, date_with_time_field, boolean_field, json_field)
VALUES ('John Doe', 'Senior Software Engineer with 5 years of experience', 'in-progress', 85000, 123.456, 72.5, '2023-12-25', '2023-12-25 14:30:00', true, '{"skills": ["Python", "JavaScript"], "level": "senior"}');

-- Without field names (values must match table column order)
INSERT INTO table_name
VALUES ('John Doe', 'Senior Software Engineer with 5 years of experience', 'in-progress', 85000, 123.456, 72.5, '2023-12-25', '2023-12-25 14:30:00', true, '{"skills": ["Python", "JavaScript"], "level": "senior"}');
```

### Delete Table

```sql
-- Delete single table without confirmation
DROP TABLE table_name;

-- Delete single table only if it exists (prevents error if table doesn't exist)
DROP TABLE IF EXISTS table_name;

-- Delete multiple tables at once
DROP TABLE table1, table2, table3;

-- Delete multiple tables if they exist
DROP TABLE IF EXISTS table1, table2, table3;
```

### Modify Table

```sql
-- Add column(s)
ALTER TABLE table_name
    ADD COLUMN field1 INT,
    ADD COLUMN field2 VARCHAR(50);

-- Modify column data type
ALTER TABLE table_name
    MODIFY COLUMN field_name VARCHAR(200);

-- Change column name and type
ALTER TABLE table_name
    CHANGE old_field_name new_field_name VARCHAR(100);

-- Drop column
ALTER TABLE table_name
    DROP COLUMN field_name;

-- Rename table
RENAME TABLE old_table_name TO new_table_name;
```

## NULL vs 0

In MySQL, NULL represents an unknown or missing value, while 0 is a specific numeric value.

```sql
-- Table with NULL and 0 values
CREATE TABLE student_scores ( id INT, score INT );
INSERT INTO student_scores VALUES  (1, 80), (2, 0), (3, NULL), (4, 90);

-- Average calculation:
SELECT  AVG(score) as average_with_nulls FROM student_scores;
```

Results:

- `average_with_nulls`: 56.67 (170/3, NULL ignored and 0 considered)

## Constraints

| Constraint     | Description                                            |
| -------------- | ------------------------------------------------------ |
| NOT NULL       | Ensures column cannot have NULL value                  |
| UNIQUE         | Ensures all values in column are different             |
| PRIMARY KEY    | `NOT NULL` + `UNIQUE`, Uniquely identifies each record |
| FOREIGN KEY    | Links two tables together                              |
| CHECK          | Ensures values meet specific conditions                |
| DEFAULT        | Sets default value for column                          |
| AUTO_INCREMENT | Automatically increments value                         |

```sql
-- Column level constraints
CREATE TABLE table_name (
    id INT PRIMARY KEY AUTO_INCREMENT,
    phone_no INT NOT NULL,
    email VARCHAR(100) UNIQUE,
    percentage_value INT DEFAULT 100 CHECK (percentage_value > 0 AND percentage_value < 101),
    department_id INT REFERENCES departments_table(id)
);

-- Table level constraints
CREATE TABLE table_name (
    id INT AUTO_INCREMENT,
    phone_no INT NOT NULL,
    email VARCHAR(100),
    percentage_value INT DEFAULT 100,
    department_id INT,
    CONSTRAINT pk_user PRIMARY KEY (id),
    CONSTRAINT uk_email UNIQUE (email),
    CONSTRAINT percent_value_constraint CHECK (percentage_value > 0 AND percentage_value  < 101),
    CONSTRAINT fk_dept FOREIGN KEY (department_id) REFERENCES departments_table(id)
);
```

## Text Encoding and Collation

| Text Encoding                                     | Text Collation                                          |
| ------------------------------------------------- | ------------------------------------------------------- |
| Which Characters are supported and can be stored. | How Characters are compared.                            |
| Ex: 't', '6', 'ṻ', '🤚'                           | EX: 'u' < 'ṻ'                                           |
| Not all encodings support all characters          | Different collations control character comparison order |
| `utf8mb4` is recommended text encoding            | `utf8mb4_unicode_ci` is recommended collation           |
| Encoding can be set at column and table level     | Collation can be set at column and table level          |

## Create Table from Another Table

```sql
-- Create new table with same structure and data
CREATE TABLE new_table AS
    SELECT * FROM table_name;

-- Create new table with same structure but no data
CREATE TABLE new_table AS
    SELECT * FROM table_name WHERE 1=0;

-- Create new table with specific fields
CREATE TABLE new_employees AS
    SELECT name_field, yearly_salary, status_field FROM table_name WHERE yearly_salary > 60000;
```

## Temporary Table

Temporary tables exist only for the current session and are automatically dropped when the session ends.

```sql
-- Create temporary table
CREATE TEMPORARY TABLE temp_high_salary_employees (emp_id INT, name VARCHAR(100), salary DECIMAL(10,2));

-- Insert data from main table
INSERT INTO temp_high_salary_employees
    SELECT id, name_field, yearly_salary FROM table_name WHERE yearly_salary > 75000;

-- Use temporary table for complex calculations
SELECT AVG(salary) as avg_high_salary FROM temp_high_salary_employees;
```

Use cases:

- Complex queries requiring multiple steps
- Storing intermediate results

## Generated Column

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    full_name VARCHAR(101) GENERATED ALWAYS AS (CONCAT(first_name, ' ', last_name)) STORED,
    full_name_reversed VARCHAR(101) GENERATED ALWAYS AS (CONCAT(last_name, ', ', first_name)) VIRTUAL
);

INSERT INTO employees (first_name, last_name) VALUES ('John', 'Doe');

-- full_name and full_name_reversed are automatically generated
SELECT * FROM employees;
```

Note:

- STORED: Generated value is stored in the table (while inserting values)
- VIRTUAL: Generated value is calculated when queried (while fetching values) (**Default**)

## Data Manipulation (CRUD)

### Create (Insert Data)

```sql
-- Insert a single row with column names specified
INSERT INTO employees (first_name, last_name, email, salary)
VALUES ('John', 'Doe', 'john.doe@example.com', 60000);

-- Insert a single row without specifying column names (must match table structure order)
INSERT INTO employees 
VALUES (NULL, 'Jane', 'Smith', 'jane.smith@example.com', 65000);

-- Insert multiple rows in a single command
INSERT INTO employees (first_name, last_name, email, salary)
VALUES 
    ('Alice', 'Johnson', 'alice.j@example.com', 55000),
    ('Bob', 'Williams', 'bob.w@example.com', 70000),
    ('Carol', 'Davis', 'carol.d@example.com', 62000);

-- Insert data from another table (using SELECT)
INSERT INTO employee_archive (first_name, last_name, email, salary)
SELECT first_name, last_name, email, salary FROM employees WHERE termination_date IS NOT NULL;
```

### Read (Select Data)

```sql
-- Fetch all columns and all rows
SELECT * FROM employees;

-- Fetch specific columns
SELECT first_name, last_name, salary FROM employees;

-- Fetch with calculated fields
SELECT first_name, last_name, salary, salary * 0.1 AS bonus FROM employees;

-- Fetch with WHERE conditions
SELECT * FROM employees 
WHERE department = 'Engineering' AND (salary > 70000 OR experience_years > 5);
```

### Update

```sql
-- Update a single record
UPDATE employees SET salary = 65000 WHERE employee_id = 101;

-- Update multiple fields
UPDATE employees SET  salary = 75000, last_updated = CURRENT_TIMESTAMP WHERE employee_id = 102;

-- Update multiple records
UPDATE employees SET salary = salary * 1.1 WHERE department = 'Sales' AND performance_rating > 8;
```

### Delete

```sql
-- Delete a single record
DELETE FROM employees WHERE employee_id = 103;

-- Delete multiple records with single command
DELETE FROM employees WHERE department = 'Marketing' AND hire_date < '2020-01-01';

-- Delete all records from a table
DELETE FROM temp_log_entries;
-- OR (faster for large tables, resets AUTO_INCREMENT)
TRUNCATE TABLE temp_log_entries;
```

### Sorting/Ordering results (ORDER BY)
```sql
-- sort selected data in ascending order
SELECT * FROM <table>
    ORDER BY <column_name>;
```

```sql
-- sort selected data in descending order
SELECT * FROM <table>
    ORDER BY <column_name> DESC;
```

### LIMIT:
```sql
-- Select first x number of rows
SELECT * FROM <table>
    LIMIT <x>;
```

### OFFSET:
```sql
-- Select x number of rows after y number of rows (Used in pagination)
SELECT * FROM <table>
    LIMIT <x>
    OFFSET <y>;
```

### DISTINCT:
```sql
-- remove duplicate field values from result set
SELECT DISTINCT <field> FROM <table>;
```

### Subqueries and Views
```sql
SELECT customer_name, product_name FROM 
    (SELECT * FROM sales WHERE volume > 1000) AS base_result;

-- We can write above using View

CREATE VIEW base_result AS SELECT * FROM sales WHERE volume > 1000;
SELECT customer_name, product_name FROM base_result;
```

### Foreign key constraints ( ON DELETE & ON UPDATE )

| Constraint | Use Case |
| :--: | :--: |
| RESTRICT | Prevent the action (e.g. deleting a related row) |
| NO ACTION | Prevent the action (e.g. deleting a related row)<br>*(Check can be different)* |
| CASCADE | Perform the same action on the row with the foreign key |
| SET NULL | Set foreign key value to NULL if the related row was deleted |
| SET DEFAULT | Set foreign key value to DEFAULT value if the related row was deleted |

### Connecting two tables (Primary key + Foreign key)

<table>
<tr><th>addresses</th><th>users</th></tr>

<tr><td>

| id |  street  | house_number |
| -- |  ------- | -------------|
| 1 | first street | 5 |
| 2 | second street | 12 |
| 3 | third street | 4 |

</td><td>

| id |  first_name  | address_id |
| -- |  ------- | -------------|
| 1 | John | 2 |
| 2 | Michael | 1 |
| 3 | Rakesh | 3 |

</td></tr>
</table>

```sql
CREATE TABLE addresses (
    id INT PRIMARY KEY AUTO_INCREMENT,
    street VARCHAR(100) NOT NULL,
    house_number VARCHAR(50) NOT NULL
);

-- If we don't use "(id)" then it will take the primary key of the table as a reference key
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    address_id INT REFERENCES addresses (id) ON DELETE CASCADE
);
-- Note: CASCADE => If I delete address having id 1, then it will delete user that has address_id 1
```

### Data Relationships (One to One, One to Many, Many to Many)

**Example:** Database of Company which contains below tables.
- **Employees:** id, first_name, last_name, email, team_id
- **Teams:** id, name, building_id
- **Projects:** id, title
- **Internet Accounts:** id, email, password
- **Buildings:** id, name

**One to One (1:1):**
- One record in Table A belongs to exactly one record in Table B.
- EX:
    - **employees - internet_accounts** (One employee has only one internet account)

**One to Many (1:n):**
- One record in Table A has one or many related records in Table B.
- Ex:
    - **employees - teams** (one team can have multiple employees)
    - **teams - buildings** (one building can have multiple teams)

**Many to Many (n:n):**
- One record in Table A has one or many related records in Table B and vice versa.
- It requires junction table to connect two tables.
- Ex:
    - **employees and projects** (There are multiple employees working on single project and one employee can also be working on multiple projects)

```sql
CREATE TABLE buildings (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE teams (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    building_id INT REFERENCES buildings (id) ON DELETE SET NULL
);

CREATE TABLE employees (
    id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    team_id INT DEFAULT 1 REFERENCES teams (id) ON DELETE SET DEFAULT
);

CREATE TABLE internet_accounts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(100) UNIQUE REFERENCES employees (email) ON DELETE CASCADE,
    password VARCHAR(100) NOT NULL
);

CREATE TABLE projects (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL
);

-- Intermediate table => n:n

-- Wrong way XXXXXXXXXXXXXXXXXXX
CREATE TABLE employees_projects (
    id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT REFERENCES employees (id) ON DELETE CASCADE,
    project_id INT REFERENCES projects (id) ON DELETE CASCADE
);

-- In above relation there may be duplicate entries in employees_projects table
-- Ex:
-- id | employee_id | project_id
-- 1  |      25     |     1
-- 2  |      25     |     1

CREATE TABLE employees_projects (
    employee_id INT REFERENCES employees (id) ON DELETE CASCADE,
    project_id INT REFERENCES projects (id) ON DELETE CASCADE,
    PRIMARY KEY (employee_id, project_id) -- composite primary keys
);
```

### Composite Primary Key
 - A combination of multiple columns can serve as a primary key, this is called composite primary key.
 - In above example, primary key of `employees_projects` is composite primary key.

 ### Composite Foreign Key
 - Composite foreign key is used to connect table with another table which has composite primary key.
 ```sql
FOREIGN KEY (beginningTime, day, tutorId) REFERENCES tutorial(beginningTime, day, tutorId);
 ```

 ### Self Referential Relationships
**One to Many (1:n)**
 - One manager can have multiple employees in his team.

    ```sql
    CREATE TABLE employees (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(50) NOT NULL,
        manager_id INT,
        FOREIGN KEY (manager_id) REFERENCES employees ON DELETE SET NULL
    );
    ```

**One to One (1:1)**
 - An employee can have only one mentor, and each mentor can mentor only one employee.

    ```sql
    CREATE TABLE employees (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(50) NOT NULL,
        mentor_id INT UNIQUE,
        FOREIGN KEY (mentor_id) REFERENCES employees ON DELETE SET NULL
    );
    ```

**Many to Many (n:n)**
 - user can have multiple friends and each friend can also have multiple friends.

    ```sql
    CREATE TABLE users (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(50) NOT NULL
    );

    CREATE TABLE user_friends (
        user_id INT,
        friend_id INT,
        CHECK (user_id < friend_id), -- to avoid duplication like 25-30 and 30-25, because both have same meaning that 25 and 30 are friends
        PRIMARY KEY (user_id, friend_id),
        FOREIGN KEY (user_id) REFERENCES users ON DELETE CASCADE,
        FOREIGN KEY (friend_id) REFERENCES users ON DELETE CASCADE
    );
    ```

### UNION

- UNION is a clause that combines multiple result sets into one result set by appending rows.

    ```sql
    SELECT * FROM users WHERE id < 3
    UNION
    SELECT * FROM users WHERE id > 5;

    -- If record is from another table then also it will combine the result sets
    --EX:

    SELECT id, first_name FROM users
    UNION
    SELECT id, street FROM addresses; -- It will add street name in first_name column
    ```

### Joining Data (Inner Join, Left Join, Cross Join)

<table>
<tr>
    <th>addresses</th>
    <th>users</th>
    <th>cities</th>
</tr>
<tr><td>

| id | street | house_number | city_id |
| -- | ------ | ------------ | ------- |
| 1 | Test street | 10A | 1 |
| 2 | Some street | 5 | 2 |
| 3 | My street | 18 | 3 |

</td><td>

| id | first_name | address_id |
| -- | ---------- | ---------- |
| 1 | Max | 1 |
| 2 | Manuel | 3 |

</td><td>

| id | name |
| -- | ---- |
| 1 | my city |
| 2 | test city |
| 3 | dummy city |

</td></tr>
</table>

- **INNER JOIN:** It will display the records that have matching values in both tables.

    ```sql
    -- Combined multiple inner joins (Output of first inner join will behave as a left table for second inner join)
    SELECT u.first_name, a.street, a.house_number, c.name AS city_name
    FROM users AS u
    INNER JOIN addresses AS a ON u.address_id = a.id
    INNER JOIN cities AS c ON a.city_id = c.id;
    ```

    **Output:**

    | first_name | street | house_number | city_name |
    | --------- | ------ | ------------- | --------- |
    | Max | Test street | 10A | my city |
    | Manuel | My street | 18 | dummy city |

- **LEFT JOIN (or LEFT OUTER JOIN):** It will display all records from the first table, and the matched records from the second table.

    ```sql
    SELECT u.first_name, a.street, a.house_number, c.name AS city_name
    FROM addresses AS a
    LEFT JOIN users AS u ON a.id = u.address_id
    INNER JOIN cities AS c ON a.city_id = c.id;
    ```

    **Output:**

    | first_name | street | house_number | city_name |
    | --------- | ------ | ------------- | --------- |
    | Max | Test street | 10A | my city |
    | NULL | Some street | 5 | test city |
    | Manuel | My street | 18 | dummy city |

- **CROSS JOIN:** It will combine every row from the first table with every row from the second table

    ```sql
    SELECT * FROM users
    CROSS JOIN addresses;
    ```

- **SELF JOIN:** Table is joined with itself. It is useful when you want to compare rows within the same table.
    - We can achieve Self Join using Left Join the table with it self.

    ```sql
    SELECT e1.name AS employee_name, e2.name AS manager_name
    FROM employees e1
    LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
    ```

- **FULL JOIN:** MySQL Does not support Full Join but we can achieve it using Union. (FULL JOIN = LEFT JOIN UNION RIGHT JOIN)
    ```sql
    SELECT u.first_name, a.street, a.house_number, c.name AS city_name
    FROM addresses AS a
    LEFT JOIN users AS u ON a.id = u.address_id
    INNER JOIN cities AS c ON a.city_id = c.id;

    UNION

    SELECT u.first_name, a.street, a.house_number, c.name AS city_name
    FROM addresses AS a
    RIGHT JOIN users AS u ON a.id = u.address_id
    INNER JOIN cities AS c ON a.city_id = c.id;
    ```

### Aggregate Functions (COUNT, MIN, MAX, SUM, AVG)
- Aggregate functions are functions that perform mathematical operation and return a single (aggregated) result.

- **Count:** The COUNT() function returns the number of records returned by a select query.
    ```sql
    SELECT COUNT(product_id) AS number_of_products FROM products;
    SELECT COUNT(DISTINCT booking_date) FROM bookings;
    ```

- **Min:** The MIN() function returns the minimum value in a set of values.
    ```sql
    SELECT MIN(price) AS smallest_price FROM products;
    ```

- **Max:** The MAX() function returns the maximum value in a set of values.
    ```sql
    SELECT MAX(price) AS largest_price FROM products;
    ```

- **Sum:** The SUM() function calculates the sum of a set of values.
    ```sql
    SELECT SUM(quantity) AS total_items_ordered FROM order_details;
    ```

- **Avg:** The AVG() function returns the average value of an expression.
    ```sql
    SELECT AVG(price) AS average_price FROM products;
    ```

- **Round:** The ROUND() function rounds a number to a specified number of decimal places.
    - **Syntax:** ROUND(number, decimals)
    ```sql
    SELECT ROUND(AVG(num_guests), 2) FROM bookings;

    SELECT ROUND(135.375, 2);
    -- Output: 135.38
    ```

### Group By
- The `GROUP BY` statement groups rows that have the same values into summary rows, like "find the number of customers in each country".
- The `GROUP BY` statement is often used with aggregate functions (`COUNT()`, `MIN()`, `MAX()`, `SUM()`, `AVG()`) to group the result-set by one or more columns.
- **Syntax:**
    ```sql
    SELECT column_name(s)
    FROM table_name
    WHERE condition
    GROUP BY column_name(s)
    ORDER BY column_name(s);
    ```

    ```sql
    -- EX: lists the number of customers in each country
    SELECT COUNT(customer_id), country
    FROM customers
    GROUP BY country;
    ```
- Group by multiple columns is say, for example, **GROUP BY column1, column2**. This means placing all the rows with the same values of columns **column 1** and **column 2** in one group.
    ```sql
    -- this count will return the count of records in that group
    SELECT subject, year, Count(*)
    FROM students
    GROUP BY subject, year;
    ```

### HAVING clause (filter grouped data)
- We know that `WHERE` clause is used to place conditions on columns but what if we want to place conditions on groups? This is where the `HAVING` clause comes into use. We can use the `HAVING` clause to filter results based on the conditions applied to grouped data, such as sums, averages or counts.
- we cannot use aggregate functions like `SUM()`, `COUNT()`, `AVG()`, etc., directly in the `WHERE` clause. Instead, we use the `HAVING` clause when we need to filter data based on the result of these aggregate functions.
- **Syntax:**
    ```sql
    SELECT column1, function_name(column2)
    FROM table_name
    WHERE condition
    GROUP BY column1, column2
    HAVING condition
    ORDER BY column1, column2;
    ```
    ```sql
    -- Filter groups whose total salary of all employees exceeds 50000.
    SELECT NAME, SUM(sal) FROM employees
    GROUP BY name
    HAVING SUM(sal)>50000; 
    ```

### Nested Subqueries
```sql
-- EX: 1
SELECT MIN(daily_sum)
FROM (
    SELECT booking_date, SUM(amount_billed) AS daily_sum
    FROM bookings
    GROUP BY booking_date
) AS daily_table;

--Ex: 2
SELECT * 
FROM employees
WHERE department=(SELECT department FROM departments WHERE dept_id=1);

-- Ex: 3
SELECT * 
FROM employees 
WHERE salary < (SELECT avg(salary) from employees)
```

### Window Function
- Using window functions, we can use aggregate functions without reducing rows like grouping. (In `GROUP BY`, it reduces the rows of same group into the single row).
- We can add result of aggregation into new column.
- **Syntax:**
    ```sql
    SELECT column_name1, 
    window_function(column_name2)
    OVER([PARTITION BY column_name1] [ORDER BY column_name3]) AS new_column
    FROM table_name;
    ```
1. Aggregate Window Functions:
    - Aggregate window functions calculate aggregates over a window of rows while retaining individual rows.
    - Aggregate functions: `COUNT()`, `MIN()`, `MAX()`, `SUM()`, `AVG()`
    - EX:
        ```sql
        SELECT name, age, department, salary,
        AVG(salary) OVER(PARTITION BY department) AS avg_salary
        FROM employees;
        -- Output: (records with same department belongs to single window)
        -- it will add column named avg_salary in which the average value of its window is stored in that column
        ```

2.  Ranking Window Functions:
    - These functions provide rankings of rows within a partition based on specific criteria.
    - Ranking Functions:
        - `RANK()`: Assigns ranks to rows, skipping ranks for duplicates.
            - EX:
                ```sql
                SELECT name, department, salary,
                    RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS emp_rank
                FROM employees;
                -- output
                -- name     | department    | salary | emp_rank
                -- -------------------------------------
                -- Ramesh   | Finance       | 50,000 | 1
                -- Suresh   | Finance       | 50,000 | 1
                -- Ram      | Finance       | 20,000 | 3
                -- Deep     | Sales         | 30,000 | 1
                -- Pradeep  | Sales         | 20,000 | 2

                -- Note:  For Ram, rank is 3 because rank for Suresh has been skipped (i.e. rank 2 is skipped). And rank from Deep will start from 1 because new window is stated (window for sales is started).
                ```
        - `DENSE_RANK()`: Assigns ranks to rows without skipping rank numbers for duplicates.
            - EX:
            ```sql
            SELECT name, department, salary,
                DENSE_RANK() OVER(PARTITION BY department ORDER BY salary DESC) AS emp_dense_rank
            FROM employees;
            -- output
            -- name     | department    | salary | emp_dense_rank
            -- -------------------------------------
            -- Ramesh   | Finance       | 50,000 | 1
            -- Suresh   | Finance       | 50,000 | 1
            -- Ram      | Finance       | 20,000 | 2
            -- Deep     | Sales         | 30,000 | 1
            -- Pradeep  | Sales         | 20,000 | 2

            -- Note: For Ram, rank is 2 because rank for suresh is not skipped.
            ```
        - `ROW_NUMBER()`: Assigns a unique number to each row in the result set.
            - EX:
            ```sql
            SELECT name, department, salary,
                ROW_NUMBER() OVER(PARTITION BY department ORDER BY salary DESC) AS emp_row_no
            FROM employees;
            -- output
            -- name     | department    | salary | emp_row_no
            -- -------------------------------------
            -- Ramesh   | Finance       | 50,000 | 1
            -- Suresh   | Finance       | 50,000 | 2
            -- Ram      | Finance       | 20,000 | 3
            -- Deep     | Sales         | 30,000 | 1
            -- Pradeep  | Sales         | 20,000 | 2
            ```

### [MYSQL Functions](https://www.w3schools.com/mysql/mysql_ref_functions.asp)
**memberships table for examples:**
```sql
CREATE TABLE memberships (
    id INT PRIMARY KEY AUTO_INCREMENT,
    membership_start DATE,
    membership_end DATE,
    last_checkin TIMESTAMP,
    last_checkout TIMESTAMP,
    consumption NUMERIC(5, 2),
    first_name VARCHAR(200),
    last_name VARCHAR(200),
    price NUMERIC(5, 2),
    billing_frequency INT,
    gender VARCHAR(200)
);
```

- Arithmetic multiplication
    ```sql
    SELECT price * billing_frequency AS annual_revenue
    FROM memberships;

    SELECT SUM(price * billing_frequency) AS total_annual_revenue
    FROM memberships;
    ```
- Mathematical Functions
    - [CEIL](https://www.w3schools.com/mysql/func_mysql_ceil.asp) Returns the smallest integer value that is >= to a number
        ```sql
        SELECT CEIL(consumption), consumption
        FROM memberships;

        -- Output:
        -- CEIL(consumption)    |   consumption
        -- ------------------------------------
        -- 27                   |   26.49
        -- 101                  |   100.26
        -- 6                    |   5.82
        ```
    - [FLOOR](https://www.w3schools.com/mysql/func_mysql_floor.asp) Returns the largest integer value that is <= to a number
        ```sql
        SELECT FLOOR(consumption), consumption
        FROM memberships;

        -- Output:
        -- FLOOR(consumption)    |   consumption
        -- ------------------------------------
        -- 26                   |   26.49
        -- 100                  |   100.26
        -- 5                    |   5.82
        ```
    - [ROUND](https://www.w3schools.com/mysql/func_mysql_round.asp) Rounds a number to a specified number of decimal places
        ```sql
        SELECT ROUND(consumption, 2) AS val_1, ROUND(consumption, 1) AS val_2, ROUND(consumption, 0) AS val_3, consumption
        FROM memberships;

        -- Output:
        -- val_1  | val_2  | val_3 | consumption
        -- ------------------------------------
        -- 26.49  | 26.5  | 26  | 26.49
        -- 100.26 | 100.3 | 100 | 100.26
        -- 5.82   | 5.8   | 6   | 5.82
        ```
    - [TRUNCATE](https://www.w3schools.com/mysql/func_mysql_truncate.asp) Truncates a number to the specified number of decimal places
        ```sql
        SELECT TRUNCATE(consumption, 2) AS val_1, TRUNCATE(consumption, 1) AS val_2, TRUNCATE(consumption, 0) AS val_3, consumption
        FROM memberships;

        -- Output:
        -- val_1  | val_2  | val_3 | consumption
        -- ------------------------------------
        -- 26.49  | 26.4  | 26  | 26.49
        -- 100.26 | 100.2 | 100 | 100.26
        -- 5.82   | 5.8   | 5   | 5.82
        ```

- String Functions
    - [CONCAT](https://www.w3schools.com/mysql/func_mysql_concat.asp) Adds two or more expressions together
        ```sql
        -- 1. Concat two strings
        SELECT CONCAT(first_name, ' ', last_name) AS full_name, first_name, last_name
        FROM memberships;
        -- O/P: Parth Patel (where first_name = Parth & last_name = Patel)

        -- 2. Concat string with number
        SELECT CONCAT("$ ", price)
        FROM memberships;
        -- O/P: $ 5 (where price = 5)
        ```
    - [LOWER](https://www.w3schools.com/mysql/func_mysql_lower.asp) Converts a string to lower-case
        ```sql
        -- 1. LOWER function with insert query
        INSERT INTO memberships (
            membership_start,
            membership_end,
            last_checkin,
            last_checkout,
            consumption,
            first_name,
            last_name,
            price,
            billing_frequency,
            gender
        )
        VALUES (
            '2025-07-23',
            '2025-08-23',
            '2025-07-23 08:56:01',
            '2025-07-23 09:20:12',
            NULL,
            'John',
            'Doe',
            19.99,
            12,
            LOWER('DivErs')
        );
        -- O/P: DivErs will convert into divers
        ```
    - [LENGTH](https://www.w3schools.com/mysql/func_mysql_length.asp) Returns the length of a string (in bytes)
        ```sql
        SELECT * FROM memberships
        WHERE LENGTH(last_name) < 4;
        -- if str = "John" then LENGTH(str) = 4
        ```
    
    - [TRIM](https://www.w3schools.com/mysql/func_mysql_trim.asp) Removes leading and trailing spaces/characters from a string
        ```sql
        SELECT trimmed_string, LENGTH(trimmed_string) FROM (
            SELECT TRIM("   SQL Tutorial    ") AS trimmed_string
        ) AS temp_table;
        -- O/P => 'SQL Tutorial', 12

        SELECT trimmed_string, LENGTH(trimmed_string) FROM (
            SELECT TRIM(LEADING ' ' FROM "   SQL Tutorial    ") AS trimmed_string
        ) AS temp_table;
        -- O/P => 'SQL Tutorial    ', 16

        SELECT trimmed_string, LENGTH(trimmed_string) FROM (
            SELECT TRIM(TRAILING ' ' FROM "   SQL Tutorial    ") AS trimmed_string
        ) AS temp_table;
        -- O/P => '   SQL Tutorial', 15

        SELECT trimmed_string, LENGTH(trimmed_string) FROM (
            SELECT TRIM(BOTH '*' FROM "***SQL Tutorial****") AS trimmed_string
        ) AS temp_table;
        -- O/P => 'SQL Tutorial', 12
        ```

- Date Functions
    - [EXTRACT](https://www.w3schools.com/mysql/func_mysql_extract.asp) Extracts a part from a given date 
        ```sql
        SELECT
            EXTRACT(MONTH FROM last_checkin),
            EXTRACT(DAY FROM last_checkin),
            EXTRACT(MINUTE FROM last_checkin),
            last_checkin
        FROM memberships;
        -- O/P => 10, 1, 17, 2025-10-01 05:17:16
        ```

    - [WEEKDAY](https://www.w3schools.com/mysql/func_mysql_weekday.asp) Returns the weekday number for a given date
        - **Note:** 0 = Monday, 1 = Tuesday, 2 = Wednesday, 3 = Thursday, 4 = Friday, 5 = Saturday, 6 = Sunday.
        ```SQL
        SELECT WEEKDAY("2025-07-24");
        -- O/P => 3
        ```
    - [CONVERT](https://www.w3schools.com/mysql/func_mysql_convert.asp) converts a value into the specified datatype or character set
        ```sql
        SELECT CONVERT("2025-07-24 08:10:15", DATE);
        -- O/P => 2025-07-24

        SELECT CONVERT("2025-07-24 08:10:15", TIME);
        -- O/P => 08:10:15

        SELECT CONVERT("2025-07-24", DATETIME);
        -- O/P => 2025-07-24 00:00:00
        ```
    
    - [TIMESTAMPDIFF](https://dev.mysql.com/doc/refman/8.4/en/date-and-time-functions.html#function_timestampdiff) Return the difference of two datetime expressions, using the units specified
        ```sql
        SELECT TIMESTAMPDIFF(MINUTE, last_checkin, last_checkout)
        FROM memberships;
        -- O/P => return difference in minutes
        ```
    - [DATEDIFF](https://www.w3schools.com/mysql/func_mysql_datediff.asp) Returns the number of days between two date values
        ```sql
        SELECT DATEDIFF("2025-06-25", "2025-06-15");
        -- O/P => 10

        SELECT DATEDIFF(NOW(), "2025-06-15");
        -- O/P => it will give no. of days between given date and current date
        ```
    
    - [DATE_ADD](https://www.w3schools.com/mysql/func_mysql_date_add.asp) Adds a time/date interval to a date and then returns the date
        ```sql
        SELECT DATE_ADD("2025-06-15", INTERVAL 10 DAY);
        -- O/P => 2025-06-25

        SELECT DATE_ADD("2025-06-15", INTERVAL 10 YEAR);
        -- O/P => 2035-06-15

        SELECT DATE_ADD("2025-06-15", INTERVAL 10 MONTH);
        -- O/P => 2026-04-15
        ```

### [LIKE](https://www.w3schools.com/mysql/mysql_like.asp)
- The `LIKE` operator is used in a WHERE clause to search for a specified pattern in a column.
    | LIKE Operator | Description |
    |---------------|-------------|
    | 'a%' | Finds any values that start with "a" |
    | '%a' | Finds any values that end with "a" |
    | '%or%' | Finds any values that have "or" in any position |
    | '_r%' | Finds any values that have "r" in the second position |
    | 'a_%' | Finds any values that start with "a" and are at least 2 characters in length |
    | 'a__%' | Finds any values that start with "a" and are at least 3 characters in length |
    | 'a%o' | 	Finds any values that start with "a" and ends with "o" |

    ```sql
    SELECT * FROM memberships
    WHERE first_name LIKE 'a%';
    ```

### [EXISTS](https://www.w3schools.com/mysql/mysql_exists.asp)
- The `EXISTS` operator is used to test for the existence of any record in a subquery.
    ```sql
    SELECT supplier_name
    FROM suppliers
    WHERE EXISTS (SELECT product_name FROM products WHERE products.supplier_id = suppliers.supplier_id AND price < 20);
    ```

### [IN](https://www.w3schools.com/mysql/mysql_in.asp) / NOT IN
- The `IN` operator allows you to specify multiple values in a WHERE clause.
- The `IN` operator is a shorthand for multiple `OR` conditions.
    ```sql
    SELECT * FROM customers
    WHERE country IN ('Germany', 'France', 'UK');

    SELECT * FROM customers
    WHERE country NOT IN ('Germany', 'France', 'UK');

    SELECT * FROM customers
    WHERE country IN (SELECT country FROM suppliers);
    ```

### [MySQL CASE Statement](https://www.w3schools.com/mysql/mysql_case.asp)
```sql
SELECT order_id, quantity,
CASE
    WHEN quantity > 30 THEN 'The quantity is greater than 30'
    WHEN quantity = 30 THEN 'The quantity is 30'
    ELSE 'The quantity is under 30'
END AS quantity_text
FROM order_details;
```

### [MySQL Transaction](https://www.geeksforgeeks.org/mysql/mysql-transaction/)
- A database transaction is a series of operations executed as a single, all-or-nothing unit of work. To be more precise, all the operations inside a transaction must be completed; otherwise, it will roll back to the previous state before the operations took place.
    ```sql
    START TRANSACTION;

    SAVEPOINT savepoint1;

    UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;

    SAVEPOINT savepoint2;

    UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;

    -- If an error occurs, roll back to a specific savepoint
    ROLLBACK TO SAVEPOINT savepoint1;

    -- OR roll back to the state before transaction
    ROLLBACK;

    -- Finally, commit the transaction
    COMMIT;
    ```
- `START TRANSACTION` begins a new transaction.
- `SAVEPOINT identifier` establishes a named checkpoint inside the current transaction. If a savepoint with the same name already exists, it's replaced.
- `ROLLBACK TO SAVEPOINT identifier` undoes all modifications made after that savepoint **without terminating the transaction**, and removes any savepoints created after it.
- `ROLLBACK` without specifying a savepoint aborts the entire transaction and resets to before the START TRANSACTION. All savepoints are cleared.
- `COMMIT` makes changes permanent, ends the current transaction, and clears all savepoints.
- **Note:** without rollback, if we start another transaction then the changes will be applied to the database permanently.

### [MySQL Indexes](https://www.geeksforgeeks.org/mysql/mysql-indexes/)
- MySQL indexes are designed tools to increase the speed and efficiency of data retrieval operations within a database.
- If we create an index on a column, the database builds a separate index structure that stores the column's values in sorted order to enable faster searches. When you look for a record, the database first searches the index. Since the index is sorted, this lookup is much faster than scanning the entire table. Once the matching key is found in the index structure, it follows the stored pointer to retrieve the corresponding record from the main data table.
- **Note:** Avoid using too many indexes. Use them only for read-intensive data (data that is retrieved frequently but not often updated). This is because if updates occur on indexed columns, the index table must also be updated. This makes update operations slower and more resource-intensive.
- `EXPLAIN` | `EXPLAIN ANALYZE`: explain with select query will display which indexes are being used and what is the cost of the query.
    ```sql
    EXPLAIN ANALYZE
    SELECT * FROM users WHERE salary > 12000;
    -- O/P => cost=0.85 rows=2 actual_time=0.069..0.075 loops=1
    ```

- Single Column Index:
    ```sql
    CREATE INDEX salary_idx ON users (salary);

    -- Index at the time of table creation
    CREATE TABLE users (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(200),
        salary INT,
        INDEX salary_idx (salary)
    );
    ```

- Unique Index:
    ```sql
    CREATE UNIQUE INDEX salary_idx ON users (salary);

    -- Unique index at the time of table creation
    CREATE TABLE users (
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(200),
        salary INT UNIQUE, -- this will create unique index by default
    );
    ```

- Multi-Column Index:
    ```sql
    CREATE INDEX address_idx ON addresses (street, city, user_id);
    /*
        this index will be used if below are in where condition of query
        1. street
        2. combination of street & city
        3. combination of street, city & user_id
    */
    ```

- Full-Text Index:
    ```sql
    CREATE FULLTEXT INDEX user_name_idx ON users (name);
    ```

- Show Indexes on table:
    ```sql
    SHOW INDEX FROM users;
    ```

- Deleting Index from table:
    ```sql
    DROP INDEX salary_idx ON users;
    ```
