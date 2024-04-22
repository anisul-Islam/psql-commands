# psql-commands

CREATE DATABASE ecommerce_sda;
DROP DATABASE ecommerce_sda;

CREATE TABLE Users(
  UserId SERIAL PRIMARY KEY,
  Username VARCHAR(50) UNIQUE NOT NULL,
  Password VARCHAR(32) NOT NULL,
  Email VARCHAR(100) UNIQUE NOT NULL,
  FullName VARCHAR(100),
  Address VARCHAR(255),
  CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO Users (Username, Password, Email, FullName, Address)
VALUES
('alex-1', '12345678', 'alex1@gmail.com', 'alex warren', '192 sheignhame avenue');

INSERT INTO Users (Username, Password, Email, FullName, Address)
VALUES
('alex-2', '12345678', 'alex2@gmail.com', 'alex2 warren', 'Dhaka'),
('alex-3', '12345678', 'alex3@gmail.com', 'alex3 warren', 'Sylhet'),
('alex-4', '12345678', 'alex4@gmail.com', 'alex4 warren', 'Dhaka'),
('alex-5', '12345678', 'alex5@gmail.com', 'alex5 warren', 'Dhaka');


SELECT UserId
FROM Users;

SELECT UserId, Username
FROM Users;

SELECT *
FROM Users;

SELECT *
FROM Users
WHERE UserId=1;

SELECT *
FROM Users
WHERE Address='Dhaka';

SELECT *
FROM Users
WHERE Address='Dhaka'
LIMIT 2;

SELECT *
FROM Users
ORDER BY Address;

SELECT *
FROM Users
ORDER BY Address DESC;

DELETE FROM tableName
WHERE condition;

DELETE FROM Users
WHERE UserId=1;

INSERT INTO Users (Username, Password, Email, FullName, Address)
VALUES
('phi', '12345678', 'phi@gmail.com', 'phi tangtong', 'China');

UPDATE Users
SET Username='Anisul'
WHERE UserId=2;


-- Create Categories
CREATE TABLE Categories(
  CategoryId INT PRIMARY KEY,
  Name VARCHAR(100) UNIQUE NOT NULL,
  Description TEXT
);

-- Insert Categories (Created Categories)

INSERT INTO Categories (CategoryId,Name, Description)
VALUES 
(1, 'Smart Phone', 'list of smart phones'),
(2, 'Laptop', 'list of laptops'),
(3, 'Cloths', 'list of cloths');

-- Read Categores (Select)
SELECT * FROM Categories;

SELECT * FROM Categories WHERE Name='Laptop';

SELECT Description FROM Categories WHERE Name='Laptop';

SELECT * FROM Categories LIMIT 5;

-- Operators 
-- Arithmetic Operator  +  - * / %

SELECT UserId + 3
FROM Users;

-- - Relational == != >= <= > < BETWEEN
SELECT *
FROM Users
WHERE UserId > 3;

SELECT *
FROM Users
WHERE UserId BETWEEN 3 AND 5;

-- - LOGICAL OR AND NOT IN LIKE
SELECT *
FROM Users
WHERE 
Address='Sylhet'
OR Address = 'Dhaka'
OR Address = 'China';

SELECT *
FROM Users
WHERE Address IN ('Sylhet','Dhaka','China');

SELECT *
FROM Users
WHERE Address NOT IN ('Sylhet','Dhaka','China');

SELECT *
FROM Users
WHERE UserName LIKE 'a%';

SELECT *
FROM Users
WHERE UserName LIKE '%a';

SELECT *
FROM Users
WHERE UserName LIKE '%le%';

-- Custom name with AS Keyword 
SELECT Email AS UserEmail
FROM Users;

-- - Functions : CONCAT(), LENGTH(), EXTRACT(), UPPER(), LOWER(), 
SELECT CONCAT(Username, ' ', Password) AS UserLoginInfo FROM Users;

SELECT Username, LENGTH(Username) AS UserInfo FROM Users;

SELECT Username, EXTRACT(YEAR FROM CreatedAt) AS CreatedYear FROM Users;

SELECT LOWER(Username) AS LowerCaseUserName FROM Users;

SELECT Upper(Username) AS UpperCaseUserName FROM Users;

-- Sub queries : Query inside another query 
SELECT *
FROM Users
WHERE UserId = (SELECT MAX(UserId) FROM Users);

-- LEAST(), GREATEST()

-- - MAX(), MIN(), SUM(), AVG(),  COUNT(), 
(SELECT MAX(UserId) FROM Users);

SELECT SUM(UserId)
FROM Users
WHERE Address = 'Dhaka';

SELECT AVG(UserId)
FROM Users
WHERE Address = 'Dhaka';


SELECT COUNT(*) AS TotalUserFromDhaka
FROM Users
WHERE Address = 'Dhaka';


Group By

SELECT Department, Sum(Salary)
FROM Teachers
GROUP BY Department;

Teachers
Id  Name Department Salary
1   X     Eng       3000
2   X     Ben       4000
3   X     Ben       3000
4   X     Eng       8000
5   X     Ben       4000
6   X     Mat       3000
7   X     Eng       8000

SELECT Address, Sum(UserId)
FROM Users
GROUP BY Address;


-- create products 

CREATE TABLE Products(
  ProductId SERIAL PRIMARY KEY,
  Name VARCHAR(50) NOT NULL,
  Description TEXT,
  Price DECIMAL(10,2) NOT NULL,
  CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  CategoryId INT,
  FOREIGN KEY(CategoryId) REFERENCES Categories(CategoryId)
);


-- DROP TABLE Products;
-- TRUNCATE TABLE Products;

-- insert products 
INSERT INTO Products (Name, Description, Price, CategoryId)
VALUES
('Apple IPhone 13', 'High quality for the camera', 1200.50, 1),
('Samsung Galaxy', 'Lowest quality for the camera', 500.50, 1),
('Dell', 'High quality Laptop', 1250.50, 2),
('HP', 'Low Qality Laptop', 950.50, 2),
('Jeans', 'Slim fit jeans for men', 120.50, 3),
('T-Shirt', 'Slim fit t-shirts for men', 80.50, 3);

-- Inner Join, Left Join, Right Join, Full Join, Cross Join, SELF JOIN

-- ProductId, ProductName, ProductDescription, ProdudctPrice, CategoryName
-- Inner Join 
SELECT p.ProductId, p.Name, p.Description, p.Price, c.Name AS CategoryName
FROM Products p
INNER JOIN Categories c ON p.CategoryId = c.CategoryId;

-- -- Left Join / Left Outer Join
-- -- - from the left all the records will be returned, from the right only the matched values
-- SELECT p.ProductId, p.Name, p.Description, p.Price, c.Name AS CategoryName
-- FROM Products p
-- LEFT JOIN Categories c ON p.CategoryId = c.CategoryId;

-- -- right Join / right Outer Join
-- -- - from the right all the records will be returned, from the left only the matched values
-- SELECT p.ProductId, p.Name, p.Description, p.Price, c.Name AS CategoryName
-- FROM Products p
-- RIGHT JOIN Categories c ON p.CategoryId = c.CategoryId;

-- -- FULL Join
-- -- - returned all the recordes when there is in either left or right table
-- SELECT p.ProductId, p.Name, p.Description, p.Price, c.Name AS CategoryName
-- FROM Products p
-- RIGHT JOIN Categories c ON p.CategoryId = c.CategoryId;


-- Order Table 
CREATE TABLE Orders(
  OrderId SERIAL PRIMARY KEY,
  OrderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  Status VARCHAR(50) DEFAULT 'Pending',
  UserId INT,
  ProductId INT,
  FOREIGN KEY(UserId) REFERENCES Users(UserId),
  FOREIGN KEY(ProductId) REFERENCES Products(ProductId)
);

INSERT INTO Orders (UserId, ProductId, Status)
VALUES
(2, 1, 'Pending');

INSERT INTO Orders (UserId, ProductId, Status)
VALUES
(3, 2, 'Pending');

INSERT INTO Orders (UserId, ProductId, Status)
VALUES
(2, 2, 'Pending');

SELECT u.UserId, u.Username, u.Address, o.Status 
FROM Users u
INNER JOIN Orders o ON u.UserId = o.UserId;


SELECT o.OrderId, o.Status, u.Username, u.Address, p.Name, p.Price, p.Description
FROM Orders o
INNER JOIN Products p ON o.ProductId = p.ProductId
INNER JOIN Users u ON o.UserId = u.UserId;

-- Join types 
-- pgAdmin
