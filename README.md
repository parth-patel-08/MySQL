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
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments_table(id)
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

### Filtering using WHERE
Note: operators in where: =, !=, >, >=, <, <=, AND, OR, BETWEEN, IS, IS NOT.
(IS & IS NOT is used for NULL, TRUE, ...etc EX: IS NULL)

#### 1) Create Sales Table

```sql
CREATE TABLE sales (
    id INT PRIMARY KEY AUTO_INCREMENT,
    date_created DATE DEFAULT (CURRENT_DATE),
    date_fulfilled DATE,
    customer_name VARCHAR(300) NOT NULL,
    product_name VARCHAR(300) NOT NULL,
    volume NUMERIC(10,3) NOT NULL CHECK (volume >= 0),
    is_recurring BOOLEAN DEFAULT FALSE,
    is_disputed BOOLEAN DEFAULT FALSE
);
```

#### 2) Insert Sample Data

```sql
INSERT INTO sales (date_created, date_fulfilled, customer_name, product_name, volume, is_recurring, is_disputed) VALUES
-- Regular sales with various volumes
('2023-01-05', '2023-01-08', 'Acme Corp', 'Premium Widget', 1500.000, FALSE, FALSE),
('2023-01-10', '2023-01-12', 'TechStart Inc', 'Basic Package', 800.500, FALSE, FALSE),
('2023-01-15', '2023-01-23', 'Global Solutions', 'Enterprise Software', 12000.000, TRUE, FALSE),
('2023-01-20', '2023-01-22', 'Smith Consulting', 'Professional Services', 3500.000, FALSE, FALSE),
('2023-01-25', '2023-01-26', 'Johnson LLC', 'Basic Widget', 500.000, FALSE, FALSE),
-- Recurring sales
('2023-02-01', '2023-02-05', 'Acme Corp', 'Support Package', 750.000, TRUE, FALSE),
('2023-02-05', '2023-02-08', 'TechStart Inc', 'Cloud Services', 2500.000, TRUE, FALSE),
('2023-02-10', '2023-02-15', 'Global Solutions', 'Maintenance Contract', 8000.000, TRUE, FALSE),
-- Disputed sales
('2023-02-15', '2023-02-20', 'Acme Corp', 'Custom Development', 6500.000, FALSE, TRUE),
('2023-02-18', '2023-02-25', 'Johnson LLC', 'Premium Package', 4500.000, FALSE, TRUE),
('2023-02-20', NULL, 'TechStart Inc', 'Hardware Bundle', 9500.000, FALSE, TRUE),
-- Sales with various fulfillment times
('2023-03-01', '2023-03-02', 'Smith Consulting', 'Rush Project', 2800.000, FALSE, FALSE),
('2023-03-05', '2023-03-15', 'Global Solutions', 'Large Implementation', 15000.000, FALSE, FALSE),
('2023-03-10', NULL, 'Johnson LLC', 'Pending Order', 1200.000, FALSE, FALSE),
-- Recent sales
('2023-03-15', '2023-03-18', 'Acme Corp', 'Standard Package', 950.000, FALSE, FALSE),
('2023-03-20', '2023-03-21', 'Smith Consulting', 'Consultation', 450.000, FALSE, FALSE),
('2023-03-25', '2023-03-28', 'Global Solutions', 'Software License', 5500.000, TRUE, FALSE),
('2023-03-28', '2023-03-30', 'TechStart Inc', 'Starter Kit', 350.000, FALSE, FALSE),
('2023-04-01', '2023-04-05', 'Johnson LLC', 'Premium Support', 1800.000, TRUE, FALSE),
('2023-04-05', NULL, 'New Customer Ltd', 'Trial Package', 250.000, FALSE, FALSE);
```

#### 3) Find All Sales with Volume > 1000

```sql
SELECT * FROM sales WHERE volume > 1000;
```

#### 4) Find All Recurring Sales

```sql
SELECT * FROM sales WHERE is_recurring = TRUE;
--OR
SELECT * FROM sales WHERE is_recurring IS TRUE;
```

#### 5) Find Disputed Sales with Volume > 5000

```sql
SELECT * FROM sales WHERE (is_disputed IS TRUE) AND (volume > 5000);
```

#### 6) Find All Sales Created Between Two Dates

```sql
SELECT * FROM sales WHERE date_created >= '2023-02-01' AND date_created <= '2023-02-28';
-- OR we can use BETWEEN (Note: both dates are inclusive)
SELECT * FROM sales WHERE date_created BETWEEN '2023-02-01' AND '2023-02-28';
```

