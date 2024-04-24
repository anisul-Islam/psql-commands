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
Id Name
1
2
3

Subjects
Id  Name Department Salary
1   X     Eng       3000
1   X     Ben       4000
1   X     Ben       3000
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



-- create a table for students where StudentId, FirstName, LastName, Gender, DOB, Age
CREATE TABLE students (
  StudentId INT PRIMARY KEY,
  FirstName VARCHAR(50),
  LastName VARCHAR(50),
  Gender VARCHAR(50),
  DOB DATE,
  AGE INT
);

-- ALTER 

ALTER TABLE students
DROP AGE;

-- INSERT RECORDS FOR THE STUDENTS TABLE 

INSERT INTO students (StudentId, FirstName, LastName, Gender, DOB) VALUES 
(1, 'firstName1', 'lastName1', 'Female', '1995-05-15'),
(2, 'firstName2', 'lastName2', 'Male', '1995-05-15'),
(3, 'firstName3', 'lastName3', 'Female', '1995-05-15'),
(4, 'firstName4', 'lastName4', 'Male', '1995-05-15'),
(5, 'firstName5', 'lastName5', 'Female', '1995-05-15'),
(6, 'firstName7', 'lastName7', 'Female', '1995-05-15'),
(8, 'firstName8', 'lastName8', 'Female', '1995-05-15');

-- create a table for grades where StudentId, GradeId, Course, Grade 
CREATE TABLE grades (
  GradeId INT PRIMARY KEY,
  StudentId INT,
  Course VARCHAR(50),
  Grade VARCHAR(50)
);

INSERT INTO grades (GradeId, StudentId, Course, Grade) VALUES 
(1, 1, 'Math', 'A'),
(2, 1, 'Science', 'A'),
(3, 2, 'Math', 'A'),
(4, 3, 'Science', 'A'),
(4, 3, 'Science', 'A'),
(7, 7, 'Math', 'A');

INSERT INTO grades (GradeId, StudentId, Course, Grade) VALUES 
(8, 7, 'Math', 'A');

SELECT s.StudentId, s.FirstName, s.LastName, s.DOB, g.Course, g.Grade 
FROM students AS s, grades AS g
WHERE s.StudentId = g.StudentId;

SELECT s.StudentId, s.FirstName, s.LastName, s.DOB, g.Course, g.Grade 
FROM students AS s INNER JOIN grades AS g
ON s.StudentId = g.StudentId;

SELECT s.StudentId, s.FirstName, s.LastName, s.DOB, g.Course, g.Grade 
FROM students AS s LEFT JOIN grades AS g
ON s.StudentId = g.StudentId;

SELECT s.StudentId, s.FirstName, s.LastName, s.DOB, g.Course, g.Grade 
FROM students AS s RIGHT JOIN grades AS g
ON s.StudentId = g.StudentId;

SELECT s.StudentId, s.FirstName, s.LastName, s.DOB, g.Course, g.Grade 
FROM students AS s FULL JOIN grades AS g
ON s.StudentId = g.StudentId;

-- View - A Virtual table 
CREATE VIEW students_view AS
SELECT StudentId, FirstName, Gender
FROM students;

SELECT * FROM students_view;



// Array, UUID Data Type
// Indexing
// Transactions


// ecommerece-api 

- Tables: users, products, categories, orders, orderProducts

- users: userId, name, email, password, address, image, mobile, createdAt, isAdmin, isBanned

