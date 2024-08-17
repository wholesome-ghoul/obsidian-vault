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

docker run --rm -v $HOME:/data --name postgres13 -e POSTGRES_PASSWORD=pass postgres:13.14
docker exet -it postgres13 bash -c "echo 'set -o vi'>~/.bashrc && echo 'set editing-mode vi'>~/.inputrc && bash"
```


```sql
ALTER TABLE <table-name> RENAME COLUMN <old-name> TO <new-name>;

-- FROM -> WHERE -> SELECT -> ORDER BY
-- FROM -> WHERE -> GROUP BY -> HAVING -> SELECT -> DISTINCT -> ORDER BY -> LIMIT
-- window function always performs calculations after JOIN, WHERE, GROUP BY and HAVING
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
--
-- When you create a temporary table that shares the same name as a permanent table,
-- you cannot access the permanent table until the temporary table is removed.
--
-- actions:
-- SET NULL, SET DEFAULT, RESTRICT, NO ACTION, CASCADE
--
-- If precision is not required, you should not use the NUMERIC type because calculations
-- on NUMERIC values are typically slower than integers, floats, and double precisions.

-- \l will show all dbs in psql server
-- \i <path-to-sql>

-- switch to db
-- \c databsaename

-- display all tables
-- \dt
-- list all tables of schema
-- \dt <schema>.*

-- all available psql cmds
-- \?

-- set table output option.
-- null will be displayed:
-- \pset null null

-- check connection info; user
-- \conninfo


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
  facid,
  date_part('month', starttime) AS month,
  SUM(slots) AS slots
FROM
  cd.bookings
WHERE
  date_part('month', starttime) = 2012
GROUP BY
  ROLLUP(facid, month)
ORDER BY
  facid,
  month;

WITH bookings AS (
  SELECT
    facid,
    date_part('month', starttime) AS month,
    slots
  FROM
    cd.bookings
  WHERE
    date_part('year', starttime) = 2012
)
SELECT facid, month, SUM(slots) FROM bookings GROUP BY facid, month
UNION ALL
SELECT facid, null, SUM(slots) FROM bookings GROUP BY facid
UNION ALL
SELECT null, null, SUM(slots) FROM bookings
ORDER BY facid, month;

-- window functions
SELECT
  COUNT(*) OVER(),
  firstname,
  surname
FROM
  cd.members
ORDER BY
  joindate;

SELECT
  ROW_NUMBER() OVER(
    ORDER BY joindate ASC
  ),
  firstname,
  surname
FROM
  cd.members;

SELECT
  facid,
  total FROM (
    SELECT
      facid,
      SUM(slots) total,
      RANK() OVER(
        ORDER BY SUM(slots) DESC
      )
    FROM
      cd.bookings
    GROUP BY
      facid
  ) as ranked
WHERE
  rank = 1;

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
  employees
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

SELECT
  facid,
  name,
  ROUND((SUM(slots) / 2.0), 2) AS "Total Hours"
FROM
  cd.bookings
INNER JOIN
  cd.facilities
USING(facid)
GROUP BY
  facid,
  name
ORDER BY
  facid;

SELECT
  name,
  rank
FROM
  (
    SELECT
      name,
      RANK() OVER(
        ORDER BY SUM(CASE WHEN memid = 0 THEN slots * guestcost ELSE slots * membercost END) DESC
      )
    FROM
      cd.bookings
    INNER JOIN
      cd.facilities
    USING(facid)
    GROUP BY
      name
  ) as subq
WHERE
  rank <= 3
ORDER BY
  rank,
  name;

SELECT
  name,
  CASE
    WHEN class = 1 THEN 'high'
    WHEN class = 2 THEN 'average'
    ELSE 'low'
  end revenue
FROM
  (
    SELECT
      name,
      NTILE(3) OVER(
        ORDER BY SUM(CASE WHEN memid = 0 THEN slots * guestcost ELSE slots * membercost END) DESC
      ) as class
    FROM
      cd.bookings
    INNER JOIN
      cd.facilities
    USING(facid)
    GROUP BY
      name
  ) as subq
