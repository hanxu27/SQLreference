# SQLreference

### CREATE TABLE
#### Data types
* VARCHAR(30)
* BLOB 65,535 Characters
* MEDIUMBLOB
* LONGBLOB
* ENUM distinct elements
* SET distinct elements
##### Numbers
* TINYINT -127-127
* SMALLINT -32768 - 32767
* MEDIUMINT -8388608 - 8388607
* INT -2147483648 - 2147483647
* BIGINT
* BOOL 0 or 1
* DECIMAL(6,2) -9999.99 - 9999.99 (sigfigs, decimal places)
##### DATETIME
* DATE '1000-01-01' - '9999-12-12'
* DATETIME
* TIMESTAMP
* TIME
* YEAR
##### KEYS
* id INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY
* type_id INT UNSIGNED NOT NULL, FOREIGN KEY(type_id) REFERENCES product_type(id)
##### CONSTRAINTS
* NOT NULL
* DEFAULT
* UNSIGNED (no negatrons)
* AUTO_INCREMENT (1 column per table)
* PRIMARY KEY
* DEFAULT: sales_tax_rate DECIMAL(5,2) NOT NULL DEFAULT 0
#### ALTER TABLE
* ALTER TABLE sales_item ADD day_of_week VARCHAR(8);
* ALTER TABLE sales_item MODIFY day_of_week VARCHAR(9) NOT NULL;
* ALTER TABLE sales_item DROP COLUMN weekday;
* RENAME TABLE transaction_type TO transaction;
* CREATE INDEX transaction_id ON transaction(name, payment_type);
* TRUNCATE TABLE transaction (delete data)
* DROP TABLE transaction

#### INSERT
* INSERT INTO product_type (name, id) VALUES ('Casual', NULL)
* INSERT INTO product_type VALUES ('Business', NULL), ('Casual', NULL), ('Athletic', NULL)

#### AND OR NOT
#### SELECT
* SELECT * FROM sales_item WHERE discount > .15 ORDER BY discount DESC LIMIT 5
* SELECT CONCAT(first_name, ' ', last_name) AS Name, phone, state FROM customer WHERE state = 'TX'
* SELECT DISTINCT state FROM customer WHERE state IN ('CA', 'NJ') ORDER BY state
* SELECT sales_order.id, sales_item.quantity, item.price, (sales_item.quantity * item.price) AS Total FROM sales_order JOIN sales_item sales_item.sales_order_id = sales_order.id JOIN item ON item.id = sales_item.item_id ORDER BY Total DESC
* SELECT name, supplier, price FROM product LEFT JOIN item ON item.product_id = product_id ORDER BY name
* SELECT first_name, last_name, street, city, zip, birth_date
FROM customer
WHERE MONTH(birth_date) = 12
UNION
SELECT first_name, last_name, street, city, zip, birth_date
FROM sales_person
WHERE MONTH(birth_date) = 12
ORDER BY birth_date
#### Wild Card % (0 or more) _ (1)
* SELECT first_name, last_name FROM customer WHERE first_name LIKE "A%"
* SELECT first_name, last_name FROM customer WHERE first_name LIKE "D%" OR last_name LIKE '%n'
* SELECT first_name, last_name FROM customer WHERE last_name REGEXP 'ez|son'
* SELECT MONTH(birth_date) AS Month, COUNT(*) AS Amount 
FROM customer GROUP BY Month ORDER BY Month
* SELECT MONTH(birth_date) AS Month, COUNT(*) AS Amount 
FROM customer
GROUP BY Month HAVING Amount > 1 
ORDER BY Month
* SELECT cust_id, company, COUNT(*) AS Purchases
FROM sales_order
JOIN customer
ON customer.id = cust_id
GROUP BY cust_id;
#### Aggregate Functions
* SELECT COUNT(*) AS Items, SUM(price) AS Value, ROUND(AVG(price), 2) AS Avg, MIN(price) AS Min, MAX(price) AS max FROM item
#### VIEW
* CREATE VIEW purchase_order_overview AS
SELECT sales_order.purchase_order_number, customer.company, 
sales_item.quantity, product.supplier, product.name, item.price, 
(sales_item.quantity * item.price) AS Total,
CONCAT(sales_person.first_name, ' ', sales_person.last_name) AS Salesperson
FROM sales_order
JOIN sales_item
ON sales_item.sales_order_id = sales_order.id
JOIN item
ON item.id = sales_item.item_id
JOIN customer
ON sales_order.cust_id = customer.id
JOIN product
ON product.id = item.product_id
JOIN sales_person
ON sales_person.id = sales_order.sales_person_id
ORDER BY purchase_order_number;