#### 7) Find All Sales Fulfilled <= 5 Days After Creation Date

```sql
SELECT *, DATEDIFF(date_fulfilled, date_created) AS days_to_fulfill FROM sales
WHERE date_fulfilled IS NOT NULL AND DATEDIFF(date_fulfilled, date_created) <= 5;
```

#### 8) Find Top 10 Sales

```sql
SELECT * FROM sales
    ORDER BY volume DESC
    LIMIT 10;
```

#### 9) Find Bottom 10 Sales

```sql
-- ASC is optional
SELECT * FROM sales
    ORDER BY volume ASC
    LIMIT 10;
```

#### 10) Find top 11th to 25th Sales
```sql
SELECT * FROM sales
    ORDER BY volume DESC
    LIMIT 15
    OFFSET 10;
```

#### 11) Get a List of Distinct Customers

```sql
SELECT DISTINCT customer_name FROM sales;
```

### Normalization

- Data normalization is the process of organizing data in a database to minimize **redundancy** and **dependency**. The goal of normalization is to ensure that the data is stored in the most efficient way while maintaining data integrity.
- Key concepts:
    - **Redundancy:** Repetition of data, which can lead to inconsistencies and inefficiencies.
    - **Dependency:** The relationship between data fields in a database. If some pieces of data depend on others, they should be stored in a way that reflects that relationship.
    - **Normalization Levels:** There are several "normal forms" (1NF, 2NF, 3NF, etc.), each with stricter rules to reduce redundancy and dependencies.

| Normal Form | Rules/Criteria | Purpose |
| ----------- | -------------- | ------- |
| **1st Normal Form (1NF)** | 	1. **Atomicity**: Each column must contain atomic (indivisible) values. This means no lists or sets in a column. <br /> 2. **Uniqueness**: Each row must be unique. | Eliminate repeating groups and ensure the table has atomic values (no multi-valued columns). |
| **2nd Normal Form (2NF)** | 	1. **Be in 1NF**. <br /> 2. **Remove Partial Dependency**: No non-key column should depend on only part of a composite primary key (i.e., if the primary key consists of more than one column, all non-key attributes must depend on the whole key, not just part of it). | Eliminate redundancy caused by partial dependency in composite keys. |
| **3rd Normal Form (3NF)** | 1. **Be in 2NF**. <br /> 2. **Remove Transitive Dependency**: Non-key columns should not depend on other non-key columns. All non-key columns should depend only on the primary key. | Remove transitive dependencies between non-key attributes. |
| **Boyce-Codd Normal Form (BCNF)** | 1. **Be in 3NF**. <br /> 2. Every **determinant** (attribute that determines another attribute) must be a **candidate key**. In other words, if a non-prime attribute (non-key attribute) determines another attribute, the determinant must be a candidate key. | Resolve issues not handled by 3NF, especially with respect to candidate keys. |
| **4th Normal Form (4NF)** | 1. **Be in BCNF**. <br /> 2. **Remove Multivalued Dependency**: If a table has two or more independent and multivalued facts about an entity, separate them into different tables. | Eliminate multivalued dependencies. |
| **5th Normal Form (5NF)** | 1. **Be in 4NF**. <br /> 2. **Remove Join Dependency**: A table should not contain any join dependency, i.e., it should be decomposable into smaller tables without loss of data. | Eliminate redundancy caused by join dependencies. |
| **Domain-Key Normal Form (DKNF)** | The table is free of all kinds of anomalies, and every constraint is a logical consequence of domain constraints (the set of valid values for a column) and key constraints (the relationships among primary and foreign keys). | Ensure the table is free from all anomalies (constraints apply directly). |

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
    id INT PRIMARY KEY AUTO_INCREMENT
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
    email VARCHAR(100) REFERENCES employees (email) ON DELETE CASCADE,
    password VARCHAR(100) NOT NULL
);

CREATE TABLE projects (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(100) NOT NULL
);

-- Intermediate table => n:n
CREATE TABLE employees_projects (
    id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT REFERENCES employees (id) ON DELETE CASCADE,
    project_id INT REFERENCES projects (id) ON DELETE CASCADE
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

- **SELF JOIN:** Table is joined with itself. It is useful when you want to compare rows within the same table.
    - We can achieve Self Join using Left Join the table with it self.

    ```sql
    SELECT e1.name AS employee_name, e2.name AS manager_name
    FROM employees e1
    LEFT JOIN employees e2 ON e1.manager_id = e2.employee_id;
    ```