ORDER BY
  class,
  name;

SELECT
  firstname,
  surname,
  hours,
  RANK() OVER(
    ORDER BY hours DESC
  )
FROM
  (
    SELECT
      firstname,
      surname,
      ((SUM(slots) + 10)/20)*10 AS hours
    FROM
      cd.members
    INNER JOIN
      cd.bookings
    USING(memid)
    GROUP BY
      memid
  ) as subq
ORDER BY
  rank,
  surname,
  firstname;

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

INSERT INTO table
  SELECT
    (SELECT max(id) FROM table) + 1,
    col2, col3;

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

UPDATE cd.facilities facs
  SET
    membercost = facs2.membercost * 1.1,
    guestcost = facs2.guestcost * 1.1
  FROM (SELECT * FROM cd.facilities WHERE facid = 0) facs2
  WHERE facs.facid = 1;

DELETE FROM 
  todos
WHERE   
  id = 1
RETURNING *;

-- Uncorrelated subquery
DELETE FROM cd.members WHERE memid NOT IN (SELECT memid FROM cd.bookings);
-- Correlated subquery
DELETE FROM cd.members WHERE NOT EXISTS (SELECT 1 FROM cd.bookings WHERE memid = mems.memid);

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

-- CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
--     [ AS { SMALLINT | INT | BIGINT } ]
--     [ INCREMENT [ BY ] increment ]
--     [ MINVALUE minvalue | NO MINVALUE ] 
--     [ MAXVALUE maxvalue | NO MAXVALUE ]
--     [ START [ WITH ] start ] 
--     [ CACHE cache ] 
--     [ [ NO ] CYCLE ]
--     [ OWNED BY { table_name.column_name | NONE } ]

-- identity column
CREATE TABLE color(
  color_id INT GENERATED ALWAYS AS IDENTITY,
  color_name VARCHAR NOT NULL
);
INSERT INTO COLOR(color_name)
VALUES('Red');
-- error
INSERT INTO COLOR(color_id,color_name)
VALUES(2,'Blue');
--overriding
INSERT INTO COLOR(color_id,color_name)
OVERRIDING SYSTEM VALUE
VALUES(2,'Blue');

CREATE TABLE color(
  color_id INT GENERATED ALWAYS AS IDENTITY
  (START WITH 10 INCREMENT BY 10),
  color_name VARCHAR NOT NULL
);

CREATE TABLE shape(
  -- important to be NOT NULL to transform the column into identity column
  shape_id INT NOT NULL,
  shape_name VARCHAR NOT NULL
);
ALTER TABLE shape
ALTER COLUMN shape_id ADD GENERATED ALWAYS AS IDENTITY;
ALTER TABLE shape
ALTER COLUMN shape_id SET GENERATED BY DEFAULT;
ALTER TABLE shape
ALTER COLUMN shape_id
DROP IDENTITY IF EXISTS;

-- ALTER
DROP TABLE IF EXISTS links;

CREATE TABLE links (
   link_id serial PRIMARY KEY,
   title VARCHAR (512) NOT NULL,
   url VARCHAR (1024) NOT NULL
);

ALTER TABLE links
ADD COLUMN active boolean;

ALTER TABLE links
DROP COLUMN active;

ALTER TABLE links
RENAME COLUMN title to link_title;

ALTER TABLE links
ADD COLUMN target VARCHAR(10);

ALTER TABLE links
ALTER COLUMN target
SET DEFAULT '_blank';

ALTER TABLE links
ADD CHECK (target IN ('_self', '_blank'));

ALTER TABLE links
ADD CONSTRAINT unique_url UNIQUE (url);

ALTER TABLE links
DROP CONSTRAINT unique_url;

ALTER TABLE links
RENAME TO urls;

ALTER TABLE books 
DROP COLUMN publisher_id CASCADE;

-- change column type
ALTER TABLE assets 
ALTER COLUMN name TYPE VARCHAR(255);

-- asset_no varchar
ALTER TABLE assets
ALTER COLUMN asset_no TYPE INT 
USING asset_no::integer;

