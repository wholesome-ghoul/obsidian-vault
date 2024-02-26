```sql
CREATE USER <user> WITH PASSWORD '<password>' CREATEDB;
```

```bash
sudo apt-get install postgresql
sudo systemctl start postgresql
sudo -u postgres psql

createdb <db-name>

psql -U <user> -d <db-name>
psql -U <user> -d <db-name> -a -f <file.sql>

# check clusters' status
pg_lsclusters

# binary locations (initdb, etc)
# /usr/lib/postgresql/<version>/bin

# Creating a cluster
mkdir /usr/local/pgsql
chown <user> /usr/local/pgsql
su <user>
pg_ctl -D /usr/local/pgsql/data initdb
pg_ctl -D /usr/local/pgsql/data -l logfile start

# Stop a cluster
# pg_ctlcluster <version> <cluster> stop
pg_ctlcluster 13 main stop

# load database
pg_restore -U postgres -d dvdrental file.tar
```


```sql
ALTER TABLE <table-name> RENAME COLUMN <old-name> TO <new-name>;

-- FROM -> WHERE -> SELECT -> ORDER BY
-- FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> DISTINCT -> ORDER BY -> LIMIT
-- 
-- USING clause is not a part of the SQL standard
--
-- Boolean
-- Character CHAR(n), VARCHAR(n), TEXT
-- Numeric SMALLINT, INT, Serial, float(n), real or float8, numeric or numeric(p,s)
-- Temporal DATE, TIME, TIMESTAMP, TIMESTAMPZ, INTERVAL
-- UUID
-- Array
-- JSON JSON, JSONB
-- hstore
-- Special box, line, point, lseg, polygon, inet, macaddr
--
-- column constraints: PRIMARY KEY, FOREIGN KEY, NOT NULL, UNIQUE, CHECK, DEFAULT

-- \l will show all dbs in psql server

-- switch to db
-- \c databsaename

-- display all tables
-- \dt

-- all available psql cmds
-- \?

-- set table output option.
-- null will be displayed:
-- \pset null null

-- concatenation
SELECT
  first_name || ' ' || last_name,
  -- with column alias
  -- first_name || ' ' || last_name AS column_alias,
  -- first_name || ' ' || last_name column_alias,
  -- first_name || ' ' || last_name "column alias",
  email
FROM
  customer;

SELECT NOW();

SELECT
  first_name,
  LENGTH(last_name) len
FROM
  customer
ORDER BY
  len DESC;

-- ORDER BY sort_expression [ASC | DESC] [NULLS FIRST | NULLS LAST]

SELECT
  distinct first_name,
  last_name,
FROM
  customer
ORDER BY
  first_name,
  last_name ;

SELECT
  first_name,
  last_name
FROM
  customer
WHERE
  first_name IN ('a', 'b', 'c');

SELECT
  first_name,
  LENGTH(first_name) name_length
FROM
  customer
WHERE
  -- ILIKE is case-insensitive
  -- ~~ LIKE
  -- ~~* ILIKE
  -- !~~ NOT LIKE
  -- !~~* NOT ILIKE
  first_name ILIKE '_A%'
  AND LENGTH(first_name) BETWEEN 3 AND 5
ORDER BY
  name_length;

SELECT
  message
FROM
  t
WHERE
  -- "regex translation" *10%*
  message LIKE '%10$%%' ESCAPE '$';

SELECT
  first_name,
  last_name,
FROM
  customer
WHERE
  first_name LIKE 'B%'
  AND last_name <> 'Motley';

SELECT
  film_id,
  title,
  release_year
FROM
  customer
ORDER BY
  film_id
LIMIT 4 OFFSET 3;

-- OFFSET row_to_skip { ROW | ROWS }
-- FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
SELECT
  film_id,
  title,
  release_year
FROM
  customer
ORDER BY
  title
OFFSET 5 ROWS
FETCH FIRST 5 ROWS ONLY;

SELECT
  payment_id,
  amount,
  payment_date
FROM
  payment
WHERE
  -- ISO 8601 format; YYYY-MM-DD
  payment_date::date IN ('2007-02-15', '2007-02-16');

SELECT
  address,
  address2
FROM
  address
WHERE
  address2 IS NULL;

SELECT
  a,
  fruit_a,
  b,
  fruit_b
FROM
  basket_a
-- LEFT JOIN same as LEFT OUTER JOIN
-- RIGHT JOIN same as RIGHT OUTER JOIN
-- FULL JOIN same as FULL OUTER JOIN
INNER JOIN basket_b
  ON fruit_a = fruit_b;

-- table alias
SELECT
  c.customer_id,
  c.first_name,
  p.amount,
  p.payment_date
FROM
  customer c
  INNER JOIN payment p ON p.customer_id = c.customer_id
ORDER BY
  p.payment_date DESC;

-- self join
SELECT
  f1.title,
  f2.title,
  f1.length
FROM
  film f
INNER JOIN film f2
  ON f1.film_id <> f2.film_id AND
    f1.length = f2.length;

SELECT
  customer_id,
  first_name,
  last_name,
  amount,
  payment_date,
FROM
  customer
  INNER JOIN payment USING(customer_id)
ORDER BY
  payment_date;

SELECT
  c.customer_id,
  c.first_name || ' ' || c.last_name customer_name,
  s.first_name || ' ' || s.last_name staff_name,
  p.amount,
  p.payment_date
FROM
  customer c
  INNER JOIN payment p USING(customer_id)
  INNER JOIN staff s USING(staff_id)
ORDER BY
  payment_date;

-- cross join
-- good for scheduling, inventory management etc
SELECT *
FROM t1
CROSS JOIN t2;

-- same as
SELECT *
FROM t1,t2
-- same as
SELECT *
FROM t1
INNER JOINT t2 ON true;

-- NATURAL JOIN
-- never use it

-- GROUP BY
SELECT
  first_name || ' ' || last_name full_name,
  SUM(amount) amount
FROM
  payment
  INNER JOIN customer USING(customer_id)
GROUP BY
  full_name
ORDER BY
  amount DESC;

-- HAVING
SELECT
  customer_id,
  SUM(amount) amount
FROM
  payment
GROUP BY
  customer_id
HAVING
  SUM(amount) > 100
GROUP BY
  amount DESC;

-- INTERSECT
-- EXCEPT
-- UNION [ALL]
SELECT * FROM top_rated_films
UNION
SELECT * FROM most_popular_films;

-- GROUPING SETS
SELECT
  brand,
  segment,
  SUM(quantity)
FROM
  sales
GROUP BY
  GROUPING SETS(
    (brand, segment),
    (brand),
    (segment),
    (),
  );

SELECT
  GROUPING(brand) grouping_brand,
  GROUPING(segment) grouping_segment,
  brand,
  segment,
  SUM(quantity)
FROM
  sales
GROUP BY
  GROUPING SETS (
    (brand),
    (segment),
    (),
  )
HAVING GROUPING(brand) = 0
ORDER BY
  brand
  segment;

SELECT
  brand,
  segment,
  SUM(quantity)
FROM
  sales
GROUP BY
  CUBE(brand, segment)
ORDER BY
  brand,
  segment;

SELECT
  brand,
  segment,
  SUM(quantity)
FROM
  sales
GROUP BY
  ROLLUP(brand, segment)
ORDER BY
  brand,
  segment;

SELECT
  city
FROM
  city
WHERE
  country_id = (
    SELECT
      country_id
    FROM
      country
    WHERE
      country = 'Name'
  )
ORDER BY
  city;

-- SOME/ANY
-- ALL
-- EXISTS
SELECT
  *
FROM
  employess
WHERE
  salary > ANY (
    SELECT
      salary
    FROM
      managers
  );

-- common table expression (CTE)
WITH action_films AS (
  SELECT
    f.title,
    f.length
  FROM
    film f
    INNER JOIN film_category fc USING(film_id)
    INNER JOIN category c USING(category_id)
  WHERE
    c.name = 'Action'
)
SELECT * FROM action_films;

WITH cte_rental AS (
  SELECT
    staff_id,
    COUNT(rental_id) rental_count
  FROM
    rental
  GROUP BY
    staff_id
)
SELECT
  s.staff_id,
  first_name,
  last_name,
  rental_count
FROM
  staff s
  INNER JOIN cte_rental USING(staff_id);

WITH film_stats AS (
  SELECT
    AVG(rental_rate) AS avg_rental_rate,
    MAX(rental_rate) AS max_length,
    MIN(rental_rate) AS min_length
  FROM film
),
customer_stats AS (
  SELECT
    COUNT(DISTINCT customer_id) AS total_customers,
    SUM(amount) AS total_payments
  FROM payment
)
SELECT
  ROUND((SELECT avg_rental_rate FROM film_stats), 2) AS avg_film_rental_rate,
  (SELECT max_length FROM film_stats) AS max_film_length,
  (SELECT min_length FROM film_stats) AS min_film_length,
  (SELECT total_customers FROM customer_stats) AS total_customers,
  (SELECT total_payments FROM customer_stats) AS total_payments;

WITH RECURSIVE subordinates AS (
  SELECT 
    employee_id, 
    manager_id, 
    full_name 
  FROM 
    employees 
  WHERE 
    employee_id = 2 
  UNION 
  SELECT 
    e.employee_id, 
    e.manager_id, 
    e.full_name 
  FROM 
    employees e 
    INNER JOIN subordinates s ON s.employee_id = e.manager_id
) 
SELECT * FROM subordinates;

INSERT INTO links (url, name)
VALUES
  ('url1', 'name1'),
  ('url2', 'name2'),
  ('url3', 'name3')
RETURNING *;

UPDATE courses
SET published_date = 'date'
WHERE course_id = 2
RETURNING *;

UPDATE courses
SET price = price * 1.05;

UPDATE 
  product
SET 
  net_price = price - price * discount
FROM
  product_segment s
WHERE
  p.segment_id = s.id;

DELETE FROM 
  todos
WHERE   
  id = 1
RETURNING *;

DELETE from todos;

DELETE FROM member m
USING denylist d
WHERE m.phone = d.phone;

DELETE FROM member
WHERE phone IN (
  SELECT
    phone
  FROM
    denylist
);

-- UPSERT
INSERT INTO inventory (id, name, price, quantity)
VALUES (1, 'D', 29.99, 20)
ON CONFLICT(id)
DO UPDATE SET
  price = EXCLUDED.price,
  quantity = EXCLUDED.quantity;

-- [BEGIN | COMMIT | ROLLBACK] [ | WORK | TRANSACTION]
BEGIN;
INSERT INTO accounts(name,balance)
VALUES('A', 10000)
COMMIT;

BEGIN;
UPDATE accounts
SET balance = balance - 1000
WHERE id = 1;
ROLLBACK;

-- copying from CSV
\copy persons(first_name,last_name,dob,email)
FROM 'path'
DELIMITER ','
CSV HEADER;

-- copying to csv file
\copy persons
TO 'path'
DELIMITER ','
CSV HEADER;

\copy (SELECT * FROM persons) to 'path' with csv;

SELECT *
INTO TEMP TABLE film_r
FROM film
WHERE rating = 'R'
ORDER BY title;

CREATE TEMP TABLE IF NOT EXISTS film_rating (rating, film_count)
AS
SELECT
  rating,
  COUNT(film_id)
FROM
  film
GROUP BY
  rating;

CREATE TABLE fruits(
  id SERIAL PRIMARY KEY,
  name VARCHAR NOT NULL
);
INSERT INTO fruits(id,name)
VALUES(DEFAULT,'Orange')
RETURNING id;

-- not transaction-safe
SELECT currval(pg_get_serial_sequence('fruits', 'id'));

CREATE TABLE baskets(
  name VARCHAR(255) NOT NULL
);
ALTER TABLE baskets
ADD COLUMN id SERIAL PRIMARY KEY;
```