CREATE TABLE users(
  userId SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(100) NOT NULL,
  adress VARCHAR(255),
  image VARCHAR(255),
  isAdmin BOOLEAN DEFAULT FALSE,
  isBanned BOOLEAN DEFAULT FALSE,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

ALTER TABLE users
RENAME COLUMN adress TO address;


INSERT INTO Users (name, email, password, address, image, isAdmin, isBanned)
VALUES 
  ('John Doe', 'john@example.com', 'password123', '123 Main St', 'profile.jpg', TRUE, FALSE),
  ('Alice Smith', 'alice@example.com', 'password456', '456 Elm St', 'avatar.png', FALSE, FALSE),
  ('Bob Johnson', 'bob@example.com', 'password789', '789 Oak St', NULL, FALSE, TRUE),
  ('Emily Davis', 'emily@example.com', 'passwordabc', NULL, 'default.jpg', FALSE, FALSE),
  ('Michael Brown', 'michael@example.com', 'passwordxyz', '567 Pine St', 'user.png', TRUE, FALSE);


- categories: categoryId, name, slug, description, createdAt

CREATE TABLE categories(
  categoryId SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  slug VARCHAR(100) UNIQUE NOT NULL,
  description TEXT,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

  INSERT INTO categories (name, slug, description)
  VALUES 
  ('Electronics', 'electronics', 'Electronic devices and accessories'),
  ('Clothing', 'clothing', 'Apparel and fashion accessories'),
  ('Books', 'books', 'Fiction and non-fiction literature'),
  ('Home Decor', 'home-decor', 'Decorative items for the home'),
  ('Toys', 'toys', 'Children''s toys and games'); 

- products: productId, name, slug, description, price, quantity, sold, createdAt, image, categoryId, shipping

CREATE TABLE products(
  productId SERIAL PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  slug VARCHAR(100) UNIQUE NOT NULL,  
  image VARCHAR(100),  
  description TEXT, 
  price DECIMAL(10,2) NOT NULL,
  quantity INT DEFAULT 0,
  sold INT DEFAULT 0,
  shipping DECIMAL(10,2) DEFAULT 0,
  categoryId INT REFERENCES categories(categoryId),
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- ALTER TABLE products
-- ALTER shipping TYPE BOOLEAN;

  INSERT INTO products (name, slug, description, price, quantity, sold, image, shipping, categoryID, createdAt)
VALUES 
  ('Laptop', 'laptop', 'High-performance laptop with SSD storage', 999.99, 50, 10, 'laptop.jpg', 0, 1, NOW()),
  ('Smartphone', 'smartphone', 'Latest smartphone model with 5G connectivity', 699.99, 100, 30, 'smartphone.jpg', 0, 1, NOW()),
  ('T-shirt', 't-shirt', 'Cotton t-shirt with trendy design', 19.99, 200, 80, 't-shirt.jpg', 2.5, 2, NOW()),
  ('Jeans', 'jeans', 'Denim jeans for casual wear', 39.99, 150, 60, 'jeans.jpg', 0, 2, NOW()),
  ('Novel', 'novel', 'Bestselling fiction novel', 12.99, 300, 120, 'novel.jpg', 0, 3, NOW()),
  ('Cookbook', 'cookbook', 'Collection of popular recipes', 24.99, 100, 40, 'cookbook.jpg', 0, 3, NOW()),
  ('Throw Pillow', 'throw-pillow', 'Decorative throw pillow for couch', 29.99, 50, 20, 'throw-pillow.jpg', 0, 4, NOW()),
  ('Wall Art', 'wall-art', 'Canvas wall art for home decor', 49.99, 80, 30, 'wall-art.jpg', 0, 4, NOW()),
  ('Action Figure', 'action-figure', 'Collectible action figure', 14.99, 120, 50, 'action-figure.jpg', 0, 5, NOW()),
  ('Board Game', 'board-game', 'Popular board game for family fun', 29.99, 70, 30, 'board-game.jpg', 0, 5, NOW()),
  ('Headphones', 'headphones', 'Wireless headphones with noise cancellation', 149.99, 90, 40, 'headphones.jpg', 1.5, 1, NOW()),
  ('Smart Watch', 'smart-watch', 'Fitness tracker smart watch', 199.99, 60, 20, 'smart-watch.jpg', 1.5, 1, NOW()),
  ('Dress', 'dress', 'Elegant dress for special occasions', 79.99, 80, 25, 'dress.jpg', 1.5, 2, NOW()),
  ('Sneakers', 'sneakers', 'Stylish sneakers for everyday wear', 59.99, 120, 45, 'sneakers.jpg', 0, 2, NOW()),
  ('Cooking Utensils Set', 'cooking-utensils-set', 'Complete set of kitchen utensils', 49.99, 100, 35, 'cooking-utensils-set.jpg', 0, 4, NOW());


- orders: odrerId, orderDate, status, payment, userId, productIDs, createdAt

CREATE TABLE orders(
  orderId SERIAL PRIMARY KEY,
  orderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  status VARCHAR(100) DEFAULT 'Pending',
  payment JSONB,
  userId INT REFERENCES users(userId),
  productIDs INT[],
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO orders (orderDate, status, userID, payment, productIDs)
VALUES
  ('2024-04-20', 'Pending', 1, '{"method": "Credit Card", "amount": 50}', '{1, 2}'),
  ('2024-04-21', 'Processing', 2, '{"method": "PayPal", "amount": 70}', '{3, 4, 5}'),
  ('2024-04-22', 'Shipped', 3, '{"method": "Bank Transfer", "amount": 100}', '{1, 3, 5}'),
  ('2024-04-23', 'Delivered', 4, '{"method": "Cash on Delivery", "amount": 80}', '{2, 4}'),
  ('2024-04-24', 'Pending', 5, '{"method": "Credit Card", "amount": 120}', '{1, 2, 3}');


SELECT p.*
FROM products p
JOIN (
SELECT UNNEST(productIDs) AS productID
FROM orders
WHERE orderId = 1
) o ON p.productId = o.productID;



-- How to Use UUID
-- Indexing with example 
-- Relationship between tables
-- REST API
-- How to connect c# .NET with psql
-- Build a REST API
DROP TABLE IF EXISTS users;

CREATE TABLE users(
  userId UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  address VARCHAR(255),
  image VARCHAR(255),
  isAdmin BOOLEAN DEFAULT FALSE,
  isBanned BOOLEAN DEFAULT FALSE,
  createdAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- uuid-ossp

SELECT * FROM pg_extension WHERE extname = 'uuid-ossp';
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";


INSERT INTO users (name, email, password, address, image, isAdmin, isBanned)
VALUES 
  ('John Doe', 'john@example.com', 'password123', '123 Main St', 'profile.jpg', TRUE, FALSE),
  ('Alice Smith', 'alice@example.com', 'password456', '456 Elm St', 'avatar.png', FALSE, FALSE),
  ('Bob Johnson', 'bob@example.com', 'password789', '789 Oak St', NULL, FALSE, TRUE),
  ('Emily Davis', 'emily@example.com', 'passwordabc', NULL, 'default.jpg', FALSE, FALSE),
  ('Michael Brown', 'michael@example.com', 'passwordxyz', '567 Pine St', 'user.png', TRUE, FALSE);

-- Indexing 

-- check indexing exists in users table
SELECT *
FROM pg_indexes
WHERE tablename = 'users';

-- search an user based on address
EXPLAIN ANALYZE SELECT *
FROM users
WHERE address = '789 Oak St';

-- CREATE INDEX 
CREATE INDEX idx_users_address ON users (address);


-- API

namespace ecommerce_api;

public record class ProductDto(int ProductId, string Name, double Price);


// HTTP
GET http://localhost:5122/hello

###
GET http://localhost:5122/products

###
GET http://localhost:5122/products/4

###
POST http://localhost:5122/products
Content-Type: application/json

{
"ProductId" : 4,
"Name" : "iphone 15",
"Price" : 101.5
}

###
DELETE http://localhost:5122/products/4

###
GET http://localhost:5122/users

###
GET http://localhost:5122/orders

###
GET http://localhost:5122/categories

using ecommerce_api;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

List<ProductDto> products = new()
{
  new ProductDto(1, "Iphone 11", 350.50),
  new ProductDto(2, "Samsung Galaxy", 450.50),
  new ProductDto(3, "Motorola XII", 250.50),
};

app.MapGet("/products", () => products);


app.MapGet("/products/{id}", (int id) => products.Find(product => product.ProductId == id));


app.MapPost("/products", (ProductDto newProduct) =>
{
  products.Add(newProduct);
  return products;
});

app.MapDelete("/products/{id}", (int id) =>
{
  var productToRemove = products.Find(product => product.ProductId == id);
  if (productToRemove != null)
  {
    products.Remove(productToRemove);
    return Results.Ok();
  }
  else
  {
    return Results.NotFound();
  }
});


app.MapGet("/users", () => "all the users are returned!");
app.MapGet("/categories", () => "all the categories are returned!");
app.MapGet("/orders", () => "all the orders are returned!");

app.Run();


// 