-- TRUNCATE
-- does not fire any ON DELETE triggers
-- transaction-safe
TRUNCATE TABLE products
RESTART IDENTITY;

-- COPY table
CREATE TABLE contact_backup AS
TABLE contacts
WITH NO DATA;

-- foreign key
--
-- [CONSTRAINT fk_name]
--    FOREIGN KEY(fk_columns) 
--    REFERENCES parent_table(parent_key_columns)
--    [ON DELETE delete_action]
--    [ON UPDATE update_action]
--
CREATE TABLE contacts(
   contact_id INT GENERATED ALWAYS AS IDENTITY,
   customer_id INT,
   contact_name VARCHAR(255) NOT NULL,
   phone VARCHAR(15),
   email VARCHAR(100),
   PRIMARY KEY(contact_id),
   CONSTRAINT fk_customer
      FOREIGN KEY(customer_id) 
        REFERENCES customers(customer_id)
        ON DELETE SET NULL
);

ALTER TABLE child_table
ADD CONSTRAINT constraint_fk
FOREIGN KEY (fk_columns)
REFERENCES parent_table(parent_key_columns)
ON DELETE CASCADE;

ALTER TABLE employees
ADD CONSTRAINT joined_date_check
CHECK ( joined_date >  birth_date );

CREATE TABLE users (
  id serial PRIMARY KEY, 
  username VARCHAR (50), 
  password VARCHAR (50), 
  email VARCHAR (50), 
  CONSTRAINT username_email_notnull CHECK (
    NOT (
      (
        username IS NULL 
        OR username = ''
      ) 
      AND (
        email IS NULL 
        OR email = ''
      )
    )
  )
);

-- DATE data type
-- YYYY-MM-DD
CREATE TABLE documents (
  document_id serial PRIMARY KEY, 
  header_text VARCHAR (255) NOT NULL, 
  posting_date DATE NOT NULL DEFAULT CURRENT_DATE
);

SELECT NOW()::date;
SELECT CURRENT_DATE;
SELECT TO_CHAR(CURRENT_DATE, 'dd/mm/yyyy');
SELECT TO_CHAR(CURRENT_DATE, 'Mon dd, yyyy');
SELECT first_name, NOW() - hire_date as diff FROM employees;

SELECT
	employee_id,
	first_name,
	last_name,
	AGE(birth_date)
FROM
	employees;

SELECT EXTRACT (YEAR FROM CURRENT_DATE) AS YEAR;

-- TIMESTAMP & TIMESTAMPZ
-- both 8 bytes
SELECT 
  typname, 
  typlen 
FROM 
  pg_type 
WHERE 
  typname ~ '^timestamp';

SHOW TIMEZONE;

SELECT CURRENT_TIMESTAMP;
SELECT CURRENT_TIME;
SELECT TIMEOFDAY();

-- convert time to another timezone's time
SELECT timezone('America/Los_Angeles','2016-06-01 00:00'::timestamptz);

-- INTERVAL
-- @ interval [ fields ] [ (p) ]
-- quantity unit [quantity unit...] [direction]
SELECT
	now(),
	now() - INTERVAL '1 year 3 hours 20 minutes' 
             AS "3 hours 20 minutes ago of last year";

SET intervalstyle = 'sql_standard';
SELECT 
  INTERVAL '6 years 5 months 4 days 3 hours 2 minutes 1 second';
--  +6-5 +4 +3:02:01

SET intervalstyle = 'postgres';
SELECT 
  INTERVAL '6 years 5 months 4 days 3 hours 2 minutes 1 second';
--  6 years 5 mons 4 days 03:02:01

SET intervalstyle = 'postgres_verbose';
SELECT 
  INTERVAL '6 years 5 months 4 days 3 hours 2 minutes 1 second';
--  @ 6 years 5 mons 4 days 3 hours 2 mins 1 sec

SET intervalstyle = 'iso_8601';
SELECT 
  INTERVAL '6 years 5 months 4 days 3 hours 2 minutes 1 second';
-- P6Y5M4DT3H2M1S

