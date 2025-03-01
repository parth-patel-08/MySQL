# MySQL Documentation

## Data Definition Operations (Create/Update Database/Table)

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
