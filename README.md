# Project Overview:
# Title: Store Product Management System (No Expired and Spoiled Products)

Description:
In a store, managing a vast array of products is essential to ensure that customers have access to up-to-date information about each item. Each product is assigned a unique ID, and products are categorized to streamline inventory management. This system is designed to manage product details, including IDs, names, prices, quantities, and categories. Additionally, it tracks sales and expiry dates to prevent the sale of expired or spoiled products.

Key Features:
Product Management: Each product has a unique ID, name, price, and quantity. Products are categorized to facilitate easy management.
Sales Tracking: Records sales by capturing product ID, name, quantity sold, and the date and time of the transaction.
Expiry Management: Monitors and records the expiry dates of products to prevent the sale of expired items.
Database Schema:
ER Diagram:
An Entity-Relationship (ER) diagram outlines the relationships between different entities such as products, stock, sales, and expired products.

Normalization:
The database schema is normalized to the third normal form (3NF) to eliminate redundancy and ensure data integrity. The normalization process includes:

First Normal Form (1NF): Ensures that each table column contains atomic values and each record is unique.
Second Normal Form (2NF): Ensures that all non-key attributes are fully functional dependent on the primary key.
Third Normal Form (3NF): Ensures that all attributes are functionally dependent only on the primary key.
Final Tables:
Product Table: Contains product details including ID, name, price, quantity, and category.
Stock Table: Contains stock details including product ID, name, and quantity.
Expired_Product Table: Contains details of expired products including product ID and expiry date.
Sales Table: Contains sales details including product ID, date of sale, and quantity sold.
SQL Scripts:
Table Creation:

sql
Copy code
CREATE TABLE Product (
    P_id NUMBER,
    P_Name VARCHAR2(20),
    P_Price NUMBER(10, 2),
    P_Quantity NUMBER(10, 2),
    P_Category VARCHAR2(20),
    C_id NUMBER,
    C_Name VARCHAR2(20),
    CONSTRAINT pk_Product PRIMARY KEY (P_id)
);

INSERT INTO Product VALUES (1, 'Rice', 70.00, 50.00, '1R', 1, 'R');
INSERT INTO Product VALUES (2, 'Bread', 35.00, 40.00, '2B', 2, 'B');
INSERT INTO Product VALUES (3, 'Dal', 100.00, 45.00, '3D', 3, 'D');
INSERT INTO Product VALUES (4, 'Pasta', 120.00, 80.00, '4P', 4, 'P');
INSERT INTO Product VALUES (5, 'Grain', 50.00, 55.00, '5G', 5, 'G');
SELECT * FROM Product;

CREATE TABLE Stock (
    P_id NUMBER,
    P_Name VARCHAR2(20),
    P_Quantity NUMBER(10, 2),
    CONSTRAINT pk_Stock PRIMARY KEY (P_id),
    CONSTRAINT fk_emp_P_id FOREIGN KEY (P_id) REFERENCES Product (P_id)
);

INSERT INTO Stock VALUES (1, 'Rice', 50.00);
INSERT INTO Stock VALUES (2, 'Bread', 40.00);
INSERT INTO Stock VALUES (3, 'Dal', 45.00);
INSERT INTO Stock VALUES (4, 'Pasta', 80.00);
INSERT INTO Stock VALUES (5, 'Grain', 55.00);
SELECT * FROM Stock;

CREATE TABLE Expired_Product (
    P_id NUMBER,
    E_date DATE,
    CONSTRAINT pk_Expired_Product PRIMARY KEY (P_id),
    CONSTRAINT fk_Expired_Product_id FOREIGN KEY (P_id) REFERENCES Stock (P_id)
);

INSERT INTO Expired_Product VALUES (1, TO_DATE('2022-02-17', 'YYYY-MM-DD'));
INSERT INTO Expired_Product VALUES (2, TO_DATE('2022-03-19', 'YYYY-MM-DD'));
INSERT INTO Expired_Product VALUES (3, TO_DATE('2022-04-20', 'YYYY-MM-DD'));
INSERT INTO Expired_Product VALUES (4, TO_DATE('2022-05-21', 'YYYY-MM-DD'));
INSERT INTO Expired_Product VALUES (5, TO_DATE('2022-06-22', 'YYYY-MM-DD'));
SELECT * FROM Expired_Product;

CREATE TABLE Sales (
    P_id NUMBER,
    S_date DATE,
    P_Quantity NUMBER(10, 2),
    CONSTRAINT pk_Sales PRIMARY KEY (P_id)
);

INSERT INTO Sales VALUES (1, TO_DATE('2022-01-22', 'YYYY-MM-DD'), 10.00);
INSERT INTO Sales VALUES (2, TO_DATE('2022-02-03', 'YYYY-MM-DD'), 20.00);
INSERT INTO Sales VALUES (3, TO_DATE('2022-03-06', 'YYYY-MM-DD'), 30.00);
INSERT INTO Sales VALUES (4, TO_DATE('2022-01-21', 'YYYY-MM-DD'), 15.00);
INSERT INTO Sales VALUES (5, TO_DATE('2022-03-11', 'YYYY-MM-DD'), 25.00);
SELECT * FROM Sales;
Join Queries:

sql
Copy code
-- Eujoin between Product and Stock Table
SELECT Product.P_Name, Product.P_id, Stock.P_id
FROM Product, Stock
WHERE Product.P_id = Stock.P_id;

-- Eujoin between Stock and Sales table to show the UNSOLD product quantity
SELECT Stock.P_id, Stock.P_Name, (Stock.P_Quantity - Sales.P_Quantity) AS Unsold
FROM Stock, Sales
WHERE Stock.P_id = Sales.P_id;

-- Eujoin between Stock and Expired_Product to show the expired product date
SELECT Stock.P_id, Stock.P_Name, Stock.P_Quantity, Expired_Product.P_id, Expired_Product.E_date
FROM Stock, Expired_Product
WHERE Stock.P_id = Expired_Product.P_id;

-- Outer join to create a relation between two tables
SELECT *
FROM Product, Sales
WHERE Product.P_id = Sales.P_id (+);
Subquery:

sql
Copy code
-- Subquery to find the product price more than Bread
SELECT *
FROM Product
WHERE P_Price > ALL (
    SELECT MIN(P_Price)
    FROM Product 
    WHERE P_Name = 'Bread'
);
View Creation:

sql
Copy code
-- View to show the product and its expiry date
CREATE VIEW Stock11 AS 
SELECT Stock.P_id, Stock.P_Name, Stock.P_Quantity, Expired_Product.E_date
FROM Stock, Expired_Product
WHERE Stock.P_id = Expired_Product.P_id;

SELECT * FROM Stock11;
Constraints and Sequence:

sql
Copy code
-- Add another column to the product table using constraint
ALTER TABLE Product
ADD E_date DATE;

-- Sequence for Product_P_Quantity
CREATE SEQUENCE Product_P_Quantity
    INCREMENT BY 1
    START WITH 1000
    MAXVALUE 10000
    NOCACHE
    NOCYCLE;

SELECT * FROM Product;
Report Queries:

Eujoin between Product and Stock Table.
Eujoin between Stock and Sales table to show the UNSOLD product quantity.
Outer join to create a relation between two tables.
Subquery to find the product price more than Bread.
View to show the product and its expiry date.
Add another column to the product table using constraint.
Conclusion:
The Store Product Management System ensures efficient management of products, tracking of sales, and monitoring of expiry dates to prevent the sale of expired or spoiled products. Through the use of SQL queries, normalization, and various database operations, the system aims to maintain an organized and efficient inventory for any store.