SELECT
    TO_CHAR(
        INTERVAL '17h 20m 05s',
        'HH24:MI:SS'
    );

SELECT
    EXTRACT (
        MINUTE
        FROM
            INTERVAL '5 hours 21 minutes'
    );

SELECT
    justify_days(INTERVAL '30 days'),
    justify_hours(INTERVAL '24 hours'),
    justify_interval(interval '1 year -1 hour');

-- TIME
-- HH:MI or HH:MI:SS.ppppp or HHMISS.ppppp
-- 00:00:00-24:00:00
SELECT LOCALTIME(N);
SELECT LOCALTIME AT TIME ZONE 'UTC-7';

-- UUID
SELECT gen_random_uuid();

-- ARRAY
CREATE TABLE contacts (
  id SERIAL PRIMARY KEY, 
  name VARCHAR (100), 
  phones TEXT []
);
INSERT INTO contacts (name, phones)
VALUES('John Doe',ARRAY [ '(408)-589-5846','(408)-589-5555' ]);
SELECT
  name,
  phones[1]
FROM
  contacts;

SELECT 
  name, 
  phones 
FROM 
  contacts 
WHERE 
  '(408)-589-5555' = ANY (phones);

SELECT
  name,
  unnest(phones)
FROM
  contacts;

-- hstore
CREATE EXTENSION hstore;
CREATE TABLE books (
	id serial primary key,
	title VARCHAR (255),
	attr hstore
);
SELECT
	attr -> 'key-name' as name
FROM
	books;

-- add or update key/value
UPDATE books
SET attr = attr || '"freeshipping"=>"yes"' :: hstore;
UPDATE books
SET attr = delete(attr, 'freeshipping');

SELECT
	title
FROM
	books
WHERE
	attr @> '"weight"=>"11.2 ounces"' :: hstore;

SELECT
	title
FROM
	books
WHERE
	attr ?& ARRAY [ 'language', 'weight' ];

SELECT
	-- avals (attr)
	-- svals (attr)
	-- skeys (attr)
	akeys (attr)
FROM
	books;

-- from hstore to json
SELECT
  title,
  hstore_to_json (attr) json
FROM
  books;

-- hstore data to sets
SELECT
	title,
	(EACH(attr) ).*
FROM
	books;

-- JSON
CREATE TABLE products(
    id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    properties JSONB
);
SELECT 
  id, 
  name, 
  properties ->> 'color' color 
FROM 
  products 
WHERE 
  properties ->> 'color' IN ('black', 'white');

SELECT
  title,
  length,
  CASE 
    WHEN length > 0 AND length <= 50 THEN 'Short'
    WHEN length > 50 AND length <= 120 THEN 'Medium'
    WHEN length > 120 THEN 'Long'
  END duration
FROM
  film
ORDER BY
  title;

SELECT 
  SUM (
    CASE WHEN rental_rate = 0.99 THEN 1 ELSE 0 END
  ) AS "Economy", 
  SUM (
    CASE WHEN rental_rate = 2.99 THEN 1 ELSE 0 END
  ) AS "Mass", 
  SUM (
    CASE WHEN rental_rate = 4.99 THEN 1 ELSE 0 END
  ) AS "Premium" 
FROM 
  film;

-- COALESCE
CREATE TABLE items (
  id SERIAL PRIMARY KEY, 
  product VARCHAR (100) NOT NULL, 
  price NUMERIC NOT NULL, 
  discount NUMERIC
);
SELECT 
  product, 
  (
    price - COALESCE(discount, 0)
  ) AS net_price 
FROM 
  items;

SELECT NULLIF(1,1); -- NULL

SELECT CAST('100' AS INTEGER);
SELECT '{1,2,3}'::INTEGER[] AS result_array;
SELECT 
  '15 minute' :: interval, 
  '2 hour' :: interval, 
  '1 day' :: interval, 
  '2 week' :: interval, 
  '3 month' :: interval;

-- grant privileges to user
GRANT ALL privileges ON database dbname TO username;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO myuser;
ALTER USER my_user WITH PASSWORD '';
```
